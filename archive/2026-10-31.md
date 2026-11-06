# AI 前沿札记

> 更新日期：2026-10-31（Asia/Shanghai） · 第 018 期
>
> 信息范围：10 月 30 日官方发布，以及即将生效的开发者计费变化；查阅截止：2026-10-31 17:06（北京时间）。发布日按原文标注，GitHub 榜单采样时间另列。

## 本期关注：AI 走向专门的工作，也开始计算更细的成本

今天最有意思的两条消息，一条在实验室，一条在太空：OpenAI 将 GPT-6 Rosalind Research 带给获批的生命科学研究团队，Google 则收到了在轨 TPU 运行模型生成的第一条回应。

回到开发者手边，变化更务实：小模型可以共享一张 GPU，DeepSeek 的搜索和网页读取工具即将按调用收费，安全研究模型也进入了新的受控云区域。它们共同指向一个问题：模型能力之外，怎样的部署方式、访问条件和成本结构，才能让 AI 真正用起来？

## 01｜GPT-6 Rosalind Research：生命科学研究进入 GPT-6 阶段

OpenAI **10 月 30 日**开始向有限的获批客户推出 GPT-6 Rosalind Research，用于多步骤生命科学研究。开放入口是 **Work、Codex，以及另行获批的 API**；它不在普通 Chat 标签页提供。API 模型名为 `gpt-6-rosalind`。

这次发布有一个容易忽略的技术细节：系统卡明确写明，**它使用与 GPT-6 Astra 相同的权重，没有为此次发布增加训练数据或进行额外后训练**。差异主要体现在面向科研用途的访问安排、模型行为及部署防护，而不是另训了一套生物学模型。

它面向的工作包括整合文献和生物数据、规划实验、协调科学工具等。对于已有科研流程的团队，值得考察的是它能否把跨数据库、跨工具的步骤连贯地完成，而不只是单题回答更好。

**开放条件：** 组织与使用者都需要 Research 资格；Discovery 资格不会自动升级，Discovery 仍使用 5.5。Enterprise 工作区先进入两周 Admin Preview，默认关闭，由管理员为获批用户启用；原有 GPT-5.5 Rosalind Research 可以继续使用。

来源：[OpenAI 模型发布说明](https://help.openai.com/en/articles/9624314-model-release-notes)、[Rosalind 系统卡补充说明](https://deploymentsafety.openai.com/gpt-rosalind-6)、[访问与设置说明](https://help.openai.com/en/articles/20001193-gpt-rosalind-for-life-sciences-research)。发布说明与系统卡：10 月 30 日；查阅：10 月 31 日。

## 02｜Google 在轨 TPU 回了一首诗：太空算力迈出小而具体的一步

Google **10 月 30 日**披露，Project Suncatcher 原型卫星完成了一个“Hello World”里程碑：地面发送提示词，卫星上的数据中心级 TPU 运行 **Gemma 4**，生成一首关于太空的俳句，并在经过挪威斯瓦尔巴群岛时把结果传回地面。

原型卫星于 **10 月 1 日**发射，此后团队完成主机和 TPU 托盘通电、早期机器学习测试等调试。本周完成的这次回应，按 Google 的说法，是数据中心级 TPU 在轨生成的首次 AI 回应；随后还测试了打油诗和载荷状态遥测提示词。

有趣的不只是“AI 会在太空写诗”，而是地面指令、在轨计算与结果回传已经串了起来。接下来要测试的是轨道环境中的**热应力与辐射影响**，再朝 2027 年两颗互联卫星的目标推进。这仍是算力基础设施研究：一次成功生成，距离长期稳定运行和经济可行性还有一段路。

来源：[Google Project Suncatcher 官方进展](https://blog.google/innovation-and-ai/models-and-research/google-research/project-suncatcher-satellite-response/)。发布：10 月 30 日；查阅：10 月 31 日。

## 03｜SageMaker GPU 分区：小模型不必独占整张卡

AWS **10 月 30 日**宣布，SageMaker AI 实时推理端点支持 GPU 分区。借助 NVIDIA Multi-Instance GPU（MIG），一张 GPU 最多可以划分为 **7 个具有独立显存和计算资源的分区**，分别承载模型。

这对嵌入模型、重排序模型、分类器和较小的微调模型尤其实际：它们可能用不满一张卡，却常常需要为整张加速器付费。现在团队可以按模型需求选择分区大小，在同一张卡上放置多个模型，并继续按模型名称调用，应用调用方式不必因此改写。

首批支持 **P4d、P5、G7、G7e** 实例，区域包括美国北弗吉尼亚、俄亥俄、俄勒冈，新加坡、东京，法兰克福和爱尔兰。配置入口是承载 inference components 的端点。

我们的判断是，这类能力对“多个小模型组合完成任务”的服务更有吸引力。是否省钱，要结合实际负载、分区规格和延迟要求来测，不能把最多 7 个分区直接理解成成本降到七分之一。

来源：[AWS GPU 分区发布说明](https://aws.amazon.com/about-aws/whats-new/2026/10/sagemaker-gpu-partitioning/)。发布：10 月 30 日；查阅：10 月 31 日。

## 04｜今晚跨过零点，DeepSeek 的搜索与网页读取开始计费

这是**即将生效的规则提醒，不是今天发布的新模型**。DeepSeek 官方文档写明，通过 DeepSeek Harness、Claude Code、Codex 使用 DeepSeek API 时，原先免费的服务端 Search、Fetch 工具调用，将于 **2026 年 11 月 1 日 00:00（北京时间）**开始按请求计费。公告中的另一种写法是 10 月 31 日 16:00 UTC，指的是同一个时间点。

Search 和 Fetch **各为每 1,000 次成功请求 0.75 美元**，与模型输入、输出 token 费用分别计算。这里的变化针对上述 DeepSeek API 工具调用，不是所有 Claude Code 或 Codex 用户统一增加一笔费用。

如果你的 Agent 会反复搜索、抓网页，今天适合检查每项任务用了多少次工具。一边限制无效重复调用，一边保留必要的原始来源核实，比单纯压低输出字数更能反映这类研究任务的真实成本。

来源：[DeepSeek 当前定价与计费规则](https://api-docs.deepseek.com/quick_start/pricing/)、[10 月 17 日官方公告](https://api-docs.deepseek.com/news/news261017/)。公告：10 月 17 日；规则生效：北京时间 11 月 1 日 00:00；查阅：10 月 31 日。

## 05｜Daybreak Red 进入 AWS GovCloud：安全研究模型扩大受控部署范围

AWS **10 月 30 日**宣布，OpenAI 的 **Daybreak Red：GPT-5.6 Cyber** 已向符合条件的 AWS GovCloud（US-East）客户开放。它用于获授权的漏洞研究、漏洞复现与缓解方案开发，配合身份验证、监控和访问控制，为经过核验的防御团队提供更充分的研究空间。

此次新增的是 **GovCloud（US-East）部署范围**；公告同时列出的可用区域还有美国东部俄亥俄。客户需要先获得 OpenAI 的 Daybreak 访问资格，再与 AWS 团队启用，并非普通 Bedrock 账户直接就能调用。

AWS 表示，模型运行在其新一代推理引擎上，推理数据不用于模型训练，客户也无需把 Daybreak Red 数据共享给 OpenAI。对于对环境和数据治理有专门要求的安全团队，部署位置与访问流程本身，就是能否采用这类模型的重要条件。

来源：[AWS Daybreak Red 官方公告](https://aws.amazon.com/about-aws/whats-new/2026/10/openai-daybreak-red-govcloud-east/)。发布：10 月 30 日；查阅：10 月 31 日。

## 每日 GitHub AI 热点

**采样：2026-10-31 17:05（北京时间，UTC+8）。** 增量与累计 stars 均来自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)快照，沿用页面 `stars today` 口径。本期选取学习、跨设备编码和工程技能四类工具。

| 项目 | 今日增长 | 同页累计 stars | 用途、许可与使用提醒 |
| --- | ---: | ---: | --- |
| [HKUDS/DeepTutor](https://github.com/HKUDS/DeepTutor) | 2,282 | 52,135 | 面向自学者的 AI 学习工作台，已有 v1.7.0，提供块式笔记、视频课程和学习资料整理。Apache-2.0；运行模型生成的办公文件代码时，需要配置相应沙箱，模型与语音服务成本另计。 |
| [pingdotgg/t3code](https://github.com/pingdotgg/t3code) | 930 | 33,200 | 从桌面、网页或手机管理电脑上的编码 Agent，支持 Codex、Claude Code 等。MIT；项目仍处早期，需先安装和登录模型提供方，远程连接前仔细设置权限和访问范围。 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 876 | 307,748 | 可组合的工程技能，强调需求澄清、项目术语和可维护的开发流程，适合希望保留过程控制的开发者。MIT；已有多种 Agent 的安装说明，插件与文件复制两种安装方式选一种，避免技能重复。 |
| [expo/skills](https://github.com/expo/skills) | 38 | 3,954 | Expo 官方 Agent 技能，覆盖 React Native 应用开发、调试、升级与部署。MIT；已有官方插件入口，SDK 开源能力与 EAS 服务计费分开，涉及构建和发布时检查套餐及环境。 |

许可证原文：[DeepTutor](https://github.com/HKUDS/DeepTutor/blob/main/LICENSE)、[T3 Code](https://github.com/pingdotgg/t3code/blob/main/LICENSE)、[Matt Pocock Skills](https://github.com/mattpocock/skills/blob/main/LICENSE)、[Expo Skills](https://github.com/expo/skills/blob/main/LICENSE)。项目原文与许可查阅于 10 月 31 日。stars 用于观察关注度；代码许可不替代外接模型和云服务条款。

## 这一期怎么看

从 Rosalind 到 Daybreak，专业 AI 的开放不再只是给出一个模型名，还包括谁能用、在哪里运行、怎样审查。GPU 分区与搜索计费，则把成本拆到了更具体的资源和动作上。

对多数开发者，眼下最值得做的是把任务账本看清楚：哪一步需要最强模型，哪一步可以由小模型完成，哪些搜索真正带来了新证据。太空算力值得期待，但先把手边的算力和工具用得恰到好处，同样是一种进步。

历史各期见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
