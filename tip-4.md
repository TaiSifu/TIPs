---
tip: 4
title: “道理”通证
description: 太乙角色的同质化通证合约，用于金融通货
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-02-07
requires: 5, 6
---

### 地址

|   网络   | 合约地址  |
|-----------------|-------------|
| ARBITRUM Goerli Network | [0x1c28fe3A46e3E2dd5a924107FECd0A2f605c7E54](https://goerli.arbiscan.io/address/0x1c28fe3A46e3E2dd5a924107FECd0A2f605c7E54) |


## 摘要

“道理”是太乙世界的一种同质化通证，用于金融通货。

## 动机

人们通过太乙协议以及之上建设的各个太乙世界，会进行大量的生产劳动，产出各种虚拟的资源、道具和信息结构。这些信息在角色之间交换的时候，其价值需要有一种通用的等价物来传递。因此，需要一种统一使用的同质化通证来进行此类价值传递。另一方面，由于通货的发行具有对经济的调节作用，也需要一种机制来发行“道理”。

## 规范说明

1. 太乙世界的同质化通证合约，参考[TIP-6](./tip-6.md)。
2. 中文名称是“道理”，简称“道”。该合约作为太乙协议寄出，一旦部署，不能被升级或替换。
3. 部署的合约中，通证名称是"Taiyi Daoli"，通证符号是"DAOLI"。
4. “道理”在太乙世界中只能通过盘古角色（[TIP-5](./tip-5.md)）操作发行。当盘古角色的所有权转交给太乙岛后，道理就由太乙岛提案发行了。

**注意**：
 - 规范中使用的程序语法使用Solidity语言，版本不低于`0.8.0`。
 - 调用者必须（MUST）处理任何通过 `returns (bool success)` 返回的 `false`状态。调用者不能（MUST NOT）对这种函数做出不会返回 `false`的假定！

### 方法和函数

#### claim

- 由盘古（角色）发行指定数量的“道理”，并将这些道理转给一个指定的角色。
- 调用者需要持有或被授权盘古角色。

``` js
function claim(uint256 _operator, uint256 _actor, uint256 _amount) public
```

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/world/AssetDaoli.sol

## 版权许可

依照[CC0](../LICENSE.md)规范放弃。
