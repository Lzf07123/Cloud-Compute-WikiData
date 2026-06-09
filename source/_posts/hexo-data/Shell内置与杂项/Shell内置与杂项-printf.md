---
title: Shell内置与杂项-printf
description: printf
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-printf` 🛠️ — 格式化输出

## 作用

printf 根据格式化字符串输出文本，比 `echo` 功能更强。源自 C 语言的 `printf` 函数，支持格式说明符、宽度对齐、进制转换等，是 Shell 脚本中表格输出的首选工具。

## 语法

```
printf 格式 [参数...]
```

## 用法

printf 的格式字符串包含普通文本和格式说明符（以 `%` 开头）。常用说明符：`%s` 字符串、`%d` 整数、`%f` 浮点数、`%x` 十六进制。`\n` 换行、`\t` 制表符。宽度对齐：`%10s` 右对齐最小宽度 10，`%-10s` 左对齐。格式字符串会循环应用到所有参数。

## 常用参数

| 说明符 | 说明     |
| ------ | -------- |
| `%s`   | 字符串   |
| `%d`   | 整数     |
| `%f`   | 浮点数   |
| `%x`   | 十六进制 |
| `\n`   | 换行     |
| `\t`   | 制表符   |

## 示例

```bash
printf "HELLO %s\n" "WORLD"       # 格式化输出字符串
printf "%10s %-10s\n" "NAME" "VALUE"  # 右对齐和左对齐
printf "%d\n" 255                  # 输出十进制整数
printf "%x\n" 255                  # 输出十六进制
printf "%f\n" 3.14                 # 输出浮点数
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-printf.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-echo %} | {% post_link Shell内置与杂项/Shell内置与杂项-read %}
