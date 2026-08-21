---
title: 网络管理-curl
description: curl
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-curl` 🌐 — 数据传输工具

## 作用

curl（client URL）是一个功能强大的命令行数据传输工具，支持 HTTP/HTTPS/FTP/SFTP 等众多协议。用于发送网络请求、下载文件、测试 API 接口、查看响应头等，是开发者日常必备的网络工具。

## 语法

```
curl [选项] URL
```

## 用法

curl 默认将响应内容输出到终端。`-O` 保存文件为远程文件名；`-o 文件名` 指定保存文件名；`-L` 跟随重定向；`-I` 仅显示响应头；`-X 方法` 指定 HTTP 方法（GET/POST/PUT/DELETE）；`-H "头: 值"` 自定义请求头；`-d 数据` 发送 POST 数据（自动用 POST）；`-K 文件` 从文件读取配置；`-v` 显示详细信息（调试用）。

## 常用参数

| 参数        | 说明             |
| ----------- | ---------------- |
| `-O`        | 保存为远程文件名 |
| `-o 文件名` | 指定保存文件名   |
| `-L`        | 跟随重定向       |
| `-I`        | 仅显示响应头     |
| `-X 方法`   | 指定 HTTP 方法   |
| `-H "头"`   | 自定义请求头     |
| `-d 数据`   | POST 数据        |
| `-K 文件`   | 配置文件         |
| `-v`        | 详细输出         |

## 示例

```bash
curl HTTP://EXAMPLE.COM                       # GET 请求获取页面内容
curl -O HTTP://EXAMPLE.COM/FILE.ZIP           # 下载文件并保存
curl -I HTTP://EXAMPLE.COM                    # 仅查看响应头
curl -X POST -d "KEY=VALUE" HTTP://API.EXAMPLE.COM  # POST 请求发送数据
curl -H "AUTHORIZATION: BEARER TOKEN" HTTP://API.EXAMPLE.COM  # 自定义请求头
curl -L HTTP://TINYURL.COM/ABCD               # 跟随重定向
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-curl.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 1：网络诊断](../学习路线与课程/week4-day1-网络诊断.md) | [Day 5：本周串联实操](../学习路线与课程/week4-day5-本周串联实操.md)

## 🔗 相关文档

[网络管理-wget](网络管理-wget.md) | [网络管理-nc](网络管理-nc.md) | [网络管理-ssh](网络管理-ssh.md) | [网络管理-rsync](网络管理-rsync.md)
