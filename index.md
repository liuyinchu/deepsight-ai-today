# AI 前沿札记

> 更新日期：2026-11-25（Asia/Shanghai） · 第 034 期
>
> 信息范围：11 月 23 日至 25 日早间的发布与核验。查阅日期：2026-11-25。

## 本期关注：什么被验证了，什么还没有？

一份 Lean 证明通过检查，说明的是写进形式系统的命题；一组翻译分数变好，说明的是指定样本和评判规则下的表现。今天两项值得关注的进展都给了可查的材料，但从证据到结论，中间不能跳步。

## 01｜VP 与 VNP：两份公开材料，先读各自证明的范围

北京时间 **11 月 24 日 05:43**，Anthropic 将关于 VP 与 VNP 分离的论文和 Lean 工程合入公开仓库。对应提交的持续集成记录显示，`lake build` 与 Palomar Comparator 对工作流声明的各项配置均成功；其中用到了另一套实现的检验内核 nanoda。这是作者公开工作流里的交叉检查，不等于第三方已独立重建或核对了整篇论文。仓库说明列出了正文与 Lean 陈述的对应关系，细节可从附录 Q 查起。

OpenAI 的项目记录也将公开日期标为 **11 月 23 日**。其 Lean 说明明确把形式化结论限定为任意域上、精确计算的非一致算术电路；论文另述的更强的边界电路复杂度下界，不应一并算作这份 Lean 工程已核验的结论。两份材料研究的是多项式的算术电路复杂度；**这不是布尔计算里的 P vs NP**，也不把两份公开材料当作彼此的独立审计。

为什么值得读：代码与通过记录让人能检查结论是怎么表述的。比起只看「通过」标记，更有用的做法是打开形式定理，对照论文里「精确计算」「近似」分别对应哪些定义与假设。

来源：[Anthropic 合并记录](https://github.com/anthropics/formal-math/pull/51) · [Lean 工程与正文对应说明](https://github.com/anthropics/formal-math/blob/11b6c7e3c927df2658f513b6b68ea7f8929c62c5/vp-vnp/README.md) · [实际核验作业](https://github.com/anthropics/formal-math/actions/runs/45272623486/job/138036167794) · [OpenAI 发布记录](https://github.com/openai/math/blob/main/history.md) · [OpenAI 的 Lean 定理范围](https://github.com/openai/math/blob/main/lean/docs/379.md)。

## 02｜腾讯开放 Hy-MT3 权重：先看译文漏没漏

腾讯 11 月 24 日公开 Hy-MT3 的 **1.8B、7B 和 30B-A3B** 三组权重，三份主模型仓库均标注 Apache-2.0。官方技术报告 v2 发布于 11 月 23 日。报告第 3.5 节在 5,000 条 WildMTBench-202611 样本上对比两代同尺寸 30B-A3B；由两套自动评判模型评估，出现至少一处遗漏或增译的样本比例从 **21.2% 降至 5.5%**。这是作者在不提供参考译文时的评测，不是独立复测，也不能移用到 1.8B、7B 或任意语言对。

译技术文档时，可以先拿一段有单位、链接、代码占位符和重复术语的真实样本做并排比较。把漏译和格式损坏单列，别让译文的流畅度掩盖这些更难察觉的问题。也不要把报告中的文字翻译指标外推到图像翻译；后者包含单独的文字识别环节。

来源：[腾讯发布仓库及各权重入口](https://github.com/Tencent-Hunyuan/Hy-MT3)（11 月 24 日） · [技术报告 §3.5、表 5](https://arxiv.org/html/2611.27673v2#S3.SS5)（11 月 23 日）。

## 03｜Agents SDK 0.26.0：升级前检查本地图片和 Docker 挂载

OpenAI 11 月 24 日发布 Python Agents SDK **0.26.0**。实验性 Codex 工具读取受信任的本地图片时，需要由宿主应用预先批准对应的图片引用；创建和恢复会话时均会检查。Docker 也不再接受旧式容器内存储挂载，即使是只读挂载；受影响的应用需要改用支持的卷或由应用管理的受信任挂载，并按新配置启动新会话。

还有一个容易误读的行为：输入护栏拒绝请求时，新版会停止尚在排队的工具；如果业务要求校验结束前**任何**工具都不能启动，还需要关闭该护栏的并行运行。升级测试应分开验证新开与恢复会话，以及拒绝时是否已经触发外部动作，不能只看运行末尾有没有返回错误。

来源：[OpenAI Agents SDK v0.26.0 发布及迁移说明](https://github.com/openai/openai-agents-python/releases/tag/v0.26.0)（11 月 24 日）。

## 补看｜Claude-Mem 16.1.3 修补 OpenRouter 队列丢失

北京时间 **11 月 25 日 02:32**发布的 v16.1.3 修复了 OpenRouter 后台工作进程的一条错误路径：有时 HTTP 状态是 200，但响应正文里有临时错误。旧版可能把它当成不可恢复错误，结束会话并丢弃排队中的观察记录。新版识别正文里的数值错误码，把 429 和 5xx 交给相应的限流或临时故障处理；已识别为模型运行后产生的输出错误仍不重发，避免重复计费。

影响范围是 OpenRouter 工作进程，不等于所有服务端错误路径都改了，也不会恢复以前丢失的记录。使用这一提供方的部署，可在升级后对照队列与会话日志，确认临时失败后记录仍能处理。

来源：[Claude-Mem v16.1.3 发布说明](https://github.com/thedotmack/claude-mem/releases/tag/v16.1.3) · [合入的修复及测试说明](https://github.com/thedotmack/claude-mem/pull/6448)。

## 补看｜ERNIE 5.5 Flash Preview 可自部署，先算权重体积

百度在 11 月 24 日公开了此前先提供 API 的 ERNIE 5.5 Flash Preview 权重。Preview 权重采用 Apache-2.0，不能把同样条款套用到单独发布的 Base。发布者模型卡标注 **280B 总参数、每 token 激活 14B**；默认 FP8/FP4 混合权重仓库的文件体积约 **166 GB**，并不是下载一个 14B 密集模型的量级。这个数是存储体积，不是显存门槛。模型卡的约 105 万 token 上下文也和示例服务配置的 131,072 token 上限不同，部署时应按目标长度和并发实测。

来源：[百度早期 API 发布说明](https://ernie.baidu.com/release/ernie-5-5-flash-preview/) · [百度发布的模型卡](https://huggingface.co/baidu/ERNIE-5.5-Flash-Preview) · [Hugging Face 仓库数据](https://huggingface.co/api/models/baidu/ERNIE-5.5-Flash-Preview) · [Preview 权重许可证](https://huggingface.co/baidu/ERNIE-5.5-Flash-Preview/blob/main/LICENSE)。

## 补看｜Qwen4-Max 预览版今日退场，排查旧模型 ID

截至今早查阅，阿里云百炼的中文模型页、套餐更新记录和团队版权益页均将 **11 月 25 日 10:00（北京时间）**列为预览结束时间。旧版文档曾列到当日午夜，不宜据此假定预览接口仍可调用；本文未实测接口。正式版 `qwen4-max` 已列入 Token Plan 个人版和团队版。若脚本仍指向 `qwen4-max-preview`，按较早时刻准备切换；尤其要回归带思考的长输出，确认推理和最终文本共用预算时，正文仍能完整返回。

来源：[阿里云百炼中文模型页](https://help.aliyun.com/zh/model-studio/qwen4-max-preview) · [百炼中文套餐更新记录](https://help.aliyun.com/zh/model-studio/token-plan-product-updates) · [百炼团队版权益](https://help.aliyun.com/zh/model-studio/token-plan-team-overview) · [正式版 API 文档](https://help.aliyun.com/zh/model-studio/qwen4-max)。

## 每日 GitHub AI 热点

2026-11-25 约 09:01（北京时间）查阅 [GitHub Trending 日榜](https://github.com/trending)。累计星数和 “stars today” 均取自该页面同一份快照，后者不等于精确滚动 24 小时。这里核对了项目文档和仓库许可，未安装或试跑；星数不是质量或安全背书。

| 项目 | 累计 star | 日榜增星 | 用途、许可与注意点 |
| --- | ---: | ---: | --- |
| [CLI-Anything](https://github.com/HKUDS/CLI-Anything) | 64,786 | +1,074 | Apache-2.0。提供把已有软件包装成命令行接口的工具和社区目录，已有 v0.4.0。适合先验证重复性操作能否脚本化；仓库许可不覆盖被包装的第三方应用，生成命令的权限和输出仍要检查。 |
| [Superpowers](https://github.com/obra/superpowers) | 325,817 | +1,003 | MIT。面向编码助手的规划、测试和审阅流程，已有 v7.0.2，提供多平台接入说明。小改动可先比较流程开销与漏检率；「有审阅步骤」不是代码已审好的证据。 |
| [AutoClip](https://github.com/zhouxiaoka/autoclip) | 15,336 | +980 | MIT。面向播客、访谈等长视频的桌面剪辑工具，11 月 24 日发布 v1.6.0。字幕、取景和切点仍需检查；使用在线模型或外部素材时，另外确认数据传输和授权。 |
| [MiniMind](https://github.com/jingyaogong/minimind) | 70,199 | +727 | Apache-2.0 仓库，提供约 6,400 万参数的小模型及训练流程，有 v2 发布，适合教学与复现练习。报告的训练时间不适用于所有显卡；数据集和外部权重许可另查。 |
| [Stitch Skills](https://github.com/google-labs-code/stitch-skills) | 12,655 | +403 | Apache-2.0 的 Stitch 技能与插件集合，已有 v1.0，有主流编码助手的接入说明，依赖 Stitch MCP 服务。界面草稿不等同于可交付前端；接入真实设计资料前检查工作区与数据权限。 |

## 还需要观察

形式化工作下一步看外部复核能否逐条对齐论文声明与 Lean 定义。翻译部署先用自己的术语和格式约束做小规模复测。SDK 与模型迁移则用两条失败用例验收：恢复旧会话时是否出现意外授权，以及输出预算不足时能否检测到正文截断。
