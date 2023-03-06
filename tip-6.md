---
tip: 6
title: 太乙同质化通证
description: 一种以太乙角色为操作者的同质化通证协议
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-02-08
requires: 30
---

## 摘要
这是一种基于[ERC20](https://eips.ethereum.org/EIPS/eip-20)扩展的同质化通证协议，太乙角色之间可以通过角色Token的序号作为这种通证操作的发起者、出账人或者入账人。

## 动机

对太乙协议的访问通常都是参与者通过太乙角色（[TIP30](./tip-30.md)）来完成的。为了方便在太乙世界中操作诸如资源、货币、信誉等同质化通证，就需要一套以太乙角色为操作者的通证合约框架。以太坊上的同质化通证协议[ERC20](https://eips.ethereum.org/EIPS/eip-20)是以地址为操作者或者接收者的，因此，这里对ERC20进行了扩展，以支持基于角色的通证操作。

## 规范说明

太乙同质化通证协议是一个兼容[ERC20](https://eips.ethereum.org/EIPS/eip-20)的同质化通证合约框架。该合约以太乙协议的角色序号（也是角色通证序号）作为通证操作参与者。本协议可作为各种太乙世界的资源或者货币的部署合约的框架。

### 方法和函数

#### allowanceActor
- 返回指定太乙角色授权某个角色的通证操作数量
``` js
function allowanceActor(uint256 _owner, uint256 _spender) external view returns (uint256);
```

#### balanceOfActor
- 返回属于指定太乙角色的通证数量
``` js
function balanceOfActor(uint256 _owner) external view returns (uint256);
```

#### approveActor
- 指定太乙角色授权自己通证给某个角色可以支配的数量
``` js
function approveActor(uint256 _from, uint256 _spender, uint256 _amount) external;
```

#### transferActor
- 指定太乙角色转给某个角色一定数量的通证
``` js
function transferActor(uint256 _from, uint256 _to, uint256 _amount) external;
```

#### transferFromActor
- 指定太乙角色执行一笔转账，将一个角色的指定数量通证转给另外一个角色
- 出账角色需要授权执行者足够数量的通证操作权。
``` js
function transferFromActor(uint256 _executor, uint256 _from, uint256 _to, uint256 _amount) external;
```
    
#### claim
- 操作员向指定角色发行一定数量的通证
- 操作员需要具备发行通证的权限。
``` js
function claim(uint256 _operator, uint256 _actor, uint256 _amount) external;
```
    
#### withdraw
- 操作员将指定角色拥有的通证，提取一定数量给该角色所有者的地址。
- 操作员需要具备提取通证的权限。
``` js
function withdraw(uint256 _operator, uint256 _actor, uint256 _amount) external;
```

### 合约事件

#### FungibleTransfer
- 通证转账时候的事件，记录了出账角色、入账角色和转账数量。
``` js
event FungibleTransfer(uint256 indexed from, uint256 indexed to, uint256 amount);
```
    
#### FungibleApproval
- 通证授权时候的事件，记录了授权角色、被授权角色和授权的数量。
``` js
event FungibleApproval(uint256 indexed from, uint256 indexed to, uint256 amount);
```

## 基本原理

- 同质化通证的原理参考[ERC20](https://eips.ethereum.org/EIPS/eip-20)。
- 具备发行通证权限和提取通证权限的操作者，通常是“噎明”（[TIP4](./tip-4.md)）角色。
- 根据太乙角色的隐式托管（参考[TIP30](./tip-30.md)），发行通证、角色和角色之间转账，这些操作执行最终仍然是按照ERC20在角色Holder地址之间转移的。

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/world/WorldFungible.sol

## 版权许可

依照[CC0](../LICENSE.md)规范放弃。
