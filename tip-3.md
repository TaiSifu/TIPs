---
tip: 3
title: 太乙合约路由序号
description: 太乙合约地址可以通过一个路由器指定序号获得
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-02-11
---

|   网络   | 合约地址  |
|-----------------|-------------|
| ARBITRUM Goerli Network | [0x816a1b3066e70DbF842f8ebC42cfdB1D737f3D03](https://goerli.arbiscan.io/address/0x816a1b3066e70DbF842f8ebC42cfdB1D737f3D03) |


## 摘要

太乙协议中各个合约在部署后，会有一个以太坊地址。通过部署在链上的一个地址路由器，可以通过约定的合约序号来获得这个合约的实际地址。

## 规范说明

1. 如果TIP是一个部署了可被使用的太乙合约，TIP序号就是该合约在地址路由器上注册的序号。
2. 部署了一个共享的地址路由器，基础的太乙合约均在这个路由器注册。
3. 路由器地址的注册，只有盘古角色（[TIP-5](./tip-5.md)）有权操作。

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/world/WorldContractRoute.sol

## 版权许可
依照[CC0](../LICENSE.md)规范放弃。
