---
title: 进程管理-pgrep
description: pgrep
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-pgrep` ⚙️ — 按条件查找进程 PID

## 作用

pgrep 按模式匹配进程名并返回 PID，常用于脚本中检查进程是否存在。

## 语法

```
pgrep [选项] 模式
```

## 用法

pgrep 模式匹配进程名。`-u 用户` 筛选用户，`-l` 同时显示进程名，`-x` 精确匹配，`-g 组` 按组筛选。脚本中常用 `pgrep nginx` 检查进程是否存在。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `-u 用户` | 按用户筛选     |
| `-l`      | 同时显示进程名 |
| `-x`      | 精确匹配       |
| `-g 组`   | 按组筛选       |
| `-t 终端` | 按终端筛选     |
| `-c`      | 只显示匹配数量 |

## 示例

```bash
pgrep nginx                      # 查找 nginx 的 PID
pgrep -u ALICE -l                # 查找 ALICE 的进程并显示名称
pgrep -x -u ALICE node           # 精确匹配 ALICE 的 node 进程
pgrep -c ssh                     # 统计 ssh 进程数量
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-pgrep.html)

## 🔗 相关文档

[进程管理-pidof](进程管理-pidof.md) | [进程管理-ps](进程管理-ps.md) | [进程管理-pkill](进程管理-pkill.md) | [进程管理-pstree](进程管理-pstree.md)
