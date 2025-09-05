---
layout:     post
title:      一文读懂 Python 复杂网络分析库 NetworkX
date:       2025-09-05
header-img: img/post-bg-desk.jpg
catalog: true
---

NetworkX 是 Python 生态中无可替代的**复杂网络建模与分析**工具。通过它，你可以在三五行代码内构建社交网络、交通路网、知识图谱，甚至深度神经网络的可视化结构。本文将用 **“安装—建模—可视化—算法”** 四步曲，带你一口气完成从入门到实战的跃迁。  

👉 [点此解锁更多 Python 数据科学精彩实战](https://okxdog.com/)

---

## 一、安装与环境准备

| 方法        | 命令／步骤 |
|-------------|-------------|
| pip         | `pip install networkx` |
| conda       | `conda install networkx` |
| 国内镜像加速| `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple networkx` |
| 可选加速可视化 | 安装 `pygraphviz` 前先在系统安装 **Graphviz** |

```bash
# Ubuntu
sudo apt-get install graphviz graphviz-dev
pip install pygraphviz
# Windows 建议使用 wheel 安装
```

---

## 二、核心数据结构：四种图

NetworkX 为不同需求给出了**四种**图类型：

1. **Graph**   无向无重边  
2. **DiGraph**  有向无重边  
3. **MultiGraph** 无向可重边  
4. **MultiDiGraph** 有向可重边  

创建一条命令即可：

```python
import networkx as nx
G = nx.Graph()      # 空的无向图
D = nx.DiGraph()    # 空的有向图
```

`G.clear()` 一键清空，方便在**循环实验**中重复使用对象。

---

## 三、Graph：无向图的全部姿势

### 1. 节点操作

- **增删**:  
  `G.add_node('Alice')`, `G.add_nodes_from(['Bob','Carol'])`  
  `G.remove_nodes_from(['Bob'])`  
- **查询**:  
  `G.nodes()`, `G.number_of_nodes()`  
- **进阶玩法**:  
  把子图作为单一节点加入，实现分层可视化。

```python
H = nx.path_graph(5)
G.add_node(H)       # “子图即节点”
```

### 2. 边操作

- **增删**：  
  三元组 `(u,v,weight)` 无缝携带**边权重**。  
  ```python
  G.add_weighted_edges_from([(1,2,0.5),(2,3,1.2)])
  ```

- **遍历**：  
  ```python
  for u,v,d in G.edges(data='weight'):
      if d < 1:
          print(u,v,d)
  ```

边权重支持浮点、整数，甚至**字典嵌套属性**。

### 3. 属性附件

把任何 Python 对象当属性挂到**图、节点、边**：

```python
G.graph['title'] = 'karate_club'      # 图
G.nodes[1]['gender'] = 'male'         # 节点
G[1][2]['label'] = 'friend'           # 边
```

属性字段全部放在标准字典中，随时增删。

### 4. 双向转换

```python
# 有向→无向
U = G.to_undirected()
# 无向→有向
D = nx.DiGraph(U)
```

---

## 四、DiGraph：有向图与精美案例

有向图与无向图的 API 几乎一致，只要记住两点：

- **draw 时使用 arrow=True 画箭头**  
- 强/弱连通分量的算法不同  

接下来 7 个**可视化案例**全部基于 DiGraph，复制即用。

### 1. k-out-随机图（边兼具透明度渐变）

```python
G = nx.random_k_out_graph(10, 3, 0.5)
pos = nx.spring_layout(G)
nx.draw_networkx_edges(G, pos, arrowstyle='->',
                       edge_color=range(G.number_of_edges()),
                       edge_cmap=plt.cm.Blues)
```

### 2. 环状树（适合展示层级）

```python
T = nx.balanced_tree(3, 5)
pos = graphviz_layout(T, prog='twopi')
nx.draw(T, pos, node_size=8, alpha=0.6)
```

### 3. 甘特权重图（阈值高亮）

```python
elarge = [(u,v) for u,v,d in G.edges(data=True) if d['weight']>0.5]
esmall = [(u,v) for u,v,d in G.edges(data=True) if d['weight']<=0.5]
nx.draw_networkx_edges(G, pos, edgelist=elarge, width=6)
nx.draw_networkx_edges(G, pos, edgelist=esmall, width=3, style='dashed')
```

### 4. Random Geometric Graph（随机几何图）

常用于无线传感器网络仿真，200 个节点可视化 0.05 秒完成：

```python
G = nx.random_geometric_graph(200, 0.125)
pos = nx.get_node_attributes(G, 'pos')
nx.draw_networkx_nodes(G, pos, node_color=[...], cmap='Reds')
```

👉 [领取 15 个 NetworkX 彩色样式模板](https://okxdog.com/)

---

## 五、实战：把 DNN 画成论文级结构图

步骤：构建 DAG → 自定义坐标 → 去除标签 → 手动加文字描述 → 导出高清 png。

### Step1 构建网络

```python
G = nx.DiGraph()
vertex = ['v'+str(i) for i in range(1,22)]
G.add_nodes_from(vertex)
edges = [...]   # 三层感知机 stride
G.add_edges_from(edges)
```

### Step2 绘图

```python
pos = { # 精细坐标字典
 'v1':(-2,1.5), 'v2':(-2,0.5), ..., 'v21':(2,-1)
}
nx.draw(G,pos,with_labels=False,node_size=300,node_color='r',edge_color='k')
plt.xlim(-2.5,2.5); plt.ylim(-3.3,3.3)
plt.savefig('dnn_raw.png')
```

### Step3 OpenCV 增强（加方框 + 文字）

```python
import cv2
img = cv2.imread('dnn_raw.png')
cv2.rectangle(img,(80,120),(450,420),(255,0,0),2)
cv2.putText(img,'Hidden Layer',(180,450),1,1.8,(0,255,0),2)
cv2.imwrite('dnn_final.png',img)
```

三种尺寸布局 640×480、1280×720、300 dpi 任君挑选，**论文一次过审**。

---

## 六、图论算法：以最短路径为例

### 单源最短路径（Dijkstra）

```python
G = nx.DiGraph()
G.add_weighted_edges_from([(0,1,1),(0,3,3),(3,6,5),(6,7,2)])
print(nx.dijkstra_path(G,0,7))       # [0,3,6,7]
print(nx.dijkstra_path_length(G,0,7))# 10
```

其他热门算法：

- **PageRank** `nx.pagerank(G)`  
- **最大流** `nx.maximum_flow(D, 's', 't')`  
- **社区发现** `nx.community.louvain_communities(G)`

---

## 七、常见疑难 FAQ

**Q1**: matplotlib 绘图时报错 “tight_layout Empty sequence”？  
**A**: PyCharm → Settings → Tools → Python Scientific，取消 “Show plots in tool window” 即可。

**Q2**: 多重量边丢失、::<、多图重叠？  
**A**: 确认使用的是 **MultiGraph/MultiDiGraph** 而非 Graph/DiGraph。

**Q3**: 节点过多时内存爆炸？  
**A**: 使用 `nx.degree_centrality()` 先行筛选 Top 10%，再缩略可视化。

**Q4**: 想做 **交互式网络可视化** 怎么办？  
**A**: 导出 gexf → **Gephi** 或 **Cytoscape** 打开；亦可调用 **Plotly** 在线渲染。

**Q5**: 如何为边添加动态宽度？  
**A**: 把权重映射到 `width` 参数：`width=[d['weight']*5 for _,_,d in G.edges(data=True)]`。

**Q6**: Jupyter Lab 无法 inline ？  
**A**: 魔法命令 `%matplotlib inline` 即可，新版 Lab 默认启用。

---

## 八、扩展阅读与资源

- 官方 Docs: https://networkx.org/documentation/stable  
- NetworkX 中文速查表（PDF）  
- 《复杂网络导论》配套代码仓库：https://github.com/albertobsd/networkx-cookbook

用 NetworkX，让任何「关系数据」秒变直观图谱。现在，就打开 Jupyter 尝试一下今天的示例吧！