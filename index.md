# AI 前沿札记

> 更新日期：2026-10-29（Asia/Shanghai） · 第 016 期
>
> 信息范围：上期之后的 10 月 27–28 日官方发布；查阅日期：2026-10-29。发布日按原文标注，GitHub 榜单采样时间另列。

## 本期关注：AI 开始把一件事接着做下去

这两天的更新有一条共同线索：让 AI 在更长的任务里保持连贯。Google 用 Carbon 挑战复杂漏洞修复，用 Genie 4 生成可以实时干预的世界；GitHub 把手机和电脑上的编码任务接成同一段工作。与此同时，应用服务正在进入搜索、聊天和短信界面。

对开发者，值得留意的是任务交接和应用接入方式；对普通用户，变化则更直接：少切换几个应用，少重复交代一次背景。

## 01｜Gemini 4 Carbon：把找漏洞和修补代码放进同一条工作流程

Google **10 月 28 日**公布 Gemini 4 Carbon，重点是网络安全和需要持续推进的工程任务。官方报告的漏洞发现测试 SEC-bench Pro 成绩为 **88.2%**，漏洞修复测试 CWE-bench 为 **71.0%**；这些成绩来自 Google 公布的评测。

更值得关注的是它要解决的问题：大型代码库里的缺陷往往牵涉多个模块，修复时还要追踪根因、尝试方案、检查影响。Google 称，内部团队已观察到 Carbon 连续工作数天，处理这类多步骤问题。对工程团队来说，后续实际试用应重点看它能否给出可审查的修复、跑通回归测试，并减少来回返工。

**开放安排：** 本周先向部分网络防御人员开放，再通过 Fairwind 计划逐步扩大到可信伙伴；普通开发者和消费者的开放时间尚待公布。

来源：[Google 官方发布](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon/)。发布：10 月 28 日；查阅：10 月 29 日。

## 02｜Genie 4：生成的世界可以听见，也可以随时改变

Google DeepMind **10 月 27 日**发布世界模型 Genie 4。它在实时生成场景的同时生成环境声、音乐和人物对话；用户可用键盘控制移动与视角，也能用文字改变天气、加入人物或物体。Gemini 模型参与场景推理，帮助前后发生的事情衔接起来。

这让创作过程更接近“边探索边修改”：例如先搭建一个场景，再改变其中的条件，观察角色和环境如何响应。对互动叙事、游戏原型和训练环境研究，这是一种值得尝试的制作方式。实际价值将取决于长时间互动后的场景一致性，以及修改是否足够可控。

目前是面向可信测试者的**研究预览**；后续开放将从 Project Genie 的 Google AI Ultra 用户，以及 Gemini API、AI Studio 的早期合作伙伴开始。

来源：[DeepMind 官方介绍与演示](https://deepmind.google/blog/genie-4-a-new-generation-of-world-simulation/)。发布：10 月 27 日；查阅：10 月 29 日。

## 03｜GitHub Copilot：手机上开工，回到电脑继续

GitHub **10 月 28 日**推出连通多个入口的 Copilot 云端体验。用户可以在手机上用语音或截图发起代码修改，再到网页、VS Code、CLI 或 Copilot app 继续处理，保留同一会话与上下文。

任务默认运行在 GitHub 托管的云沙箱中；沙箱关闭时保存快照，之后可以恢复。对经常在不同设备间切换的人，这意味着发现问题时就能交代任务，回到电脑后直接检查进展。团队已有的权限和连接器管理规则也会沿用到新体验。

这项功能已正式发布，正向**付费 Copilot 用户**逐步开放。云沙箱从 **2026 年 11 月**开始计费，可使用套餐所含的 Actions 共享计算额度；团队可以一起规划构建与 Agent 任务的用量。

来源：[GitHub 更新日志](https://github.blog/changelog/2026-10-28-work-from-anywhere-with-a-new-connected-copilot-cloud-experience/)。发布：10 月 28 日；查阅：10 月 29 日。

## 04｜Google 连接应用平台：服务商多了一个触达用户的入口

Google **10 月 28 日**推出 Connected Apps Platform 预览，为应用接入 Gemini 和 Search 提供统一的开发与管理入口。开发者可通过云端 MCP 服务接入，再到 Google Connected Console 注册、测试和提交审核；Android 应用另有基于 AppFunctions 的本地接入路径。

云端接入的应用可以选择进入 Search、Gemini 或两者，并查看流量、延迟和错误率。对已有服务的团队，意义在于把查询和办事能力带到用户正在使用的 AI 界面中；用户授权连接账号后，可以在对话里调用这些服务。

平台正在分批开放。Google 同时宣布 Adobe、Peloton 等新伙伴已在 Gemini 提供连接应用，Search 端将在未来几周跟进。开发者现在最值得准备的是清晰的工具接口、可靠的授权流程，以及调用失败时的处理方式。

来源：[Google 官方公告](https://blog.google/innovation-and-ai/technology/developers-tools/connected-apps-one-place/)、[开发文档](https://developers.google.com/connected-apps)。公告发布：10 月 28 日；查阅：10 月 29 日。

## 05｜ChatGPT：通过短信联系 dot，部分订阅用户可领取 API 优惠额度

OpenAI **10 月 28 日**开始向**美国 Pro 用户**推出 dot 短信功能测试。用户可通过 iMessage 或 RCS 联系自己的 dot，沿用 ChatGPT 内的记忆与插件，也能收到任务进展等主动更新。它把联系助手的入口放到了手机已有的消息应用里；通信范围限于用户与自己的 dot。

同日，符合条件的 Pro 和 Business 订阅者可以激活 API 优惠额度，用于关联的 API 组织。这为想把个人工作流程做成应用的人提供了试验空间，领取条件和具体额度以账户活动说明为准，API 额度与 ChatGPT 使用额度分开计算。

来源：[ChatGPT 官方更新日志](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)、[API 优惠额度说明](https://help.openai.com/en/articles/20001626-api-credits-benefit-for-eligible-chatgpt-subscriptions)。更新日志发布：10 月 28 日；查阅：10 月 29 日。

## 每日 GitHub AI 热点

**采样：2026-10-29 15:46（北京时间，UTC+8）。** 增量与累计 stars 均来自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)快照，沿用页面的 `stars today` 口径。

| 项目 | 今日增长 | 同页累计 stars | 用途、许可与使用提醒 |
| --- | ---: | ---: | --- |
| [HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor) | 2,371 | 48,675 | 面向自学者的 AI 学习工作台。10 月 28 日发布 v1.7.0，加入模块化安装、块式笔记与带旁白的视频课程。Apache-2.0；启用代码执行时，优先配置 runner 或 bubblewrap 隔离。 |
| [obra/superpowers](https://github.com/obra/superpowers) | 1,095 | 307,716 | 将需求澄清、计划、测试与代码审查串成编码 Agent 工作流，已有多种工具的安装指南。MIT；自动工作流会影响执行方式与 token 开销，小任务宜先试用。 |
| [makecindy/cindy](https://github.com/makecindy/cindy) | 728 | 8,345 | 把 Claude Code、Codex 与 Pi 放进跨设备工作台，提供桌面和移动端。客户端为 Apache-2.0；云端伙伴会同步会话、记忆与共享附件，接入前检查数据去向及遥测设置。 |
| [expo/skills](https://github.com/expo/skills) | 90 | 3,684 | Expo 官方维护的 Agent 技能集，覆盖应用开发、升级、构建与发布，支持插件安装。MIT；EAS 服务涉及计费，实验性迁移技能单独提供。 |

许可证原文：[DeepTutor](https://github.com/HKUDS/DeepTutor/blob/main/LICENSE)、[Superpowers](https://github.com/obra/superpowers/blob/main/LICENSE)、[Cindy](https://github.com/makecindy/cindy/blob/main/LICENSE)、[Expo Skills](https://github.com/expo/skills/blob/main/LICENSE)。代码许可与外接模型、云服务条款分别适用；stars 用于观察关注度。

## 这一期怎么看

本期的变化集中在三个环节：模型能否持续解决复杂问题，任务能否跨设备接续，服务能否在对话里完成。接下来最有价值的观察，是把这些功能放进一件具体工作里，看它省下多少交接时间，又留下多少需要人工检查的步骤。

历史各期见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
