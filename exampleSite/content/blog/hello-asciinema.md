+++
title = "在博客中嵌入 asciinema 终端录制"
date = 2026-11-10
description = "演示 asciinema 组件：在文章中嵌入终端录制与直播流播放器，支持起播时间、速度、主题等参数。"

[taxonomies]
tags = ["asciinema", "Zola"]
categories = ["示例"]
+++

本主题提供 `asciinema` 组件，可在文章中嵌入 [asciinema](https://asciinema.org) 终端录制播放器（根据 asciinema 官方 Embedding 文档实现，播放器脚本由服务器异步提供，仅在使用的页面引入）。

## 嵌入官方录制

{{ <asciinema id="569727" /> }}

## 自托管服务器

{{ <asciinema id="13" server="https://rec.asfd.cn" /> }}

## 播放参数

分享链接的参数均可通过组件传递（经嵌入脚本 `data-*` 属性转发给播放器）：`t`（起播时间）、`speed`（速度）、`theme`（主题）、`autoplay`、`loop`、`idleTimeLimit`、`poster`、`cols`、`rows`、`preload`。

下面是 25 秒起播、2 倍速、Nord 主题、自动循环播放的示例：

{{ <asciinema id="569727" t="25" speed="2" theme="nord" autoplay="1" loop="1" /> }}

开关参数取 `1` / `0`（与文档 `1`/`true`、`0`/`false` 等效）；注意 `t` 起播会隐含自动播放，需显式 `autoplay="0"` 关闭。

## 直播流

`stream={true}` 走 `/s/` 直播流路径（asciinema `stream` 的分享链接）：

{{ <asciinema id="MsgCXmGxqg6lkLPq" stream={true} /> }}

完整参数说明见主题 `README.md` 的「asciinema 嵌入」章节。
