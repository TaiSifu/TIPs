[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/hongzhongx/TIPs?style=flat-square)](https://hub.docker.com/r/hongzhongx/TIPs/tags)
[![GitHub license](https://img.shields.io/github/license/hongzhongx/TIPs?style=flat-square)](https://github.com/hongzhongx/TIPs/blob/main/LICENSE)
[![GitHub contributors](https://img.shields.io/github/contributors-anon/hongzhongx/TIPs?style=flat-square)](https://github.com/hongzhongx/TIPs/graphs/contributors)
[![GitHub commit activity](https://img.shields.io/github/commit-activity/y/hongzhongx/TIPs?style=flat-square)](https://github.com/hongzhongx/TIPs/commits/main)

# 太乙改进建议集（Taiyi Improvement Proposals——TIPs）。

TIP项目的目标是为太乙网络本身及其上构建的各类规范实现标准化并提供高质量文档。该仓库通过太乙改进提案（TIPs）的形式追踪太乙网络的过往及持续改进。[TIP-1](./TIPs/tip-1.md)规定了TIP的发布流程。

[状态页面](https://tips.taiyi.org/)追踪并列出各类TIP，主要可分为以下类别：
- [核心TIP](https://tips.taiyi.org/core) 针对太乙网络共识协议的改进
- [网络TIP](https://tips.taiyi.org/networking) 规定太乙点对点网络层协议
- [接口TIP](https://tips.taiyi.org/interface) 标准化与太乙网络交互的接口，决定用户和应用如何与区块链交互
- [TRC](https://tips.taiyi.org/erc) 制定应用层标准，决定太乙网络上应用间的交互方式
- [元类TIP](https://tips.taiyi.org/meta) 需要共识但未归类的其他改进
- [信息类TIP](https://tips.taiyi.org/informational) 无需共识的非标准化改进

**在撰写TIP前，相关构想必须经过[TIPs论坛](https://github.com/hongzhongx/TIPs/discussions)、[太乙网络项目论坛](https://github.com/hongzhongx/taiyi/discussions/)和[合约应用实践论坛](https://github.com/hongzhongx/taiyi-contracts/discussions)或者[坐忘道Discord开发频道](https://discord.com/channels/1296049199397339230/1299582831428763668)充分讨论。达成共识后，请仔细阅读阐述TIP流程的[TIP-1](./TIPs/tip-1.md)。**

请注意本仓库仅用于标准文档维护，不提供实现支持。此类问题请提交至[太乙网络项目论坛](https://github.com/hongzhongx/taiyi/discussions)和[合约应用实践论坛](https://github.com/hongzhongx/taiyi-contracts/discussions)。针对特定TIP的疑问，建议在该TIP前言中`discussions-to`标签指定的讨论帖留言。

若您希望成为TIP编辑，请阅读[TIP-2](./TIPs/tip-2.md)。

## 建议引用格式
任何已经达到草案状态的TIP，其规范URL开头为<https://tips.taiyi.org/>。例如，TIP-1的规范URL是<https://tips.taiyi.org/TIPs/tip-1>。

未在<https://tips.taiyi.org/>发布的文档应视为工作文件。此外，状态为"草案"（draft）、"审核"（review）或"最后公示"（last call）的已发布TIP应视为未完成稿，请注意其规范内容可能发生变动。
