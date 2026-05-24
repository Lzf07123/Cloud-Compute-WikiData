---
title: 网络管理-scp
description: scp
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-scp` 🌐 — 远程文件复制

## 作用

scp（secure copy）通过 SSH 协议在本地与远程主机之间安全地复制文件和目录。加密传输，操作简单，适合单次文件传输任务。

## 语法

```
scp [选项] 来源 目标
```

## 用法

scp 的传输方向由路径格式决定：`本地文件 用户@主机:远程路径` 上传到远程；`用户@主机:远程路径 本地目录` 从远程下载。`-r` 递归复制目录；`-P 端口` 指定 SSH 端口（大写 P）；`-C` 启用压缩；`-q` 静默模式；`-v` 显示详细调试信息；`-l` 限速；`-i 密钥文件` 指定私钥文件。支持 `-3` 通过本机中转两个远程主机间的传输。

## 常用参数

| 参数      | 说明             |
| --------- | ---------------- |
| `-r`      | 递归复制目录     |
| `-P 端口` | 指定 SSH 端口    |
| `-C`      | 启用压缩         |
| `-q`      | 静默模式         |
| `-v`      | 详细调试         |
| `-l 限速` | 限制带宽         |
| `-i 密钥` | 指定私钥         |
| `-3`      | 两个远程主机中转 |

## 示例

```bash
scp FILE.TXT USER@HOST:/HOME/USER/               # 上传文件到远程主机
scp -r DIR/ USER@HOST:/HOME/USER/                # 递归上传目录
scp USER@HOST:/HOME/USER/FILE.TXT ./             # 从远程下载文件
scp -P 2222 FILE.TXT USER@HOST:/HOME/USER/       # 指定端口 2222 传输
scp -C FILE.TXT USER@HOST:/HOME/USER/            # 启用压缩传输
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-scp.html)
