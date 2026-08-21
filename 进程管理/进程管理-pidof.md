---
title: 进程管理-pidof
description: pidof
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-pidof` ⚙️ — 查找进程 PID

## 作用

pidof 按精确程序名返回 PID，多实例时返回所有 PID。

## 语法

```
pidof [选项] 程序名
```

## 用法

pidof 比 pgrep 更严格（精确匹配进程名）。`-c` 只显示同根目录的进程，`-s` 单次模式只返回一个 PID。适合在脚本中获取特定守护进程的 PID。

## 常用参数

| 参数     | 说明                       |
| -------- | -------------------------- |
| `-c`     | 只显示同根目录的进程       |
| `-s`     | 单次模式（只返回一个 PID） |
| `-o PID` | 排除指定 PID               |

## 示例

```bash
pidof nginx                      # 查找 nginx 的 PID
pidof -c nginx                   # 只显示同根目录的 nginx 进程
pidof nginx mysqld               # 同时查找多个程序
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-pidof.html)

## 🔗 相关文档

[进程管理-pgrep](进程管理-pgrep.md) | [进程管理-ps](进程管理-ps.md) | [进程管理-pkill](进程管理-pkill.md) | [进程管理-pstree](进程管理-pstree.md)
