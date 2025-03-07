---
tip: 2
title: TIP 编辑手册
description: TIP编辑们的参考手册
author: 红中🀄️(@hongzhongx)
discussions-to: https://github.com/hongzhongx/TIPs/discussions/2
status: Living
type: Informational
created: 2025-03-07
requires: 1
---

## 摘要
太乙改进提案（TIP）是为太乙社区提供信息的设计文档，或是描述太乙网络及其流程或环境新增功能的规范文件。TIP标准化流程是提出新功能、收集社区技术意见、记录太乙网络设计决策的机制。由于改进提案是太乙网络区块链的核心组成部分，在达到"最终"（Final）状态前必须经过充分审核至关重要。TIP以文本文件形式存储在由TIP编辑监管的版本控制仓库中。

本TIP阐述成为TIP编辑的推荐流程。

## 规范
本文件中使用的关键词"必须"（MUST）、"禁止"（MUST NOT）、"必需"（REQUIRED）、"应"（SHALL）、"不应"（SHALL NOT）、"应当"（SHOULD）、"不应"（SHOULD NOT）、"推荐"（RECOMMENDED）、"可以"（MAY）、"可选"（OPTIONAL）均遵循RFC 2119标准解释。

### 申请与入职流程
任何具备以下条件者可申请成为TIP编辑：
- 深入理解TIP标准化流程与网络升级流程
- 在太乙网络区块链核心层/应用层具备中级实践经验
- 愿意参与流程管理
潜在TIP编辑应当具备以下技能：
- 良好的沟通能力
- 处理争议性讨论的能力
- 每周1-5小时可支配时间

理解TIP流程的最佳资源是[TIP-1](./tip-1.md)。任何希望成为TIP编辑者必须通晓该文档。建议通过参与TIP流程（如对PR和issue发表意见、提出改进建议）来熟悉流程。新任编辑的贡献行为将受到其他TIP编辑监督。

符合上述要求者可通过提交PR完成以下操作申请成为TIP编辑：
1. 在编辑名单中添加自己
2. 在[TIP-1](./tip-1.md)中更新编辑列表
若现有TIP编辑批准申请，申请人即成为正式TIP编辑。此举将通知编辑关注TIP仓库中新提交的提案，编辑将负责审核并合并PR：

```diff
diff --git a/TIPs/tip-1.md b/TIPs/tip-1.md
index 79558a3..079b196 100644
--- a/TIPs/tip-1.md
+++ b/TIPs/tip-1.md
@@ -207,6 +211,7 @@ 当前TIPs的编辑
 - 红中🀄️ (@hongzhongx)
+- 北风 (@beifeng)

 已隐退的TIPs编辑：
```

### 本TIP的特殊合并规则
本TIP必须遵循与[TIP-1](./tip-1.md)相同的变更规则。

## 制定依据
- 选择"6个月"作为标记`停滞`状态TIP进入`最终停滞`状态的临界值属于经验性决策
- 本TIP需要特殊合并规则的原因与[EIP-1](./eip-1.md)相同

## 版权声明
依据[CC0](../LICENSE)协议放弃所有版权及相关权利。
