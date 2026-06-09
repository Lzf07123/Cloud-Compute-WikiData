---
title: dig-nslookup-host
description: dig / nslookup / host
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-dig-nslookup-host` / `nslookup` / `host` 🌐 — DNS 查询

## 作用

dig、nslookup、host 是三个常用的 DNS 查询工具，用于解析域名、查看 DNS 记录、排查域名解析问题。dig 功能最强；nslookup 简单易用；host 输出最简洁。

## dig

```
dig [选项] 域名 [记录类型]
```

DNS 查询工具，默认查询 A 记录。`+short` 简洁输出；`@DNS服务器` 指定 DNS 服务器（如 `@8.8.8.8`）；`-x IP` 反向查询 PTR 记录；`MX`、`TXT`、`CNAME` 等指定记录类型。支持批量查询和 `+trace` 跟踪解析路径。

## nslookup

```
nslookup [域名/IP]
```

交互式或单次 DNS 查询。支持正向（域名→IP）和反向（IP→域名）查询。不带参数进入交互模式，输入 `exit` 退出。

## host

```
host [选项] 域名
```

DNS 查找工具，输出比 dig 更简洁。`-t 类型` 指定记录类型（如 `-t MX`）；`-a` 显示所有记录；`-v` 详细输出。

## 示例

```bash
dig EXAMPLE.COM                 # 查询域名的 A 记录
dig +short EXAMPLE.COM          # 简洁输出（仅显示 IP）
dig @8.8.8.8 EXAMPLE.COM MX     # 指定 DNS 服务器查询 MX 记录
dig -x 8.8.8.8                  # 反向查询 PTR 记录
nslookup EXAMPLE.COM            # 查询域名对应的 IP
host EXAMPLE.COM                # 简洁 DNS 查找
host -t MX EXAMPLE.COM          # 查询 MX 邮件交换记录
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-dig.html)
