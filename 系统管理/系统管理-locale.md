---
title: 系统管理-locale
description: locale
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-locale` 🖥️ — 查看系统语言环境

## 作用

locale 查看和设置系统的语言环境（locale）信息，包括语言编码、字符集、日期格式、货币符号等。影响终端显示和程序的语言行为。

## 语法

```
locale [选项]
```

## 用法

locale 不加参数列出所有 locale 环境变量的当前值。`-a` 列出系统支持的所有可用 locale；`-m 图表` 显示字符映射表；`-k 变量名` 显示指定变量的值；`-c 类别` 显示指定类别的信息。常见 locale 如 `zh_CN.UTF-8`（中文）、`en_US.UTF-8`（美式英文）。通过 `localectl list-locales` 管理可用 locale。

## 常用参数

| 参数        | 说明                |
| ----------- | ------------------- |
| `-a`        | 列出所有可用 locale |
| `-m 图表`   | 显示字符映射表      |
| `-k 变量名` | 显示指定变量        |
| `-c 类别`   | 显示指定类别        |

## 示例

```bash
locale                          # 查看所有 locale 变量
locale -a                       # 列出系统支持的所有 locale
locale -k LANG                  # 查看 LANG 变量的值
locale -c LC_TIME               # 查看时间格式类别
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-locale.html)

## 🔗 相关文档

[系统管理-hostname](系统管理-hostname.md) | [系统管理-lscpu](系统管理-lscpu.md)
