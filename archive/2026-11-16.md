# AI 前沿札记

> 更新日期：2026-11-16（Asia/Shanghai） · 第 025 期
>
> 信息范围：11 月 13 日至 11 月 16 日查得的公告、文档更新和版本记录；查阅日期：2026-11-16。作者公告日期沿用原文，GitHub 发布时间换算为北京时间。

## 本期关注：选模型之前，先确定它要跑在哪里

华为云为昇腾和鸿蒙开发者提供了新的盘古托管入口；Datasette 则让数据助手能接到团队自己运行的模型。两者表面上都在增加模型选项，但真正需要验证的是：能不能在指定环境中完成任务、数据经过哪里、出问题时能不能换回原有方案。

## 01｜华为云 MaaS 上线 openPangu-2.1-Pro

华为云预告 **11 月 15 日**上线 openPangu-2.1-Pro，同日更新的体验文档已经给出预置服务和 API 调用步骤。目前该体验文档仅列出 **西南-贵阳一** 区域；API 调用需开通服务并使用 API Key，按实际用量计费。华为云把 ArkTS 生成和昇腾算子开发列为升级重点，并称 Agent、代码生成和吞吐有所提升；本期未独立验证这些能力。此次核实的是云服务入口，不据此推定权重下载或生产许可的范围。

**我们的观察：** 有现成鸿蒙项目的团队，可以把「能否在原工程中通过构建和测试」作为验收项，而不是只比较生成的代码片段。仍在用 2.0 的团队应对照官方停服表核查迁移计划：贵阳一的 2.0 Flash 和 Pro 所列版本将于 **11 月 30 日 00:00（北京时间）**下线。

来源：[华为云上线预告](https://www.huaweicloud.com/partners/support/news/latestnews/20261106.html)（11 月 6 日） · [模型体验文档](https://support.huaweicloud.com/bestpractice-maas/bestpractice_maas_0021_03.html)（11 月 15 日更新） · [下线公告](https://support.huaweicloud.com/bulletin-maas/bulletin-maas-0002.html)。查阅：2026-11-16。

## 02｜Datasette Agent 可以接到本地模型

`datasette-llm` 于 **11 月 15 日 07:59（北京时间）**发布 **0.1a11**，新增 OpenAI 兼容端点支持。它是 Datasette 插件共用的模型配置层；作者演示了如何让 Datasette Agent 使用通过 LM Studio、Ollama 或 llama.cpp 提供的本地模型。这意味着要换模型服务时，可以先改端点配置，而不必为每个插件重写一套对接逻辑。这个版本仍是 **alpha**；接口兼容也不等于每个模型都同样擅长调用工具或生成 SQL。

**我们的观察：** 可以拿一份脱敏 SQLite 副本比较本地与托管模型，分别记录查询是否正确、是否需要人工修正，以及实际出站流量。本地模型端点不保证其他插件、日志或工具调用也完全留在本机。

来源：[项目发布记录](https://github.com/datasette/datasette-llm/releases/tag/0.1a11) · [作者的运行说明](https://simonwillison.net/2026/Nov/14/datasette-llm/)（原文日期 11 月 14 日）。查阅：2026-11-16。

## 03｜火山方舟补上 Seedance 2.5 Flash API 档位

火山方舟 **11 月 15 日**更新系列教程，将 `doubao-seedance-2-5-flash-261115` 列入视频生成 API。Flash 支持 **480p 和 720p（8 bit）**，不提供同系列旗舰档的 1080p。这里的新进展是 **Flash 档的 API**，不是 Seedance 2.5 系列首次发布，后者的官方介绍发表于 7 月 31 日。

**我们的观察：** 做短片的团队可以用相同提示和参考素材，比较两个档位产出一条可用样片需要重试多少次。低分辨率能否满足分镜和预览，与最终交付是否用同一个档位，是两个问题。

来源：[火山方舟系列教程](https://docs.volcengine.com/docs/ark/seedance-2-5?lang=zh)（11 月 15 日更新） · [字节跳动 Seed 首发说明](https://seed.bytedance.com/en/blog/one-take-creation-flexible-referencing-introducing-seedance-2-5)（7 月 31 日）。查阅：2026-11-16。

## 补看｜Haru-4B 的 65 毫秒测了什么？

higurashi **11 月 13 日**介绍了能同时听和说的研究用语音基础模型 Haru-4B，并公开权重与推理代码。作者报告约 **65 毫秒的中位延迟**：M1 Max、int8、优化过的原生 Swift/Metal 实现、有线麦克风和耳机；数字**不含音频分帧**，后者还可能增加最多约 **133 毫秒**。这不是从人说完一句话到助手答完的时间，也不是浏览器、蓝牙设备或云端服务的实测。做语音原型时，建议另外测量用户开始插话到系统停止播放的时间，并确认参考人声的使用授权。

来源：[作者技术说明](https://blog.higurashi.ai/melte/haru4b/)（11 月 13 日） · [模型卡](https://huggingface.co/higurashi-ai/Haru-4B)（权重 Apache-2.0） · [推理代码](https://github.com/higurashi-ai/Haru)。查阅：2026-11-16。

## 每日 GitHub AI 热点

**采样：2026-11-16 08:59（Asia/Shanghai，UTC+8）。** 两列星数取自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily) 读取；「今日新增」沿用页面 `stars today`，不是精确滚动 24 小时。下表为选出的五个项目，按本次新增重排。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [backnotprop/plannotator](https://github.com/backnotprop/plannotator) | 866 | 11,839 | 用批注把编码助手的计划和差异交给人审。Apache-2.0，已有 0.28 系列；内容默认留在本地，使用分享功能前要核对链接可见性。 |
| [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | 717 | 56,931 | 为编码助手接入 Chrome 调试、性能记录等能力。Apache-2.0，已有 1.13 系列；适合前端测试，建议使用隔离的浏览器配置，避免暴露个人会话。 |
| [alibaba/open-code-review](https://github.com/alibaba/open-code-review) | 691 | 52,100 | 用规则和模型辅助审查代码差异。Apache-2.0，已有 1.13 系列；企业接入前核对所配模型会收到哪些源码，审查意见仍要人工复核。 |
| [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills) | 247 | 9,299 | 给支持 Agent Skills 的编码助手接入 Stitch 设计流程。仓库 Apache-2.0，已有 1.0 标签；技能代码和外部服务不是同一许可，发送内部设计素材前先核对服务条款。 |
| [NVIDIA/SkillEvaluator](https://github.com/NVIDIA/SkillEvaluator) | 122 | 1,684 | 从技能本身的检查延伸到实际任务评测。Apache-2.0，仍在 0.x；评测结果应注明模型、任务和工具权限，不宜直接套到别的配置。 |

项目原始资料为上表所链 GitHub 仓库的 README、许可证与发布记录，查阅于 2026-11-16。星数反映关注度，不代表质量或安全；本期未实装测试。

### 版本动态

- **Claude-Mem v15.18.5**（11 月 16 日 02:10，北京时间）修复了部分网络下 Tree-sitter CLI 下载失败、并行任务重复删除损坏 Chroma 集合，以及某些命令读取 `--help` 时误进运行流程的问题。后者见[对应修复](https://github.com/thedotmack/claude-mem/pull/6142)。[Mem0/Supermemory 导入](https://github.com/thedotmack/claude-mem/commit/5cc2fb16aa6d36b47d777d58dfd6456065510f9f)随后进入主分支，**不在这个发布标签里**。来源：[v15.18.5 发布记录](https://github.com/thedotmack/claude-mem/releases/tag/v15.18.5)。
- **Open Code Review v1.13.10**（11 月 15 日 10:56，北京时间）为 workspace 模式加入 `--resume`；只指定 `--from` 时，比较终点默认取 HEAD。接进流水线时，先核对默认比较范围是否与原来的策略一致。来源：[v1.13.10 发布记录](https://github.com/alibaba/open-code-review/releases/tag/v1.13.10)。

## 接下来值得跟进

在更换端点或运行环境前，保留一组现有系统能完成的任务当基线。除了结果是否正确，分别记录数据去向、运行条件和重试次数，才知道变得更方便的是接入，还是交付本身。

上一期见[第 024 期：给助手接上工具后，怎么判断它有没有解决问题？](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-15.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
