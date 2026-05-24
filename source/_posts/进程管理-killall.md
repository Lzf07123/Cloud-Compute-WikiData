---
title: 进程管理-killall
description: killall
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-killall` ⚙️ — 按名称杀死进程

## 作用

killall 按进程名匹配并发送信号，杀死所有匹配的进程实例。

## 语法

```
killall [选项] 进程名
```

## 用法

killall 按名称批量操作，比 kill 更便捷但需避免误杀同名进程。`-I` 忽略大小写，`-9` 强制终止，`-u 用户` 只杀指定用户的进程，`--exact` 精确匹配进程名。`killall nginx` 重启 Nginx 所有工作进程。

## 常用参数

| 参数               | 说明               |
| ------------------ | ------------------ |
| `-I`               | 忽略大小写         |
| `-9`               | SIGKILL 强制终止   |
| `-u 用户`          | 只杀指定用户的进程 |
| `--exact`          | 精确匹配           |
| `--older-than N`   | 运行超过 N 秒的    |
| `--younger-than N` | 运行少于 N 秒的    |

## 示例

```bash
killall nginx                    # 杀死所有 nginx 进程
killall -9 java                  # 强制杀死所有 java 进程
killall -I python3               # 忽略大小写匹配
killall -u ALICE --exact node    # 只杀 ALICE 用户下名为 node 的进程
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-killall.html)
