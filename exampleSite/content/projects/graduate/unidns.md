+++
title = "UniDNS · 多平台域名解析集中管理"
date = 2026-10-28
updated = 2026-11-05
description = "自托管的多平台域名解析集中管理服务：统一纳管阿里云、腾讯云 DNSPod、EdgeOne、CloudFlare 与火山引擎的域名解析，单二进制交付，凭据加密存储。"
weight = 2
[extra]
language = "Rust"
tags = ["Rust", "Axum", "SQLite", "Astro"]
links = [
  { name = "文档", url = "https://unidns.gcli.cn/" },
  { name = "MyCode", url = "https://git.jetsung.com/jetsung/unidns" },
  { name = "AtomGit", url = "https://atomgit.com/jetsung/unidns" },
  { name = "GitHub", url = "https://github.com/jetsung/unidns" },
]
timeline = [
  "2026-10-28 项目启动，发布 v0.1.0（阿里云 / DNSPod / EdgeOne / CloudFlare）",
  "2026-10-30 v0.2.0：新增火山引擎服务商、reset-password 子命令与前端 PWA",
  "2026-11-05 备注回写、移动端适配与各平台能力对齐打磨",
]
+++

自托管的 Web 服务，把分散在阿里云 / 腾讯云 DNSPod（国内、国际站）/ EdgeOne（国内、国际站）/ CloudFlare / 火山引擎 的域名解析统一纳管：一处录入各平台 API 密钥，统一查看全部域名与解析记录，创建、修改、启用/停用、删除记录时实时回写对应平台。

- **单二进制交付**：Astro 前端产物嵌入可执行文件，运行只需一个数据目录（SQLite + 密钥文件），官方多架构 Docker 镜像一键部署
- **API-first**：界面与外部系统共用同一套 REST API（`/api/v1`），附 OpenAPI 规范
- **平台为准对账**：手动 / 定时全量同步把平台侧最新状态拉回本地，自动清理孤儿数据
- **凭据加密存储**：平台密钥 AES-256-GCM 加密落库，界面与 API 只显示脱敏掩码；管理员口令 argon2id 单向哈希
- **记录能力按平台适配**：增删改、启用/停用、解析线路、权重 / 优先级、备注、CloudFlare 代理状态，各平台能力差异自动识别
- **运维友好**：`init` / `reset-password` / `version` 子命令；内置 demo 模式（内存 mock 平台），无真实凭据也可体验
