---
title: 系统管理-hostname
description: hostname
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-hostname` 🖥️ — 显示或设置主机名

## 作用

hostname 查看或设置系统主机名，root 可修改。

## 语法

```
hostname [选项] [新主机名]
```

## 用法

hostname 无参数显示当前主机名。`-I` 显示主机关联的 IP 地址，`-F 文件` 从文件读取主机名，`-d` 显示 DNS 域名。修改主机名需 root，建议同时修改 /etc/hostname 持久化。

## 常用参数

| 参数      | 说明             |
| --------- | ---------------- |
| `-I`      | 显示所有 IP 地址 |
| `-F 文件` | 从文件读取主机名 |
| `-d`      | 显示 DNS 域名    |
| `-s`      | 短格式主机名     |

## 示例

```bash
hostname                     # 查看当前主机名
hostname -I                  # 查看本机 IP 地址
hostname NEW-HOST            # 临时设置主机名
hostname -F /etc/hostname    # 从文件读取主机名
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-hostname.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-uname %} | {% post_link 系统管理/系统管理-locale %}
