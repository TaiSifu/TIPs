---
tip: 4
title: 太乙“噎明”注册表
description: 一个记录获得了“噎明”权限的角色的数据集
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-03-06
---

|   网络   | 合约地址  |
|-----------------|-------------|
| ARBITRUM Goerli Network | [0xA0146938AA98765d7471dCe3d52FcF9a984637eE](https://goerli.arbiscan.io/address/0xA0146938AA98765d7471dCe3d52FcF9a984637eE) |


## 摘要

太乙协议以及各个太乙世界里，“噎明”注册表是记录各种行使管理、控制甚至设计操作的内置角色的合约。

## 动机

太乙协议以及建设在之上的各个太乙世界，维护着一系列虚拟且情景化的因果逻辑。世界的发展和各种资源的生产都是在这些因果逻辑中完成，而很多因果逻辑又基于太乙角色[TIP30](./tip-30.md)来参与操作触发。因此，需要有一批独立于任何个人意志的中立角色来执行，这些中立角色也不应该属于任何个人。我们称这些有权限行使因果规则、管理、控制甚至设计操作的角色为“噎明”。

## 规范说明

1. “噎明”只是一个代称，并不特指某一个角色，而是指具备一系列权限的一类角色。
2. 在“噎明”注册表上注册一个角色，就等于赋予了这个角色“噎明”的权限。
3. 只有太乙岛（DAO）有权限注册一个角色成为“噎明”。

### 方法和函数

#### setTaiyiDAO
- 设置新的太乙岛地址。只有当前的太乙岛才有权限设置。
``` js
function setTaiyiDAO(address _taiyiDAO) external;
```
    
#### YeMings
- 返回指定角色作为“噎明”管理或者控制的合约地址
``` js
function YeMings(uint256 _actor) external view returns (address);
```

#### YeMings
- 查询指定角色是否是“噎明”
``` js
function isYeMing(uint256 _actor) external view returns (bool);
```

### 合约事件

#### TaiyiDAOUpdated
- 太乙岛地址变更的事件，记录了新的太乙岛地址。
``` js
event TaiyiDAOUpdated(address taiyiDAO);
```

## 基本原理

- “噎明”角色的NFT通常不属于任何个人地址，而是属于某个合约，这种合约通过这个噎明角色来与其他太乙合约互操作。这些合约部署后，合约设计者会铸造一个角色并转让给这个合约拥有。经过太乙岛审计，认为该合约的代码，以及以这个角色作为操作员处理的各种合约事务都合法且没有问题时，太乙岛会投票执行注册该角色为“噎明”的操作。

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/world/dataset/WorldYemings.sol

## 讨论
1. 太乙岛治理测试用例（https://mirror.xyz/taiyidao.eth/v3LhEZsRNOD_1XUniieYy18i9oBFEROY6sGtBgVMRls）
2. 再生一个娃（https://mirror.xyz/taiyidao.eth/H1AtWsw_rgjtkDHYfftTtGPGCZveqUFy5ZsI_52uZLI）
3. 再谈一下生娃娃（https://mirror.xyz/taiyidao.eth/HzdtZi2kIqUiCOQkfDov9rLrBjxEPmPodt8-pxMkR7Y）

## 版权许可
依照[CC0](../LICENSE.md)规范放弃。
