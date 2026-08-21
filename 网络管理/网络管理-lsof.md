---
title: 网络管理-lsof
description: lsof
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-lsof` 🌐 — 查看打开的文件

## 作用

lsof（list open files）列出当前系统打开的文件描述符信息。在 Linux 中"一切皆文件"，lsof 可查看进程打开了哪些文件、目录、网络连接、设备等，常用于排查文件占用、端口占用问题。

## 语法

```
lsof [选项]
```

## 用法

lsof 不加参数显示所有打开的文件，信息量极大。常用过滤：`-i :端口` 查看占用指定端口的进程；`-U` 查看 UNIX 域套接字；`-p 进程号` 查看指定进程打开的文件；`-c 命令名` 查看指定命令打开的文件；`+D 目录` 查看目录下被打开的文件；`-u 用户名` 查看指定用户的文件；`-n` 不解析主机名（加快速度）。`lsof -i :80` 是最常用的查端口占用方式。

## 常用参数

| 参数        | 说明             |
| ----------- | ---------------- |
| `-i :端口`  | 查看端口占用     |
| `-U`        | UNIX 域套接字    |
| `-p 进程号` | 指定进程         |
| `-c 命令`   | 指定命令         |
| `+D 目录`   | 目录下打开的文件 |
| `-u 用户`   | 指定用户         |
| `-n`        | 不解析主机名     |

## 示例

```bash
lsof -i :80                                     # 查看占用 80 端口的进程
lsof -i :22                                     # 查看占用 22 端口的进程
lsof -p 1234                                    # 查看 PID 1234 打开的文件
lsof -u USER                                    # 查看指定用户的打开文件
lsof +D /HOME/USER                              # 查看目录下哪些文件被打开
lsof -c ssh                                     # 查看 ssh 命令打开的文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-lsof.html)

## 🔗 相关文档

[网络管理-ss](网络管理-ss.md) | [网络管理-netstat](网络管理-netstat.md) | [网络管理-tcpdump](网络管理-tcpdump.md) | [网络管理-ssh](网络管理-ssh.md)
