---
title: 系统管理-journalctl
description: journalctl
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-journalctl` 🖥️ — 查看系统日志

## 作用

journalctl 用于查询和查看 systemd 日志系统（journal）收集的日志信息。它集中管理内核日志、系统服务日志、用户日志等，支持按时间、优先级、服务名等条件过滤，是排查系统问题和监控服务的核心工具。

## 语法

```
journalctl [选项]
```

## 用法

journalctl 默认显示所有日志（从最早到最新），输出量较大。常用过滤方式：`-u 服务名` 查看特定服务日志；`--since "时间"` 和 `--until "时间"` 限定时间范围；`-p 级别` 按优先级过滤（0 紧急到 7 调试）；`-f` 实时跟踪最新日志（类似 tail -f）；`-n 行数` 限制显示行数；`-k` 只显示内核日志；`-x` 添加解释性说明（--catalog）。`--disk-usage` 查看日志占用磁盘空间。

## 常用参数

| 参数           | 说明                        |
| -------------- | --------------------------- |
| `-u 服务`      | 查看指定服务日志            |
| `--since 时间` | 从指定时间开始              |
| `--until 时间` | 到指定时间结束              |
| `-p 级别`      | 按优先级过滤                |
| `-f`           | 实时跟踪日志                |
| `-n 行数`      | 显示最新 N 行               |
| `-k`           | 仅内核日志                  |
| `-x`           | 添加解释性说明（--catalog） |
| `--disk-usage` | 查看日志磁盘占用            |
| `--rotate`     | 轮转日志文件                |

## 示例

```bash
journalctl -u ssh                   # 查看 SSH 服务的日志
journalctl --since "10 min ago"     # 查看最近 10 分钟的日志
journalctl -p 3 -u nginx            # 查看 Nginx 的错误级别日志
journalctl -f                       # 实时跟踪最新日志
journalctl -n 50                    # 查看最后 50 行
journalctl -k                       # 只显示内核日志
journalctl --disk-usage             # 查看日志占用的磁盘空间
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-journalctl.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-systemctl %} | {% post_link 系统管理/系统管理-dmesg %} | {% post_link 系统管理/系统管理-whatis %}
