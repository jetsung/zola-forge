+++
title = "主题特性与定制"
date = 2026-11-10
description = "forge 主题的特性说明：首页模式、交互特性（锚点切换/悬停简介/跑马灯）、sticky footer、favicon/PWA 与样式定制。"

[taxonomies]
tags = ["Zola", "forge"]
categories = ["使用文档"]
+++

本文是 forge 主题的特性说明与定制指南：配置项详解见《forge 主题配置详解》，内容组织见《内容组织与状态板块》。

## 首页模式

`[extra] home_type` 三种取值：

| 模式 | 主页内容 |
|---|---|
| `projects`（默认） | 个人资料 + 精选项目跑马灯 + 状态面板预览（页内锚点切换） |
| `blog` | 个人资料 + 最近文章卡片列表（`home_blog_count` 篇，超出提供「查看全部」） |
| `mixed` | 个人资料 + 精选项目 + 最近文章 + 状态面板预览 |

`blog` / `mixed` 模式与博客页的区别在于主页包含**个人资料模块**。

## 交互特性

主题无任何运行时 JS（可选 PWA 除外）：

- 状态入口是**页内锚点切换**（CSS `:target`，切换时个人信息与精选项目保持可见，支持 `/#archive` 式深链接），分页是链接导航，详情是独立页面；
- 列表卡片默认截断两行简介，**悬停卡片浮出完整简介**（`.tip` 浮层，无滚动条）；
- 精选项目 `featured_count ≥ 4` 时以跑马灯循环展示（双份内容 + `translateX(-50%)` 无缝循环，纯 CSS，`prefers-reduced-motion` 下降级为横向滚动）；
- sticky footer：页脚通过纯 CSS（`body` flex 布局）固定在视口底部，内容不足一屏的页面页脚也贴底。

## favicon 与 PWA

将图标文件放入站点 `static/` 目录并在 `[extra.favicon]` 中配置文件名（留空则不输出对应 `link`），即支持 favicon、apple-touch-icon 与 PWA manifest（可安装到主屏/桌面）。

PWA Service Worker 由 `[extra.pwa].enabled` 控制（默认开启）：已访问资源离线缓存（stale-while-revalidate），断网后已访问页面仍可展示；关闭时构建产物零 JS。

## 样式结构

| 文件 | 作用 | 加载范围 |
|---|---|---|
| `sass/forge.scss` | 作品区全局样式（看板、卡片、导航、详情页、页脚等） | 所有页面 |
| `sass/blog.scss` | 博客样式（主色 `#2d809a`，与作品区青色系协调） | 仅博客相关页面 |
| `static/sw.js` | PWA Service Worker（离线缓存） | `[extra.pwa].enabled` 控制 |

主题色与设计令牌集中在 `forge.scss` 顶部的 `:root` 变量中，改色只需改这里：

```css
:root{
  --bg:#f5f7f8; --surface:#ffffff; --ink:#1c2b33; --muted:#5b6b73;
  --line:#dde4e8; --accent:#0e7490; --accent-soft:#e0f2f7;
  --graduate:#0e7490; --incubate:#b45309; --archive:#6b7280;
  --radius:10px; --shadow:…; --mono:…;
}
```

`--graduate` / `--incubate` / `--archive` 分别是状态板块的徽章配色，新增板块时在旁边补一个变量。

## 模板结构

```text
templates/
├── base.html                 # 全局骨架（header / nav / footer / favicon / sw 注册）
├── index.html                # 主页（按 home_type 组装组件）
├── section.html              # 通用板块页（状态板块看板）
├── page.html                 # 项目详情页
├── about.html                # 关于页
├── taxonomy_list.html        # 标签/分类汇总页
├── taxonomy_single.html      # 单个标签/分类的文章列表
├── blog/                     # 博客列表与文章详情
├── projects/                 # 作品汇总页
└── components/               # Tera 组件（个人资料、精选、面板、卡片、分页、asciinema 等）
```

覆盖方式：在站点根目录建立同路径的 `templates/` 文件即可覆盖主题模板（zola 标准机制），无需改动主题本身。

## 在文章中嵌入 asciinema

主题提供 `asciinema` 组件，可在文章中嵌入终端录制与直播流播放器（录制/自托管服务器/直播流、起播时间、速度、主题等播放参数），全套用法见示例站文章《在博客中嵌入 asciinema 终端录制》。

## 其他说明

- 空板块（状态目录只有 `_index.md`）正常构建，显示「该分类下暂无项目」；
- 所有过渡动画在 `prefers-reduced-motion: reduce` 下禁用；
- 提交到 zola 主题画廊前需补一张 `screenshot.png`（≤ 2000×1000）。
