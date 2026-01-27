---
tip: 1
title: TIP用途与指南
status: Living
type: Meta
author: 红中🀄️(@hongzhongx)，等等
discussions-to: https://github.com/hongzhongx/TIPs/discussions/1
created: 2025-03-07
---

## 什么是TIP？

TIP是指太乙网络改进提案（Taiyi Improvement Proposal ）。 TIP是一份向太乙社区提供信息，或描述太乙网络新功能、流程、环境的设计文档。TIP应提供功能的简明技术规范和基本原理。TIP作者有责任在社区内建立TIP共识并记录异议。

## TIP原理

我们打算让TIP成为主要机制，用于提出新功能，收集社区对某一问题的技术意见，并记录太乙网络的设计决策。因TIP以文本文件在版本化存储库中维护，因此版本修订历史就是功能提案的历史记录。

对于太乙网络实施者来说，TIP是跟踪其实施进度的便捷方式。理想情况下，每个实施维护者都会列出他们已实施的TIP。这将给终端用户提供一个方便的方式来了解一个特定的实现或库的当前状态。

## TIP类型

TIP有3种类型：

- **标准类TIP（Standards Track TIP）** 描述了影响多数或全部太乙网络实现的任何更改，例如网络协议的更改、块或交易有效性规则的更改、提议的应用程序标准/约定，或影响太乙应用程序交互的任何更改或添加。标准类TIP由三部分组成 - 设计文档、实施、（如果有必要）对[正式规范](https://github.com/hongzhongx/taiyi-yellowpaper)的更新。此外，标准类TIP可细分为：
  - **核心（Core）**: 需要共识分叉的改进（如[TIP-X](./eip-x.md)）, 以及那些非关键共识但可能与[“核心开发”讨论相关的变化](https://github.com/hongzhongx/pm)（如[TIP-X]()）。
  - **网络（Networking）**: 包括对网络协议规范的改进（如[TIP-X]()）。
  - **接口（Interface）**: 包括围绕客户端[API/RPC]()规格和标准的改进，还有某些语言级别的标准，如（[TIP-X]()）。标签“interface”与[interfaces repo]一致，在将TIP提交到TIP存储库之前，讨论应该主要发生在该库中。
  - **应用层标准（TRC）**：是应用层面的标准和约定，包括智能游戏脚本（SGS）的合约标准，如NFA实体标准（[TRC-X]()）、角色Actor标准（[TRC-X]()）、地区Zone标准（[TRC-X]()）、URI方案、库/包格式和玄牝、大傩客户端格式等。

- **元TIP（Meta TIP）** 描述了围绕太乙网络的一个流程或提出对一个流程的变更（或一个事件）。流程TIP类似于标准类TIP，但流程TIP适用于太乙网络协议之外的领域。他们可能会提出一个实施方案，但不会针对太乙网络代码库；他们通常需要社区达成共识；与信息类TIP不同，它们不仅仅是建议，用户一般不能随意忽略它们。这方面的提案包括程序、指南、决策过程的更改以及太乙网络开发中使用的工具或环境的更改。任何 meta-TIP 也会被视为一个流程TIP。

- **信息类TIP（Informational TIP）** 描述太乙网络的设计问题，或向太乙社区提供一般的指南或资讯，而非提出新功能。它不一定代表太乙社区的共识或建议，所以用户和实施者可以自由的选择遵循或忽略信息类TIP所提出的建议。

强烈建议单个TIP只包含一个关键提案或新想法。TIP的重点越突出，就越能成功。对一个客户端的更改不需要TIP；影响多个客户端或供多个应用程序使用的标准定义的更改就需要TIP。

TIP必须满足某些最低标准。它必须清楚和完整地描述提案的改进措施。这种增强必须是一种净改进。提案的实施（如果适用）必须是可靠的，并且不得使协议过度复杂化。

## TIP工作流程

### 发起一个TIP

TIP流程的参与方是你、倡导者或*TIP作者*、[*TIP编辑*](#tip-editors)、以及[*太乙网络核心开发者*](https://github.com/hongzhongx/pm)。

在开始编写正式的TIP之前，你应该仔细推敲你的想法。首先询问太乙社区这个想法是否是原创的，避免在基于先前研究而被拒绝的事情上浪费时间。因此建议在[TIPs论坛](https://github.com/hongzhongx/TIPs/discussions)、[太乙网络项目论坛](https://github.com/hongzhongx/taiyi/discussions/)和[合约应用实践论坛](https://github.com/hongzhongx/taiyi-contracts/discussions)上打开一个主题贴来讨论它。

一旦想法通过审核，您的下一个责任将是（通过TIP）向审稿人和所有感兴趣的各方介绍您的想法，邀请编辑、开发人员和社区，在上述论坛提供反馈。 你应尝试平衡对你的TIP的兴趣是否与实施它所涉及的工作以及有多少方必须遵守它相称。例如，实施核心TIP所需的工作将比TRC大得多，并且TIP需要坐忘道、袄景教徒们表现出足够的兴趣。考虑社区负面反馈，这可能将会阻止您的TIP通过草案阶段。

### 核心TIPs

对于核心TIPs，鉴于它们要求客户端实现才能被认为是**Final**（见下文“TIPs流程”），你需要为客户端提供一个实现或者说服客户端实现你的TIP。

让客户端实施者审阅你的TIP的最好方法是在AllCoreDevs的电话会议上介绍它。你可以通过在 [AllCoreDevs议程GitHub Issue](https://github.com/hongzhongx/pm/issues) 上发布一条链接你的TIP的评论来请求这样做。

AllCoreDevs电话会议中客户端实施者能做到三件事。第一，讨论TIP技术优点；第二，衡量其他客户端将实施什么；第三，协调网络升级的TIP实施。

这些通话通常会就应实施哪些TIP达成“大致的共识”。这种“大致的共识”建立在这样的假设上：TIPs没有争议到导致网络分裂的程度，且TIPs在技术上是合理的。

:warning: TIPs流程和AllCoreDevs电话不是为了解决有争议的非技术问题，但由于缺乏其他方法来解决这些问题，最终往往被这些问题所纠缠。这使得客户端实施者承担了尝试和衡量社区情绪的负担，这阻碍了TIPs和AllCoreDevs电话的技术协调功能。如果你正在指导一个TIP，你可以通过确保你的TIP的[太乙项目论坛](https://github.com/hongzhongx/taiyi/discussions/)主题包括或链接到尽可能多的社区讨论，并确保各利益相关者得到足够的权利，从而使建立社区共识的过程更容易。

*简而言之，你作为倡导者的角色是使用下面描述的风格和格式来编写TIP，在适当的论坛上引导讨论，并围绕这个想法建立社区共识。*

### TIP流程

以下是所有情况中所有TIP的标准化流程。

![TIP 状态图](../assets/tip-1/TIP-process-update.jpg)

**想法-Idea** - 一个预先起草的想法，不在TIP资源库中跟踪。

**草稿-Draft** - TIP进展中的第一个正式跟踪阶段。当格式正确时，TIP编辑会将TIP合并到TIP仓库中。

**评审-Review** - TIP作者将TIP标记为准备好并请求同行评审。

**最后公示- Last Call** - 这是TIP进入`Final`的最后公示窗口。TIP编辑将指定`Last Call`状态，并设定公示期限 (`Last-call-deadline`)，通常是14天后。

如果这段时间产生了必要的规范性修改，它将把TIP恢复为`Review`。

**终版-Final** - 该TIP代表了最终标准。终版TIP是以最终状态存在的，只应在纠正勘误和增加非规范性澄清时进行更新。

**停滞-Stagnant** - 任何处于`DRAFT`或`REVIEW`或`Last Call`状态的TIP，如果在6个月或更长时间内没有活动，将移至`Stagnant`。 TIP可由作者或编辑通过将其移回`草案`或早期状态，将其从停滞中复活。如果不复活，一个提案可能会永远停留在这个状态。
> *TIP作者会被通知其TIP状态的任何变化。*

**撤回-Withdrawn** - TIP作者已撤回提案的TIP。这种状态具有终结性，不能再使用这个TIP编号复活。如果这个想法在以后的日子里被推行，它将被视为一个新的提案。

**活动-Living** - TIP的特殊状态，适用于那些旨在持续更新而未达到终版状态的TIP。这包括最引人注目的TIP-1。

## 一个成功的TIP应该包括什么?

每个TIP应该有以下部分：

- 序言（Preamble） - RFC 822风格的标头，包含有关TIP的元数据，包括TIP编号、简短的描述性标题（最多44个字符）、描述（最多140个字符）和作者的详细信息。无论哪种类别，标题和描述都不应包含TIP编号。详见[下文](./tip-1.md#TIP头部序言)。
- 摘要（Abstract） - 摘要是一个多句话（短段落）的技术摘要。这应该是规范部分的一个非常简明扼要、可供人类阅读的版本。应该能有人只阅读摘要就能了解本规范的要点。
- 动机（Motivation）*（可选）* - 动机部分对于想要改变太乙协议的TIP来说至关重要。它应当清楚地解释为什么现有的协议规范不足以解决此TIP要解决的问题。如果动机很明显，这部分可以省略。
- 规范（Specification） - 技术规范应描述任何新功能的语法和语义。该规范应该足够详细，以允许当前任何太乙相关项目(taiyi、taiyi-js、ctaiyi或其他) 的竞争性、可互操作的实现。
- 基本原理（Rationale） - 基本原理充实了规范的内容，描述了设计的动机，以及为什么做出了特定的设计决定。它应该描述考虑过的其他设计和相关的工作，例如，该功能如何在其他语言中得到支持。基本原理应该讨论在围绕TIP的讨论中提出的重要反对意见或顾虑。
- 向后兼容（Backwards Compatibility）*（可选）* - 所有引入向后不兼容的TIP必须包括一个描述这些不兼容及其后果的部分。TIP必须解释作者建议如何处理这些不兼容的问题。如果提案没有引入任何向后不兼容的问题，这部分可以省略，但如果存在向后不兼容的问题，这部分必须包括。
- 测试案例（Test Cases）*（可选）* - 对于影响共识变化的TIP来说，实施的测试案例是强制性的。测试应该作为数据内联在TIP中(如输入/期望输出对，或者包含在`../assets/tip-###/<filename> `中）。对于非核心提案，这一部分可以省略。
- 参考实现（Refrence Implementation）*（可选）* - 这是一个可选的部分，包含人们可以用来帮助理解或实现本规范的参考/示例实现。所有 TIP都可以省略这部分。
- 安全考虑（Security Considerations） - 所有的TIP都必须包含一个讨论与提议的更改相关的安全影响/考虑因素的部分。包括对安全讨论可能很重要的信息、表面风险以及可在提案的整个生命周期内使用的信息。例如，包括与安全相关的设计决定、关注点、重要的讨论、特定的实施指导和陷阱、威胁和风险的概要以及如何解决这些问题。缺少“安全考虑因素”部分的TIP提交将被拒绝。如果没有评审人员认为有足够的安全考虑因素讨论，TIP不能进入“终版”状态。
- 版权豁免（Copyright Waiver） - 所有的TIP必须是在公共领域。版权放弃必须（MUST）链接到许可证文件并使用以下措辞。`Copyright and related rights waived via [CC0](.../LICENSE.md)`（译：通过CC0放弃版权和相关权利）。


## TIP格式与模板

TIP应以[markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)格式编写。有一个[模板](./tip-template.md)可效仿。


## TIP头部序言

每个TIP必须以[RFC 822](https://www.ietf.org/rfc/rfc822.txt)风格的头部序言开始，前面和后面有三个连字符 (`---`)。这个头部也被Jeyll称为["front matter"](https://jekyllrb.com/docs/front-matter/)。 头部必须按以下顺序显示：

`tip:` *TIP编号*（这由TIP编辑者决定）。

`title:` *TIP的标题是几个字，不是一个完整的句子*

`description:` *描述是一个完整的（短）句子*

`author:` 作者的姓名和/或用户名，或姓名和电子邮件的列表。详情见下文。

`* discussions-to:` *指向官方讨论主题的网址*

`status`: *状态：Draft, Review, Last Call, Final, Stagnant, Withdrawn, Living*

`last-call-deadline`: *终审截止日期* (可选字段，只在状态为 `Last Call`时需要)。

`type`: *`Standards Track`, `Meta`或 `Informational`中的一个*。

`category`: *`Core`、`Networking`、`Interface`、或`TRC`*之一 (可选字段，仅在`Standards Track` TIP中需要)

`created:` *TIP的创建日期*

`* requires:` *（依赖的）TIP 编号(可选字段)*

`withdrawal-reason`: *一句话解释TIP被撤销的原因。* （可选字段，仅在状态为`Withdrawn`时需要）

头部可能有的列表必须用逗号分隔元素。

头部要求日期始终以ISO 8601的格式 (yyyy-mm-dd) 进行。

### `author` 头

这个`author`头部列出TIP的作者/所有者的姓名、电子邮件地址或用户名。喜欢匿名的人可以只使用用户名，或使用名字和用户名。`author` 头内容格式必须是包括电子邮件地址：

> Random J. User &lt;address@dom.ain&gt;

或包括 GitHub 用户名：

> Random J. User (@username)

以及没有给出电子邮件地址：

> Random J. User

不可能同时使用电子邮件和 GitHub 用户名。如果必须同时使用，可以把名字写两次，一次是GitHub用户名，另一次是电子邮件。

必须至少有一位作者使用 GitHub 用户名，这样才能收到修改请求的通知，并有权批准或拒绝它们。

### `discussions-to` 头

虽然TIP是草案，但 `discussions-to` 头将指明讨论TIP的网址。

首选讨论网站是[TIPs论坛](https://github.com/hongzhongx/TIPs/discussions)、[太乙网络项目论坛](https://github.com/hongzhongx/taiyi/discussions/)和[合约应用实践论坛](https://github.com/hongzhongx/taiyi-contracts/discussions)的主题贴。该网址不能指向 Github PR，任何短暂的网址，以及任何可能随着时间推移而被锁定的网址（如 Reddit 主题贴）。

### `type` 头

`type` 头指定了TIP类型: Standards Track、Meta或Informational。如果类目是标准类，请包括子类别（core、networking、interface 或TRC）。

### `category` 头

`category` 头指定TIP的类别。这仅适用于标准类TIP。

### `created` 头

`created`头记录了TIP被分配一个编号的日期。这两个头都应采用 yyyy-mm-dd 格式，例如 2001-08-14。

### `requires` 头

TIP可能有一个 `requires` 头，表示该TIP所依赖的TIP编号。如果存在这种依赖关系，此字段是必需的。

当当前TIP在没有另一个TIP的概念或技术要素的情况下不能被理解或实施时，就会产生`requires`依赖关系。仅提及另一个TIP并不一定会产生这种依赖关系。

## 关联外部资源

除了下面列出的特定例外情况，外部资源的链接 **不应该** 包括在内。外部资源可能消失、移动或突发变更。

管理允许的外部资源的流程在[TIP-3](./tip-3.md)中描述。

### 共识层规范

太乙共识层规范的链接可以使用正常的 markdown 语法包含，如：

```markdown
[起源](https://github.com/hongzhongx/consensus-specs/specs/origin-chain.md)
```

展示为:

[起源](https://github.com/hongzhongx/consensus-specs/specs/origin-chain.md)

允许的共识层规范的网址必须锚定到一个特定的提交，因此必须匹配这个正则表达式。

```regex
^https://github.com/hongzhongx/consensus-specs/blob/[0-9a-f]{40}/.*$
```

### 网络规范

太乙网络规范的链接可以使用常规的 markdown 语法包含，如：

```markdown
[太乙网络连接协议](https://github.com/hongzhongx/devp2p/caps/taiyi.md)
```

展示为:

[太乙网络连接协议](https://github.com/hongzhongx/devp2p/caps/taiyi.md)

允许的网络规范的网址必须锚定到一个特定的提交，因此必须匹配这个正则表达式。

```regex
^https://github.com/hongzhongx/devp2p/blob/[0-9a-f]{40}/.*$
```

### 数字对象标识符系统

符合数字对象标识符（DOI）条件的链接可以使用以下语法：

````markdown
这是个带有脚注的句子。[^1]

[^1]:
    ```csl-json
    {
      "type": "article",
      "id": 1,
      "author": [
        {
          "family": "Jameson",
          "given": "Hudson"
        }
      ],
      "DOI": "00.0000/a00000-000-0000-y",
      "title": "一个有意思的情况",
      "original-date": {
        "date-parts": [
          [2022, 12, 31]
        ]
      },
      "URL": "https://sly-hub.invalid/00.0000/a00000-000-0000-y",
      "custom": {
        "additional-urls": [
          "https://example.com/an-interesting-article.pdf"
        ]
      }
    }
    ```
````
展示为:

<!-- markdownlint-capture -->
<!-- markdownlint-disable code-block-style -->

这是个带有脚注的句子。[^1]

[^1]:
    ```csl-json
    {
      "type": "article",
      "id": 1,
      "author": [
        {
          "family": "Jameson",
          "given": "Hudson"
        }
      ],
      "DOI": "00.0000/a00000-000-0000-y",
      "title": "一个有意思的情况",
      "original-date": {
        "date-parts": [
          [2022, 12, 31]
        ]
      },
      "URL": "https://sly-hub.invalid/00.0000/a00000-000-0000-y",
      "custom": {
        "additional-urls": [
          "https://example.com/an-interesting-article.pdf"
        ]
      }
    }
    ```
<!-- markdownlint-restore -->

关于支持的字段，请参见[引用文献样式语言模式](https://resource.citationstyles.org/schema/v1.0/input/json/csl-data.json)。除了通过对该模式的验证外，参考文献必须包括 DOI 和至少一个 URL。

顶层的 URL 字段必须解析到一个可以零成本查看参考文献的副本。`additional-urls`下的值也必须解析到参考文献的副本，但可能会收取费用。

## 关联到其他TIP

对其他TIP的引用应遵循`TIP-N`格式，其中`N`是您所引用的TIP编号。在TIP中被引用的每个TIP **必须（MUST）** 在第一次被引用时都有一个相对的markdown链接，而 **可以（MAY）** 在随后的引用都有一个链接。该链接 **必须（MUST）** 始终用相对路径，这样链接才能在 GitHub 存储库、此存储库的分支、TIP主站点、TIP主站点的镜像中工作，例如，您可以使用 `./tip-1.md` 链接到此TIP。

## 辅助文件

图片、图表和辅助文件应包含在该TIP的 `assets` 文件夹的子目录中，如下所示：`assets/tip-N`（将 **N** 替换为TIP编号）。链接到TIP中的图片时，使用相对链接，例如 `../assets/tip-1/image.png`。

## 转让TIP所有权

有时需要将TIP的所有权转让给新的倡导者。一般来说，我们希望保留原作者作为转让的TIP的共同作者，但这完全取决于原作者。转让所有权的一个很好的理由是，原作者不再有时间或兴趣来更新它或跟进TIP流程，或者已经从网上失联（即无法联系或不回复电子邮件）。转让所有权的一个坏理由是你不同意TIP的发展方向。我们试图围绕一个TIP建立共识，但如果这不可能，你总能提交一个竞争性的TIP。

如果你有兴趣获得TIP的所有权，请向原作者和TIP编辑者发送要求接管的消息。如果原作者没有及时回复邮件，TIP编辑人会做出单方面的决定（这种决定也不是不能推翻的:））。

## TIP的编辑们

当前TIPs的编辑：

- 红中 (@hongzongx)
- CyberParticle (@CyberPoincare)
- 北风🀃 (@TaiSifu)

已隐退的TIPs编辑：

- 

假如你希望成为一名TIP编辑，请参考[TIP-2](./tip-2.md)。

## TIP编辑的职责

对于每个新进入的TIP，编辑会做如下工作：

- 阅读TIP，检查它是否准备好了：健全且完整。这些想法必须在技术上有意义，即使它们看来不可能进入 final 状态。
- 标题应准确地描述内容。
- 检查TIP语言（拼写、语法、句子结构等）、标记（GitHub 风格的 Markdown ）、代码风格

如果TIP还没有准备好，编辑会把它发回给作者进行修改，并附有具体指示。

TIP一旦准备就绪，TIP编辑将：

- 分配一个TIP编号(一般为PR编号，但决定权在于编辑)
- 合并对应的[拉取请求](https://github.com/hongzhongx/TIPs/pulls)
- 向TIP作者发回信息，说明下一步工作。

许多TIP是由拥有太乙网络项目代码写入权限的开发人员编写和维护的。TIP编辑们监控TIP的变化，并纠正我们看到的任何结构、语法、拼写或标记错误。

编辑们不对TIP进行评判。我们只是做了行政管理&编辑的部分。

## 风格指南

### 标题

序言中的 `title` 字段:

- 不应包括“标准”一词或其任何变体；
- 且不应包含TIP编号。

### 描述

序言中的 `description` 字段:

- 不应包括“标准”一词或其任何变体；
- 且不应包含TIP编号。

### TIP 编号

当通过编号引用TIP时，应以带连字符的形式书写 `TIP-X`，其中 `X` 是TIP的分配编号。

### RFC 2119 和 RFC 8174

我们鼓励TIP遵循 [RFC 2119](https://www.ietf.org/rfc/rfc2119.html) 和 [RFC 8174](https://www.ietf.org/rfc/rfc8174.html) 的术语，并在规范部分的开头插入以下内容：

> 本文件中的关键词“必须（MUST）”、“禁止（MUST NOT）”、“必要的（REQUIRED）”、“应当（SHALL）”、“不应当（SHALL NOT）”、“应该（SHOULD）”、“不应该（SHOULD NOT）”、“推荐的（RECOMMENDED）”、“不推荐（NOT RECOMMENDED）”、“可能（MAY）”和“可选的（OPTIONAL）” 按 RFC 2119 和 RFC 8174 的规定进行解释。

## 历史

此文档在很大程度上源自 Amir Taaki 编写的[比特币 BIP-0001](https://github.com/bitcoin/bips)，而 BIP-0001 又源于[ Python 的 PEP-0001](https://peps.python.org/)。很多地方的文字都是简单的复制和修改。尽管 PEP-0001 文本是由 Barry Warsaw、Jeremy Hylton 和 David Goodger 编写的，但他们不对其在太乙网络改进过程中的使用负责，也不应被打扰处理与太乙网络或TIP有关的技术问题。请将所有意见直接提交给TIP编辑们。

## 版权

依据[CC0](../LICENSE)协议放弃所有版权及相关权利。
