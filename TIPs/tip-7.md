---
title: NFA 行为命令开发规范
description: 规范太乙网络 NFA 作为可编程实体所绑定的 SGS 主合约中的行为命令开发规范，包括 do 型和 eval 型行为命令。
author: CyberParticle (@CyberPoincare)
discussions-to: https://github.com/hongzhongx/TIPs/discussions/7
status: Draft
type: Standards Track
category: Interface
created: 2026-01-28
requires: 5
---

## 摘要

本提案规范了太乙网络中非同质资产（NFA）主合约（SGS）的行为命令开发标准。通过定义指令的 ABI 预声明方式、函数命名规范以及返回结果处理机制，确保 NFA 行为能够被统一地触发和查询。本规范旨在支持账号通过 `action` 上链操作运行具有状态影响的 NFA 行为（`do` 型），并支持通过“白玉京” API 同步请求 NFA 行为获取反馈信息（`eval` 型）。

## 动机

在太乙网络中，NFA 被视为具有主观能动性的可编程实体。为了让“心素”（SGS 开发者）开发的 NFA 合约能够被账号以统一命令方式调用（比如MUD游戏中的命令行），需要一套标准化的命令接口协议：

1. **统一交互体验**：开发者无需为每个客户端编写特定的适配代码。
2. **性能与安全**：明确区分“产生因果（写操作）”与“获取反馈（读操作）”，在保证链上数据一致性的同时，提供极速的 API 查询反馈。
3. **可扩展性**：为更高级的 NFA 标准提供底层调用规范。

## 规范说明

本文中的关键术语 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY" 和 "OPTIONAL" 均按照 RFC 2119 解释。

### ABI 预声明

在 NFA 的主合约（SGS）中，必须在全局作用域对所有暴露给外部的行为命令进行预声明。声明格式为一个 Lua 表，其中必须包含 `consequence` 字段。

```lua
-- 格式：指令名 = { consequence = 布尔值 }
look = { consequence = false }
go = { consequence = true }
```

* **`consequence = true`**：表示该指令为 **`do` 型（行为型）**。
* **`consequence = false`**：表示该指令为 **`eval` 型（查询型）**。

### 指令类型与行为规范

#### `do` 型行为命令 (Consequence = true)

用于改变 NFA 状态、触发链上事件或消耗资源的异步操作。

* **函数命名**：必须以 `do_` 为前缀，后接预声明的指令名。例如：`do_go(dir)`。
* **执行方式**：太乙网络节点在一个上链交易中通过 `action_nfa_operation` 协议上链执行。
* **真气消耗**：执行此类命令通常会消耗调用者的真气（Qi）。
* **反馈机制**：主要通过 `contract_helper:narrate(msg, true/false)` 抛出叙述性日志（作为链上交易的 Affected 项存储），或抛出断言（Assert）中止执行。

#### `eval` 型行为命令 (Consequence = false)

用于查询 NFA 状态或获得计算型行为结果的只读同步操作。

* **函数命名**：必须以 `eval_` 为前缀，后接预声明的指令名。例如：`eval_look(target)`。
* **执行方式**：通过节点服务的白玉京 API（如 `eval_nfa_action`）同步调用。
* **状态限制**：**MUST NOT** 执行任何写操作（如修改 `nfa` 中的私有数据、转移资产等）。
* **真气消耗**：不消耗真气。
* **反馈机制**：
  * **叙述日志**：通过 `contract_helper:narrate(msg, false/true)` 抛出的信息会被 API 捕获并返回。
  * **显式返回**：函数本身的 Lua 返回值（通常是一个 table 或变量列表）会被 API 完整返回。

### 应用推荐实践

* **心素**：在设计复杂交互时，建议将简单的描述逻辑放在 `eval_` 函数中，而将涉及消耗、移动或属性变更的逻辑放在 `do_` 函数中。
* **客户端开发者**：
  * 在 UI 展示阶段，优先调用 `eval_` 命令以获取即时反馈。
  * 在玩家确认意图后，发起同步的 `do_` 动作交易。

## 基本原理

1. **分层执行**：太乙网络节点保证了链上行为的原子性和因果律；通过白玉京 API 提供了轻量级的只读查询入口。
2. **函数前缀区分**：使用 `do_` 和 `eval_` 前缀可以在同一个合约中清晰地分离“写逻辑”与“读逻辑”，避免开发者在 `eval` 过程中意外触发昂贵的计算或不可逆的状态变更。
3. **元数据驱动**：ABI 预声明使得天道网络能够预先知道哪些操作是安全的（`consequence = false`），从而在 API 层直接拦截非法尝试。

## 向后兼容性

本提案为新标准，不影响现有 NFA 合约的虚拟机解析。历史合约若未遵循此规范，可能无法被白玉京 API 的 `eval` 相关接口正确识别，建议逐步进行标准化重构。

## 测试用例

参考太乙合约用例 [`taiyi-contracts`](https://github.com/hongzhongx/taiyi-contracts) 项目中 `nfas/luyin.lua` 的实现片段：

```lua
-- ABI 声明
short = { consequence = false }
touch = { consequence = true }

-- eval 型实现：返回简短描述并打印叙述
function eval_short()
    contract_helper:narrate("你看到了一本路引。", false)
    return { "路引" }
end

-- do 型实现：实际执行升级逻辑
function do_touch(from_who)
    -- ... 检查逻辑 ...
    contract_helper:change_nfa_contract(actor.nfa_id, "contract.actor.cultivator")
    contract_helper:narrate(string.format('%s 摸了摸路引，整个人气质都变了。', from_who), true)
end
```

## 安全性注意事项

* `eval` 函数中严禁使用任何上下文 `nfa_helper` 和 `contract_helper` 的写操作函数，如 `:write_contract_data` 或资产操作函数。
* 天道网络在执行 `eval` 调用时应强制开启只读模式，任何试图修改状态的操作都应导致异常。

## 版权许可

依据 [CC0](../LICENSE) 协议放弃所有版权及相关权利。
