# AI 前沿札记

> 更新日期：2026-11-27（Asia/Shanghai） · 第 036 期
>
> 信息范围：11 月 25 日至 27 日早间的发布与核验。查阅日期：2026-11-27。

## 本期关注：AI 交付的结果，怎么复查？

给编码助手的工作加一个可回看的审阅队列，下载图像模型用自己的素材复测，以及让视频的镜头绕一圈后回到同一位置。今天三条进展都提供了一种具体的复查办法：找回来源、重做一次，或者把输出放到同一视角下比较。

## 01｜Claude-Mem 17 把代理的交付放进审阅队列

11 月 26 日发布的 **17.0.0** 给 My Work 加入 Review Stack。代理可以批量提交工作卡片，卡片可附来源链接、原文摘录或图片；读者可以标记已阅、收藏、留待跟进，也可以撤销批量已阅操作。配对了 Cloud Sync 并启用共享 Work View 的设备可以同步这些状态；只想在本机使用，可以保留本地模式。新版另有可选的 macOS Messages 观察器，文档称它从选定会话中的新消息开始读取，本地 Messages 数据源需要完全磁盘访问权限。这是独立的功能，不是使用 Review Stack 的前提。

**17.0.2** 在北京时间 27 日 04:45 发布。截至 09:00 左右，npm 的 `latest` 与内置更新清单均指向 17.0.2。这个补丁修复了批准 Cloud Sync 后已运行的默认 worker 未载入同步设置的问题：安装过程最多等待后台任务五分钟，再重启一次；自行管理的 worker 不会由它重启。搜索提示词现在按全部查询词匹配，不再要求整句原样出现。

实用之处是能在同一处回看交付物和依据；「已阅」仍只表示队列状态。把这套流程用于团队工作时，值得先用不含敏感数据的几张卡片确认来源可打开、局部撤销符合预期，以及同步范围符合团队设置。本期核对的是发布记录与分发端点，未试跑升级。

来源：[17.0.0 发布记录](https://github.com/thedotmack/claude-mem/releases/tag/v17.0.0) · [Review Stack 文档](https://github.com/thedotmack/claude-mem/blob/v17.0.0/docs/public/usage/my-work.mdx#review-stack) · [17.0.2 修复](https://github.com/thedotmack/claude-mem/releases/tag/v17.0.2) · [npm 标签](https://registry.npmjs.org/-/package/claude-mem/dist-tags) · [内置更新清单](https://raw.githubusercontent.com/thedotmack/claude-mem/main/releases/promoted-release.json)。

## 02｜Ming-Image 开放新版权重，榜单和部署各有一道门槛

蚂蚁 inclusionAI 于 11 月 25 日上传 **Ming-Image-0.2-Design** 权重，26 日更新模型仓库。模型卡标注 **MIT**，扩散 Transformer 为 **60 亿参数**。官方推荐 2048×2048、12 步、BF16；发布方验证的配置是单张 **80 GiB** CUDA GPU。这个显存数字是已验证配置，不是对量化版或更小显存的下限测量。

查阅时，Artificial Analysis 的 AA-Image-T2I v2.1 盲选榜把该模型列在开放权重组第 **1**、全榜第 **24**，Elo **1,095 ± 12**。榜单衡量的是成对盲选偏好。如果实际任务是做海报或界面稿，更应单测文字拼写、尺寸变化后的排版，以及设计稿是否容易继续编辑。模型卡区分了这次发布的 0.2 Design 和 0.1 的 Design-Layer，不宜把旧分层版的能力直接算在新权重上。

来源：[Ming-Image 0.2 模型卡（固定修订）](https://huggingface.co/inclusionAI/Ming-Image-0.2-Design/tree/3538f4c311cf8f5256d631ddb7f47793f741aa2e) · [Hugging Face 元数据](https://huggingface.co/api/models/inclusionAI/Ming-Image-0.2-Design) · [开放权重榜](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) · [全榜与评测方法](https://artificialanalysis.ai/image/leaderboard/text-to-image/)。

## 03｜SPARC 让镜头绕一圈，专门检查模型有没有忘掉场景

11 月 25 日的 SPARC 预印本用几何关系给视频生成模型选择历史信息：已经见过的区域优先查对应历史位置和局部邻域，没见过的区域则保留更完整的注意力。作者用 WorldScore-Static 的 **50 个场景**做闭环镜头测试，回到同一视角时比较重访画面。第二次重访中，结构相似度 SSIM 为 **0.5479**，同一方法的四步蒸馏版为 **0.5331**；感知距离 LPIPS 为 **0.1992**，对照为 **0.2256**，越低越好。这里比较的是作者指定的静态场景与对照，不是任意长视频的效果。

表 4 的消融实验也分开了两笔账：在单张 96 GB GPU、每段 72 帧且约 99% 目标 token 已被观察的设置下，保持 50 步和引导不变，带局部窗口的剪枝版本让每段扩散主干推理快约 **2.2 倍**；缩到四步并取消无分类器引导后才达到 **53.6 倍**。后一数字不含端到端的外部几何估计。项目仓库查阅时只有说明和素材，未见可运行的推理代码或模型权重；值得等有代码后用遮挡更多的路径测整条流水线。

来源：[SPARC 预印本，第 4 节与表 1、4](https://arxiv.org/html/2611.34064v1#S4) · [作者项目仓库](https://github.com/zju3dv/SPARC)。

## 补看｜阿里云百炼提供专门的分类、判断和评分接口

11 月 25 日上线的 `decision-model` 接受文本和图像，返回分类、是非或评分相关结果及置信信息；规格页不支持函数调用、网页搜索或通用的 Structured Outputs 功能。对工单分流一类任务，先用带标签的历史样本检查误判，尤其看高置信错误和需要人工兜底的类别；接口返回的分数不能未经校准直接当成命中概率。

来源：[百炼上线记录](https://help.aliyun.com/en/model-studio/newly-released-models) · [模型能力和限制](https://help.aliyun.com/en/model-studio/decision-model-stable)。

## 每日 GitHub AI 热点

2026-11-27 约 09:00（北京时间）查阅 [GitHub Trending 日榜](https://github.com/trending?since=daily)。下列累计 star 与「stars today」取自同一页面，不把日榜数字解释为精确滚动 24 小时。仅核对仓库、版本和许可，未安装试跑。

| 项目 | 累计 star | 日榜增星 | 用途、许可与注意点 |
| --- | ---: | ---: | --- |
| [MiniMind](https://github.com/jingyaogong/minimind) | 71,644 | +702 | Apache-2.0 教学仓库，有 v2 发布。11 月 26 日主分支合入[学习率预热修正](https://github.com/jingyaogong/minimind/pull/960)，避免启用 warmup 时跳过第一档；默认不启用预热的路径不变。复现实验应固定提交，数据及权重另核许可。 |
| [AutoClip](https://github.com/zhouxiaoka/autoclip) | 16,936 | +774 | MIT，处理访谈、播客等长视频。当前稳定版 1.6.1，11 月 26 日的 [1.6.2](https://github.com/zhouxiaoka/autoclip/releases/tag/v1.6.2) 是预发布，新增三款短片模板。桌面包未签名或公证；选择云端转写时音频需要上传。 |
| [Cursor Plugins](https://github.com/cursor/plugins) | 19,790 | +270 | README 标注 MIT，收录 Cursor 插件规范与第一方、第三方插件示例，未见 GitHub 独立发布。适合对照插件目录和声明；安装前还要逐项检查外部服务、许可和权限。 |
| [Stitch Skills](https://github.com/google-labs-code/stitch-skills) | 13,293 | +309 | Apache-2.0，有 v1.0 发布。界面设计、页面生成及导出技能依赖 Stitch MCP；仓库可读不等于远程服务无需账号。适合先用样例项目验证生成文件是否符合已有设计约束。 |

## 还需要观察

Review Stack 的实际价值取决于来源能否长期追溯。Ming-Image 值得在真实中英混排与长文案上做对照；SPARC 则等公开推理材料后检验重访和几何估计是否仍能同时兼顾。三条都已有明确的检查对象，但不能用队列状态、榜单名次或单段加速倍数替代实际验收。
