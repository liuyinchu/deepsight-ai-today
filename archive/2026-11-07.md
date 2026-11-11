# AI 前沿札记

> 更新日期：2026-11-07（Asia/Shanghai） · 第 020 期
>
> 信息范围：11 月 5 日至 11 月 7 日查得的官方公告、研究与项目更新；查阅日期：2026-11-07。发布日期与实验发生时间分列，GitHub 日榜采样时间见下文。

## 本期关注：AI 怎样把一项工作持续做下去

一项任务做久了，AI 会遇到不少实际问题：哪些经验值得留下，环境变化后要不要改计划，多名 Agent 如何发现彼此的错误，已经生成的素材能否接着修改。

今天选取的四条进展，分别涉及记忆、长任务评测、视频编辑和服务连接的安全性。它们提供了一些可以直接用来设计试验、检查工作流程的线索。

## 01｜Anthropic 拆解 Agent 记忆：保存经验，还要能核对和修正

Anthropic **11 月 6 日**发布工程研究《The components of agent memory》，介绍跨会话记忆的几层组织方式：原始记录、记忆笔记、关联知识页，以及每次会话预先载入的少量内容。离线整理会回看原始记录，纠正笔记、合并重复内容，再调整哪些信息值得优先读取。

在一项包含约 **35 万份内部文档、25 道问题**的实验中，Anthropic 报告：仅用搜索的评分为 **53%**，搜索结合经过多轮整理的知识页达到 **61%**，每题还减少约 2 万 token。另一些信息集中在单份文档里的任务，建立知识页没有带来明显收益。这些数字来自项目方的特定评测。

**我们的观察：** 做知识库助手时，可以先区分两类信息：随时能从系统读到的当前状态，以及需要从经历中总结的规则和计划。后者更值得长期保存；同时保留来源，才能在流程变化或结论出错后更新它。

来源：[Anthropic：The components of agent memory](https://www.anthropic.com/research/components-of-agent-memory)。发布：2026-11-06；查阅：2026-11-07。

## 02｜Project EVE：长时间执行有进步，判断变化与识别异常仍吃力

Anthropic **11 月 6 日**公布 Project EVE，研究于 **2026 年 4 月至 7 月**开展。在 EVE Online 部分，团队重放游戏市场的历史订单，让 Claude 在模拟器中交易；在 EVE Frontier 部分，则让四个 Agent 在真实游戏里合作完成任务。

研究团队称，较新的模型更擅长把买卖价差策略写成程序并持续执行，但当任务改为判断价格走势时，Fable 5 也只在约一半的运行中盈利。四人团队通常能协作，面对研究者安排的内部破坏行为，却很少识别出问题。

模拟市场不会对 Agent 的行为作出真实反应，游戏中的收益也不能直接套用到现实交易。更有参考价值的是这些实验暴露的工作习惯：策略一旦奏效，Agent 可能长期沿用，对费用或规则变化反应迟缓。

**我们的观察：** 评估长期运行的 Agent，可以主动插入规则变更、过期信息和同伴出错等情形，观察它何时发现异常、如何调整。任务运行了多久之外，也值得记录它重新检查假设的频率。

来源：[Anthropic：Project EVE](https://www.anthropic.com/research/project-eve-claude-plays-mmorpgs)。发布：2026-11-06；实验期：2026 年 4 月至 7 月；查阅：2026-11-07。

## 03｜Gemini Omni 1.2 Flash：15 秒视频与多轮修改进入正式服务

Google **11 月 5 日**发布 **Gemini Omni 1.2 Flash**，并宣布在 Gemini API 与 Gemini for business 正式可用。单次生成和编辑的原生时长从上一版的 10 秒延长至 **15 秒**，可在 3 至 15 秒之间按整秒指定；720p 视频公布价格为 **每秒 0.07 美元**，较 1.1 版下降 30%。

这一版支持在同一段视频上连续修改背景、光线、产品或画面比例。Google 称参考素材遵循和角色一致性有所改善；生成结果带有 SynthID 水印，快速运动与复杂物理表现仍在改进。Gemini 应用端则面向 **Google AI Ultra 订阅者逐步推出**。

**我们的观察：** 对产品演示与短广告，值得测试的是“改一处，其他地方能否保留”。用同一份素材连续改三轮，记录人物、文字和镜头是否漂移，再比较最终可用片段的成本，会比只看第一条生成效果更接近真实制作需求。

来源：[Google 发布公告](https://blog.google/innovation-and-ai/technology/developers-tools/introducing-gemini-omni-1-2-flash/)、[DeepMind 模型卡](https://deepmind.google/models/model-cards/gemini-omni-1-2-flash/)。发布：2026-11-05；查阅：2026-11-07。

## 04｜OpenAI 与 Hacktron 合作，重点检查服务之间的连接

OpenAI **11 月 6 日**宣布与安全研究团队 Hacktron 合作，共同选择高优先级区域开展测试，重点关注 OpenAI 系统与第三方服务的交互，并通过 Daybreak Red 扩大 Hacktron 对前沿模型的访问。

公告介绍，Hacktron 使用 AI Agent 做代码审查、渗透测试和红队研究。这次合作公布的是测试安排，尚未给出新一轮发现或修复的数量。

**我们的观察：** 当 Agent 串起邮箱、代码仓库和云服务，审计也要沿着数据和权限的流向检查：来源是否可信、权限如何传递、输出最终进入哪里。对正在接入工具的团队，先画清这些连接，比单独检查每个工具更容易发现遗漏。

来源：[OpenAI：Partnering with Hacktron](https://openai.com/index/partnering-with-hacktron/)。发布：2026-11-06；查阅：2026-11-07。

## 每日 GitHub AI 热点

**采样：2026-11-07 20:19（Asia/Shanghai，UTC+8）。** 增量与累计 stars 取自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)页面读取，沿用 `stars today` 口径，按本期所选项目的当日增星排序。日榜展示的是关注度，不是严格滚动 24 小时的全站 AI 项目增长排名。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) | 2,088 | 64,808 | 为编码 Agent 组织分阶段安全审计、独立复核与结构化报告，适合开发与安全团队。MIT；已有校验器和测试，执行目标代码需要操作系统级沙箱，单次审计仍可能漏报。 |
| [milind-soni/OpenMausBot](https://github.com/milind-soni/OpenMausBot) | 1,986 | 16,211 | 用聊天界面管理多名 Agent，连接本地 CLI、电脑和外部应用，适合研究个人 Agent 工作流。Apache-2.0；11 月 7 日发布 0.1.146，已有桌面安装包，Ubuntu AppImage 标注 beta。接入真实账号前，应检查文件访问、操作审批和第三方数据流向。 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 1,111 | 317,168 | 将需求澄清、TDD、调试与代码审查整理成可组合技能，适合希望保留工程流程控制权的开发者。MIT；提供多种编码 Agent 安装入口，需按项目裁剪，并用自己的测试与验收标准检查效果。 |
| [google/skills](https://github.com/google/skills) | 449 | 23,057 | Google 产品与云服务的 Agent 技能集，覆盖模型部署、RAG、GKE、IAM 和排障。Apache-2.0；已有官方技能目录及插件入口，部分能力关联预览服务，执行前检查目标项目、云权限与费用。 |

项目 README、许可证与状态均查阅于 2026-11-07：

- Cloudflare：[README](https://github.com/cloudflare/security-audit-skill/blob/main/README.md) · [MIT 许可证](https://github.com/cloudflare/security-audit-skill/blob/main/LICENSE)
- OpenMausBot：[README](https://github.com/milind-soni/OpenMausBot/blob/main/README.md) · [Apache-2.0 许可证](https://github.com/milind-soni/OpenMausBot/blob/main/LICENSE) · [0.1.146 发布记录](https://github.com/milind-soni/OpenMausBot/releases/tag/v0.1.146)
- Matt Pocock Skills：[README](https://github.com/mattpocock/skills/blob/main/README.md) · [MIT 许可证](https://github.com/mattpocock/skills/blob/main/LICENSE)
- Google Skills：[README](https://github.com/google/skills/blob/main/README.md) · [Apache-2.0 许可证](https://github.com/google/skills/blob/main/LICENSE)

以上是依据项目文档作出的编辑判断，本期未实机运行。仓库许可证只覆盖相应代码或技能材料；使用的模型、云电脑和连接服务仍各有条款与费用。选型时，把所需权限、失败恢复和输出验收一并纳入试用。

## 接下来值得跟进

本期留下三个具体问题：记忆整理的收益能否抵过额外计算成本？长期任务发现环境变化后，能否及时修正计划？多轮视频编辑保留原有细节的成功率有多高？

我们的判断是，这些问题都适合用连续任务来检验。保留中间记录，安排一次变化，再看最终交付，能更清楚地判断 AI 在自己的工作里是否可靠。

上一期见[第 019 期：让 AI 工作更连贯，也更容易接手](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-06.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
