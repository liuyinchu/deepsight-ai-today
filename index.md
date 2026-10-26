# AI 前沿札记

> 更新日期：2026-10-26（Asia/Shanghai） · 第 014 期
>
> 信息范围：重点核实 10 月 24–26 日的新进展，补报 23 日尚未覆盖的语音与医疗应用；研究补读明确标注原始日期，不将周末转载当成当天发布。
>
> 查阅截至：2026-10-26 16:38（北京时间，UTC+8）。厂商自测、研究预览、未来计划与二手线索分开说明；GitHub 采样时间另列。

## 本期关注：开放模型添新选择，语音与科学应用更需要核验交付边界

这个周末值得看的不只是下一款旗舰模型的传闻。Nanbeige 发布了面向编程和办公 Agent 的 33B 权重；Block 展示只从语音学习对话的 Dessa；数学社区开始组织修订 AI 生成的论文；Google 则总结了结核病筛查从模型走向一线的路径。

**先看这五件事：**

- **Nanbeige4.3-33B：** 权重已公开，512K 上下文，面向 Agent；更小的 3B 学生模型仍属后续计划。
- **Dessa：** 全双工、纯语音训练是研究方向，官网仍在招募少量安全研究者，不是人人可下载的新应用。
- **oai-math-revised：** 社区补引用、改表达、查错误；启动修订站不等于原有数学结论已全部通过验证。
- **Google TB 筛查：** 实际部署和开放基础模型是两条相关但不同的线，不能把具体工具的认证移植到所有模型上。
- **科学 Agent 补读：** 光镊控制和晶界研究展示人机协作；一个是实验、一个主要是仿真，都不是“自主科学家已经取代研究者”。

## 一、开放模型：Nanbeige4.3-33B 先做教师，再向小模型蒸馏

### 01｜可下载的 33B Agent 模型，重点在完整任务而非单个榜单

Nanbeige 官方仓库历史显示：**10 月 24 日上传权重，25 日更新模型说明**。当前 Nanbeige4.3-33B 与 Base 均公开，标注 **Apache-2.0**；聊天模型支持最高 **524,288 token（512K）**上下文。团队把 33B 定位为教师模型，后续用于蒸馏更小的 Agent，例如尚待推出的 Nanbeige4.3-3B。

模型卡对比了约 30B 规模的 Qwen3.8-27B、Muse-Glimmer-30B 和 Gemma4-31B，报告多项编程、工具使用和办公任务成绩。**这些是团队自测，不是本刊独立复现，也不意味着每项都领先。** 比如 SWE-Bench Pro 与 GDPval 上，表中 Qwen 的成绩更高。

**部署边界：** 文件页显示仓库约 **66.3 GB**，这不等于运行所需显存；长上下文缓存还要额外占资源。官方提供 Transformers、SGLang 与 vLLM 示例，Base 则适合自行后训练，不能把它直接视为同等对话成品。

**解读：** 对本地 Agent 来说，有权重、许可和可读配置，比“追平某闭源模型”的口号更有用。应在自己的任务中检查工具调用稳定性、长上下文质量和资源开销，而不是仅按参数量选择。

来源：[官方模型卡](https://huggingface.co/Nanbeige/Nanbeige4.3-33B)、[提交历史](https://huggingface.co/Nanbeige/Nanbeige4.3-33B/commits/main)、[权重文件](https://huggingface.co/Nanbeige/Nanbeige4.3-33B/tree/main)、[Base 模型](https://huggingface.co/Nanbeige/Nanbeige4.3-33B-Base)。事件：2026-10-24–25；查阅：2026-10-26。本期未下载或运行模型。

## 二、语音交互：Dessa 把语气、停顿和插话当作模型输入

### 02｜Block 展示纯语音训练路线，但公众开放仍未落地

Block 的 Dessa 官方研究页称，模型从人类对话录音开始训练，不使用文本数据或预训练模型；输入输出都是声音，采用可以同时听和说的全双工方式。团队展示了插话、笑声和语调变化等录音，并称可在 **MacBook Pro M4 Max 或更高配置**上本地运行。

**开放状态：** 官网提供演示，邀请一小组 AI 安全研究者申请早期访问，页面仍提示发布链接即将提供。本期没有核实到公众下载包、公开 API 或可分发权重。页面上的 Download / waitlist 字样，不能单独作为已经全面上线的证据。

**日期边界：** 本期补报这一周末话题；官方研究页没有单列发布日期，10 月 23 日的公布日期由媒体报道记录，不能写成 26 日首发。

**解读：** 保留声音而非只保留转写文本，有望改善轮流发言与情绪线索处理；但听起来像真人，不等于事实更可靠或真正具有情感。部署时还要检验身份披露、误导风险与异常对话的处理。上述能力与硬件要求均为团队声明，本刊未实测。

来源：[Block Dessa 官方研究页](https://block.xyz/dessa)；日期旁证：[TechCrunch 10 月 24 日报道](https://techcrunch.com/2026/10/24/blocks-new-voice-model-sounds-eerily-human/)。查阅：2026-10-26。

## 三、AI 与数学：从批量生成转向可追踪的社区修订

### 03｜oai-math-revised 邀请补引用、改写与纠错

**10 月 24 日**，Terence Tao 博客刊登 Abhinav Nellore、Johannes Schmitt、Soledad Villar 与 Rachel Ward 的客座文章，介绍 **oai-math-revised**。发起者认为 OpenAI 数学文稿存在难读、引用不全等问题，希望组织社区改进。

公开仓库提供三类参与方式：提交修订 PR、用 issue 提出修改、为各篇文稿维护注释与贡献记录。内容包括补充相关文献、改进解释、发现并尽可能修复错误；项目要求披露较大程度的自动化工具使用，提交内容仍需审阅。

**解读：** 这条新闻的新意不在再次宣布 AI 解出了多少数学题，而在建立能够逐篇追问和纠错的工作流。社区 fork 与修订入口的存在，不代表所有论文已获认可，也不是新的同行评审结论。

**还需观察：** 后续应看具体修订、审核记录、证明状态和贡献署名，而非仅看论文总量或仓库关注度。本刊未重新核验数学证明。

来源：[10 月 24 日客座文章](https://terrytao.wordpress.com/2026/10/24/oai-math-revised-a-community-hub-for-revising-openais-math-write-ups/)、[社区仓库与贡献规范](https://github.com/mathcommunity/oai-math-revised)。发布：2026-10-24；查阅：2026-10-26。

## 四、医疗应用：结核病筛查的价值，在能否进入真实工作流程

### 04｜Google 总结部署经验，不是宣布一个可直接诊断的新模型

Google Research **10 月 23 日**发表结核病筛查进展，本期补报。文章介绍南非矿区的便携 X 光与离线 AI 筛查：发现疑似病例后，还要转介确认检测；也介绍印度尼西亚利用本地胸片微调 MedGemma、MedSigLIP 的工作，以及 HeAR 咳嗽声音筛查的合作研究。

**必须分清：** Google 所述已用于一线诊所、取得 CE 认证的 Nexus AI 工具，与开放的基础模型、正在开展的声音研究不是同一个交付物。不能据此声称 MedGemma 普遍获得临床认证，或仅凭咳嗽就能确诊。印尼每年筛查 5,000 万人的数字是目标，不是已完成数量。

**解读：** 这比单项影像识别跑分更接近落地问题：断网能否工作、数据是否留在本地、疑似患者能否得到确认检测，以及模型是否在当地人群验证。后续值得追踪的是具体产品的适用范围与前瞻性临床证据。

来源：[Google Research 原文](https://www.research.google/blog/bringing-foundational-models-to-the-front-lines-of-tb-screening/)。发布：2026-10-23；查阅：2026-10-26。本节为研究与应用资讯，不是个人诊断建议。

## 五、研究补读：把 Agent 的角色说清楚

### 05｜光镊实验与晶界仿真：策略协助不等于无人监督

两篇本月预印本在周末资讯中再次被讨论，但**原始提交日期分别是 10 月 18 日、20 日，并非 25–26 日新论文**：

- **光镊阵列控制：** 研究把通用 LLM 用作实验策略层，依据反馈切换校准方法。对于一个 10×10 阵列，作者报告高斯峰值变异系数从 20.30% 降至 1.57%。数值计算由确定性软件完成，实验人员逐步核验并批准波形；这是物理子系统的实验，不是自主操作整台量子计算机。
- **GBClaw 晶界研究：** 论文将编排器、专门 Agent、领域工具、仿真接口和分层记忆结合，用于镍的 14 种一般非对称倾斜晶界。作者报告结构与变形机制分析，并明确部分后续计算由研究者引导；这是原子尺度计算研究，不是材料已投入生产。

**解读：** 两项工作的共同点是把模型放在有反馈、有领域工具和人类判断的位置。判断“AI 科学家”新闻时，先问清模型负责策略、计算、验证还是最后决策，比先问用了什么模型更重要。两者均为预印本，本刊未复现实验或仿真。

来源：[光镊论文与提交历史](https://arxiv.org/abs/2610.25014)、[GBClaw 论文与提交历史](https://arxiv.org/abs/2610.28629)。提交：2026-10-18、2026-10-20；查阅：2026-10-26。

## 六、Tibo 动态：有新一轮 reset 线索，但原帖仍未直接核实

本期检索定位到 **10 月 25 日 Day 20** 与 **10 月 26 日 Day 21** 的新帖线索。非官方追踪站将前一轮标为完成、后一轮标为宣布；**两者不是同一个事件，不能把昨天的完成状态套到今天。**

**来源边界：** 本次访问原始 X 页面返回 403，未独立核实原帖内容、适用套餐与传播状态，因此不把第三方的完成标记当作官方确认，也不声称个人额度已经到账。实际可用额度仍以账号用量页为准。

来源：[非官方 Day 21 追踪条目](https://retroaitracker.com/codex-resets/2026-10-26-reset-announced)、[非官方 Day 20 追踪条目](https://resetcue.com/events/2026-10-25-codex-usage-reset-completed)；待核实原帖：[Day 21](https://x.com/thsottiaux/status/2114588174249071102)、[Day 20 后续](https://x.com/thsottiaux/status/2114237083652882932)。查阅：2026-10-26。以上仅作线索记录，不作额度或未来模型发布承诺。

## 七、每日 GitHub AI 热点：技能层与本地应用仍在升温

**采样：2026-10-26 16:33（北京时间）。** 下表为本次读取 [GitHub Trending 的 Today 页面](https://github.com/trending) 所见快照，增长严格采用页面的 `stars today` 标签，并非北京时间零点以来、精确滚动24小时或我们自行统计的净增；累计 stars 单独列出。榜单与仓库主页存在缓存和采样时间差，不以二者差值计算增长。这是筛选后的 AI 相关项目，不是完整榜单排名。

| 项目 | 页面显示今日增长 | 同页累计 stars | 看点与边界 |
| --- | ---: | ---: | --- |
| [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) | 2,878 | 47,461 | 编码 Agent 的分阶段安全审计技能：独立验证器复核候选问题、输出结构化记录。代码采用 MIT；它要求隔离执行环境，星数不证明扫描完整或结果可靠。 |
| [debpalash/VoiceStudio](https://github.com/debpalash/VoiceStudio) | 1,592 | 87,479 | 语音克隆、配音、转写等本地工作流，可接本地 API/MCP；远程服务是可选项。应用采用 AGPL-3.0-only，默认 OmniVoice 权重为 CC-BY-NC，模型和 tokenizer 另有条款；不能把本地运行等同可商用，本期未测试音质。 |
| [Nutlope/hallmark](https://github.com/Nutlope/hallmark) | 1,317 | 34,585 | 面向 Claude Code、Codex、Cursor 等的设计技能，结合主题、真实浏览器检查及独立复核，目标是减少模板化的“AI味”。代码采用 MIT；不是新模型，也不保证生成结果自动达标。 |
| [makecindy/cindy](https://github.com/makecindy/cindy) | 675 | 5,350 | 桌面与移动端 AI Agent 客户端，Apache-2.0。仓库不包含后端服务；可跳过 Cindy 登录使用本地 Agent，但服务端功能不会因此变成离线可用。 |

**札记判断：** 当模型供给越来越接近，开发者的注意力继续流向可复用技能、验证流程和具体工作台。不过，关注度只是筛选线索，不能替代安装前的源码、权限和模型许可检查。

项目原文与许可：[Cloudflare README](https://github.com/cloudflare/security-audit-skill)、[MIT](https://github.com/cloudflare/security-audit-skill/blob/main/LICENSE)；[VoiceStudio README](https://github.com/debpalash/VoiceStudio)、[许可说明](https://github.com/debpalash/VoiceStudio/blob/main/LICENSE-NOTICE.md)；[Hallmark README](https://github.com/Nutlope/hallmark)、[MIT](https://github.com/Nutlope/hallmark/blob/main/LICENSE)；[Cindy README](https://github.com/makecindy/cindy)、[Apache-2.0](https://github.com/makecindy/cindy/blob/main/LICENSE)。均查阅于 2026-10-26；本期未安装或实测。VoiceStudio 的应用商业许可选项不自动覆盖模型权重和 tokenizer 条款。

## 读完之后，值得追问的三件事

1. **下载与运行是两回事：** 新权重是否有清晰许可，自己的硬件能否承受真实上下文和并发，而不只是把文件装进去？
2. **演示与交付是两回事：** 语音预览是否有公众入口、独立测试和合成身份披露，而不只是录音听起来逼真？
3. **生成与验证是两回事：** 论文、诊疗辅助和实验策略的最后判断由谁负责，错误如何被发现、记录和纠正？

本期基于可读取的原始资料；明确区分厂商自测、预印本、部署总结与二手动态。没有实测模型或语音质量，没有独立复现数学证明、实验或仿真，也没有核实个人账号额度。历史各期见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
