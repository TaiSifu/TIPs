---
tip: 6
title: 太乙非同质化通证
description: 一种以太乙角色为操作者的非同质化通证协议
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-02-08
requires: 30
---

## 摘要
这是一种基于[ERC721](https://eips.ethereum.org/EIPS/eip-721)扩展的非同质化通证协议，太乙角色之间可以通过角色Token的序号作为这种通证操作的发起者、出账人或者入账人。

## 动机
对太乙协议的访问通常都是参与者通过太乙角色（[TIP30](./tip-30.md)）来完成的。为了方便在太乙世界中操作诸如道具、社交身份等非同质化通证，就需要一套以太乙角色为操作者的通证合约框架。以太坊上的非同质化通证协议[ERC721](https://eips.ethereum.org/EIPS/eip-721)是以地址为操作者或者接收者的，因此，这里对ERC721进行了扩展，以支持基于角色的通证操作。

## 规范说明
太乙非同质化通证协议是一个兼容[ERC721](https://eips.ethereum.org/EIPS/eip-721)的非同质化通证合约框架。该合约以太乙协议的角色序号（也是角色通证序号）作为通证操作参与者。本协议可作为各种太乙世界的道具、名称、区域或者社交身份的部署合约的框架。

### 方法和函数

#### tokenOfActorByIndex
- 返回指定太乙角色所有的通证中，指定遍历序号的通证
``` js
function tokenOfActorByIndex(uint256 _owner, uint256 _index) external view returns (uint256);
```
    
#### balanceOfActor
- 返回指定太乙角色拥有通证的数量
``` js
function balanceOfActor(uint256 _owner) external view returns (uint256);
```

#### ownerActorOf
- 返回指定通证的拥有者角色
``` js
function ownerActorOf(uint256 _tokenId) external view returns (uint256);
```
    
#### getApprovedActor
- 返回指定通证的授权委托角色
``` js
function getApprovedActor(uint256 _tokenId) external view returns (uint256);
```
    
#### isApprovedForAllActor
- 查询指定角色的托管地址上的所有通证是否全权委托给了某个角色
``` js
function isApprovedForAllActor(uint256 _owner, uint256 _operator) external view returns (bool);
```

#### approveActor
- 委托某个角色代理指定角色行使指定通证的相关操作
``` js
function approveActor(uint256 _from, uint256 _to, uint256 _tokenId) external;
```
    
#### setApprovalForAllActor
- 委托某个角色全权代理指定角色拥有的所有通证的各种操作
``` js
function setApprovalForAllActor(uint256 _from, uint256 _operator, bool _approved) external;
```
    
#### safeTransferActor
- 将指定通证从一个角色转给另外一个角色
- 该操作使用目标调用数据保障通证调用安全
``` js
function safeTransferActor(uint256 _from, uint256 _to, uint256 _tokenId, bytes calldata _data) external;
```
    
#### safeTransferActor
- 将指定通证从一个角色转给另外一个角色
- 该操作自动保障通证调用安全
``` js
function safeTransferActor(uint256 _from, uint256 _to, uint256 _tokenId) external;
```
    
#### transferActor
- 将指定通证从一个角色转给另外一个角色
``` js
function transferActor(uint256 _from, uint256 _to, uint256 _tokenId) external;
```
    
#### safeTransferFromActor
- 将指定通证从一个角色转给另外一个角色
- 该操作由一个执行角色执行
- 该操作使用目标调用数据保障通证调用安全
``` js
function safeTransferFromActor(uint256 _executor, uint256 _from, uint256 _to, uint256 _tokenId, bytes calldata _data) external;
```
    
#### safeTransferFromActor
- 将指定通证从一个角色转给另外一个角色
- 该操作由一个执行角色执行
- 该操作自动保障通证调用安全
``` js
function safeTransferFromActor(uint256 _executor, uint256 _from, uint256 _to, uint256 _tokenId) external;
```
    
#### transferFromActor
- 将指定通证从一个角色转给另外一个角色
- 该操作由一个执行角色执行
``` js
function transferFromActor(uint256 _executor, uint256 _from, uint256 _to, uint256 _tokenId) external;
```

### 合约事件

#### NonfungibleTransfer
- 通证转移时候的事件，记录了出账角色、入账角色和通证号。
``` js
event NonfungibleTransfer(uint256 indexed from, uint256 indexed to, uint256 indexed tokenId);
```

#### NonfungibleApproval
- 通证授权委托时候的事件，记录了授权角色、被授权角色和授权的通证。
``` js
event NonfungibleApproval(uint256 indexed owner, uint256 indexed approved, uint256 indexed tokenId);
```

#### NonfungibleApprovalForAll
- 角色拥有通证全权授权委托时候的事件，记录了授权角色、被授权角色和授权变更。
``` js
event NonfungibleApprovalForAll(uint256 indexed owner, uint256 indexed operator, bool approved);
```


## 基本原理
- 非同质化通证的原理参考[ERC721](https://eips.ethereum.org/EIPS/eip-721)。
- 具备铸造太乙非同质化通证权限和销毁通证权限的操作者，通常是“噎明”（[TIP4](./tip-4.md)）角色。
- 根据太乙角色的隐式托管（参考[TIP30](./tip-30.md)），铸造通证、角色和角色之间转移，这些操作执行最终仍然是按照ERC721在角色Holder地址之间进行的。

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/world/WorldNonfungible.sol

## 讨论
1. 五大NFT（https://mirror.xyz/taiyidao.eth/3bCoHzrUWM8IuYCWUFC5oXYev_QhfoR8ablZ_9GJgc4）

## 版权许可

依照[CC0](../LICENSE.md)规范放弃。
