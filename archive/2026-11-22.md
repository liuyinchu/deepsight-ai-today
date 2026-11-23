# AI 前沿札记

> 更新日期：2026-11-22（Asia/Shanghai） · 第 031 期
>
> 信息范围：11 月 20 日至 22 日早间的模型、开发工具和研究进展；GitHub 日榜采样于 11 月 22 日。查阅日期：2026-11-22。

## 本期关注：AI 给出结果后，怎样检查它真的做了对的事？

黎曼猜想的形式化核验把两个问题分开了：证明能不能通过检查，以及它证明的是不是原来的命题。类似的区分也适用于工程：多模态模型有音频输入，不代表每种视听任务都更准；编程代理能调用工具，也不代表工具都走同一套审批流程。

## 01｜黎曼猜想通过 Lean 核验，还用约数和做了另一种表述的检查

**11 月 21 日**，Lean FRO 与 Mathlib Initiative 确认，Levent Alpöge 在 Claude 协助下公开的形式化证明通过了核验。两套比对工具确认它证明的正是 Mathlib 的经典黎曼猜想，Lean 官方内核及多个独立检查器接受了导出的证明。与仅确认代码能编译相比，这一步还检查了证明目标有没有被换掉。

Andrew Sutherland 的核验记录还列出狄利克雷 L 函数的结果，范围不包括一般的自守 L 函数。团队把 Alpöge 的证明与另行形式化的等价判据结合，检查了两条约数和不等式。例如，Lagarias 判据对每个正整数 n 只涉及它的约数和 σ(n) 与调和数 Hₙ：σ(n) ≤ Hₙ + exp(Hₙ) log(Hₙ)。这条目标不提 ζ 函数，却仍由同一份证明和既有等价关系推出，不是第二份独立证明。

**我们的观察：** 遇到 AI 生成的证明，除了让检查器接受其推导，还应该把要证明的原命题单独写出并比对。形式化核验通过，也不代替接下来解释这份证明的关键想法。

来源：[Lean 社区核验声明](https://leanprover-community.github.io/blog/posts/riemann-hypothesis/)（11 月 21 日） · [Sutherland 的 MIT 核验记录](https://math.mit.edu/~drew/grh/) · [约数和命题比对日志](https://math.mit.edu/~drew/grh/arith-con-leche-compare.log) · [Lagarias 的等价判据](https://arxiv.org/abs/math/0008177)。

## 02｜腾讯 WeMM-Video 开放权重：多听一路音频，不一定每项都更准

腾讯微信视觉团队于 **11 月 20 日（北京时间）**发布 WeMM-Video 的 2B 和 35B-A3B 权重，**21 日**在旗舰模型卡补充音频与音乐评测。模型接收采样视频帧和可选的原始音轨，以中英文输出文本，用于视听理解而非视频生成。2B 权重标注 Apache-2.0；其 BF16 文件合计约 6.12 GiB，不是实际推理显存。旗舰版是约 35B 总参数、约 3B 激活的混合专家模型，不能把它与 2B 稠密版混为一谈。

**已知边界：** 同一张表里，作者报告旗舰模型从 Instruct 切换到 Thinking 后，带音频的 Video-MME 从 **84.2 升至 86.4**，但 JointAVBench 从 **67.4 降至 65.9**。本期未复跑该结果。

**我们的观察：** 对课程录像、访谈或带背景声的素材，用同一组视频分别测「有音轨／无音轨」与「是否启用 Thinking」，再看在哪类题上改善。总分高一点，并不能回答音频是否帮模型理解了某个具体情节。

来源：[腾讯项目与使用说明](https://github.com/Tencent/WeMM-Video) · [2B 权重及许可](https://huggingface.co/tencent/WeMM-Video-2B) · [35B-A3B 模型卡](https://huggingface.co/tencent/WeMM-Video-35BA3B) · [预印本评测表](https://arxiv.org/html/2611.26409v1#S5)。

## 03｜OpenAI Agents SDK 预览版内嵌 Codex，审批仍需分两条路径测

OpenAI 在 **11 月 20 日 UTC（北京时间 21 日）**发布 Python Agents SDK v1.0.0-alpha.1。预览 wheel 仅在 GitHub Releases 提供，嵌入 Codex Core，需显式选用；工作流仍由 Python Runner 管理。SDK 的 Python 工具需要批准时可暂停和恢复；如果是 Codex 原生工具提出批准，这个预览版不会把它变成可恢复的审批，而会让该次模型调用失败。旧预览的会话状态与新 Runner 的恢复状态也不可直接互换。

**我们的观察：** 用这版试验时，分别验证 Python 工具「审批后恢复」和 Codex 原生工具「请求审批后调用失败」，再考虑迁移。官方 CI 通过四种平台的 wheel 构建及安装检查；发布记录给出的真实模型调用示例是 Linux x86-64，不能推出其他平台都跑过端到端用例。

来源：[GitHub 预览发行版](https://github.com/openai/openai-agents-python/releases/tag/v1.0.0-alpha.1) · [对应版本的执行及审批说明](https://github.com/openai/openai-agents-python/blob/87e8f533c3b457fe632a5eb38ac42590d7f4d9fc/docs/models/codex.md#configure-execution) · [官方 wheel CI](https://github.com/openai/openai-agents-python/actions/runs/44792830563)。

## 补看｜同一模型加代理适配器，收益取决于工作流

Hugging Face **11 月 20 日**公布实验：在同一冻结的 Qwen3.8-27B 基座上，把多套代理工作流训练的 LoRA 合成一个秩为 96 的适配器。在各工作流自己的 SWE-bench Pro 难题子集上，OpenCode **47.1%→66.7%**，Pi 保持 **74.0%**；换到另一个工作流的 300 项 AutomationBench，成功率只从 **23.0% 到 23.7%**。这不是对代理框架的横向排名，也没有显示普遍可迁移的增益。

**我们的观察：** 比较模型或适配器时，尽量固定任务、工具权限和运行设置，再判断收益是否来自适配器。作者报告的工具调用缩减只针对关闭 Thinking 时两组都解出的任务，不能据此推算整体成本。

来源：[Hugging Face：适配器合并结果与独立工作流测试](https://huggingface.co/blog/harness-adapters-at-scale#adapter-merging-evaluation-results)（11 月 20 日）。

## 每日 GitHub AI 热点

采样：2026-11-22 约 09:03（Asia/Shanghai），[GitHub Trending 日榜](https://github.com/trending?since=daily)。原页的「stars today」不是精确滚动 24 小时。本期只列与本文主题相近、有明确用途的项目；星数不是质量评分，未安装或运行这些仓库。

| 项目 | 累计 star | 日榜增星 | 用途、许可与注意点 |
| --- | ---: | ---: | --- |
| [OpenSRE](https://github.com/Tracer-Cloud/opensre) | 13,989 | +450 | 可自托管的 CI/CD 故障分析代理，Apache-2.0，有 0.1 系列发行版。适合先在测试仓库核对日志的数据去向、模型权限和建议质量。 |
| [App Store Compliance](https://github.com/mjmirza/app-store-compliance) | 3,401 | +720 | 汇集应用商店规则、检查清单和辅助脚本，适合上架前做初筛，不能代替 Apple／Google 的现行规则和实际审核。仓库现用带商业条件的 OpenRoots Agent License 2.3，不是 MIT。 |
| [Cursor Plugins](https://github.com/cursor/plugins) | 18,110 | +260 | 官方插件规范与示例库，仓库 README 标注 MIT。适合作为清单和分发结构参考；实际接入前逐个核对插件所请求的权限及外部服务条款。 |

## 还需要观察

下一步值得追踪三件事：数论研究者能否把形式证明提炼成可读的核心论证，WeMM-Video 的音轨收益能否在实际素材中复现，以及 Agents SDK 正式版会怎样处理来自 Codex 原生工具的审批。
