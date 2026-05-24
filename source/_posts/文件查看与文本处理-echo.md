---
title: 文件查看与文本处理-echo
description: echo
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-echo` 📄 — 输出文本

## 作用

echo 输出一行文本到标准输出，支持变量展开和命令替换，是 Shell 脚本中最基础的输出命令。

## 语法

```
echo [选项] 字符串
```

## 用法

echo 自动在输出末尾添加换行符。`-n` 不输出换行，`-e` 启用转义字符（`\\n` 换行、`\\t` 制表符）。支持变量展开 `$VAR`、命令替换 `$(CMD)`、花括号展开 `{1..5}`。多数系统中 `echo` 是 Shell 内置命令。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-n` | 不输出换行   |
| `-e` | 启用转义字符 |

## 示例

```bash
echo HELLO                       # 输出文本
echo "HELLO, $USER"              # 输出变量内容
echo -n "NO NEWLINE"             # 不输出换行
echo -e "LINE1\nLINE2"          # 转义字符换行
echo {1..5}                      # 花括号展开输出 1 2 3 4 5
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-echo.html)
