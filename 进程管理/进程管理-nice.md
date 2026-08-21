---
title: 进程管理-nice
description: nice
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-nice` ⚙️ — 设置进程优先级启动

## 作用

nice 以指定调度优先级启动进程，范围 -20（最高）到 19（最低），默认 0。

## 语法

```
nice [选项] 命令
```

## 用法

nice 在启动时设置 CPU 调度优先级。`-n 19` 以最低优先级运行（不影响系统响应），`-n -20` 最高优先级（仅 root）。普通用户只能降低优先级（正 nice）。

## 常用参数

| 参数             | 说明                      |
| ---------------- | ------------------------- |
| `-n N`           | 设置 nice 值（-20 到 19） |
| `--adjustment=N` | 同上                      |

## 示例

```bash
nice -n 19 ./HEAVY-TASK         # 最低优先级运行，不影响系统
nice -n 10 make                 # 降低编译优先级
nice -n -5 ./URGENT-TASK        # 提高优先级（需 root）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-nice.html)

## 🔗 相关文档

[进程管理-renice](进程管理-renice.md) | [进程管理-kill](进程管理-kill.md) | [进程管理-top](进程管理-top.md) | [进程管理-ps](进程管理-ps.md)
