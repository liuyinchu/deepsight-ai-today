# AI 前沿札记

> 更新日期：2026-11-24（Asia/Shanghai） · 第 033 期
>
> 信息范围：11 月 23 日至 24 日早间的发布与核验。查阅日期：2026-11-24。

## 本期关注：代理给出判断后，谁负责放行？

代码审阅里的「发现问题」「修复问题」和「批准合并」是三件事。Anthropic 这次把它们拆开，适合用来检查团队的分工。文档检索也有类似的边界：模型可以帮忙把相关页排到前面，但这并不证明最终答案正确。

## 01｜Claude Ship 把 PR 审阅、修复和批准分开

Anthropic 11 月 23 日宣布，Claude Ship 面向使用 GitHub 的 Claude Team 和 Enterprise。原有的 Code Review 已正式可用，新增的 PR Steward 和 Claude Approvals 仍是测试版。Steward 会处理失败的 CI、合并冲突与审阅意见，推送修订后说明改了什么、如何撤销；它不负责批准或合并。Approvals 根据团队放在仓库里的规则决定哪些常规改动可以自动批准，哪些仍需人签字。批准不是自动合并，现有 GitHub 分支规则仍然适用。

团队最值得先看的不是省了多少评论，而是测试版会不会错误放行原本该由人处理的改动。可先在单个仓库用观察模式对照人工决定，不增加合并权限。观察模式也不是零成本：政策若调用 Code Review，仍会产生用量费用。

来源：[Anthropic：Claude Ship 发布与适用范围](https://claude.com/resources/articles/introducing-claude-ship)（11 月 23 日） · [Claude Ship：工作模式与计费说明](https://claude.com/product/claude-ship)。

## 02｜Illuin 发布 IVE-4B：让相关页面排在更前面

Illuin 11 月 23 日发布 IVE-4B，以及由它蒸馏得到的 Ivonnet-0.8B。它们可以接收文档页图和问题，把一次推理用来为预先给定的选项打分，而不是生成一段解释。在文档搜索里，团队让模型判断初筛的 50 页分别是否相关，再按「是」的概率排序。模型权重和项目的推理服务器采用 Apache-2.0。

在团队公布的 ViDoRe V3 八个公开领域、七种初筛方法的平均结果中，前十项排序分数 nDCG@10（按 100 分表示）从 **62.7 升至 66.7**；表中专门用于重排的 Nemotron-3.5-Rerank-8B 为 **68.0**。这是作者在英语查询上的评测，不是独立复测，不能拿来推断中文扫描件或最终问答的准确率。

值得先测的是：正确页面原本就进了候选集，却排序靠后，还是一开始就没被找到？只有前一种情况是这条重排链路直接能改善的。试用时把前十页命中率、最终答案准确率和重排增加的延迟分开记录。

来源：[Illuin 发布与评测表](https://huggingface.co/blog/QuentinJG/ive-release)（11 月 23 日） · [IVE-4B 模型卡](https://huggingface.co/illuin/IVE-4B) · [Ivonnet-0.8B 模型卡](https://huggingface.co/illuin/Ivonnet-0.8B)。

## 03｜Claude Opus 4.5 的 API 停用日定在 2027 年 1 月 22 日

Anthropic 在 11 月 23 日宣布弃用 Opus 4.5：带日期的 `claude-opus-4-5-20251101` 和短别名 `claude-opus-4-5` 都在公告内，Claude API 的停用日期是 **2027 年 1 月 22 日**，官方推荐迁到 `claude-opus-5-5`。弃用公告不等于当日已经下线，这个日期也不替第三方托管平台规定期限。同一张表上，Sonnet 4.5 的 Claude API 停用日更近，已排定为 **11 月 30 日**。

迁移前不要只改主服务里的模型名。离线批次、旧评测和故障回退可能还指向旧版本。先用真实任务回归工具调用与结构化输出，再看延迟和用量；不同模型即便都能回答同一个提示，也未必以相同方式触发下游动作。

来源：[Anthropic 平台发布记录](https://platform.claude.com/docs/en/release-notes/overview)（11 月 23 日） · [官方模型停用与替代清单](https://platform.claude.com/docs/en/about-claude/model-deprecations)。

## 补看｜Lean 核验续篇：同一份导出，单线程检查从约 48 小时降至约 9 小时

MIT 的 Andrew Sutherland 在 11 月 23 日 23:45 UTC 更新记录。上期核对的是离线重建能否产出逐字节相同的导出；新增的是检查过程的对照：同一份导出、同一版 Lean v4.36.0 官方内核，调低内存分配器定期全堆回收的频率后，单线程检查由 **48 小时 03 分**降至 **9 小时 08 分**，两次均成功，峰值内存仍约 **82.6 GiB**。这是本次核验的运行数据，不是 Lean 普遍快了五倍，更不是减少了检查项。

审计另记载，独立的 Comparator v4.33.0 已通过；采用规范导出的 v4.36.0 `lake comparator` 路径仍在运行。前者复用了原构建产物并另行编译挑战文件，不能代替后者的完成结果。这几项都是对既有证明的核验，不是新的证明。

来源：[MIT 核验表](https://math.mit.edu/~drew/grh/) · [未调优内核日志](https://math.mit.edu/~drew/grh/lean-default-kernel.log) · [调优后内核日志](https://math.mit.edu/~drew/grh/lean-default-kernel-tuned.log)。

## 补看｜Claude-Mem 把同步和模型服务拆开设置

北京时间 11 月 23 日发布的 v16.1.0 新增了安装器里的独立 Cloud Sync 选项。用户可以保留已有的记忆提供方，只在需要时购买跨设备同步，不必另买托管模型额度。变化在于安装入口和服务可分开选择，不是首次推出同步，也不意味着所有本地历史都会迁移。仅用本地记忆的用户不必启用。

来源：[Claude-Mem v16.1.0](https://github.com/thedotmack/claude-mem/releases/tag/v16.1.0) · [Cloud Sync 官方说明](https://cmem.ai/cloud-sync)。

## 每日 GitHub AI 热点

2026-11-24 约 09:02（北京时间）查阅 [GitHub Trending 日榜](https://github.com/trending)。使用同一份榜单页面快照的累计星数和 “stars today”；后者是榜单原口径，不是精确滚动 24 小时。以下只核对仓库与发布记录，没有安装或运行。

| 项目 | 累计 star | 日榜增星 | 用途、许可与注意点 |
| --- | ---: | ---: | --- |
| [last30days](https://github.com/mvanhorn/last30days-skill) | 73,444 | +797 | 有 v4.6.0 发布的多来源研究技能，MIT。适合先找讨论与线索；不同平台依赖不同接入条件。帖子热度不能替代原文核实，也不能视为覆盖了整个平台。 |
| [Diagram Design](https://github.com/cathrynlavery/diagram-design) | 70,971 | +620 | MIT 的编码助手技能，生成独立的 HTML/SVG 图表文件。适合整理技术文档中的流程和结构图。成图仍要核对箭头关系与数值；涉及内部资料时先看项目的隐私说明。 |
| [OpenSRE](https://github.com/Tracer-Cloud/opensre) | 14,839 | +436 | Apache-2.0 的自托管 CI/CD 排障助手，目前为公开 Alpha。可读取失败运行并提出供审阅的修复；接入真实仓库前先限定日志访问和写入权限，不能拿诊断替代验证。 |
| [OpenVINO](https://github.com/openvinotoolkit/openvino) | 12,285 | +184 | Apache-2.0 的模型推理工具包，已有 2026.4.1 正式发布。适合验证 CPU、Intel GPU 或 NPU 上的部署；设备、算子与模型权重兼容性和许可要分别核对。 |

## 还需要观察

对 PR 审阅，先记录观察模式与人工批准的分歧。对文档检索，下一步是用中文扫描件和跨页问题验证重排收益；对模型迁移，优先排查即将停用的 Sonnet 4.5，再用生产样本验证 Opus 的替代配置。
