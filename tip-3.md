---
tip: 3
title: 太乙角色
description: 太乙角色通证合约，太乙身份的定义
author: 太师傅(@TaiSifu)
discussions-to: 
status: Draft
type: Standards Track
category: Core
created: 2023-02-07
requires: 4, 5
---

### 地址

|   网络   | 合约地址  |
|-----------------|-------------|
| ARBITRUM Goerli Network | [0xa6aCb1c4876d6824a8ec02a624e827d0cED49F72](https://goerli.arbiscan.io/address/0xa6aCb1c4876d6824a8ec02a624e827d0cED49F72) |


## 摘要

“太乙角色”是一种非同质化通证(NFT)，任何人都可以从角色合约铸造太乙角色通证（Token）。太乙角色的铸造是无准入的自由铸造，只需要铸造者支付一定的铸造费用。铸造费用是用太乙协议中的一种同质化通证（FT）来支付的。

## 动机

“太乙协议”尝试引导出一种以神话和武侠为题材的虚拟身份，形成架空世界虚拟社区和治理模式。这种身份需要一种特定的非同质化通证来承载。通过该通证，无论是太乙协议的实施者、太乙世界的建设者还是用户，都能和各种协议、架构、数据集进行统一地交互。

太乙协议或者太乙世界中使用了各种各样的NFT和FT，来表示自然资源、虚拟物品等等，这些通证通常是属于某一个太乙角色的。因此，需要在太乙角色上维护其拥有的各种通证，这也要求需要有一个可以内涵其他事物的整体性的身份存在。

太乙协议及其之上构建的各个世界，都是去中心化的无准入系统。任何人或者机器都需要自由且公平地获得不止一个身份通证。


## 规范说明

“太乙角色”通证合约是一个兼容[ERC721](https://eips.ethereum.org/EIPS/eip-721)的非同质化通证合约。该合约作为太乙协议的身份基础，一旦部署就不能被升级或替换。

1. 太乙角色合约具有[ERC721](https://eips.ethereum.org/EIPS/eip-721)兼容的所有接口。
2. 太乙角色合约规定“`角色序号`”作为用户和太乙协议其他合约或者系统交互的身份ID。这个角色序号是用ERC721协议中的Token序号来实现的。
3. 太乙角色的铸造是无准入的自由铸造，但是需要铸造者支付一定的铸造费用。这个铸造费用采用了太乙协议规定的一种特定同质化通证，名为“[道理](./tip-4.md)”。每个“太乙角色”的铸造价格不是恒定的，采用一种[VRGDA](https://www.paradigm.xyz/2022/08/vrgda)价格模型来自动调节。
4. 太乙角色通证计划100年内发行10亿个。
5. 太乙角色在文献或者代码中可以使用英文单词“Actor”表示，也可以称作“太乙师傅”或者简称为“师傅”。
6. 太乙角色对其拥有的NFT和FT进行合约内托管，称为“隐式托管”。每个太乙角色对应角色合约内一个名为“持有者（Holder）”的内嵌合约实体，通过该内嵌的持有者对兼容[ERC721](https://eips.ethereum.org/EIPS/eip-721)和[ERC20](https://eips.ethereum.org/EIPS/eip-20)的各种通证进行管理。这个内嵌持有者相当于角色对应的虚拟账号。
7. 在铸造角色时支付的道理，其中一部分由角色合约自动打入太乙岛的金库。太乙角色合约需要维护一个“太乙岛金库”地址。

**注意**：
 - 规范中使用的程序语法使用Solidity语言，版本不低于`0.8.0`。
 - 调用者必须（MUST）处理任何通过 `returns (bool success)` 返回的 `false`状态。调用者不能（MUST NOT）对这种函数做出不会返回 `false`的假定！

### 数据结构

#### Actor

- 合约内部维护的一个角色数据结构，对角色通证序号、通证拥有者和专属的虚拟账号进行关联。

``` js
struct Actor 
{
    address owner;       //角色通证的拥有者
    address account;     //角色通证内联的账号，即虚拟“持有人”的地址
    uint256 actorId;     //角色序号，角色ID
}
```

### 方法和函数

#### mintTime
- 返回指定太乙角色通证的铸造时间
``` js
function mintTime(uint256 _actor) external view returns (uint256);
```

#### nextActor
- 返回下一个待铸造的角色序号
``` js
function nextActor() external view returns (uint256);
```

#### mintActor
- 铸造一个新角色，`maxPrice`指定了铸造者能支付的铸造费用上限，如果当前铸造价格超过这个上限，则执行失败。
- 函数执行中会对铸造者（函数调用者）扣除一定的道理（[TIP-4](./tip-4.md)）作为铸造费用。
- 函数执行后返回新铸造角色的序号。

``` js
function mintActor(uint256 maxPrice) external returns(uint256 actorId);
```

#### changeActorRenderMode
- 改变指定角色TokenURI中image的渲染模式。默认情况下，角色TokenURI中image的渲染模式是0

``` js
function changeActorRenderMode(uint256 _actor, uint256 _mode) external;
```

#### setTaiyiDAO
- 设置太乙岛的地址。该地址用于接收铸造新角色时的部分铸造费用。
- 只有当前太乙岛地址有权执行这个函数。
``` js
function setTaiyiDAO(address _taiyiDAO) external;
```

#### actorPrice
- 返回当前角色的铸造价格，需要花费多少“道理”。精度为18位小数的整型
``` js
function actorPrice() external view returns (uint256);
```

#### getActor
- 返回指定角色序号的角色结构数据
``` js
function getActor(uint256 _actor) external view returns (Actor memory);
```

#### getActorByHolder
- 返回指定持有人地址的角色结构数据
``` js
function getActorByHolder(address _holder) external view returns (Actor memory);
```

#### getActorsByOwner
- 返回指定拥有人地址的角色结构
``` js
function getActorsByOwner(address _owner) external view returns (Actor[] memory);
```

#### isHolderExist
- 返回指定持有人（角色虚拟账号的地址）是否存在

``` js
function isHolderExist(address _holder) external view returns (bool);
```

### 合约事件

#### TaiyiDAOUpdated
- 太乙岛地址被更新时的事件，记录了新设置的太乙岛地址。

``` js
event TaiyiDAOUpdated(address taiyiDAO);
```

#### ActorMinted
- 太乙角色被铸造时的事件，记录了新角色的拥有者地址，角色序号和铸造时间戳。
``` js
event ActorMinted(address indexed owner, uint256 indexed actorId, uint256 indexed time);
```

#### ActorPurchased
- 太乙角色在铸造时的事件，如果铸造过程中铸造者使用道理付费，则会发出该事件。事件记录了付费者地址，新铸造的角色序号和以多少铸造价格付费。
``` js
event ActorPurchased(address indexed payer, uint256 indexed actorId, uint256 price);
```

## 基本原理

- 非同质化通证的原理参考[ERC721](https://eips.ethereum.org/EIPS/eip-721)
- mintActor接口被调用的时候，合约会自动扣除调用者的“道理”通证作为铸造费。调用地址需要具有足够的“道理”来付费，否则无法铸造。
- 首个角色的角色序号为1，并且在铸造时不收取铸造者费用。该角色用于特殊用途，见[TIP-5](./tip-5.md)。
- 角色的铸造价格调节机制参考[VRGDA](https://www.paradigm.xyz/2022/08/vrgda)
- 太乙角色的内置虚拟“持有人（Holder）”本身是一个合约实例，它是在太乙角色被铸造时同时由角色合约自动创建的。这个合约实例有对应的地址，但是不能由此地址逆向推断出私钥。那么，如果持有人地址拥有一些NFT或者FT，任何个人均不可能通过私钥来直接操作这些通证。

## 参考实现

1. https://github.com/TaiSifu/TaiyiProtocol/blob/v0.1/packages/taiyi-contracts/contracts/Actors.sol

## 版权许可

依照[CC0](../LICENSE.md)规范放弃。
