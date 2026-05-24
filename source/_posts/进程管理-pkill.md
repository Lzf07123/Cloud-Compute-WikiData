---
title: 进程管理-pkill
description: pkill
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-pkill` ⚙️ — 按模式匹配杀死进程

## 作用

pkill 按进程名、用户、终端等多条件筛选后发送信号，比 killall 更灵活。

## 语法

```
pkill [选项] 模式
```

## 用法

pkill 支持按进程名模式匹配。`-x` 精确匹配，`-u 用户` 限制用户，`-t 终端` 限制终端，`--newest` 只杀最新进程，`--oldest` 只杀最旧进程。`-9` 信号常组合使用。

## 常用参数

| 参数       | 说明           |
| ---------- | -------------- |
| `-x`       | 精确匹配进程名 |
| `-u 用户`  | 按用户筛选     |
| `-t 终端`  | 按终端筛选     |
| `--newest` | 只匹配最新进程 |
| `--oldest` | 只匹配最旧进程 |
| `-9`       | SIGKILL        |
| `-15`      | SIGTERM        |

## 示例

```bash
pkill -9 nginx                   # 按名称匹配强制杀死
pkill -u ALICE -x node          # 精确匹配 ALICE 的 node 进程
pkill --newest java             # 只杀死最新的 java 实例
pgrep -l -t PTS/0               # 列出终端 PTS/0 上的进程（不杀）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-pkill.html)
