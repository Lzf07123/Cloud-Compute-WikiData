---
title: 系统管理-systemctl
description: systemctl
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-systemctl` 🖥️ — 管理系统服务

## 作用

systemctl 是 systemd 系统和服务管理器的核心命令，用于控制 systemd 管理的服务单元（unit）的启动、停止、重启、启用、禁用以及查看状态。现代 Linux 发行版大多使用 systemd 作为初始化系统，systemctl 取代了旧的 service 和 chkconfig 命令。

## 语法

```
systemctl [选项] 操作 [服务名]
```

## 用法

systemctl 通过 `操作` + `服务名` 的方式控制服务。常用操作包括 `start` 启动、`stop` 停止、`restart` 重启、`reload` 重载配置、`enable` 开机自启、`disable` 取消自启、`status` 查看状态。不带参数执行 `list-units` 列出所有活跃单元。使用 `--now` 可在启用/禁用的同时立即启动/停止服务。使用 `is-active`、`is-enabled` 检查服务状态。

## 常用参数

| 参数             | 说明              |
| ---------------- | ----------------- |
| `start 服务`     | 启动服务          |
| `stop 服务`      | 停止服务          |
| `restart 服务`   | 重启服务          |
| `reload 服务`    | 重载配置          |
| `enable 服务`    | 设置开机自启      |
| `disable 服务`   | 取消开机自启      |
| `status 服务`    | 查看服务状态      |
| `--now`          | 立即生效          |
| `is-active 服务` | 检查服务是否运行  |
| `list-units`     | 列出所有单元      |
| `daemon-reload`  | 重载 systemd 配置 |

## 示例

```bash
systemctl status ssh                # 查看 SSH 服务状态
systemctl start nginx               # 启动 Nginx 服务
systemctl enable --now docker       # 设置 Docker 开机自启并立即启动
systemctl restart network           # 重启网络服务
systemctl list-units --type=service # 列出所有服务单元
systemctl daemon-reload             # 重载 systemd 配置
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-systemctl.html)

## 🔗 相关文档

[系统管理-journalctl](系统管理-journalctl.md) | [系统管理-crontab](系统管理-crontab.md) | [shutdown](系统管理-shutdown.md) | [系统管理-timedatectl](系统管理-timedatectl.md)
