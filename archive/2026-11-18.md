# AI 前沿札记

> 更新日期：2026-11-18（Asia/Shanghai） · 第 027 期
>
> 信息范围：11 月 17–18 日查得的公告、模型卡和版本记录；查阅日期：2026-11-18。实验性经济统计的参考期为 2023–2024 年。

## 本期关注：成本降了，怎样确认任务真的做完了？

OpenAI 和微软都发布了面向工作或编程的新模型，商汤则用重复执行的模型层换取更深的计算。API 单价、每次任务花多少 tokens、本地要多少显存，是三个不同的问题。把它们和验收结果一起看，才知道节省发生在哪里。

## 01｜GPT‑6.2：把账算到交付，不只算到每百万 tokens

OpenAI **11 月 17 日**发布 GPT‑6.2 Astra 和 Sol，开始在 ChatGPT Work、Codex 推出，并提供 `gpt-6.2-astra` 与 `gpt-6.2-sol` API。标准 API 每百万输入／缓存输入／输出 tokens，Astra 分别为 **8／0.40／40 美元**，Sol 为 **2／0.10／10 美元**；新版 Astra 的普通输入和输出单价分别比上一代低 20%。在 OpenAI 自行报告的 GDPval v2 专业任务评估中，Astra 相比 GPT‑6 Astra 高 6.2 个百分点，输出 tokens 少 22%。这是产品方的基准结果，不是所有生产流程的降本比例。

**我们的观察：** 评估代理时，除了成功任务的总费用，单独统计「没有完成却没有把阻塞写清楚」的比例。对同一组长任务记录工具调用、返工次数和人工补做时间，比只比较 API 单价更接近日常成本。

来源：[OpenAI 发布公告、评估和价格](https://openai.com/index/introducing-gpt-6-2-astra-sol/)（11 月 17 日）。查阅：2026-11-18。

## 02｜微软把两档编码模型放进同一套工作流

微软 **11 月 17 日**在 Ignite 公布 MAI Code 2 和 Code 2 Flash，两款均进入 GitHub Copilot 模型选择器，但可用档位和组织策略不同；Code 2 Flash 在 Foundry 仍为限量预览。微软把前者定位为复杂、耗时较长的仓库任务，把 Flash 定位为局部修改、代码解释等高频短任务。微软用四项编程基准的平均成绩和自身计价条件做成本对比；不同推理设置、重试率和任务组合会改变对比。同期宣布的 MAI Licensed Weights 面向获准的客户，不等于权重公开下载。

**我们的观察：** 用小改动和跨文件修复分别做评估，检查差异是否真能靠分流节省成本。除了单测，还应核对需求遗漏、改动范围和中途停下时给出的说明，避免快速档把返工转给评审者。GitHub 另宣布 Copilot 中旧版 MAI-Code-1.1-Flash 将于 12 月 18 日退役；有固定模型配置的团队可先验证管理员是否启用了新模型。

来源：[微软 AI 两款模型说明](https://microsoft.ai/news/introducing-mai-code-2-and-mai-code-2-flash/) · [GitHub Copilot 可用范围](https://github.blog/changelog/2026-11-17-mai-code-2-and-mai-code-2-flash-in-github-copilot/) · [旧模型退役通知](https://github.blog/changelog/2026-11-17-upcoming-deprecation-of-mai-code-1-1-flash-in-december/)（11 月 17 日）。查阅：2026-11-18。

## 03｜SenseNova-Loop-9B：显存数字要连着上下文长度一起读

商汤 **11 月 17 日**发布 SenseNova-Loop-9B 正式版，开放权重和代码，模型卡列出 Apache-2.0 许可。模型约有 90 亿个参数，以共享权重重复执行中间的 24 层：存储 32 个不同的解码层，每次前向执行 56 层。它输入文字、图片并输出文字，支持最高 512K tokens；模型卡所列约 **15 GiB** 是开始部署前可以参考的估算，条件是 **256K 上下文、4 位权重和 8 位 KV 缓存**，不是 512K 的显存需求。厂商所报效果有强有弱：与 Qwen3.8-27B 相比，AIDABench 为 65.31 对 65.01，NL2Repo 为 41.36 对 44.60。

**我们的观察：** 用量化版试部署时，除了测能否载入，还应测首次输出时间、越长越难的输入以及并发下的显存。共享权重减少需要存储的参数，并不意味着重复执行的层没有耗时。

来源：[SenseNova 正式模型卡、权重与条件](https://huggingface.co/sensenova/SenseNova-Loop-9B)（11 月 17 日；预览版为 10 月 31 日）。查阅：2026-11-18。

## 补看｜美国首次发布这套广义 AI 生产核算，口径并非生产率

美国经济分析局 **11 月 17 日**发布实验性统计。按其广义 AI 口径，2024 年相关生产贡献 **1,411 亿美元现价增加值**，占美国 GDP 约 **0.5%**，比 2023 年名义增长 **27.6%**。按原表推算，数据中心建设增加值贡献了这部分增加值增量的约 **26%**。报告使用的 “super intelligence” 是这套统计对广义 AI 的命名，并非在衡量达到某个智能门槛的系统。

这份数据反映 2023–2024 年的生产，不是 2026 年的采用率，也不能用来推断 AI 带来的劳动生产率增幅。厂商成本和效率变了多少，与宏观统计测到了多少经济活动，需要两套证据。

来源：[美国经济分析局文章与表 1](https://apps.bea.gov/scb/issues/2026/11-november/1126-experimental-super-intelligence.htm) · [统计口径与数据入口](https://www.bea.gov/data/special-topics/super-intelligence)（11 月 17 日）。查阅：2026-11-18。

## 每日 GitHub AI 热点

**采样：2026-11-18 08:57（Asia/Shanghai，UTC+8）。** 新增与累计来自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily) 读取；新增按页面 `stars today`，不等于精确滚动 24 小时。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [obra/superpowers](https://github.com/obra/superpowers) | 1,917 | 319,332 | 为编程助手提供可组合的规划、测试和评审流程。MIT，已有 v7.0.2；助手遵循流程不等于代码已经过独立审查。 |
| [alibaba/open-code-review](https://github.com/alibaba/open-code-review) | 763 | 53,726 | 把改动和相关文件交给可配置模型，生成结构化代码审查结果。Apache-2.0，已有 v1.13.11；接入私有仓库前检查模型的数据处理边界，并抽样核对误报。 |
| [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill) | 696 | 69,061 | 给助手加入面向近期信息的多来源检索能力。MIT，已有 v4.3.2；不同上游接口可用性不同，写结论前仍要回到原始出处。 |
| [Tencent/LoopForge](https://github.com/Tencent/LoopForge) | 591 | 3,142 | 自托管编程任务工作流，支持 Docker 和人工审批节点。v0.4.0；仓库许可证为 MIT，第三方组件各有条款。运行隔离与凭证权限仍须自行配置。 |
| [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills) | 350 | 10,105 | 为使用 Stitch 的助手提供设计与前端工作流技能。Apache-2.0，已有 v1.0；仓库许可不等于托管设计服务的使用条款。 |
| [jingyaogong/minimind](https://github.com/jingyaogong/minimind) | 192 | 65,967 | 面向学习者的小模型训练项目，提供 v2 方案。代码 Apache-2.0；训练耗时取决于设备与参数，训练数据和权重的条款应分别核对。 |

以上用途、许可和版本查自各仓库 README、许可证及发布记录（查阅：2026-11-18）。星数反映关注度，不代表采用建议；本期未安装这些项目。

### 版本动态｜Claude-Mem 的导入预览不再依赖后台服务

承接上期的跨库导入：v15.20.0 改善了 Supermemory 文档重导的判重，文档内容没变时，供应商处理元数据不再让新机制重复写入。但若某个文档最初由 v15.19.0 导入，且之后供应商改过这些元数据，升级后第一次重导仍可能多导一次。北京时间 **11 月 18 日**发布的 v15.21.0 又把 `import --dry-run` 与本地服务设置和认证分开；真正写入时仍会先检查服务。想迁移的团队可先预览，再用少量记录核对版本和重复条数。

来源：[v15.20.0 发布说明](https://github.com/thedotmack/claude-mem/releases/tag/v15.20.0) · [旧版兼容边界](https://github.com/thedotmack/claude-mem/pull/6206) · [v15.21.0 发布说明](https://github.com/thedotmack/claude-mem/releases/tag/v15.21.0)。查阅：2026-11-18。

## 接下来值得跟进

把同一批真实任务分成短改动、长流程和本地长上下文三组。每组先写明验收条件，再记录成功率、人工补做时间，以及每个成功任务的实际成本。若失败的任务没被计入，模型再便宜也不能证明交付变便宜了。

上一期：[第 026 期：接入变简单之后，什么还需要自己验证？](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-17.md) · [归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)
