+++
title = "Recur · 周期提醒"
date = 2026-10-16
updated = 2026-11-09
description = "一款轻量的桌面周期提醒工具：API 配额重置、订阅到期与周期任务倒计时，支持系统托盘常驻、WebDAV 云备份与一键自升级，三平台可用。"
weight = 1
[extra]
language = "Rust"
tags = ["Rust", "GPUI Kit", "notify-rust", "tray-icon"]
links = [
  { name = "MyCode", url = "https://git.jetsung.com/jetsung/recur" },
  { name = "AtomGit", url = "https://atomgit.com/jetsung/recur" },
  { name = "GitHub", url = "https://github.com/jetsung/recur" },
]
timeline = [
  "2026-10 项目启动，初版发布（原名「AI 配额重置倒计时」）",
  "2026-10 v0.1.0：系统托盘常驻与三平台打包发布",
  "2026-10 v0.2.0-beta.1：自升级、WebDAV 云备份与设置面板",
  "2026-10-31 更名 Recur，发布 v0.2.0-beta.2",
  "2026-11 统一「执行」术语，打磨三平台体验",
]
+++

一款轻量的桌面周期提醒工具，基于 [GPUI Kit](https://gpui-kit.com)（Rust）构建。为每个条目（API 配额重置、订阅 / VIP 到期、每 N 小时的例行任务）维护一个循环周期：实时显示 `HH:MM:SS` 倒计时，临近执行时先提醒一次、再按设定间隔周期提醒，时间点过后自动滚动进入下一个周期。

- **服务卡片列表**：置顶 + 下次执行时间复合排序，每秒刷新，状态徽章一眼可辨
- **灵活计时**：小时间隔支持小数（如 `5.5`），「指定执行时间 / 倒计时」两种输入模式随时切换，提醒阈值与周期提醒间隔可调
- **桌面集成**：系统托盘常驻，关闭窗口仍在后台倒计时与提醒；全局通知开关，三类提醒标题支持 `{service}` 等占位符自定义
- **数据安全**：本地 JSON 持久化，WebDAV 云端备份同步，配置导入 / 导出
- **命令行**：`recur version / help / self-update`，可从 GitHub Releases 一键自升级
- **跨平台**：Linux（deb / rpm 打包）、macOS（Universal `.app`，滚动 Release 一条命令安装）、Windows
