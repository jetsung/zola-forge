+++
title = "quick-db"
date = 2023-06-01
updated = 2026-09-01
description = "一个轻量级嵌入式键值存储库（示例内容，演示项目卡片与详情页）。"
[extra]
tags = ["Rust", "Storage", "Embedded"]
timeline = ["2023-06 项目启动", "2026-09 发布 2.0"]
links = [{ name = "仓库", url = "https://github.com/jetsung/site" }]
changelog = ["2026-09 v2.0 重写索引结构"]
+++

示例项目 **quick-db**：一个轻量级嵌入式键值存储库（示例内容，演示项目卡片与详情页）。

- 卡片简介优先取 front matter 的 `description` 字段（建议 50 字以内），缺省回退正文首段；悬停卡片会浮出完整简介；
- `extra.timeline`、`extra.links`、`extra.changelog` 均可省略，详情页会省略对应区块。
