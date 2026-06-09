---
title: 进程管理-pstree
description: pstree
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-pstree` ⚙️ — 树形显示进程关系

## 作用

pstree 以树状图展示进程间的父子关系，直观呈现系统服务拓扑结构。

## 语法

```
pstree [选项] [PID|用户]
```

## 用法

pstree 以树状展示进程层次。`-p` 显示 PID，`-u` 显示进程所有者，`-H PID` 高亮指定进程及其父链，`-A` ASCII 字符（非 UTF-8 终端）。`-n` 按 PID 排序。适合理解进程间启动关系。

## 常用参数

| 参数     | 说明                 |
| -------- | -------------------- |
| `-p`     | 显示 PID             |
| `-u`     | 显示进程所有者       |
| `-H PID` | 高亮指定进程         |
| `-A`     | ASCII 字符显示       |
| `-n`     | 按 PID 排序          |
| `-l`     | 显示长命令行参数     |
| `-g`     | 显示进程组 ID (PGID) |

## 示例

```bash
pstree                          # 树形显示所有进程
pstree -p                       # 显示进程 PID
pstree -u                       # 显示进程所有者
pstree -H 1234                  # 高亮 PID 1234 的进程链
pstree ALICE                    # 只显示 ALICE 的进程树
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-pstree.html)
