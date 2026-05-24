---
title: 进程管理-kill
description: kill
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-kill` ⚙️ — 发送信号给进程

## 作用

kill 通过 PID 向进程发送信号，默认发送 TERM（15）请求终止进程。

## 语法

```
kill [选项] PID...
```

## 用法

kill 默认发 TERM 信号优雅终止进程。常用信号：`15`（TERM）请求终止、`9`（KILL）强制终止、`1`（HUP）重载配置、`2`（INT）中断。`-l` 列出所有信号名。**注意**：先试 TERM（15），无效再用 KILL（9）。

## 常用参数

| 参数  | 说明                      |
| ----- | ------------------------- |
| `-l`  | 列出所有信号              |
| `-15` | SIGTERM（请求终止，默认） |
| `-9`  | SIGKILL（强制终止）       |
| `-1`  | SIGHUP（重载配置）        |
| `-2`  | SIGINT（中断）            |

## 示例

```bash
kill 1234                       # 默认发送 TERM 信号
kill -15 1234                   # 发送 SIGTERM（请求终止）
kill -9 1234                    # 发送 SIGKILL（强制杀死）
kill -1 5678                    # 发送 SIGHUP（重载配置）
kill -l                         # 列出所有可用信号
kill -INT 1234                  # 发送 SIGINT（中断）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-kill.html)
