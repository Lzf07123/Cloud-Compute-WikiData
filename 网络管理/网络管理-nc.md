---
title: 网络管理-nc
description: nc
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-nc` 🌐 — 网络工具（瑞士军刀）

## 作用

nc（netcat）是一个功能强大的网络调试工具，能读写 TCP 或 UDP 网络连接，被誉为网络的"瑞士军刀"。可用于端口扫描、文件传输、端口监听、聊天、代理等。

## 语法

```
nc [选项] [主机] [端口]
```

## 用法

nc 有两种模式：客户端模式 `nc HOST PORT` 连接到远程主机端口；监听模式 `nc -l -p 端口` 在本地监听端口等待连接。`-v` 显示详细信息；`-z` 仅扫描不发送数据（结合 `-v` 可做端口扫描）；`-n` 不做 DNS 解析；`-w 秒` 超时时间。可传输文件：接收端 `nc -l -p 1234 > FILE`，发送端 `nc HOST 1234 < FILE`。`-u` 使用 UDP 协议；`-U` 连接 UNIX 域套接字。

## 常用参数

| 参数      | 说明             |
| --------- | ---------------- |
| `-l`      | 监听模式         |
| `-p 端口` | 指定端口         |
| `-v`      | 详细输出         |
| `-z`      | 零模式（仅扫描） |
| `-n`      | 不解析 DNS       |
| `-w 秒`   | 超时时间         |
| `-u`      | UDP 模式         |
| `-U`      | UNIX 域套接字    |

## 示例

```bash
nc -v -z HOST 1-1000                            # 扫描 1-1000 端口
nc -l -p 1234                                   # 监听本地 1234 端口
nc -l -p 1234 > RECEIVED_FILE                    # 接收端：监听并保存文件
nc HOST 1234 < FILE_TO_SEND                      # 发送端：传输文件到远程
echo "HELLO" | nc HOST 8080                      # 发送 HTTP 请求
nc -U /var/run/docker.sock                       # 连接 UNIX 域套接字
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-nc.html)

## 🔗 相关文档

[网络管理-nmap](网络管理-nmap.md) | [网络管理-ping](网络管理-ping.md) | [网络管理-tcpdump](网络管理-tcpdump.md) | [网络管理-curl](网络管理-curl.md)
