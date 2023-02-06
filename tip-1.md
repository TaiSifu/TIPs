---
tip: 1
title: TIP目的和准则
status: Living
type: Meta
author: 太师傅(@TaiSifu)，等等
created: 2023-02-06
---

## 什么是TIP？

TIP是太乙改进提案的缩写（Taiyi Improvement Proposals）。一个TIP是一份设计文档，它向太乙社区提供各种信息，可以是定义一个新的功能特点，也可以是描述一项工作及其进度，还可以是描述一个故事情节和相关资源。这个TIP应该提供功能的简明技术规范、基本原理和逻辑理由。TIP的作者负责在社区内建立共识并记录不同意见。

## 为什么要构建TIP

我们希望TIP成为提出新功能，收集社区技术意见以及记录太乙协议设计决策的主要机制。由于TIP是作为文本文件在一个有版本控制的项目库中进行维护，因此其修订的历史便是这个功能建议的发展历史。

对于太乙世界的实施者来说，TIP是跟踪其实施进度的便捷方式。理想情况下，每个实际实现的维护者都会列出他们已经实现的TIP。这将为最终用户提供一种方便的方式来了解给定实现或者库的当前状态。

## TIP的类型

有三种类型的TIP：

- 
- 
- 


## TIP的格式和模板

TIP应该按照[Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)格式书写。这里有一个可以参考的[模板](./tip-template.md)。


## 链接到其他TIP

引用其他的TIP应该遵循`TIP-N`的格式，其中的`N`就是你引用的TIP序号。TIP中每个被引用的TIP，在首次被引用的地方**必须**附上一条Markdown格式的相对链接，而且之后的引用也**可以**附上链接。这条链接**必须**总是用相对路径，这样才能保证在GitHub的项目库中有效，在分叉的项目库中也能有效，在TIP相关的网站中也能有效，等等。例如，你可以引用本条TIP为`./tip-1.md`。

## 补充文件

图像，图表或者各种补充文件，都应该被包含在`assets`目录下的子目录下面，比如：`assets/tip-N` （这里 **N** 换成TIP序号）。当在一个TIP中引用一个图片时，使用相对路径，例如 `../assets/tip-1/image.png`。

## 转移TIP的所有权

偶尔，有必要对TIP的所有权进行转移。通常，我们倾向于在转移后的TIP里面以一个协作者的身份保留原始作者，当然，这取决于这个原始作者本人。转移一个TIP的原因，一般是由于原始作者不再有时间或者兴趣来更新这个TIP或者不愿意再按照TIP的流程行事，也可能由于这位原始作者`失联`（即找不到人或者邮件联系不到了）。如果你对这个TIP的发展方向不再满意，你最好不要转移作者权利。我们在一条TIP上会尝试建立共识，如果不能达成，你总可以提交一个另外的竞争性TIP。

如果你对某个TIP的作者权利感兴趣，你可以同时对原作者和TIP编辑发送消息来获得。假如原作者在一段过长的时间内没有对邮件进行响应，那么TIP的编辑将单方面做出决定（当然他这个决定也不是不可逆转的 :)）。

## TIP的编辑们

当前TIP的编辑：

- 太师傅 (@TaiSifu)

已隐退的TIP编辑：

- 

假如你希望成为一名TIP编辑，请参考[TIP-2](./tip-2.md)。

## TIP编辑的职责

对于每一个新的TIP，一名编辑应该进行如下事项：

- 阅读这个TIP并且检查它是否有效，即描述充分和完备的。这个提案或者点子必须要有意义，即便它看起来不像会达到最终状态。
- 标题必须准确描述内容。
- 检查TIP的文字（拼写、语法、句子结构等等），标记（GitHub兼容的Markdown标记），代码风格。

如果这个TIP还没有准备好，编辑要发回给作者去修订，并给予具体的说明。

一旦这个TIP可以入库了，TIP编辑将会：

- 赋予一个TIP序号（通常是PR序号，当然决定权在编辑自己）
- 合并相关的[提交请求（pull request）](https://github.com/TaiSifu/TIPs/pulls)
- 通知TIP的作者下一步该做什么

许多TIP都是由太乙协议代码的开发者所撰写和维护的。TIP的编辑们监控TIP的变化，同时会修正大家发现的任何结构、语法、拼写或者标记错误。

编辑们不会在TIP上表达喜好和裁决。我们仅仅负责管理和编辑的部分。

## 风格

### 标题

文件头中的 `title` 域：

- 不能含有“标准”或任何变体
- 不能包含TIP的序号

### 描述

文件头中的 `description` 域：

- 不能含有“标准”或任何变体
- 不能包含TIP的序号

### TIP序号

当对TIP以`TRC`（太乙征求意见）的类型被引用时，必须写成`TRC-X`这种形式，其中`X`表示TIP被赋予的序号。当TIP被作为其他类型引用时，必须写成`TIP-X`这种形式，其中`X`表示TIP被赋予的序号。

## 版权许可

依照[CC0](../LICENSE.md)规范放弃。