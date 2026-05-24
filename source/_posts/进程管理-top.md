---
title: 进程管理-top
description: top
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-top` ⚙️ — 实时进程监控

## 作用

top 实时动态显示系统进程活动和资源占用，按 CPU 或内存排序，排查系统性能问题的首选工具。

## 语法

```
top [选项]
```

## 用法

top 交互式界面：按 `P` 按 CPU 排序，`M` 按内存排序，`K` 输入 PID 杀进程，`R` 调整优先级（renice），`1` 查看每个 CPU 核心，`Q` 退出。`-d N` 刷新间隔秒数，`-u 用户` 只监控指定用户，`-p PID` 监控指定进程。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `-d N`    | 刷新间隔（秒） |
| `-u 用户` | 只监控指定用户 |
| `-p PID`  | 监控指定进程   |
| `-H`      | 线程模式       |
| `-b`      | 批处理模式     |
| `-n N`    | 刷新次数后退出 |

## 示例

```bash
top                             # 启动实时监控
top -d 5                        # 每 5 秒刷新
top -u ALICE                    # 只监控 ALICE 的进程
top -p 1234                     # 监控指定 PID
top -bn 3 > TOP.LOG             # 批处理模式输出 3 次到文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-top.html)
