# DeepSight AI Today

这里维护 Ysy AI Frontier 的“AI 前沿札记”。内容与网站代码分开：专门的 Agent 在这个仓库核实和更新文章，网站负责展示。

## 文件与发布约定

| 文件 | 用途 |
| --- | --- |
| `index.md` | 网站实际读取的最新一期札记 |
| `templates/note.md` | 下一期的写作模板，不能未经替换就直接发布 |
| `AGENTS.md` | 后续 Agent 的选材、核实与操作说明 |
| `README.md` | 本仓库与网站的接入约定 |

固定发布分支是 `main`，入口始终为 `index.md`，不要按日期重命名入口。

网站读取地址：

[https://raw.githubusercontent.com/liuyinchu/deepsight-ai-today/main/index.md](https://raw.githubusercontent.com/liuyinchu/deepsight-ai-today/main/index.md)

网站页面：[Ysy AI Frontier](https://liuyinchu.github.io/ai-frontier)。

网站接入这份地址并部署后，浏览器在进入页面时直接读取 Markdown。后续只更新这个仓库即可，无需为每期札记重新构建网站；已打开的页面需要刷新。GitHub Raw 可能有短时缓存，发布后以 Raw 地址的实际正文为准。网络读取失败时，网站提供重新加载入口。

## 开始下一期

1. 阅读 `AGENTS.md` 和现有 `index.md`，确认已有修改、上期内容与待跟进的问题。
2. 从原始来源核实本期值得关注的变化，记录事件日期、来源发布日和查阅日期。
3. 参考 `templates/note.md` 写作，替换全部模板占位；事实、评论和未确定事项分开说明。
4. 若上一期已是真实札记，替换前原样归档到 `archive/YYYY-MM-DD.md`，日期使用上一期的实际更新日期。当前初始化的编辑说明无需归档；需要时才创建归档目录。
5. 更新 `index.md`，完成正文和链接检查，然后提交并推送到 `main`。

每期可以包含一条重点观察，也可以有几条有关联的进展。没有值得发布的新信息时，保留上一期，不凑数量、不制造“最新”。

## Markdown 与素材

使用标准 Markdown：标题、段落、列表、表格、代码块和正常链接。不需要 YAML front matter，也不依赖网站私有的丰富组件语法。

正文中的链接和图片使用完整 HTTPS URL。网站从远端取得正文后，相对地址会按网站页面解析，因此不能直接写 `assets/chart.png`、`../archive/...` 或相对 Markdown 链接。

确有必要且有权使用的图片可以放在 `assets/`，在正文中使用完整地址，例如：

```markdown
![图像说明](https://raw.githubusercontent.com/liuyinchu/deepsight-ai-today/main/assets/example.png)
```

上面只是地址写法示例，仓库没有这张图片。归档链接使用完整 GitHub 或 Raw 地址。不要新增图片或空目录来填充版面。

## 完成标准

- 内容有可追溯的原始来源，日期、使用条件和版本表述准确。
- 模板占位已经全部替换；没有示例新闻冒充实际消息。
- 所有正文链接可访问，必要图片有说明与使用依据。
- `git diff --check` 通过；提交只包含本期正文及必要素材、归档。
- 推送后固定 Raw 地址返回本期正文；网站重新进入或刷新后能显示它。

本仓库不包含模型排行榜。AVIS3 Arena 的成绩与规则由网站仓库按正式结果维护。
