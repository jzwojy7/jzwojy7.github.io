---
layout:     post
title:      一文精通 vsBond：vsToken 不可或缺的另一半
date:       2025-09-05
header-img: img/post-bg-desk.jpg
catalog: true
---

> 本文字数约 1,800 字，阅读约需 5 分钟  
> 关键词：Bifrost、vsBond、SALP、流动性衍生资产、波卡 Crowdloan、期货折价

---

## vsBond 的诞生缘起：为流动性“拆骨”而非“拆弹”

在波卡（Polkadot）与 Kusama 都要**竞拍平行链插槽**的规则下，用户需要把 DOT / KSM 长期锁仓给项目方来提供 Crowdloan 支持。**如果所有代币都冻结，DeFi 生态将失去活跃度。**  
为此，Bifrost 开发了 **SALP（Slot Auction Liquidity Protocol）**，通过发放两种衍生品——**同质化的 vsToken（vsDOT / vsKSM）** 与 **半同质化的 vsBond**，把“到期兑付”和“流动性交易”这两件事剥离开来：

* **vsToken（vsDOT / vsKSM）**：随时流通、收益稳定，占**质押价值的 50%+**；  
* **vsBond**：记录每轮插槽的“到期日期 + 对应平行链”的兑付凭证，额度灵活，价格随时间稀释。

因此，vsBond 天生不是一个独立的投机筹码，而是 vsToken 的另一半：**二者合一，等于原先锁仓的 DOT/KSM**。

👉 [想先人一步体验 vsToken 与 vsBond 组合拳，立刻前往实战演练区](https://okxdog.com/)

---

## 三个关键词看懂 vsBond

| 关键词 | 说明 | 影响流动性 |
|---|---|---|
| 兑付期 | vsBond 背面写的赎回日期，未到不能赎回 | 决定折溢价幅度 |
| 对应资产 | DOT 或 KSM，取决于参与的是 Polkadot 还是 Kusama Crowdloan | 价格计算基础 |
| 平行链名称 | 只用于 UI 展示，实质不影响价值 | 可忽略 |

如 vsBond(20241015-KSM-moonbeam) 与 vsBond(20241015-KSM-aca) 在 **兑付期当天以后其实等值**。

---

## vsBond 的七大核心用法

### 1. 到期赎回：零摩擦领回 DOT / KSM
在兑付日及之后的任意时刻，**每 1 vsToken + 1 vsBond 可换回 1 原生代币**。

### 2. 单独挂单：赚取时间折价差
vsBond Marketplace 已上线，用户可挂单“吃折价”或“卖时间溢价”。  
行情围绕 **（到期时间、市场利率、原生代币价格）** 三大因子震荡。

### 3. 组合现货：组合交易功能
把 vsToken 与 等数量 vsBond 打包为“Future DOT / KSM”组合单，即可**做波卡生态内 DOT 或 KSM 的无缝期货**。

### 4. Farming 额外收益
锁定“vsToken + vsBond”，Bifrost 与平行链项目方将**按总锁定量补贴奖励**。收益率 = 公示年息补贴 + 额外众筹奖励再分配。

### 5. 抵扣借贷抵押
部分 DeFi 协议已接受 vsToken 为抵押物，**vsBond 则作为可替换的“块头凭证”**，有机会叠加息差。

### 6. 二级做市搬砖
vsBond 折现率 = **现货价 × 折现系数**，专业量化可用不同交割日 vsBond 套利。

### 7. 长尾效应管理工具
若你来不及用 vsToken 再做其他用途，**直接挂单卖出 vsBond** 也能快速收拢资金，不落袋 vsToken 即可。  

👉 [即刻查看如何质押 + 交易一体化，不用抢秒卖单](https://okxdog.com/)

---

## vsBond 价格结构：看懂“时间折损”与“波动加成”

1. **到期前组合价** = 现货 DOT/KSM 的期望现值（PV）  
   例：距离兑付 1 年，市场加权年化 15% → 组合价 ≈ 0.85 DOT  
   其中 vsToken 分配固定比例 0.5 DOT，则 vsBond 理论价 ≈ 0.35 DOT。

2. **到期后组合价 = 1 DOT**  
   此时 vsToken + vsBond 可直接 1:1 赎回，再不见到期折价。

3. **波动放大器**  
   越逼近兑付日，vsBond 所承载的“剩余时间价值”就越低，价格曲线呈 **倒数衰减**，交易量剧烈放大。

---

## FAQ：关于 vsBond 最常问的 5 件事

**Q1：vsBond 会过期作废吗？**  
A：不会。只要不赎回，它永远有效；只是过了兑付日，其“时间权利”变零，市场照价折算。

**Q2：我把 vsBond 卖掉了，Crowdloan 奖励会终止吗？**  
A：不会。奖励是对“原始 Crowdloan 贡献地址”发放，只要初始地址没变，vsBond 的买卖不影响空投和分润。

**Q3：囤一堆 vsToken 不参与竞拍会贬值吗？**  
A：vsToken 本身锚定 DOT/KSM 的质押收益，**份额无上限、腋下永存**。真正稀缺的是 vsBond，囤积 vsToken 并不会稀释其收益率。

**Q4：为什么交易市场用订单簿而不是 AMM？**  
A：由于 vsBond 必须绑定“兑付日期”，品类繁多，AMM 会出现分段流动性碎片化；订单簿让 **限价挂单、做市空间大、对手滑点更低**。

**Q5：我能否把 vsToken 与 vsBond 永久合成一个 fToken？**  
A：社区曾讨论过此改进，但为避免复杂度，暂搁置。不过 **组合交易市场** 已等效支持“一篮子打包”效果。

---

## 实战案例：新手也能 3 步玩转 vsBond

假设现在距离兑付日期 180 天，KSM 现货价 = 100 USDT：

1. **市场发现**  
   vsToken-KSM = 48 USDT  
   vsBond-180d-KSM = 32 USDT  
   组合价 = 80 USDT（折价 20%）

2. **买入 vsBond**  
   你花费 32 USDT 购入 1 张 vsBond-180d-KSM。

3. **到期操作**  
   180 天后，用 48 USDT 市价再买 1 份 vsToken-KSM，**兑换 100 USDT 的现货 KSM**；或提前挂单，吃进时间折价 20%。

年化 ≈ (100 / 80)^ (365/180) −1 ≈ 58% 的无杠杆收益。

---

## vsBond、vsToken、原生资产三角关系图  
（此图已抽象为文字描述）

* 左：vsToken ≈ 抵押资产的“股权证”，随时可流通  
* 右：vsBond ≈ 抵押资产的“提货单”，兑付时点  
* 上端：原生 DOT / KSM  
* 下方：诸多 DeFi 应用场景，质押收益叠加 vsBond 市场套利通道

---

## 结语：多一种选择，多一重流动性

vsBond 的诞生，并非设计复杂的“炫技”，而是把**流动性和时间属性解耦**后的必然结果：  
- **对于长线 Hodler**：锁定 vsBond 得到稳定预期，不为短期波动恐慌。  
- **对于短线交易员**：买卖 vsBond 与期现套利空间，提升资金利用率。  

SALP 的双衍生品架构，本质上让“一条链的资产”拆出了 **两条自由跑道**——**一条跑 vsToken 的赛道，另一条跑 vsBond 的赛道**。  
用最简的一句话概括：**“vsToken 卖流动性，vsBond 卖时间；自由组合或单独拆卖，ROI 不就上来了？”**