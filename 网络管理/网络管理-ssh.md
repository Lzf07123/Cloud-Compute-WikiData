---
title: 网络管理-ssh
description: ssh
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-ssh` 🌐 — 远程登录与命令执行

## 作用

ssh（secure shell）通过加密通道远程登录到另一台主机，或远程执行命令。是 Linux 系统管理中最常用的远程连接工具，默认使用 22 端口。

## 语法

```
ssh [选项] [用户@]主机 [命令]
```

## 用法

ssh 基本连接：`ssh USER@HOST` 登录远程主机。`-p 端口` 指定端口；`-i 密钥文件` 指定私钥认证；`-v` 调试模式（显示详细连接过程）；`-N` 仅建立连接不执行命令（常用于端口转发）；`-L 本地端口:目标:远程端口` 本地端口转发；`-R 远程端口:目标:本地端口` 远程端口转发；`-D 端口` 动态端口转发（SOCKS 代理）。在命令后直接跟远程命令，如 `ssh USER@HOST 'ls -la'`。

## 常用参数

| 参数                | 说明         |
| ------------------- | ------------ |
| `-p 端口`           | 指定端口     |
| `-i 密钥`           | 指定私钥文件 |
| `-v`                | 调试模式     |
| `-N`                | 不执行命令   |
| `-L 端口:目标:端口` | 本地端口转发 |
| `-R 端口:目标:端口` | 远程端口转发 |
| `-D 端口`           | 动态端口转发 |

## 示例

```bash
ssh USER@HOST                                    # 远程登录主机
ssh -p 2222 USER@HOST                            # 指定端口 2222 登录
ssh -i ~/.ssh/id_rsa USER@HOST                   # 使用指定私钥认证
ssh USER@HOST 'ls -la'                           # 远程执行命令
ssh -L 8080:LOCALHOST:80 USER@HOST               # 本地端口转发（本机 8080→远程 80）
ssh -D 1080 USER@HOST                            # 动态端口转发（SOCKS 代理）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-ssh.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 2：SSH 远程连接与文件传输](../学习路线与课程/week4-day2-SSH远程连接.md) | [Day 5：本周串联实操](../学习路线与课程/week4-day5-本周串联实操.md)

## 🔗 相关文档

[网络管理-scp](网络管理-scp.md) | [网络管理-rsync](网络管理-rsync.md) | [网络管理-curl](网络管理-curl.md) | [网络管理-lsof](网络管理-lsof.md)
