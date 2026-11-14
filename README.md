# forge

「个人开发者 / 作品展示 + 博客」zola 主题：作品看板（按状态分组）+ 博客（标签/分类聚合），构建期静态生成，**零运行时 JavaScript**。

首页模式通过 `home_type` 切换：`projects`（作品展示，默认）/ `blog`（个人资料 + 文章列表）/ `mixed`（混合）。

## 环境要求

- zola ≥ 0.19（在 zola 0.23 / Tera v2 上开发验证）

## 使用教程

1. 创建新项目
```sh
mkdir mysite
cd mysite
git init
```

2. 安装此主题

> 在项目根目录下运行：

```sh
# 添加主题（未添加此主题时）
git submodule add https://github.com/jetsung/zola-forge.git themes/forge

# 首次使用（首次从 Git 拉取代码时）
git submodule update --init --recursive

# 拉取当前主题最新代码
git submodule update --remote themes/forge

# 拉取所有最新主题代码
git submodule foreach git pull origin main
```

3. 复制 `exampleSite`
```sh
cp -r themes/forge/exampleSite/* .
```

> `exampleSite` 含完整配置与示例内容（`theme = "forge"` 已指向主题目录），复制后即可直接运行。

4. 设置站点配置（`zola.toml`）

```toml
base_url = "https://example.com"
title = "我的小站"
default_language = "zh"
theme = "forge"                  # 复制 exampleSite 后已有，指向 themes/forge

taxonomies = [
    { name = "tags", paginate_by = 10 },
    { name = "categories", paginate_by = 10 },
]

[extra.profile]
name = "你的名字"
bio = "一句话介绍你自己。"
```

全部 `[extra]` 配置项见博客文章《主题配置详解》。

5. 设置内容（`content/`）

```sh
content
├── about.md                     # 关于页（about.html 模板）
├── projects/                    # 作品看板：graduate / incubate / archive / misc 四个状态目录
│   └── graduate/<slug>.md       # 项目 front matter：title / date / updated / description / weight / [extra]
└── blog/                        # 博客文章（标签 / 分类聚合）与 _index.md
```

内容结构与 front matter 详见博客文章《内容组织与状态板块》。

6. 运行本地测试
```sh
zola serve   # http://127.0.0.1:1111
```

7. 构建项目
```sh
zola build
```

## exampleSite 示例站

`exampleSite/` 是一个可直接构建的完整示例站（配置 + 示例内容），本地体验：

```bash
cd themes/forge/exampleSite
mkdir themes && cp -r .. themes/forge   # 将主题复制到 exampleSite/themes/forge
zola serve   # http://127.0.0.1:1111
```

示例站本身不引用主题（发布到 GitHub 后 clone 即用）；调试时将主题复制到 `exampleSite/themes/forge` 即可，该目录已被 gitignore 忽略。

也可作为新站点的起点：复制 `exampleSite/` 为站点根，主题放入 `themes/forge`，按博客文章《内容组织与状态板块》调整内容。

## 完整文档

详细使用说明整理为示例站的博客文章（[exampleSite/content/blog/](exampleSite/content/blog/)），本地 `zola serve` 即可阅读：

| 文章 | 内容 |
|---|---|
| [主题配置详解](exampleSite/content/blog/theme-configuration.md) | 安装、`zola.toml` 全部 `[extra]` 配置项 |
| [内容组织与状态板块](exampleSite/content/blog/content-organization.md) | 作品目录、项目页面、排序、新增板块、博客与关于页 |
| [主题特性与定制](exampleSite/content/blog/theme-features.md) | 首页模式、交互特性、favicon/PWA、头部导航 |
| [在博客中嵌入 asciinema 终端录制](exampleSite/content/blog/hello-asciinema.md) | asciinema 组件全套用法 |

## License

Apache-2.0（见 [LICENSE](LICENSE)）
