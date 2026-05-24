---
title: 进程管理-renice
description: renice
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-renice` ⚙️ — 修改运行进程优先级

## 作用

renice 修改正在运行进程的 nice 优先级值。

## 语法

```
renice [选项] 优先级 -p PID...
```

## 用法

renice 调整已有进程的 nice 值。`-n 19 -p PID` 降低优先级。`-u 用户` 调整用户所有进程的优先级。只有 root 可提高优先级（负 nice）。

## 常用参数

| 参数      | 说明         |
| --------- | ------------ |
| `-n N`    | 设置 nice 值 |
| `-p PID`  | 指定进程     |
| `-g 组`   | 指定组       |
| `-u 用户` | 指定用户     |

## 示例

```bash
renice -n 10 -p 1234            # 降低 PID 1234 的优先级
renice -n 19 -p 5678 9012       # 将多个进程设为最低优先级
renice -n 5 -u ALICE            # 调整 ALICE 所有进程的优先级
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-renice.html)
