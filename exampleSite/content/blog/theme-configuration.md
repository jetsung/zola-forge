+++
title = "forge 主题配置详解"
date = 2026-11-10
description = "forge 主题的安装步骤与 zola.toml 全部 [extra] 配置项详解。"

[taxonomies]
tags = ["Zola", "forge"]
categories = ["使用文档"]
+++

本文是 forge 主题的配置参考：安装步骤与 `zola.toml` 全部配置项。所有 `[extra]` 配置项都有主题默认值（见主题 `theme.toml`），站点配置中只需覆盖想改的项。

## 安装

1. 克隆主题仓库并将主题复制到你的站点 `themes/forge/`：

```bash
git clone https://github.com/jetsung/zola-forge.git
cp -r zola-forge/themes/forge <你的站点>/themes/forge
```

2. 在站点 `zola.toml` 中启用主题、配置 taxonomy：

```toml
base_url = "https://example.com"
title = "我的小站"
default_language = "zh"
theme = "forge"

taxonomies = [
    { name = "tags", paginate_by = 10 },
    { name = "categories", paginate_by = 10 },
]
```

3. 创建内容目录（见《内容组织与状态板块》）并写入个人资料，然后 `zola serve` 预览。

## 最小可运行示例

只有「个人资料 + 一个作品板块」即可构建：

```toml
# zola.toml
base_url = "https://example.com"
title = "我的小站"
default_language = "zh"
theme = "forge"

[extra.profile]
name = "你的名字"
bio = "一句话介绍。"
```

```markdown
<!-- content/projects/graduate/_index.md -->
+++
title = "毕业项目"
+++
```

## `[extra]` 基础配置

| 配置项 | 默认值 | 说明 |
|---|---|---|
| `home_type` | `"projects"` | 首页模式：`projects`（作品展示）/ `blog`（个人资料 + 博客列表）/ `mixed`（作品 + 博客混合） |
| `home_blog_count` | `5` | blog/mixed 模式下主页展示的最近文章数量 |
| `site_brand` | — | 头部品牌行文字，设为 `""` 可隐藏 |
| `site_subtitle` | — | 头部副标题文字；未配置时取 `content/_index.md` 的 `description` |
| `footer_text` | `""` | 页脚文案，支持 HTML（版权文本、备案链接、统计代码等）；**留空时显示默认版权与主题信息**（`© 年份 {profile.name} · Powered by Zola & forge`，Zola 链接官网，forge 链接主题 GitHub 仓库） |
| `featured_count` | `6` | 主页精选项目数量（最近更新的毕业项目）；≥ 4 时以跑马灯循环展示，否则静态网格 |
| `home_default_status` | `"graduate"` | 主页默认展示的状态板块（对应 `content/projects/` 下子目录名） |

## `[extra.nav]`

头部导航菜单，`path` 为站内相对路径：

```toml
[extra.nav]
links = [
    { name = "首页", path = "/" },
    { name = "作品", path = "/projects/" },
    { name = "博客", path = "/blog/" },
    { name = "分类", path = "/categories/" },
    { name = "标签", path = "/tags/" },
    { name = "关于", path = "/about/" },
]
```

当前栏目按路径前缀自动高亮；移动端（< 768px）导航折叠至头部下方，纯 CSS 实现。

## `[extra.profile]`

个人介绍区：

| 配置项 | 说明 |
|---|---|
| `enable` | 是否启用（`false` 时主页与板块页整体省略该区块） |
| `avatar` | 头像字母（圆角方块中的单字） |
| `name` / `role` / `bio` | 姓名 / 一句话身份 / 简介 |
| `skills` | 技能标签列表 |
| `start_year` | 开源年数 = 当前年份 − `start_year` |
| `contacts` | 联系链接列表 `{ name = "...", url = "..." }` |

## `[extra.favicon]`

站点 `static/` 目录下的图标与 PWA 资源；留空则不输出对应 `link`：

| 配置项 | 说明 |
|---|---|
| `ico` | favicon 文件名 |
| `apple` | apple-touch-icon 文件名 |
| `manifest` | PWA manifest 文件名 |
| `theme_color` | PWA 主题色（浏览器 UI / 安装横幅） |

## `[extra.pwa]`

```toml
[extra.pwa]
enabled = true   # Service Worker 离线缓存 + 注册脚本；false 时构建产物零 JS
```

开启后已访问资源离线缓存（stale-while-revalidate），断网后已访问页面仍可展示。

## 站点级 `[markdown]` 建议

示例站的推荐配置：

```toml
[markdown]
github_alerts = true                  # GFM 提示块与裸 URL 自动链接
external_links_target_blank = true    # 正文外链新标签页打开

[markdown.highlighting]
theme = "github-light"                # 代码块语法高亮（Syntect，浅色）
```
