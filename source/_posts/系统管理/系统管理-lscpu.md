---
title: 系统管理-lscpu
description: lscpu
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-lscpu` 🖥️ — 显示 CPU 架构信息

## 作用

lscpu 从 sysfs、`/proc/cpuinfo` 等系统文件中读取并汇总 CPU 架构信息，包括 CPU 核心数、线程数、型号、频率、缓存大小、NUMA 节点等。是快速了解服务器 CPU 配置的便捷命令。

## 语法

```
lscpu [选项]
```

## 用法

lscpu 默认以表格形式输出 CPU 概要信息。`-e` 以扩展格式表格输出；`-p` 以可解析格式输出（CSV），方便脚本解析；`-b` 仅显示在线 CPU；`-c` 显示 CPU 缓存信息；`-y` 按 CPU 缓存层次结构显示。不带参数直接执行即可获得 CPU 信息总览。

## 常用参数

| 参数 | 说明                  |
| ---- | --------------------- |
| `-e` | 扩展格式表格输出      |
| `-p` | 可解析格式输出（CSV） |
| `-b` | 仅在线 CPU            |
| `-c` | 显示 CPU 缓存信息     |
| `-y` | 按缓存层级显示        |
| `-V` | 显示版本信息          |

## 示例

```bash
lscpu                           # 显示 CPU 概要信息
lscpu -e                        # 扩展格式表格输出
lscpu -p                        # 可解析格式输出（CSV）
lscpu -b                        # 仅显示在线 CPU
lscpu -y                        # 按缓存层级显示
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-lscpu.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-uname %} | {% post_link 系统管理/系统管理-lspci %} | {% post_link 系统管理/系统管理-lsusb %} | {% post_link 系统管理/系统管理-free %}
