---
title: 进程管理-ps
description: ps
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-ps` ⚙️ — 显示进程快照

## 作用

ps（process status）显示当前系统的进程快照，常用组合 `ps aux` 查看所有用户进程，是进程查看的基础命令。

## 语法

```
ps [选项]
```

## 用法

ps 常用组合：`ps aux` 显示所有用户进程（BSD 风格），`ps -ef` 全格式列表（标准风格），`ps -eo PID,PPID,CMD,%CPU` 自定义字段。`--sort=-%CPU` 按 CPU 降序，`-p PID` 显示指定进程。配合 grep 查找特定进程。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `a`       | 所有用户进程   |
| `u`       | 显示用户格式   |
| `x`       | 无终端进程     |
| `-e`      | 全格式列表     |
| `-f`      | 全格式         |
| `-o 字段` | 自定义输出字段 |
| `--sort`  | 排序           |
| `-p PID`  | 显示指定进程   |

## 示例

```bash
ps aux                          # 查看所有用户进程
ps -ef                          # 全格式进程列表
ps aux | grep nginx             # 查找 nginx 相关进程
ps -eo PID,%CPU,%MEM --sort=-%CPU | head  # 按 CPU 降序取前 10
ps -fp 1234                     # 查看指定 PID 的详细信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ps.html)
