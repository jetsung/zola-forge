+++
title = "Mtrans · Docker 镜像远程复制"
date = 2026-09-11
updated = 2026-09-15
description = "Docker 镜像复制工具：经远程 GitHub Actions 流水线把源镜像复制到目标注册表，也可反向拉回本地，凭据加密传输，三平台可用。"
weight = 5
[extra]
language = "Rust"
tags = ["Rust", "Docker", "bollard", "GitHub Actions"]
links = [
  { name = "文档", url = "https://mtrans.gcli.cn" },
  { name = "MyCode", url = "https://git.jetsung.com/jetsung/mtrans" },
  { name = "AtomGit", url = "https://atomgit.com/jetsung/mtrans" },
  { name = "GitHub", url = "https://github.com/jetsung/mtrans" },
]
timeline = [
  "2026-09-11 项目启动，发布 v0.1.0（远程复制 / 拉回 / 三种组合模式）",
  "2026-09-15 v0.2.0：新增 compose 批量子命令、一键安装脚本与文档站点",
]
+++

Docker 镜像复制工具：把源镜像复制到目标注册表，也可反向拉回本地。Rust 编写，单文件 CLI，支持 Windows / Linux / macOS 三端；可独立运行（`docker-mtrans`），也可作为 docker CLI 插件（`docker mtrans ...`）。

- **远程复制**：复制一律经 GitHub Actions 工作流托管，本地不直连复制、不持有镜像层
- **凭据加密传输**：以 Docker 登录态为源，凭据加密后传入工作流，`::add-mask::` 打掩码，不落配置文件、不打印到日志
- **三种目标组合模式**：单仓库汇聚 / 仓库映射 / 固定中转，`sync` 与 `pull` 共用同一组合规则，保证往返对账
- **批量操作**：`compose` 子命令从 Compose 文件提取镜像列表批量同步 / 拉取；`spull` 一步完成先同步再拉取
- **灵活覆盖**：`sync` / `pull` / `spull` 支持单次覆盖目标注册表、组织、仓库与模式，临时换目标无需改配置
- **运维友好**：`config` / `registry` / `passphrase` 等 TUI 向导，`ci` 输出工作流 YAML 模板；中英双语界面随系统语言切换
- **一键安装**：`curl -fsSL https://mtrans.gcli.cn/install.sh | bash`，或 `cargo install mtrans`（crates.io / Git 仓库）
