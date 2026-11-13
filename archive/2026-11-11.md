# AI 前沿札记

> 更新日期：2026-11-11（Asia/Shanghai） · 第 021 期
>
> 信息范围：11 月 9 日至 11 月 11 日查得的官方公告、产品文档与项目状态；查阅日期：2026-11-11。下文发布日沿用来源标注，不把海外 11 月 10 日公告统一改写成北京时间 11 日发布。GitHub 日榜另列实时采样时间。

## 本期关注：更强的模型，怎样进入日常工作

过去两天，Google 和 Anthropic 都扩大了高能力模型的选择，产品侧也在补齐表格、白板、数据分析与商家服务。值得一起看的还有另一类进展：AI 已开始参与为真实软件生成可由机器检查的数学证明。

这期挑出六条变化。选型时，除了能力表现，也要看任务总成本、数据保留条件，以及功能究竟已经开放还是仍在预告。

## 01｜Gemini 4 Carbon 扩大开放，限时 API 价格到月底

Google **11 月 10 日**宣布，Gemini 4 Carbon 向付费开发者和企业开放 API，并向 Google AI Ultra 订阅者逐步推出 Gemini app、Antigravity 与 Search AI Mode 中的访问。Google 将它定位于复杂编码、长任务和开放式科研；Antigravity 同时预览 `/swarm`，让多名 Agent 分工并行处理任务。

公开版 Carbon 的 API 入门价为**每百万输入 token 2 美元、输出 10 美元**。优惠期于 **2026 年 11 月 30 日结束**，之后为 4／20 美元。专门的 Carbon for cyber 仍走 Fairwind Program 申请，开放范围不同。

**我们的观察：** 这给复杂项目增加了一个值得比较的选项。Google 也说明 Carbon 会比 Argon 使用更多 token，因此单价低不能直接换算为每项任务便宜。实际试用最好固定同一项跨文件改动，比较完成质量、重试次数和总账单。

来源：[Google：Gemini 4 Carbon is now available](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-4-carbon-now-available/)。发布：2026-11-10；查阅：2026-11-11。

## 02｜Claude Fable 5.2／Mythos 5.2：高难度任务升级，也有迁移条件

Anthropic **11 月 9 日**发布 Fable 5.2 和 Mythos 5.2。两者基于同一底层模型，Fable 带有面向网络安全和生物领域的防滥用措施并广泛开放；Mythos 面向通过相应验证计划的组织。

官方称输出速度较 5.1 最高提升 30%。标准 API 价格为**每百万输入／输出 token 8／40 美元**，可选 fast mode 为 16／80 美元。Fable 文档列出 **100 万 token 上下文、12.8 万 token 最大输出**。这些规格与厂商评测不等于我们的独立实测。

开发者尤其需要留意：自适应思考始终开启；强制指定工具的部分参数会报错；Fable 5.2 要求 **30 天数据保留**，除非 Anthropic 明确授权，否则不支持零数据保留。已有严格数据条款的应用应先核对这一条件。

**我们的观察：** 可以把它放进复杂调试、迁移或研究任务的候选池，并保留较小模型处理常规步骤。先用一组已有答案的任务做回归测试，通常比直接替换所有调用更容易判断收益。

来源：[Anthropic 发布公告](https://www.anthropic.com/claude-fable-and-mythos-5-2)、[Fable 5.2 官方规格与限制](https://platform.claude.com/docs/en/models/fable-5-2/overview)。发布：2026-11-09；文档查阅：2026-11-11。

## 03｜Claude 加入协作表格、白板和数据功能，会议助手仍是预告

Anthropic **11 月 10 日**推出新的工作产物类型：Sheets 可与团队共同编辑多标签表格和公式，Whiteboards 用于整理想法，Data 连接业务数据生成仪表盘和数据故事。互动连接器也允许合作伙伴把应用界面嵌入 Claude。

当前开放条件需要分开看：

- **Sheets、Whiteboards 处于 beta**，面向 Pro、Max、Ultra、Team、Enterprise；Team 默认开启，Enterprise 需要管理员开启。
- Data 已列入付费套餐的可用模板；Enterprise 计划于 **11 月 19 日**默认开启，已由管理员保存的设置优先。
- **Claude Meetings 仍标注 coming soon**。公告预告它可以加入通话、记笔记并修改团队文档，没有给出明确上线日。

**我们的观察：** 这些功能减少了从聊天结果搬运到办公文件的步骤。团队试用时，适合重点检查多人同时编辑、公式变化和数据刷新后的结果，尤其要确认共享内容与原始数据的访问权限是否符合预期。

来源：[Claude 综合发布公告](https://claude.com/de/resources/articles/more-ways-to-work-with-claude-sheets-whiteboards-and-interactive-connectors)、[官方 Artifacts 帮助文档](https://support.claude.com/en/articles/17153992-what-are-artifacts-and-how-do-i-use-them)。公告发布：2026-11-10；开放范围查阅：2026-11-11。公告链接为可读取的德语路径，正文为英文。

## 04｜ChatGPT 商家入口进入 beta，可管理资料与查看聚合数据

OpenAI **11 月 10 日**推出 Businesses in ChatGPT beta。符合条件、位于 ChatGPT 可用地区的企业可以认领商家资料，维护信息，并提供含价格和库存的商品数据。

Business Hub 展示提及量、网站点击量及发现主题等**聚合洞察**。OpenAI 表示，达到一定交互量门槛后才提供相关数据，不向商家分享用户的 ChatGPT 对话或 Business Agent 聊天。

Business Agents 首先开放给**美国符合条件的电商和线上服务企业**。商家代理对话会有明确标识，与用户原始 ChatGPT 对话及独立回答分开；不能把商家资料认领的地域范围套用到代理功能上。

**我们的观察：** 对小型产品和商家，近期最直接的工作是保持资料、价格与库存准确，再观察访问是否真正带来有效咨询。商家可控制的数据输入与 ChatGPT 的独立回答仍需分别理解。

来源：[OpenAI：Introducing Businesses in ChatGPT](https://openai.com/index/introducing-businesses-chatgpt/)。发布：2026-11-10；查阅：2026-11-11。

## 05｜AlphaProof Code：为 Signal 核心密码代码生成机器可检验的证明

Google DeepMind **11 月 10 日**公布 AlphaProof Code。研究者先与模型共同写出安全目标的形式化规格并逐一人工审查，再让由 Gemini Carbon 驱动的 Agent 生成 Lean 证明，由 Lean 检查。

团队报告，用四周时间为 Signal 核心密码实现的若干关键安全性质完成验证，涉及编译后代码，而不只停留在协议设计层。研究还重新放入两个已修复漏洞，系统对这两个有问题的版本生成了不安全性的机器检查证明。这些结果来自研究团队的报告。

**我们的观察：** 最值得借鉴的是验收方式：把部分工作交给可重复运行的检查器，让人工精力集中在规格是否正确、证明覆盖了什么。结论的范围是被指定和验证的安全性质，不能扩大为整个 Signal 应用没有漏洞。更广泛的软件适用性仍需后续研究。

来源：[Google DeepMind：Introducing AlphaProof Code](https://deepmind.google/blog/introducing-alphaproof-code/)。发布：2026-11-10；查阅：2026-11-11。

## 06｜产业观察：Anthropic 启动 IPO 路演，尚未完成上市

Anthropic **11 月 10 日**宣布启动拟议 IPO 路演，计划发行及由现有股东出售合计 **8.25 亿股** Class A 股票，预期每股 **70 至 78 美元**，已申请在纳斯达克以 **ANT** 为代码上市。

同日 SEC 修订招股文件还披露，与 AMD、Broadcom、Google、Micron、Microsoft、NVIDIA 和 TSMC Partners 的并行私募合计 **260 亿美元**，以 IPO 完成及其他条件满足为前提。公告明确，注册声明**尚未生效**，不能把报价区间或融资安排写成已成交结果。

**我们的观察：** 这让前沿模型公司与芯片、云计算供应商之间的资本联系更清晰。接下来应跟踪最终定价、实际募集金额与资金使用，而不是只看估值标题。本节为产业信息，不构成投资建议。

来源：[Anthropic 路演公告](https://www.anthropic.com/news/roadshow-launch)、[SEC：2026-11-10 S-1/A](https://www.sec.gov/Archives/edgar/data/2130357/000162828026072484/anthropicpbc-sx1ax2.htm)。公告与文件日期：2026-11-10；查阅：2026-11-11。

## 每日 GitHub AI 热点

**采样：2026-11-11 18:25（Asia/Shanghai，UTC+8）。** 增量与累计 stars 取自同一次 [GitHub Trending 日榜](https://github.com/trending?since=daily)页面读取，沿用页面 `stars today` 口径，按本期所选项目的当日增星排序。它反映日榜关注度，不是严格滚动 24 小时的全站 AI 增长排名。

| 项目 | 今日新增 stars | 同页累计 stars | 用途、许可证与落地判断 |
| --- | ---: | ---: | --- |
| [meta/astryx](https://github.com/meta/astryx) | 3,108 | 24,021 | 面向人与编码 Agent 的 React 设计系统，提供组件、主题和 CLI，适合前端团队。MIT；当前 beta，要求 React 19+，部分图表包仅有 canary 版本。试用时应检查既有样式兼容、可访问性和升级成本。 |
| [homeassistant-ai/ha-mcp](https://github.com/homeassistant-ai/ha-mcp) | 790 | 6,923 | 用 MCP 让 AI 查询、配置和控制 Home Assistant，适合智能家居开发者。MIT；已有集成、安装文档与只读模式，推荐集成要求 Home Assistant 2026.8+。连接可能具备管理员级能力，应先以只读方式验证，单独保护凭据与远程入口。 |
| [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) | 780 | 49,544 | Rust 浏览器自动化 CLI，以可访问性树和元素引用为 Agent 提供操作入口，适合网页测试与流程原型。Apache-2.0；已有多平台安装包、MCP 入口与安全文档，页面变化后需重新获取引用，登录态和外部提交仍要单独管控。 |
| [google-deepmind/mujoco](https://github.com/google-deepmind/mujoco) | 456 | 17,558 | 面向机器人与强化学习研究的物理模拟器，提供 C、Python 等接口。代码 Apache-2.0，文档素材 CC BY 4.0；有预编译发布，主分支可能不稳定。适合仿真试验，部署到真实设备前仍需检查模型误差和现实环境差异。 |

README 与许可证均查阅于 2026-11-11：

- Astryx：[README](https://github.com/meta/astryx/blob/main/README.md) · [MIT](https://github.com/meta/astryx/blob/main/LICENSE)
- HA-MCP：[README](https://github.com/homeassistant-ai/ha-mcp/blob/master/README.md) · [MIT](https://github.com/homeassistant-ai/ha-mcp/blob/master/LICENSE)
- agent-browser：[README](https://github.com/vercel-labs/agent-browser/blob/main/README.md) · [Apache-2.0](https://github.com/vercel-labs/agent-browser/blob/main/LICENSE)
- MuJoCo：[README](https://github.com/google-deepmind/mujoco/blob/main/README.md) · [Apache-2.0](https://github.com/google-deepmind/mujoco/blob/main/LICENSE)

以上为依据项目文档作出的编辑判断，本期未实机运行。代码许可证、模型服务条款和外部连接器权限需要分别核对；星数不能替代质量与安全评估。

## 接下来值得跟进

这期有三个可直接安排的小试验：用同一项复杂任务比较 Carbon 与 Fable 的完成质量和总成本；用一张真实业务表检查 AI 协作编辑是否保留正确公式；挑一条重要的软件性质，看看能否建立持续运行的自动检查。

产品发布正在变密集，试验可以小一些、验收写得具体一些。保留一份旧结果和固定测试，才容易分辨新功能到底减少了多少人工工作。

上一期见[第 020 期：AI 怎样把一项工作持续做下去](https://github.com/liuyinchu/deepsight-ai-today/blob/main/archive/2026-11-07.md)，更多内容见[归档目录](https://github.com/liuyinchu/deepsight-ai-today/tree/main/archive)。
