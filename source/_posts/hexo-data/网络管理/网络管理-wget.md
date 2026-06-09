---
title: 网络管理-wget
description: wget
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-wget` 🌐 — 网络文件下载

## 作用

wget 是一个非交互式网络下载工具，支持 HTTP/HTTPS/FTP 协议，可在后台运行、断点续传、递归下载。适合脚本自动化下载、批量文件获取、镜像站点等场景。

## 语法

```
wget [选项] URL
```

## 用法

wget 默认将文件下载到当前目录。`-O 文件名` 指定保存文件名；`-P 目录` 指定保存目录；`-c` 断点续传；`-b` 后台下载；`-q` 静默模式（不输出日志）；`--limit-rate=速率` 限制下载速度；`-r` 递归下载（谨慎使用）；`-l 层级` 递归下载深度；`--no-parent` 不超过上级目录；`--tries=次数` 设置重试次数。支持通配符下载多个文件。注意：小写 `-o` 用于日志输出，大写 `-O` 用于指定保存文件名。

## 常用参数

| 参数                | 说明           |
| ------------------- | -------------- |
| `-O 文件名`         | 指定保存文件名 |
| `-P 目录`           | 指定保存目录   |
| `-c`                | 断点续传       |
| `-b`                | 后台下载       |
| `-q`                | 静默模式       |
| `--limit-rate=速率` | 限制速度       |
| `--tries=次数`      | 重试次数       |
| `-r`                | 递归下载       |

## 示例

```bash
wget HTTP://EXAMPLE.COM/FILE.ZIP                  # 下载文件到当前目录
wget -O OUTPUT.ZIP HTTP://EXAMPLE.COM/FILE.ZIP    # 另存为指定文件名
wget -c HTTP://EXAMPLE.COM/FILE.ZIP               # 断点续传
wget -b -q HTTP://EXAMPLE.COM/FILE.ZIP            # 后台静默下载
wget --limit-rate=1M HTTP://EXAMPLE.COM/FILE.ZIP  # 限速 1MB/s 下载
wget -P /tmp HTTP://EXAMPLE.COM/PKG.DEB           # 下载到指定目录
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-wget.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week4-day1-网络诊断 "Day 1：网络诊断" %} | {% post_link 学习路线与课程/week4-day5-本周串联实操 "Day 5：本周串联实操" %}
