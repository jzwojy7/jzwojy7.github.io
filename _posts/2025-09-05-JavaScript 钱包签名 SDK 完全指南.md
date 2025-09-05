---
layout:     post
title:      JavaScript 钱包签名 SDK 完全指南
date:       2025-09-05
header-img: img/post-bg-desk.jpg
catalog: true
---

> 关键词：JavaScript 签名 SDK、钱包签名、DEX API、Web3 集成、私钥管理、区块链交易、加密货币

## 快速概览
Js-wallet-sdk 是一款基于 TypeScript / JavaScript 的多链钱包解决方案，内嵌常用的密码学算法与通用功能。开发者可直接使用它完成私钥生成、地址创建、交易拼装及离线签名等常见钱包动作。目前覆盖 BTC、ETH、SOL、APT、SUI 等主流网络，未来还将持续扩展。  
👉 [立即体验极速上手的钱包签核示范](https://okxdog.com/)

### 支持平台
由于完全以 JavaScript 编写，SDK 兼容：
* Web 前端（Chrome、Firefox、Safari）
* Node.js 服务端
* Electron／React Native／Uni-app 等跨端框架

## 安装与构建

### NPM 安装（推荐）
公共总包（一次性引入全部链）  
```bash
npm install @okxweb3/js-wallet-sdk
```

单币种精准引入示例  
```bash
# 仅以太坊
npm install @okxweb3/coin-ethereum
# 仅比特币
npm install @okxweb3/coin-bitcoin
```

### 本地源码构建
1. 克隆仓库  
2. 安装依赖并打包  
```bash
git clone https://github.com/okx/js-wallet-sdk.git
cd js-wallet-sdk && npm ci && npm run build
```

## 核心功能总览
| 模块 | 作用 | 高频操作 |
|---|---|---|
| crypto-lib | 密码学基础封装：BIP32、BIP39、ECDSA、ED25519 | 助记词 → 私钥 → 地址 |
| coin-base | 链级接口抽象：地址验证、手续费预估、交易哈希计算 | 快速接入多链共识层 |
| coin-* | 具体链的实现：交易拼装、离线签名、消息签名 | 与区块网络直接交互 |

以最常见的以太坊场景举例，仅需 3 步即可完成转账：
```javascript
import { EthWallet } from '@okxweb3/coin-ethereum';

// 1. 生成私钥
const privateKey = await EthWallet.getRandomPrivateKey();
// 2. 推导地址
const address   = await EthWallet.getNewAddress(privateKey);
// 3. 签名交易
const signedTx  = await EthWallet.signTransaction(txData, privateKey);
```

## 单链功能拆解
本节逐链梳理关键调用，方便 Ctrl+F 速查。

### coin-base：跨链基座
* getRandomPrivateKey / getDerivedPrivateKey  
* getNewAddress / validAddress  
* signTransaction / signMessage / verifyMessage  
* estimateFee：快速估算 gas  
👉 [查看 coin-base 参数与返回示例](https://okxdog.com/)

### BTC 生态 (BTC/BCH/LTC/DOGE/BSV)
```
getAddressByPublicKey、calcTxHash、SegWit 版本地址推导等
支持地址形态：Legacy / P2SH / Bech32 / Bech32m
```

### EVM 生态 (ETH、Arbitrum、Base、Polygon...)
支持 EIP-1559 & 遗留费用模型，并提供硬件钱包通道：
* getHardWareRawTransaction：为 Ledger / Trezor 生成待签原文  
* validSignedTransaction：本地验签防篡改

### Cosmos 生态 (Atom、Osmosis、Juno...)
```
BIP44 路径统一 m/44'/118'/0'/0/0，支持多 token 并行签名
```

### Solana
```
拥有强大的 ED25519 原生支持
实现 transfer / tokenTransfer / dApp 消息三种签名场景
```

### Layer2 & ZK 系列 (Starknet、zkSync、ZKSpace...)
```
signMessage 额外支持「帐户抽象」路径与 changePubkey 交易
```

## FAQ

### Q1：我的 DApp 只用到以太坊主网与 L2，需要安装全部模块吗？
完全不必。如需最小化体积，仅需  
```bash
npm install @okxweb3/coin-ethereum @okxweb3/coin-base
```
即可离线签名 ETH、ERC-20 及所有 EVM L2。

### Q2：私钥是否会泄露到网络？
SDK 所有签名过程均为 **本地计算**，私钥 never leave your memory。对于硬件钱包场景，SDK 只生成未签名原文与最终签名，敏感数据始终停留在 Ledger/Trezor 里。

### Q3：为何 estimateFee 返回的 gasPrice 为 0？
该字段代表「网络实时费率」需要连网 RPC 才准确。若离线调用，SDK 会给出默认 0 值。推荐配合 `@ethersproject/providers` 等库在线刷新后回填。  
👉 [一键阅览集成最佳实例](https://okxdog.com/)

### Q4：测试币水龙头 & 用例在哪里？
每一链的 GitHub 目录下均附 `/tests` 用例及 README。举比特币为例：
```bash
cd packages/coin-bitcoin/tests
npm run test
```
自动跑通主网与测试网签名。

### Q5：可生成助记词吗？
配合 crypto-lib 一行搞定：
```javascript
import { bip39 } from '@okxweb3/crypto-lib';
const mnemonic = bip39.entropyToMnemonic(bip39.randomBytes(16));
```
密码强度默认 128 bit，支持中/英文词表。

## 常用派生路径速查
* BTC 普通地址：`m/44'/0'/0'/0/0`  
* BTC SegWit：`m/84'/0'/0'/0/0`  
* ETH & L2：`m/44'/60'/0'/0/0`  
* Cosmos：`m/44'/118'/0'/0/0`  
* Solana：`m/44'/501'/0'/0/0`  

将路径作为 DerivePriKeyParams 的字段，即可快速生成兼容主流钱包的私钥/地址。

## 小结
Js-wallet-sdk 提供「多链兼容 + 极度精简 + 完全离线」三位一体的签名能力。希望本篇指南助你 15 分钟完成从入门到上线的整个流程。所有模块开源可查，若在使用中遇到更多个性化需求，欢迎深入阅读 GitHub 实战代码或直接参与贡献。祝开发愉快！