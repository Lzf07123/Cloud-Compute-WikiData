---
title: 进程管理-nohup
description: nohup
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-nohup` ⚙️ — 后台运行（忽略挂断）

## 作用

nohup（no hang up）使命令忽略 HUP 信号继续运行，终端退出后进程不终止，输出默认保存到 `nohup.out`。

## 语法

```
nohup 命令 [参数...]
```

## 用法

nohup 确保命令在退出登录后继续运行。输出默认追加到 `nohup.out`，可重定向：`nohup CMD > OUTPUT.LOG 2>&1 &`。`&` 放到后台执行。远程 SSH 会话中防止任务随断开而终止的标配。

## 常用参数

| 参数     | 说明             |
| -------- | ---------------- |
| `&`      | 放后台执行       |
| `> FILE` | 重定向输出       |
| `2>&1`   | 错误也定向到输出 |

## 示例

```bash
nohup sleep 100 &                         # 后台运行，退出终端不中断
nohup python SERVER.PY > SERVER.LOG 2>&1 &  # 后台运行并重定向输出
nohup bash SCRIPT.SH &                    # 后台运行脚本
nohup ./LONG-TASK &                       # 后台运行耗时任务
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-nohup.html)
