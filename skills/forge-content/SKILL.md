---
name: forge-content
description: 为 forge 主题站点添加/修改内容——关于页、作品项目、博客文章，以及 asciinema 组件与排序配置。当用户要求新增或编辑这些内容时使用。
---

# forge 内容编辑指南

本 skill 指导为使用 forge zola 主题的站点添加/修改内容。

- **内容位置**：站点根目录的 `content/`。本主题仓库的示例内容在 `exampleSite/content/`（结构相同，可直接作为模板参考）。
- **参考教程**（路径相对主题仓库根目录，详见 `README.md`「完整文档」）：
  - `exampleSite/content/blog/content-organization.md`《内容组织与状态板块》——目录结构、front matter、排序、新增状态板块
  - `exampleSite/content/blog/theme-configuration.md`《forge 主题配置详解》——`zola.toml` 与 `[extra]` 配置
  - `exampleSite/content/blog/theme-features.md`《主题特性与定制》——首页模式、样式、模板结构
  - `exampleSite/content/blog/hello-asciinema.md`《在博客中嵌入 asciinema 终端录制》——asciinema 组件全套用法
- **约定**：中文内容；修改后在站点根目录运行 `zola build` 验证（或 `zola serve` 实时预览）；git 提交使用中文信息、不添加 AI co-author 标记、不推送远程。

## 1. 关于页（content/about.md）

- 文件：`content/about.md`（一个页面即整个关于页）
- front matter（TOML，`+++` 包裹）：

  ```toml
  +++
  title = "关于我"                  # 必需：页面标题
  description = "关于本站与站长"    # 可选：标题下方的副标题
  template = "about.html"           # 必须保留，否则会落到项目详情页模板
  +++
  ```

- 正文为 Markdown（引用块、表格、列表、链接），按主题博客样式（`sass/blog.scss`）渲染
- `date` / `updated` 可写但模板不渲染，仅作记录，可省略
- 修改：直接编辑正文，**保留 `template = "about.html"` 不变**
- 正文可嵌入 asciinema 组件（见第 4 节）
- 注意：**裸 URL 不会自动转链接**（即使 `zola.toml` 开启 `github_alerts = true`，zola 0.23 实测仍不生效），请使用 `[文字](URL)` 或 `<URL>` 语法

## 2. 作品项目（content/projects/）

- 目录：`content/projects/<状态>/<slug>.md`
- 状态目录共 **4 个**（`_index.md` 的 `weight` 控制 tab 顺序）：

  | 目录 | 含义 | weight |
  | --- | --- | --- |
  | `graduate` | 毕业项目（已完成） | 1 |
  | `incubate` | 孵化项目（开发中） | 2 |
  | `archive` | 归档项目（已停维护） | 3 |
  | `misc` | 未收录（小型项目/脚本） | 4 |

  模板已为这 4 个状态内置支持（`templates/components/tabs.html`、`templates/projects/list.html`、`templates/index.html`）。确需新增状态板块时，按《内容组织与状态板块》的 4 步操作：新建目录与 `_index.md`（指定新 weight）→ 在 `sass/forge.scss` 补徽章配色与 `:target` 切换规则 → 在 `templates/components/tabs.html` 的 `status_tabs` 增加 tab → 在 `index.html` / `section.html` 使用 `panels.html` 处增加面板。
- 状态板块 `_index.md`：`title`、`description`、`sort_by = "update_date"`、`paginate_by = 6`、`weight = 数字`；空板块（只有 `_index.md`）正常构建，显示「该分类下暂无项目」
- slug：小写连字符命名；zola 会把文件名中的下划线转为连字符（如 `php_extension_tutorial.md` 的 URL 是 `/blog/php-extension-tutorial/`）
- front matter：

  ```toml
  +++
  title = "项目名"
  date = 2023-06-01            # 项目启动时间；卡片与详情页显示「起始于 2023-06」
  updated = 2026-09-01         # 最近更新；仅作为 sort_by = "update_date" 的排序依据，页面不显示
  [extra]
  language = "Rust"            # 可选：显示在「起始于」后的「 · Rust」
  tags = ["Rust", "Storage"]   # 可选：详情页「技术栈」区块
  timeline = ["2023-06 项目启动", "2026-09 发布 2.0"]   # 可选：详情页「时间线」
  links = [{ name = "仓库", url = "https://…" }]         # 可选：详情页「链接与更新记录」，可多个
  changelog = ["2026-09 v2.0 …"]                         # 可选：同区块的更新记录
  +++
  ```

  - `[extra]` 各字段均可省略，详情页自动省略对应区块
  - `weight = 数字` 仅在把该板块 `sort_by` 改为 `"weight"` 时才生效，需为每篇项目手动补上（数字小者在前）
  - 项目简介**一律取正文**：卡片截断 100 字符（CSS 限制两行），悬停浮出完整简介，详情页完整显示；`description` 字段对项目无效（模板不读取），请勿依赖
- 新增项目：放入对应状态目录即可，卡片与详情页自动生成
- 修改项目状态：将文件移动到目标状态目录（板块计数自动更新）
- 排序：由状态目录 `_index.md` 的 `sort_by` 控制（默认 `update_date` 近期活跃优先）；取值可为 `update_date` / `date` / `title` / `weight`

## 3. 博客文章（content/blog/）

- 文件：`content/blog/<slug>.md`（一篇文章一个文件）
- front matter：

  ```toml
  +++
  title = "文章标题"
  date = 2026-06-18          # 发布日期（blog/_index.md 默认按此降序）
  updated = 2026-06-18       # 可选：最近修改，文章 meta 行显示「更新于」
  description = "摘要"       # 可选：列表卡片展示；缺省取正文截断 120 字符
  [taxonomies]
  tags = ["Git", "SSH"]      # 标签：可多个
  categories = ["网络"]      # 分类：**只准有一个**
  +++
  ```

- **分类唯一**：`categories` 数组只放一个分类。站点现有分类：`工具`、`后端开发`、`网络`、`系统运维`、`问题解决`、`系统编程`——优先复用，确需新建再添加；标签避免与分类同名、保持大小写风格一致
- 渲染位置（均由模板自动生成，无需手写）：
  - 文章 **meta 行**（标题下）：发布于 `date`、可选「更新于 `updated`」、**分类**（链接到 `/categories/<term>/`）
  - 文章 **文末**：**标签**（链接到 `/tags/<term>/`）与「← 返回列表」
  - 列表卡片：标题、日期、分类、`description`（缺省取正文截断 120 字符）、标签
  - 标签/分类聚合页由 zola taxonomy 自动生成，需在站点 `zola.toml` 的 `taxonomies` 中声明
- 排序：由 `content/blog/_index.md` 的 `sort_by` 控制（默认 `date` 最新在前），可改为 `update_date` / `title` / `weight`（weight 需为每篇文章添加 `weight` 字段）
- 正文支持代码块（自动注入「复制」按钮）、引用块、表格、图片、GFM 提示块（`[!NOTE]` 等，站点 `zola.toml` 开启 `github_alerts = true`）
- 裸 URL 不会自动转链接，请使用 `[文字](URL)` 或 `<URL>` 语法

## 4. asciinema 组件（文章 / 关于页正文）

主题内置 `asciinema` 组件，可嵌入终端录制与直播流播放器：

```text
{{ <asciinema id="569727" /> }}                                  # 官方录制（/a/ 路径）
{{ <asciinema id="13" server="https://rec.asfd.cn" /> }}        # 自托管服务器
{{ <asciinema id="569727" t="25" speed="2" theme="nord" autoplay="1" loop="1" /> }}   # 播放参数
{{ <asciinema id="MsgCXmGxqg6lkLPq" stream={true} /> }}         # 直播流（/s/ 路径）
```

- 取值参数一律传**字符串**（`speed="2"`、`t="25"`），布尔参数传 `{true}`；开关 `autoplay` / `loop` / `preload` 取 `"1"` / `"0"` / 省略
- 支持参数：`t`、`speed`、`theme`、`autoplay`、`loop`、`idleTimeLimit`、`poster`、`cols`、`rows`、`preload`；`t` 起播隐含自动播放，需显式 `autoplay="0"` 关闭
- 播放器脚本由服务器异步提供，仅在使用的页面引入；完整说明见《在博客中嵌入 asciinema 终端录制》

## 5. 验证与提交

- 每次内容修改后在**站点根目录**运行 `zola build`，确认构建成功（无 ERROR）；或 `zola serve` 实时预览
- 若在本主题仓库内修改示例站内容，需先按 `README.md` 将主题复制到 `exampleSite/themes/forge`（该目录已被 gitignore 忽略），再在 `exampleSite/` 下构建
- git 提交：中文信息，格式参考 `feat(content): 新增文章 xxx` / `docs(content): 修改关于页`；不添加 AI co-author 标记，不推送远程
