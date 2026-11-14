# AI 前沿札记

> 更新日期：2026-11-14（Asia/Shanghai） · 第 023 期
>
> 信息范围：11 月 11 日至 11 月 13 日发布或更新的官方公告与项目记录；查阅日期：2026-11-14。公告日期沿用原文，GitHub 发布时刻另换算为北京时间。

## 本期关注：模型能做什么，部署时又需要什么

ChatGPT Wallet 开始让助手代为结账，但每次付款仍由用户确认。Inkling 2 Small 现在可以下载权重，但自托管所需的显存并不小。AWS Emulator 和 llama.cpp 1.0 则提供了两个更具体的工程检查点：在本地验证应用行为，升级前核对接口兼容性。

## 01｜ChatGPT Wallet 开始推出，每笔付款都要确认

OpenAI **11 月 13 日**宣布在 dots 中逐步推出 ChatGPT Wallet。首批面向**年满 18 岁、持符合条件的美国发行卡的美国 ChatGPT Pro 订阅者**。用户先查看商品、总价和选用的卡，再通过面容或指纹等安全验证确认付款。随后，支付网络提供用于付款的代用令牌；按 OpenAI 的说明，真实卡资料不交给助手或商家。

商家仍负责履约、退货和客服。OpenAI 也提醒，商家的风控系统可能拒绝合法的助手交易。公告提到按类别、限额或时间段提前授权是未来方向，**本次推出的流程仍是逐笔确认**。

**我们的观察：** 确认页能不能看清总价是一回事，付款后能不能顺利找到订单、处理取消或退货是另一回事。评估代购流程时，两段都值得检查。

来源：[OpenAI：Introducing ChatGPT Wallet](https://openai.com/index/introducing-chatgpt-wallet/)。发布：2026-11-13；查阅：2026-11-14。

## 02｜Inkling 2 Small 开放权重，自托管先算显存

Thinking Machines Lab 在 **11 月 12 日**更新公告，将此前通过 Tinker 提供的 Inkling 2 Small 权重开放下载，模型卡标注 **Apache-2.0**。这是总参数 **594B**、每次激活 **17B** 的混合专家模型，可输入图像和文本、输出文本，上下文上限为 **100 万 token**。

官方模型卡给出的自托管门槛是：主推的 NVFP4 检查点至少需要 **500 GB 集群总显存**，MXFP8 检查点至少 **800 GB**。这是项目方给出的硬件要求，本期未实测部署。模型初次公告是 11 月 5 日，本次新增的是权重下载。

**我们的观察：** 17B 激活参数有助于理解每次推理的计算量，不能拿来估算完整权重的显存。做选型时，可以先用现有 API 验证质量，再把硬件、并发量和运维放在一起比较。

来源：[Thinking Machines Lab：11 月 12 日公告更新](https://thinkingmachines.ai/news/inkling-2-small/) · [官方模型卡](https://thinkingmachines.ai/model-card/inkling-2-small/) · [权重仓库](https://huggingface.co/thinkingmachines/Inkling-2-Small)。原公告：2026-11-05；开放权重：2026-11-12；查阅：2026-11-14。

## 03｜AWS Emulator：让编码 Agent 先在本地验证云应用

AWS **11 月 11 日**正式推出 AWS Emulator，**12 日**在发布公告中补充 GitHub 源码链接。现有 AWS CLI 和 SDK 可以把调用发往本地端点，开发者或编码 Agent 因而能先在本地或 CI 检查应用行为。模拟器本身不额外收费，无需 AWS 账户。源码主体采用 **CC0-1.0**，仓库中 `vendor/` 目录为 **Apache-2.0**。

服务覆盖程度不一；不支持的操作可能返回 501。AWS 明确说明，模拟器不会按真实环境的方式评估 IAM 或资源策略，也不模拟跨账户、跨区域行为。

**我们的观察：** 适合先在本地跑读写与失败路径，把权限和跨区域行为留给隔离的云测试环境。两层测试各有用处，本地通过不等于可以直接部署。

来源：[AWS 发布说明](https://aws.amazon.com/blogs/aws/test-your-applications-against-aws-locally-with-aws-emulator/) · [AWS Emulator 源码](https://github.com/aws/aws-emulator)。发布：2026-11-11；公告更新：2026-11-12；查阅：2026-11-14。

## 产业快讯｜NVIDIA 启动 CEO 交接

NVIDIA 董事会 **11 月 12 日**宣布，黄仁勋辞去总裁、CEO 和董事职务，即时生效。Colette Kress 出任临时 CEO，并继续担任 CFO；Harvey C. Jones 出任董事会主席。董事会已启动永久 CEO 的遴选，并表示预计客户承诺和产品路线图继续执行。这是董事会当时的预期，本期未发现交接公告同时宣布产品路线图调整。

来源：[NVIDIA：CEO 交接公告](https://nvidianews.nvidia.com/news/nvidia-announces-ceo-transition)。发布：2026-11-12；查阅：2026-11-14。

## 每日 GitHub AI 热点

**采样：2026-11-14 09:03（Asia/Shanghai，UTC+8）。** 两列星数均取自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)读取，保留页面 `stars today` 口径。下表只列本期选取的四个项目，按页面所示当日增星排序，不是全站 AI 项目排名，也不是精确滚动 24 小时的数据。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | 256 | 71,602 | 将多市场行情和新闻整理成自选股看板与推送，适合研究信息整理流程。MIT；已提供 Web 工作台与容器部署文档。行情和模型服务的条款需另查，生成分析可能错引数据，不应据此直接交易。 |
| [alibaba/open-code-review](https://github.com/alibaba/open-code-review) | 215 | 50,626 | 结合规则与模型生成代码评审意见，适合希望在 CI 中做辅助审查的团队。Apache-2.0；已有 CLI、插件和 CI 集成。先检查模型服务会收到哪些代码，再以人工评审判断误报和漏报。 |
| [backnotprop/plannotator](https://github.com/backnotprop/plannotator) | 108 | 10,460 | 给编码 Agent 的计划、文档和差异添加批注并回传，适合需要审阅交付物的开发者。MIT 或 Apache-2.0 二选一；已有多种 Agent 集成与版本发布。共享批注前检查是否带出内部代码或文档。 |
| [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | 67 | 55,455 | 让编码 Agent 借助 MCP 或 CLI 检查 Chrome、录制性能轨迹与调试页面，适合前端开发者。Apache-2.0；正式支持 Google Chrome 和 Chrome for Testing。工具可读写相连浏览器的数据，建议使用隔离的测试配置。 |

README 与许可证（查阅：2026-11-14）：

- Daily Stock Analysis：[README](https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/README.md) · [MIT](https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/LICENSE)
- OpenCodeReview：[README](https://github.com/alibaba/open-code-review/blob/main/README.md) · [Apache-2.0](https://github.com/alibaba/open-code-review/blob/main/LICENSE)
- Plannotator：[README](https://github.com/backnotprop/plannotator/blob/main/README.md) · [MIT](https://github.com/backnotprop/plannotator/blob/main/LICENSE-MIT) · [Apache-2.0](https://github.com/backnotprop/plannotator/blob/main/LICENSE-APACHE)
- Chrome DevTools MCP：[README](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md) · [Apache-2.0](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/LICENSE)

以上落地判断依据公开文档，本期未安装实测。星数反映关注度，不能代替质量或安全评估；第三方服务、模型权重和依赖的许可应分别核对。

### 版本动态

- **llama.cpp / ggml 1.0.0**：llama.cpp 于 **11 月 13 日 18:20（北京时间）**发布 1.0.0。项目改用语义化版本，并在 1.0.0 移除已经弃用的接口。版本也新增 Kimi K3.1 Pro 支持与 FP8 KV 缓存。维护集成时，先核对旧接口，再用原有模型和固定输入做一次回归。来源：[发布记录](https://github.com/ggml-org/llama.cpp/releases/tag/v1.0.0) · [维护团队说明](https://huggingface.co/blog/ggml-org/llama-cpp-v1)。
- **Hugging Face PEFT v0.21.3**：发布于 **11 月 13 日 18:11（北京时间）**。对使用 DTensor 张量并行的模型，保存时只汇集适配器权重，避免 0.21.2 在该场景下汇集整个分片模型。值得在受影响的训练配置上对比检查点保存耗时。来源：[发布记录](https://github.com/huggingface/peft/releases/tag/v0.21.3)。

## 接下来值得跟进

如果正在为编码 Agent 接入测试环境，可以先选一个最常用的云 API，把本地回归和云端权限检查分开。已经集成 llama.cpp 的项目，则先检查迁移成本与当前配置的回归结果，再决定是否升级。

上一期见[第 022 期：把任务分给合适的模型，也把验收留下来](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-13.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
