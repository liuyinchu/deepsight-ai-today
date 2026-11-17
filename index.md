# AI 前沿札记

> 更新日期：2026-11-17（Asia/Shanghai） · 第 026 期
>
> 信息范围：11 月 15 日至 17 日查得的公告、文档和版本记录；查阅日期：2026-11-17。研究解读涉及 10 月旧预印本，另行注明。

## 本期关注：接入变简单之后，什么还需要自己验证？

阿里云提供了新的旗舰模型入口，Hugging Face 把语言模型的托管路由交给 OpenRouter，Liquid AI 则把音频纳入结构化提取流程。三项变化都减少了集成工作，但它们没有替团队回答三个实际问题：任务要花多少钱、输入经过谁、结果错了能不能发现。

## 01｜Qwen4-Max 正式上线，先用完整任务算成本

阿里云 **11 月 16 日**宣布推出 Qwen4-Max；Model Studio 同日更新正式模型 `qwen4-max` 的调用文档。它接受文本、图片及视频输入，输出文本；**思考模式最大输入 983,616 tokens**，不等于可输出相同长度。查阅时，新加坡区域文档列出的标准价为每百万输入 tokens **1.50 美元**、每百万输出 tokens **4.50 美元**，思考 tokens 按输出计费。隐式缓存命中价是另一口径，不能当作所有请求的价格。本条核实的是托管 API，不代表同一模型权重开放下载。

**我们的观察：** 评估跨文件或长视频任务时，建议同时记录完成率、输入与输出用量和重试成本；否则长上下文带来的便利容易被高估。阿里自己的基准可作为选测试题的参考，不能代替团队样本。

来源：[阿里云发布公告](https://www.alibabacloud.com/en/press-room/alibaba-unveils-qwen4-max?_p_lc=1) · [Model Studio 模型、区域与计费](https://www.alibabacloud.com/help/en/model-studio/qwen4-max)（11 月 16 日）。查阅：2026-11-17。

## 02｜Hugging Face 的语言模型调用改由 OpenRouter 路由

Hugging Face **11 月 16 日**宣布，Inference Providers 的语言模型请求改由 OpenRouter 路由。开发者仍可使用 Hugging Face token 与原有计费入口；同一模型有多个提供商时可以自动切换，也可用 `模型:提供商` 显式指定。旧有的语言模型自带上游密钥方式不再支持；图像、音频等非语言模型推理仍沿用原来的路由。这里的适用范围是已有推理提供商上线的模型，不是整个 Hub 都可以立即调用。

**我们的观察：** 面向普通用户的应用可对比自动切换前后的失败率、延迟和完成成本。受地区或数据处理约束的工作负载，应先固定经过批准的提供商，并验证所选配置是否满足要求，不能把同一 API 地址当成同一数据路径。

来源：[Hugging Face 官方说明](https://huggingface.co/blog/openrouter)（11 月 16 日）。查阅：2026-11-17。

## 03｜Liquid AI 的决策模型可以从音频中提取结构化结果

Liquid AI **11 月 16 日**发布 `d1.2` 和 `d1.2-ultra`，称当日已可通过 API 使用。两款新增音频输入，以及值抽取、文本片段定位、图片框选、数量估计和多选分类五类任务。这里的变化不是让模型输出语音，而是让应用能把音频直接交给决策模型执行分类或抽取等工作。厂商公布的效果比较不是本期独立测试结果。

**我们的观察：** 既有「先转写、再提取」系统不必立即替换。用同一组经授权的录音，对照两条路线对数字、人名、否定语句和缺失字段的处理；除了抽取准确率，还要看哪些错误可以回溯到原始录音。

来源：[Liquid AI 技术发布与 API 示例](https://www.liquid.ai/blog/d1-2)（11 月 16 日）。查阅：2026-11-17。

## 补看｜格点上的标准模型候选构造，Lean 检查了什么？

物理学家 Nathaniel Craig 在 **11 月 15 日**公开说明了他如何与 GPT‑6 Astra 合作，推进标准模型的一个有限格距候选构造。结果来自 **10 月 6 日**的预印本，此次新内容是作者解读。预印本随附的 Lean 报告称，数学配套材料的 16 个模块通过编译，证明经过内核重放；报告也记录了原参考比对没有通过，不能笼统说所有比对检查全过。本期没有独立复跑。

有限格距上的构造不等于已证明反射正性，也不等于已得到完整相互作用理论的连续极限或一套实用模拟方法。把已检查的数学陈述和仍待解决的物理问题分开，比简单问「AI 做没做出物理发现」更有用。

来源：[Craig 的原文](https://openai.com/index/step-toward-defining-the-standard-model/)（11 月 15 日） · [预印本](https://arxiv.org/abs/2610.09093)（10 月 6 日） · [随附 Lean 报告](https://arxiv.org/src/2610.09093v1/anc/lean-verification-report.pdf)。查阅：2026-11-17。

## 每日 GitHub AI 热点

**采样：2026-11-17 08:57（Asia/Shanghai，UTC+8）。** 两列来自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily) 读取；「今日新增」按页面 `stars today`，不是精确滚动 24 小时。本表选取四个项目，按当次新增重排。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [dream-num/univer](https://github.com/dream-num/univer) | 1,828 | 27,782 | 给应用嵌入电子表格、编辑和计算能力，可作为 Agent 的办公界面。开源核心 Apache-2.0，有 v1.5.2 发布；选型时把核心能力和商业扩展逐项对照，不把后者视作同一许可。 |
| [ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui) | 791 | 18,986 | 为 Agent 与前端约定消息、状态等事件，适合需要流式显示任务进度的团队。MIT，有日期版本；协议不替应用做用户权限隔离。 |
| [kajisho5/ffmpeg-skill](https://github.com/kajisho5/ffmpeg-skill) | 367 | 6,727 | 把剪切、字幕等 FFmpeg 操作包装成编码助手可调用的本地工具；仓库 MIT，已有 v3.10.1。FFmpeg 构建与编解码器许可另算，处理原素材建议留副本。 |
| [google-research/paper-orchestra](https://github.com/google-research/paper-orchestra) | 313 | 1,519 | 辅助整理研究材料与撰写论文草稿的多 Agent 框架。Apache-2.0；采样时未见正式 GitHub Release。作者仍应逐条核对引用、结果及署名；未公开材料接入外部模型前需确认数据去向。 |

项目原始资料为上表各仓库的 README、许可证和发布记录，查阅于 2026-11-17。星数反映关注度，不代表质量或安全；本期未实装。

### 版本动态｜Claude-Mem 的跨库导入进入正式版本

上期还只在主分支的 Mem0／Supermemory 导入，已随 **Claude-Mem v15.19.0** 发布（11 月 16 日 10:04，北京时间）。这是一次性的单向导入，过程不调用模型；支持按项目导入并先用 `--dry-run` 预览，不是跨服务持续同步。迁移前建议先用少量已脱敏记录核对项目归属与导入结果。

来源：[Claude-Mem v15.19.0 发布说明](https://github.com/thedotmack/claude-mem/releases/tag/v15.19.0)。查阅：2026-11-17。

## 接下来值得跟进

同一批任务跑两遍：一次用原方案，一次用新接口。至少记录成功率、每个成功任务的实际费用，以及哪些数据发送给了哪家服务。接入步骤少了多少是一件事，交付质量有没有变好是另一件事。

上一期：[第 025 期：选模型之前，先确定它要跑在哪里](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-16.md) · [归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)
