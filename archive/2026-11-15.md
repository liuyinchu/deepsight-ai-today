# AI 前沿札记

> 更新日期：2026-11-15（Asia/Shanghai） · 第 024 期
>
> 信息范围：11 月 13 日至 11 月 14 日发布的官方公告和版本记录；查阅日期：2026-11-15。公告日期沿用原文，GitHub 版本发布时间换算为北京时间。

## 本期关注：给助手接上工具后，怎么判断它有没有解决问题？

DeepSeek 把搜索、读链接和解析文件接入现有 Flash API，开发者可以少维护几层工具。Ai2 的 XYBench 则提醒我们：即使助手能查资料，它也可能沿着用户提出的错误思路继续走。对正在更换 Copilot 模型的团队来说，这类问题正好可以纳入回归测试。

## 01｜DeepSeek Flash API 增加搜索、文件和链接能力

DeepSeek **11 月 13 日**开放了原先在自家应用里使用的一批能力。现有的 V4.1-Flash（`deepseek-flash`）在 Messages、Responses 和 Chat Completions 接口中提供联网搜索、链接读取和文件解析。调用方启用相应功能后，工具调用和对话上下文的拼接由 DeepSeek 服务端处理。非思考模式下可检索网页或图片并附引用；思考模式下则可在多步推理中搜索和读网页。搜索、网页抓取、OCR、语音识别和语音合成也有独立接口。

这次变化是 **API 新增能力**。扩展工具调用费在 token 费用之外计收。官方也明确要求对外提供应用服务时启用安全校验，且开发者仍需负责内容审核等义务。

**我们的观察：** 做资料问答时，可以先用公开文档比较同一批问题的引用准确率和单次请求总成本。打算上传内部文件的团队，还要先核对数据处理条款和可上传的范围。

来源：[DeepSeek 中文公告](https://api-docs.deepseek.com/zh-cn/news/news261113/)。发布：2026-11-13；查阅：2026-11-15。

## 02｜Ai2 发布 XYBench，检查助手能否识别问错的方向

Ai2 **11 月 13 日**公告发布 XYBench。一项典型测试是这样的：有人问怎么换一台吸力更大的吸尘器，但真正要解决的是当前吸力变弱，先检查软管有没有堵塞可能更有用。论文收录 **8,115** 个编程和日常问题，分别衡量回答有没有给出能解决实际问题的建议、有没有把重点放在那里，以及有没有指出原先的误解。

据作者公布的单轮评测，在 Stack Overflow 子集上，五个受测模型有 **75%–86%** 的回答给出了字面请求的解法，**59%–71%** 给出了实际目标的解法。这里的比例只适用于该子集和测试设置；部分题目是合成的，评分主要由模型完成，并抽样与人工评分对照。数据卡标注 **CC BY-NC-SA 3.0**，下载需通过 Hugging Face 的访问条件。

**我们的观察：** 可以在客服或编程助手的回归题中加入几道「用户已经选错方案」的例子，分别记录助手是否发现问题、是否给出能用的替代办法。也应保留用户意图明确的对照题，看看助手会不会过度纠正。

来源：[Ai2 公告](https://allenai.org/blog/xybench) · [论文](https://arxiv.org/abs/2609.06842) · [数据卡](https://huggingface.co/datasets/allenai/XYBench-dataset)。论文预印本提交：2026-09-06；发布公告：2026-11-13；查阅：2026-11-15。

## 03｜GitHub Copilot 公布 12 月换模计划

GitHub **11 月 13 日**宣布 Gemini 4 Hydrogen 在 Copilot 正式可用，向 Pro、Pro+、Max、Business 和 Enterprise **逐步推出**。Business 和 Enterprise 的管理员可以控制模型访问。GitHub 同日预告，Gemini 3.8 Flash 和 Gemini 4 Argon 等模型将于 **12 月 14 日**在 Copilot 停用，分别推荐替换为 Hydrogen 和 Carbon。这里新增的是 Copilot 的使用入口和停用时间表；Hydrogen 模型本身已在[第 022 期](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-13.md)介绍。

**我们的观察：** 如果团队的工作流固定了模型，先检查管理员策略，再拿同一批任务比较完成率、耗时和费用。建议把多步操作中需要人工确认的节点也列入验收。

来源：[Hydrogen 上线公告](https://github.blog/changelog/2026-11-13-gemini-4-hydrogen-in-github-copilot/) · [12 月停用清单与例外](https://github.blog/changelog/2026-11-13-upcoming-deprecation-of-selected-github-copilot-models-in-december/)。发布：2026-11-13；查阅：2026-11-15。

## 每日 GitHub AI 热点

**采样：2026-11-15 09:03（Asia/Shanghai，UTC+8）。** 两列星数来自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)读取；「今日新增」保留页面 `stars today` 口径，不表示精确的滚动 24 小时。以下为本期选取的五个项目，按该页面当日增星重排。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [dream-num/univer](https://github.com/dream-num/univer) | 495 | 24,503 | 把文档和表格编辑、公式计算接入自己的产品或助手。开源 Web SDK 核心为 Apache-2.0；已发布 1.5.0，商用扩展另有许可。评估前先明确功能是否需要商用插件，并测试文件往返兼容性。 |
| [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) | 227 | 120,080 | 为编码助手提供简写技能，并可用代理压缩部分工具输出。仓库为 Apache-2.0；已有 4.1.0 版。适合想控制 token 开销的团队，先看压缩后是否仍保留诊断、日志和评审所需的信息。 |
| [ashhart/TensorFold](https://github.com/ashhart/TensorFold) | 37 | 2,165 | 本地模型推理引擎，适合对比不同硬件上的服务表现。引擎为 Apache-2.0，权重许可另查；已有版本发布，但一些模型和功能仍是实验性支持，不能把一套硬件的表现套用到其他配置。 |
| [Tencent/LoopForge](https://github.com/Tencent/LoopForge) | 33 | 914 | 团队自托管的编码助手工作流，把实现、评审和人工批准串起来。主体为 MIT，第三方组件按各自条款；已有 0.3 系列和候选版。项目说明其默认 Docker 部署适用于受信团队，不宜当作不受信租户间的强隔离。 |
| [ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui) | 9 | 17,371 | 用事件协议把助手的流式回复、工具状态和交互接入前端。MIT；已有 SDK、参考实现和版本发布。接入时要分别检查前端展示、服务端授权和会话间的数据隔离。 |

项目与许可（查阅：2026-11-15）：

- Univer：[README](https://github.com/dream-num/univer/blob/dev/README.md) · [许可证](https://github.com/dream-num/univer/blob/dev/LICENSE)
- Caveman：[README](https://github.com/JuliusBrussee/caveman/blob/main/README.md) · [许可证](https://github.com/JuliusBrussee/caveman/blob/main/LICENSE) · [v4.1.0](https://github.com/JuliusBrussee/caveman/releases/tag/v4.1.0)
- TensorFold：[README](https://github.com/ashhart/TensorFold/blob/main/README.md) · [许可证](https://github.com/ashhart/TensorFold/blob/main/LICENSE)
- LoopForge：[README](https://github.com/Tencent/LoopForge/blob/main/README.md) · [许可证](https://github.com/Tencent/LoopForge/blob/main/LICENSE)
- AG-UI：[README](https://github.com/ag-ui-protocol/ag-ui/blob/main/README.md) · [许可证](https://github.com/ag-ui-protocol/ag-ui/blob/main/LICENSE)

以上判断依据公开资料，本期未安装实测。星数反映关注度，不能代替质量和安全评估。模型、服务和依赖的许可需分别核对。

### 版本动态

- **Univer v1.5.0**：于 11 月 14 日 18:33（北京时间）发布。增加独立图表工作表、堆叠折线图和误差线。版本说明也记录了公式计算的内存改动；嵌入式应用可以用既有的复杂工作簿测试保存再打开的效果。来源：[发布记录](https://github.com/dream-num/univer/releases/tag/v1.5.0)。
- **TensorFold v1.2.8**：于 11 月 14 日 06:47（北京时间）发布。新增按请求 ID 取消推理，CUDA 后端支持 Qwen3 嵌入和重排模型，Metal 上的 DeepSeek-V4.1 文本推理仍标为实验性。来源：[发布记录](https://github.com/ashhart/TensorFold/releases/tag/v1.2.8)。

## 接下来值得跟进

下一次给助手换模型或接新工具时，可以用同一组真实任务测三个结果：它有没有找到可靠来源、有没有解决实际问题、完成一次要花多少钱。三项分开记，才能看出改动改善了哪一环。

上一期见[第 023 期：模型能做什么，部署时又需要什么](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-14.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
