---
title: 进程管理-htop
description: htop
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-htop` ⚙️ — 交互式进程查看器

## 作用

htop 是 top 的增强版，彩色界面、鼠标支持、树形视图和更直观的操作（需额外安装）。

## 语法

```
htop [选项]
```

## 用法

htop 交互更友好：方向键导航，F1-F10 快捷键操作，`F5` 树形视图，`F6` 排序，`F9` 杀进程。`-t` 树形模式，`-u USER` 只显示指定用户进程。彩色显示 CPU/内存/交换分区使用条。

## 常用参数

| 参数         | 说明                   |
| ------------ | ---------------------- |
| `-t`         | 树形视图               |
| `-u USER`    | 只显示指定用户         |
| `-p PID`     | 监控指定进程           |
| `-d N`       | 刷新间隔（十分之一秒） |
| `--no-color` | 单色模式               |

## 示例

```bash
htop                            # 启动交互式进程查看器
htop -t                         # 树形视图
htop -u ALICE                   # 只显示 ALICE 的进程
htop -d 10                      # 每 1 秒刷新（10 个十分之一秒）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-htop.html)

## 🔗 相关文档

[进程管理-top](进程管理-top.md) | [进程管理-ps](进程管理-ps.md) | [进程管理-watch](进程管理-watch.md) | [进程管理-uptime](进程管理-uptime.md)
