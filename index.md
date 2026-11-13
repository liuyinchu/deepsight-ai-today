# AI 前沿札记

> 更新日期：2026-11-13（Asia/Shanghai） · 第 022 期
>
> 信息范围：11 月 11 日至 11 月 13 日查得的官方公告、发布记录与项目文档；查阅日期：2026-11-13。新闻日期沿用来源标注，GitHub 发布时刻另换算为北京时间，日榜采样时间单列。

## 本期关注：把任务分给合适的模型，也把验收留下来

这两天的新进展，值得从“怎样分工”来理解：通用模型尝试在同一任务里按难度切换，专用模型负责快速选择，科学模型则把预测细化到不同细胞。开源工具也在补齐可恢复扫描、后台任务通知和本地推理修复。

更快、更便宜只是起点。真正接入工作时，还要弄清自动决策的边界、结果如何复核，以及出错后能否继续。

## 01｜Gemini 4 Hydrogen：日常执行模型与多模型调度一起推出

Google **11 月 12 日**发布 Gemini 4 Hydrogen，面向日常编码和 Agent 工作流。它提供 **100 万 token 上下文**与 low、medium、high、xhigh 四档思考，API 价格为每百万输入／输出 token **1.50／7.50 美元**。

同期推出的 Antigravity Orchestrator 可在同一会话中动态切换 Carbon、Hydrogen 等模型。Google 自报，在 Terminal-Bench 4.0 上，成功率接近 Carbon，平均配额消耗少 24%、耗时少 19%；这是厂商测试，不是本期独立实测。

开放范围包括 Gemini API、AI Studio、企业及消费产品。Antigravity 的 Hydrogen 面向付费 Google AI Pro／Ultra 用户；Gemini app 免费用户通过**自动选模**获得访问，不能据此推断人人都可手选。

**我们的观察：** 可以用同一组真实任务比较固定模型与自动调度，记录总费用、用时和人工返工。调度节省的配额，不一定等于相同比例的账单节省。

来源：[Google：Introducing Gemini 4 Hydrogen](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/)。发布：2026-11-12；查阅：2026-11-13。

## 02｜Gemini Decisions API：把有限选项交给专用决策模型

Google **11 月 11 日**推出由 Gemini Decision 1 驱动的 Decisions API，可在同一份多模态输入上评估最多 **1,000 个独立问题**，支持是／否、选择与量表评分，返回概率和置信度。输入上限为 **256K token**，价格为每百万输入 token **0.10 美元**，输出不计费。

它已在 Google AI Studio 的 Gemini API 开放，并有原型开发免费层；Google Cloud 支持仍标为即将推出。官方给出的 **150–250 毫秒端到端延迟**，对应不超过 16K token 等测试条件，不应扩大到所有长度或全部 1,000 个问题。

**我们的观察：** 工具选择、模型路由等步骤有机会减少生成长文本的成本。应先用带答案的样本校准阈值，并为低置信度结果保留人工或更强模型复核；分数本身不是正确性的保证。

来源：[Google：Gemini Decisions API](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/)。发布：2026-11-11；查阅：2026-11-13。

## 03｜AlphaGenome 2：从组织平均值，走向具体细胞状态

Google DeepMind **11 月 11 日**发布 AlphaGenome 2。模型在 DNA 序列之外引入细胞类型或状态作为条件，训练数据包括跨 68 种组织、超过 1 亿份单细胞数据，并新增 DNA 甲基化预测。

研究团队报告，在 TenK10K 的 28 类原代免疫细胞评估中，因果变异识别由对比模型的 16% 提升到 30%。这一结果有特定数据集和比较条件，不能概括成所有疾病预测能力翻倍。

**非商业研究 API 已开放，商业研究仍是私有预览。** 官方明确，模型仅供研究，尚未经过临床或诊断用途验证和批准。

**我们的观察：** 细胞状态更细，有助于研究者优先挑选值得做实验的候选。下一步仍要看新数据和实验验证，尤其是模型没有充分见过的细胞状态。

来源：[Google DeepMind：AlphaGenome 2](https://deepmind.google/blog/alphagenome-2-deciphering-how-cells-read-dna-to-better-understand-disease/)。发布：2026-11-11；查阅：2026-11-13。

## 04｜安全快讯：儿童 AI 研究承诺强调公开负面结果

OpenAI 在 **11 月 12 日**公告中表示，已于当周巴黎和平论坛签署儿童安全 AI 承诺宪章，并承诺提供 **200 万美元**支持相关科学研究网络。公告强调研究团队保留结论自主权，负面或无明确结论的结果也应发表。

**我们的观察：** 这是资金与研究治理承诺，还不是已经完成的安全验证。后续值得看公开的研究方法、指标和结果，而不只看参与名单。

来源：[OpenAI：Supporting research on AI and children’s safety](https://openai.com/index/children-first-declaration/)。公告发布：2026-11-12；事件发生于当周，原文未列具体签署日；查阅：2026-11-13。

## 每日 GitHub AI 热点

**采样：2026-11-13 09:06（Asia/Shanghai，UTC+8）。** 增量与累计 stars 取自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)实时页面读取，保留页面 `stars today` 口径，按本期所选项目的当日增星排序。这是日榜关注度采样，不是严格滚动 24 小时的全站 AI 增长排名。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | 816 | 116,674 | 面向编码 Agent 的需求、测试、评审与发布工作流，适合希望统一工程验收的团队。MIT；已提供多工具集成。单独安装某项 skill 可能缺少仓库级共享清单，接入前应核对路径，并审阅自动执行步骤的权限。 |
| [GoogleChrome/modern-web-guidance-src](https://github.com/GoogleChrome/modern-web-guidance-src) | 739 | 2,963 | 为编码 Agent 提供现代 Web 开发指导的源内容与评估仓库，适合前端工具作者。已有指南、CLI 与评估；代码 Apache-2.0，guides 内容 CC-BY-4.0。安装使用分发仓库；默认遥测含工具搜索词，接入前检查隐私设置与目标浏览器兼容。 |
| [openai/codex-security](https://github.com/openai/codex-security) | 643 | 12,427 | 用 CLI／TypeScript SDK 查找、验证和修复漏洞，适合有授权的代码安全审查。Apache-2.0；已有版本发布。依赖模型服务与运行环境，扫描预算不等于硬上限，自动生成修补仍需测试和人工评审。 |
| [CopilotKit/OpenBot](https://github.com/CopilotKit/OpenBot) | 298 | 11,616 | 为 Agent 配备浏览器、文件和工具的应用模板，适合工作流原型。模板为 MIT，Intelligence 另需许可；本地免费许可、评估及生产许可应分开核对。默认单用户模式不适合直接暴露给团队或公网。 |
| [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) | 176 | 134,559 | C／C++ 本地大模型推理工具，适合部署与性能实验。MIT；持续提供 nightly 发布。硬件后端与模型兼容需实测，模型权重许可另算；新版本应做固定样本回归再替换。 |
| [QwenLM/qwen-code](https://github.com/QwenLM/qwen-code) | 151 | 29,813 | 终端编码 Agent，适合代码库维护和后台任务。Apache-2.0；正式版与 1.0 预览支线并行，Web UI／daemon 仍标为实验性。模型服务、凭据和外部通知权限需单独管理，预览变更不能混作正式功能。 |

### 版本动态：今天能具体检查什么

- **Codex Security 0.10.0**：发布于 11 月 12 日 10:18（北京时间），加入 Standard 扫描断点恢复、批量／组件扫描共享估算预算，并改善修补受阻时的说明。适合先验证一次中断后能否恢复，再逐步扩大扫描范围。来源：[发布记录](https://github.com/openai/codex-security/releases/tag/npm-v0.10.0)。
- **Qwen Code v0.27.3**：发布于 11 月 13 日 08:24（北京时间），加入钉钉 Webhook 单向通知、任务通知目的地和 Web Shell 后台任务展示等改进。与 1.0 预览支线区分使用。来源：[发布记录](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.3)。
- **llama.cpp b12336**：发布于 11 月 13 日 08:34（北京时间），为 nightly／预发布，修复 HunyuanVL 加载时的位置 embedding 类型校验，避免首张图片请求中的缓冲区溢出。仅将此作为相应模型的更新检查点，不称作稳定版发布。来源：[发布记录](https://github.com/ggml-org/llama.cpp/releases/tag/b12336)。

README 与许可证核验说明（查阅：2026-11-13）：

- Agent Skills：[README](https://github.com/addyosmani/agent-skills/blob/main/README.md) · [MIT](https://github.com/addyosmani/agent-skills/blob/main/LICENSE)
- Modern Web Guidance：[源库 README 与双许可说明](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/README.md) · [Apache-2.0](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/LICENSE) · [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) · [安装分发仓库](https://github.com/GoogleChrome/modern-web-guidance)
- Codex Security：[README](https://github.com/openai/codex-security/blob/main/README.md) · [Apache-2.0](https://github.com/openai/codex-security/blob/main/LICENSE)
- OpenBot：[README 与 Intelligence 授权说明](https://github.com/CopilotKit/OpenBot/blob/main/README.md) · [模板 MIT](https://github.com/CopilotKit/OpenBot/blob/main/LICENSE)
- llama.cpp：[README](https://github.com/ggml-org/llama.cpp/blob/master/README.md) · [MIT](https://github.com/ggml-org/llama.cpp/blob/master/LICENSE)
- Qwen Code：[README](https://github.com/QwenLM/qwen-code/blob/main/README.md) · [Apache-2.0](https://github.com/QwenLM/qwen-code/blob/main/LICENSE)

以上用途与风险为依据文档作出的编辑判断，本期未安装或实机运行这些项目。代码许可证、模型权重、模型服务和附加组件许可须分别核对；星数不能替代质量与安全评估。

## 接下来值得跟进

如果只做一个小试验，可以挑一项现有编码任务，保留固定验收测试，比较自动选模和固定模型的用时、费用与返工量。需要 Agent 长时间执行的团队，则优先试一次任务中断、恢复和通知，确认失败能被及时发现。

对于科学与儿童安全研究，本期关注的是可复核的证据能否继续积累，避免把研究工具和未来承诺直接当作已经验证的实际效果。

上一期见[第 021 期：更强的模型，怎样进入日常工作](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-11.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
