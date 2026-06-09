---
title: Shell内置与杂项-echo
description: echo
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-echo` 🛠️ — 输出文本

## 作用

echo 将指定文本输出到标准输出，是 Shell 脚本中最常用的输出命令。支持转义字符和变量替换，用于打印信息、调试脚本、生成文件内容等。

## 语法

```
echo [选项] [字符串...]
```

## 用法

echo 默认在输出末尾添加换行符。`-n` 不输出换行；`-e` 启用转义字符（如 `\n` 换行、`\t` 制表符、`\\` 反斜杠）。转义字符需用 `-e` 选项或在字符串外使用 `$'...'` 语法。变量替换直接用 `$VAR`。多个字符串自动用空格连接。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-n` | 不输出换行   |
| `-e` | 启用转义字符 |

## 示例

```bash
echo "HELLO WORLD"                  # 输出文本
echo -n "NO NEWLINE"               # 输出不换行
echo -e "LINE1\nLINE2"            # 启用转义（\n 换行）
echo "USER: $USER"                  # 输出变量值
echo -e "COL1\tCOL2\tCOL3"       # 制表符对齐输出
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-echo.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-printf %} | {% post_link Shell内置与杂项/Shell内置与杂项-read %} | {% post_link Shell内置与杂项/Shell内置与杂项-test %}
