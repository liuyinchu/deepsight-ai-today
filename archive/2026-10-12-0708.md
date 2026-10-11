# AI 前沿札记

> 更新日期：2026-10-12（Asia/Shanghai） · 第 001 期（增补）
>
> 信息范围：2026-10-06 至 2026-10-11；本次增补核查截至 2026-10-12 07:08（Asia/Shanghai）。
>
> [初版](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-10-12.md)与[本次增补前版本](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-10-12-0641.md)均已原样归档。

## Tibo 本人动态：reset 完成声明与 Pro 用量投票

**本人声明。** 北京时间 2026-10-11 12:01，Tibo（@thsottiaux）发帖称，此次 reset 已全部传播完成；该帖引用了他此前“当天结束前进行全局重置”的预告。因此，这条消息是完成声明，而非下一次 reset 的预告。[完成声明](https://x.com/thsottiaux/status/2109132167112782291) · [此前预告](https://x.com/thsottiaux/status/2109045392684032299)

**最新征询。** 同日北京时间 13:22，他向 Pro 用户发起用量方案投票：设置 5 小时限额、周用量翻倍，或不设 5 小时限额、周用量维持不变。[本人投票](https://x.com/thsottiaux/status/2109152716954222849)

**适用范围与编辑判断。** reset 短帖没有展开各产品、套餐或账户的到账情况，本期未登录账户验证；不能将其扩大解释为所有 OpenAI 产品的额度都已恢复。投票说明他在征求偏好，不代表新配额政策已经上线，也不足以推断下一次 reset 的时间。上述内容来自本人公开原帖，不作为匿名传闻处理。

## 10 月 11 日增补：本地推理的可靠性与开销

本次新增两条北京时间 10 月 11 日发布的运行时更新，保留后文 10 月 6—7 日的模型观察。它们关系到“回答是否用了正确上下文”和“高频检索请求是否被后台工作拖慢”。

### llama.cpp b11552：修复忙碌槽位的上下文被改写

**已核实的版本变化。** b11552 于 2026-10-10 19:52 UTC 发布，即北京时间 10 月 11 日 03:52，GitHub 标为 **Pre-release（预发布）**。发布说明描述：新请求指定一个仍在生成的 `id_slot` 时，旧逻辑可能先把 RAM 中更匹配的提示缓存载入该槽位，再等待；这会让原请求在错误上下文上继续生成。该版本改为保持忙碌槽位不动，让新请求等待。[发布记录](https://github.com/ggml-org/llama.cpp/releases/tag/b11552)

**项目方说明与编辑判断。** [合并修复 #30295](https://github.com/ggml-org/llama.cpp/pull/30295)附有回归测试，讨论中还有用户的限定配置测试与作者的回归检查；这些不代表所有部署已验证。对显式指定槽位、并发使用提示缓存的部署，这比单纯提升吞吐更值得检查。这里是风险判断，本期没有运行测试，也不把预发布构建当作已验证的生产升级建议。

### Ollama v0.40.3：暂时移除后台模型兼容性迁移

**已核实的版本变化。** v0.40.3 于 2026-10-11 01:39 UTC 发布，即北京时间 10 月 11 日 09:39。[发布记录](https://github.com/ollama/ollama/releases/tag/v0.40.3)将暂停后台模型升级列为面向 embedding 等请求性能的调整；对应[修复 #18908](https://github.com/ollama/ollama/pull/18908)进一步明确，暂时移除的是本地模型兼容性迁移，项目方称其给高请求量场景带来了逐请求开销和垃圾回收压力。

**编辑判断与待验证项。** 这与前文端侧检索的成本问题直接相关，但发布记录没有给出提速幅度。若运行旧版 Ollama 并批量生成向量，可用同一模型和同一批文本比较升级前后的尾延迟、内存与吞吐，再判断是否有收益；本期未做这个实验，也未据此确认它支持本文提到的每一个模型。

## 本期关注：云端短任务降价，端侧检索走向多模态

做摘要、分类和资料检索，往往需要反复处理大量小任务。本期关注两种不同的变化：Claude Haiku 5.5 调整云端调用的成本与迁移条件；EmbeddingGemma 2 把端侧向量检索扩展到文字、图像、音频和视频。它们分别服务生成任务与检索环节，不能按同一套跑分直接比较。

## 发生了什么

### Claude Haiku 5.5：看清价格门槛和迁移条件

**发布事实。** Anthropic 于 2026-10-07 发布 Haiku 5.5，Claude Platform 模型 ID 为 `claude-haiku-5-5`；公告也列出 AWS、Google Cloud 和 Microsoft Azure。它是首个提供可调 effort 设置的 Haiku 系列模型。这里核实的是官方发布与文档，不代表已逐地区、逐账户验证可用性。[发布公告](https://www.anthropic.com/claude-haiku-5-5)

**计费条件。** Claude API 标准价格以美元、每百万 token 计：提示不超过 100,000 token 时，普通输入为 0.10、输出为 0.50；超过门槛则分别为 0.50、2.50。门槛按全部输入计算，包含缓存读写；缓存、批处理、工具及地域选项另有计费规则，不能把这个单价当作完整任务账单。[价格文档](https://platform.claude.com/docs/en/about-claude/pricing)

**迁移限制。** 官方迁移指南提示重新统计 token，并调整 thinking 配置、采样参数和 assistant prefill；因此旧代码只替换模型 ID 未必能用。文档还明确，Amazon Bedrock 上的 Haiku 5.5 不支持 structured outputs，不能假设各平台功能完全一致。[迁移指南](https://platform.claude.com/docs/en/models/haiku-5-5/migration-guide)

### EmbeddingGemma 2：在设备上检索多种媒体

**发布事实。** Google 于 2026-10-06 发布 EmbeddingGemma 2，将文本（含代码）、图像、视频和音频映射到共享向量空间，面向本地搜索与检索增强生成（RAG）。官方已给出模型权重下载入口；公告中的 Gemini Enterprise Agent Platform Model Garden 支持仍标为即将提供。[发布公告](https://blog.google/innovation-and-ai/technology/developers-tools/embeddinggemma-2/)

**模型规格与开放范围。** Google 的模型卡及官方权重页列明 Apache 2.0 许可。完整模型为 7.4 亿参数，纯文本配置为 2.7 亿参数，视觉和音频编码器可按需加载；共享上下文为 8,192 token，默认向量为 768 维，可截短到 512、256 或 128 维。[模型卡](https://ai.google.dev/gemma/docs/embeddinggemma/model_card_2) · [Google 官方权重页](https://huggingface.co/google/embeddinggemma-2)

**项目方报告的限制。** 模型卡提醒，截短向量后需要重新归一化；128 维会明显损失多模态质量，更适合文本任务。它还要求使用 bfloat16 或 float32，避免 float16 导致 NaN 或静默质量退化。本期没有独立运行模型或复现厂商评测，不把公告里的领先性表述当作独立验证结果。[模型卡](https://ai.google.dev/gemma/docs/embeddinggemma/model_card_2)

## 为什么值得关注

以下是编辑判断：

- **云端方案值得比较“每个成功任务的成本”。** Haiku 的价格变化给高频短任务提供了新的候选，但 token 数、重试、工具调用和人工返工都可能改变最终支出。更有用的比较是在相同质量门槛下，用自己的样本测完成率、延迟和总费用。
- **端侧方案值得从检索环节试起。** EmbeddingGemma 2 的模块化结构和统一向量空间，给本地混合媒体索引提供了可试验的组件。是否省内存、是否足够快，要把编码器、索引和后续生成模型一起计算；仅在本地生成向量，也不能证明整条应用链路都不上传数据。
- **两项发布共同提示一种工程选择：按工作环节选模型。** 摘要或分类可以评估云端小模型，私有资料的召回可以评估端侧嵌入模型。这个组合只是待验证的设计思路，本期没有测试二者的集成效果。

## 还需要观察

1. Haiku 5.5 在中文摘要、结构化抽取及多轮工具调用中，相对现有方案的成功率和实际账单怎样？提示跨过 100,000 token 门槛时是否仍划算？
2. EmbeddingGemma 2 在中文音频、截图和视频片段上的检索效果如何？不同设备、量化方式及向量维度，会怎样影响召回率、峰值内存和延迟？
3. 两种方案都需要业务样本验证。平台上线声明、模型规格和项目方测试，尚不能替代真实应用中的独立评估。

## 来源

以下均为原始来源；查阅日期统一为 2026-10-12（Asia/Shanghai）。动态文档未标注日期时，不推断其首次发布时间。

1. [Anthropic：Introducing Claude Haiku 5.5](https://www.anthropic.com/claude-haiku-5-5)
   发布日期：2026-10-07；对应事件日期：2026-10-07。
2. [Claude Platform Docs：Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
   发布／更新日期：页面未标注；引用本次查阅时的费率与门槛规则。
3. [Claude Platform Docs：Claude Haiku 5.5 migration guide](https://platform.claude.com/docs/en/models/haiku-5-5/migration-guide)
   发布／更新日期：页面未标注；引用本次查阅时的迁移与平台限制。
4. [Google：EmbeddingGemma 2: an open, lightweight multimodal embedding model](https://blog.google/innovation-and-ai/technology/developers-tools/embeddinggemma-2/)
   发布日期：2026-10-06；对应事件日期：2026-10-06。
5. [Google AI for Developers：EmbeddingGemma 2 model card](https://ai.google.dev/gemma/docs/embeddinggemma/model_card_2)
   首次发布日期：页面未标注；页面标注最后更新：2026-10-06 UTC。
6. [Google 官方模型仓库：google/embeddinggemma-2](https://huggingface.co/google/embeddinggemma-2)
   首次发布日期：页面未标注；用于交叉核对模型身份、许可和规格。
7. [llama.cpp：b11552 发布记录](https://github.com/ggml-org/llama.cpp/releases/tag/b11552)
   发布／事件日期：2026-10-10 19:52 UTC，折合 2026-10-11 03:52（Asia/Shanghai）；预发布状态与时间由 GitHub Releases 元数据交叉核对。
8. [llama.cpp：server: leave a busy slot untouched when a request pins it，PR #30295](https://github.com/ggml-org/llama.cpp/pull/30295)
   创建日期：2026-10-10；合并日期：2026-10-10（UTC）；用于核对修复条件、回归测试声明与未确认范围。
9. [Ollama：v0.40.3 发布记录](https://github.com/ollama/ollama/releases/tag/v0.40.3)
   发布／事件日期：2026-10-11 01:39 UTC，折合 2026-10-11 09:39（Asia/Shanghai）；时间由 GitHub Releases 元数据交叉核对。
10. [Ollama：server: skip local model compatibility migration，PR #18908](https://github.com/ollama/ollama/pull/18908)
    创建／合并日期：2026-10-09（UTC）；用于核对后台迁移变更的原因，不将合并日写成版本发布日期。
11. [Tibo（@thsottiaux）：reset 完成声明，X 原帖](https://x.com/thsottiaux/status/2109132167112782291)
    发布日期：2026-10-11 04:01 UTC，折合 2026-10-11 12:01（Asia/Shanghai）；查阅日期：2026-10-12。
12. [Tibo（@thsottiaux）：Global reset by EOD，X 原帖](https://x.com/thsottiaux/status/2109045392684032299)
    发布日期：2026-10-10 22:16 UTC，折合 2026-10-11 06:16（Asia/Shanghai）；查阅日期：2026-10-12。
13. [Tibo（@thsottiaux）：Pro 用量方案投票，X 原帖](https://x.com/thsottiaux/status/2109152716954222849)
    发布日期：2026-10-11 05:22 UTC，折合 2026-10-11 13:22（Asia/Shanghai）；查阅日期：2026-10-12。

