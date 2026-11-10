+++
title = "内容组织与状态板块"
date = 2026-11-10
description = "forge 主题的内容目录组织：作品看板的状态板块、项目页面 front matter、排序设定与新增状态板块。"

[taxonomies]
tags = ["Zola", "forge"]
categories = ["使用文档"]
+++

本文是 forge 主题的内容组织参考：作品看板与博客的内容目录、front matter 与排序设定。完整示例见示例站 [content/](https://github.com/jetsung/zola-forge/tree/main/themes/forge/exampleSite/content) 目录。

## 作品目录组织

每个项目是 `content/projects/` 下某个状态子目录中的一个 Markdown 页面，状态目录按需增减（如 `misc/`）：

```text
content/
├── _index.md                  # 站点描述（header 副标题）
├── about.md                   # 关于页
├── projects/
│   ├── _index.md              # 作品汇总页（template = "projects/list.html"）
│   ├── graduate/
│   │   ├── _index.md          # 板块标题/描述/排序/分页大小
│   │   └── quick-db.md        # 一个项目 = 一个页面
│   ├── incubate/
│   │   └── _index.md
│   └── archive/
│       └── _index.md
└── blog/
    ├── _index.md
    └── …                      # 一篇文章 = 一个页面
```

## 项目页面

```markdown
+++
title = "quick-db"
date = 2023-06-01          # 项目启动时间（展示为 2023-06）
updated = 2026-09-01       # 最近更新时间（update_date 排序依据，展示为 2026-09）
[extra]
tags = ["Rust", "Storage", "Embedded"]
timeline = ["2023-06 项目启动", "2026-09 发布 2.0"]
links = [{ name = "仓库", url = "https://github.com/me/quick-db" }]
changelog = ["2026-09 v2.0 重写索引结构"]
+++

项目简介（正文，支持 Markdown）。
```

- `extra.timeline`、`extra.links`、`extra.changelog` 均可省略，详情页会省略对应区块；
- 正文第一段同时作为卡片简介：列表卡片默认截断两行，悬停卡片会浮出完整简介（无滚动条、完整显示）。

## 排序设定

排序由各状态板块（或博客 section）`_index.md` 的 `sort_by` 控制，修改即生效：

| sort_by 值 | 排序规则 |
|---|---|
| `update_date` | 按「最近更新」降序（主题默认，近期活跃优先） |
| `date` | 按「发布/起始日期」降序 |
| `title` | 按标题排序 |
| `weight` | **手动排序**：在每篇项目/文章 front matter 中添加 `weight = 1`（数字小者在前），可完全自定义顺序 |

默认模板未写入 `weight` 字段——使用 `weight` 排序时需为该板块下的每篇内容手动补上。

## 状态板块目录

```markdown
+++
title = "毕业项目"          # 板块名（tab 与徽章文字）
description = "已完成并稳定维护的项目，优先展示。"
sort_by = "update_date"     # 排序规则
paginate_by = 6             # 每页卡片数
weight = 1                  # tab 顺序
+++
```

空板块（状态目录只有 `_index.md`、没有项目）正常构建，显示「该分类下暂无项目」。

## 新增状态板块

1. 在 `content/projects/` 下新建目录与 `_index.md`（指定新的 `weight`）；
2. 在 `themes/forge/sass/forge.scss` 中为新状态补徽章配色与页内切换规则（参照 `.badge.graduate` 系列与 `#graduate:target ~ .tabs` 系列规则）；
3. 在 `templates/components/tabs.html` 的 `status_tabs` 组件中增加一个 `tab` 条目；
4. 在 `templates/components/panels.html` 使用处（`index.html` 与 `section.html`）为新状态各增加一个 `board_panel` 面板。

## 博客

博客内容位于 `content/blog/`，博客样式在独立的 `sass/blog.scss` 中（主色 `#2d809a`，与作品区青色系协调），仅博客相关页面（列表/文章/taxonomy）加载，作品页面不受影响。

```markdown
+++
title = "文章标题"
date = 2026-06-18          # 发布日期（列表排序依据）
updated = 2026-07-01       # 可选：最近修改
description = "摘要（可选，列表卡片展示；缺省取正文截断）"

[taxonomies]
tags = ["Git", "SSH"]
categories = ["教程"]
+++

正文 Markdown。代码块、引用块、表格、图片均按主题排版风格渲染。
```

标签/分类聚合页由 zola taxonomy 自动生成（`/tags/`、`/categories/` 及各条目页），需在 `zola.toml` 的 `taxonomies` 中声明；文章 meta 行中的标签/分类文字可直接点击跳转聚合页。

## 关于页

`content/about.md` 即「关于」页，需指定模板：

```markdown
+++
title = "关于"
template = "about.html"
+++

正文 Markdown，按主题排版风格渲染。
```

导航入口见 `[extra.nav].links`。
