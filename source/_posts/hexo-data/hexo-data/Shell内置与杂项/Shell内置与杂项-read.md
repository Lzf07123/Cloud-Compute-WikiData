---
title: Shell内置与杂项-read
description: read
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-read` 🛠️ — 从标准输入读取

## 作用

read 从标准输入（或文件描述符）读取一行内容并赋值给变量，是 Shell 脚本中获取用户输入和解析文本文件的核心命令。

## 语法

```
read [选项] 变量名...
```

## 用法

read 将输入行按 IFS（Internal Field Separator）分割后赋值给变量，多余的合并到最后一个变量。`-p "提示"` 显示提示信息；`-s` 静默模式（不回显，适合密码输入）；`-t 秒` 设置超时；`-n 字符数` 精确读取 N 个字符后返回；`-a 数组名` 读取到数组；`-d 分隔符` 自定义分隔符；`-r` 原始模式（不处理转义字符）。

## 常用参数

| 参数        | 说明         |
| ----------- | ------------ |
| `-p "提示"` | 显示提示信息 |
| `-s`        | 静默模式     |
| `-t 秒`     | 超时时间     |
| `-n 数`     | 读取字符数   |
| `-a 数组`   | 读取到数组   |
| `-d 分隔符` | 自定义分隔符 |
| `-r`        | 原始模式     |

## 示例

```bash
read -p "ENTER NAME: " NAME         # 带提示读取输入
read -s -p "PASSWORD: " PASS       # 静默模式读取密码
read -t 5 -p "INPUT (5S): " DATA   # 设置 5 秒超时
read -a ARR <<< "A B C"            # 读取到数组
read -r LINE                       # 原始模式读取（不处理转义）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-read.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-echo %} | {% post_link Shell内置与杂项/Shell内置与杂项-test %} | {% post_link Shell内置与杂项/Shell内置与杂项-source %}
