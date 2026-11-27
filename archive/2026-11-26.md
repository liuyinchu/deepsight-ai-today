# AI 前沿札记

> 更新日期：2026-11-26（Asia/Shanghai） · 第 035 期
>
> 信息范围：11 月 24 日至 26 日早间的发布与核验。查阅日期：2026-11-26。

## 本期关注：一次通过，离稳定可靠还有多远？

Anthropic 重写 Claude 的 API 时，约 98% 的移植测试已经通过，抽查生产流量却只有约四分之一匹配。DeepMind 的新评测也问了一个类似的问题：模型能答对一道题，换张图、换种问法再多试几次，结果还能保持吗？两件事都值得看评测是怎么做的，而不只看最后一个百分比。

## 01｜Claude Messages API 迁往 Rust：复用同一次模型输出，再查服务端差异

Anthropic 11 月 25 日发布工程复盘。团队把 Python 实现的集成测试迁到 Rust；4 月约 98% 的测试已通过，但生产流量对照中仅约四分之一的输入匹配。检查发现，移植后的测试有 100 多处断言变宽。团队改用真实请求对照新旧实现，并设法把模型的随机性与服务端差异分开：模型只生成一次，同一份输出再交给新实现，分别比较推理前后的处理。

行为一致也不总意味着需求满足。一次修复把网页搜索工具的描述写死，虽然通过了对照，工程师仍否决了，因为服务需要动态获取当前描述。迁移后，Anthropic 报告一个区域的每请求 CPU 用量约降 **70%**；另在所观测的 Claude Code 客户端流量里，首 token 时间中位数约降 **20%**。两组指标的观测范围不同，也不是所有应用都能达到的收益。做类似重写时，值得先保留一套不随新实现改写的验收输入，再明确哪些旧行为应该保留。

来源：[Anthropic 工程复盘、生产流量对照与图 15](https://www.anthropic.com/research/migrating-api-to-rust)（11 月 25 日）。

## 02｜DeepMind 的 HFE：单次答对率与反复答对率分开看

Google DeepMind 11 月 24 日发布 Humanity’s First Exam。报告将 **544** 个文字或图像提示分为 **136** 组，每组有四个变体，每个提示生成六次。严格组分数估计的是：每个变体从六次结果里不放回抽四次，四个变体共十六次全部正确的概率。若某一变体六次只答对五次，即使其他变体全对，这一组的概率也只有三分之一。

在报告表 4，GPT-6.2 Astra（xhigh）从单次 **97.8%** 降到严格组 **92.4%**；Claude Fable 5.2（max）从 **93.4%** 降到 **73.7%**。两者在同表「十六次至少答对一次」这一较宽松指标上的点估计却都是 100%。这些是作者在指定推理设置下、单轮且不使用工具的结果，评分采用 Gemini 3.1 Pro；不能当作实际工作流的可靠率。对于要自动执行的步骤，比起让模型多试几次后只记最好结果，更有用的是记录错误是否会随提示变体重复出现。

来源：[DeepMind 发布说明](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/humanitys-first-exam/)（11 月 24 日） · [技术报告 §§3.3–4.1、表 4](https://storage.googleapis.com/deepmind-media/papers/hfe_tech_paper.pdf)。

## 03｜VP 与 VNP 后续：OpenAI 扩到四层算术复杂度

OpenAI 的项目记录将新版标为 11 月 24 日，仓库随后合入了扩展稿。与上期提到的精确 VP/VNP 分离相比，新说明列出任意域上的三个严格包含：**VF ⊊ VBP ⊊ VP ⊊ VNP**。作者分别用路径、完全二叉树和方格上的状态和构造分隔例，对应算术公式、算术分支程序、算术电路及其验证器求和所定义的层级。

核验范围也有一处实质变化：新 Lean 范围说明称，方格族还排除了多项式规模的「按次数分量取极限」的边界电路，但论文更强的显式定量下界仍未形式化。公开 Comparator 挑战文件陈述的是**精确类**的三级分离；合并记录没有报告为这次合并运行 Lean 或 LaTeX 构建。这里核对的是源码和作者对范围的说明，未独立重跑。这仍不是布尔计算里的 P vs NP。

来源：[前版范围](https://github.com/openai/math/blob/f5702854040e97fa7675c21ba37a9608f5f92dd3/lean/docs/379.md) · [新版范围（固定提交）](https://github.com/openai/math/blob/3344da59991ed94f9ba711217b29c1d760badfa6/lean/docs/379.md) · [Comparator 陈述](https://github.com/openai/math/blob/3344da59991ed94f9ba711217b29c1d760badfa6/lean/ComparatorChallenges/ArithmeticHierarchy.lean) · [合并与验证记录](https://github.com/openai/math/pull/6)。

## 补看｜ChatGPT 与 Codex 将共享用量，Instant 不扣这份额度

截至 11 月 26 日查阅，OpenAI 帮助中心称 Plus、Pro、Business 从 12 月开始陆续使用 ChatGPT 与 Codex 的共享用量，少量账号会提前启用。用量按 token 计，模型和任务会影响消耗，Instant 聊天不扣这份共享额度。启用后，能在「设置 → 用量」查看合计剩余及重置时间。公告没有给出每个账号启用的具体日期，也不是 API 计费变更。

来源：[OpenAI：ChatGPT 与 Codex 用量说明](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits)。

## 补看｜Claude Code 2.1.332 可加载 MCP 技能，旧版需单独测试

11 月 25 日发布的版本加入 MCP 服务端技能（Beta），在菜单中以 `server:skill` 显示；同时加入 `PreSkillShellCommand`，可在技能或命令载入时拦下其内联 shell 命令。发布说明提醒，2.1.331 及更早版本读取含这个事件的配置会在启动时弹出对话框。团队若共用配置，先核对版本，并用无副作用的命令验证拒绝规则；该钩子不等于对插件或整个 MCP 服务做过审计。

来源：[Claude Code v2.1.332 发布说明](https://github.com/anthropics/claude-code/releases/tag/v2.1.332)。

## 补看｜ggml 1.1.0 减少空等，并补 Metal 的 NVFP4 支持

11 月 25 日的 ggml **v1.1.0** 和 llama.cpp 预发布 **b12707** 都包含非 OpenMP 构建过度轮询 CPU 的修复。另一个独立变化是 ggml 1.1.0 为 Metal 增加 NVFP4 GGUF 的矩阵运算内核；维护者说明对应运算此前会退回 CPU。对 Apple GPU 部署，先确认实际构建包含哪些提交，再用目标模型和批大小比较推理速度与 CPU 占用。上游发布不代表下游包已跟进。

来源：[ggml v1.1.0](https://github.com/ggml-org/ggml/releases/tag/v1.1.0) · [llama.cpp b12707](https://github.com/ggml-org/llama.cpp/releases/tag/b12707) · [Metal 实现与测试条件](https://github.com/ggml-org/llama.cpp/pull/32825)。

## 每日 GitHub AI 热点

2026-11-26 约 09:03（北京时间）读取 [GitHub Trending 日榜](https://github.com/trending)。累计 star 与「stars today」来自同一份页面快照；后者不代表精确滚动 24 小时。核对了仓库说明、版本与许可，未安装或试跑。

| 项目 | 累计 star | 日榜增星 | 用途、许可与注意点 |
| --- | ---: | ---: | --- |
| [Diagram Design](https://github.com/cathrynlavery/diagram-design) | 73,041 | +1,141 | MIT。给编码助手的 HTML/SVG 图解技能，仓库有示例图库，未见独立版本发布。适合做文档配图；发布前仍要核对图形关系、标签和窄屏可读性。 |
| [Firecrawl](https://github.com/firecrawl/firecrawl) | 210,046 | +1,135 | 主体 AGPL-3.0，SDK 和部分界面组件另用 MIT。有自托管方案和 2.x 发布，适合把网页转换成可检索材料。自托管需管理浏览器与队列；从外部抓来的正文仍是不受信任的输入。 |
| [AFFiNE](https://github.com/toeverything/AFFiNE) | 78,495 | +574 | 可自托管的文档和白板工作区，有社区版和 Canary 构建。仓库许可有边界：一般代码按 MIT，指定后端目录及相关原生组件另受条款约束；别把 Canary 当稳定版，AI 接入也需单独检查数据流向。 |
| [OpenSRE](https://github.com/Tracer-Cloud/opensre) | 15,519 | +348 | Apache-2.0，已有 0.1 系列发布。可用自有模型诊断 CI/CD 失败并提出修复，适合先用测试仓库试用。CI 日志可能含敏感信息，也不宜把诊断工具直接接到默认可写的生产凭证。 |
| [OpenVINO](https://github.com/openvinotoolkit/openvino) | 12,652 | +191 | Apache-2.0 的推理部署工具，有 2026.4.x 稳定发布。适合按目标 Intel 硬件比较模型部署效果；转换是否支持特定算子、外部权重能否再分发，应按模型和后端分别判断。 |

## 还需要观察

线上迁移值得追问哪些真实请求仍没有覆盖；模型评测则要看同一类错误是否会在换问法后重现。算术复杂度项目的下一步是第三方按固定提交重跑，并对照边界电路的论文表述与 Lean 定理。涉及版本或用量的切换，以自己环境中的配置与用量页为准。
