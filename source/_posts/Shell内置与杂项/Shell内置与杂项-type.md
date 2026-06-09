---
title: Shell内置与杂项-type
description: type
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-type` 🛠️ — 显示命令类型

## 作用

type 显示指定命令的类型（内置命令、外部程序、别名、函数或关键字），帮助理解 Shell 如何解析和执行命令。

## 语法

```
type [选项] 命令...
```

## 用法

type 比 `which` 提供更多信息。`-a` 显示所有匹配位置（别名、内置、PATH 中的外部程序）；`-p` 仅显示外部命令路径（类似 `which`）；`-t` 仅显示类型名称（简单文本输出）；`-W` 显示类型（别名、关键字、函数、内置、文件）。

## 常用参数

| 参数 | 说明           |
| ---- | -------------- |
| `-a` | 所有匹配位置   |
| `-p` | 仅外部命令路径 |
| `-t` | 仅类型名称     |
| `-W` | 显示类型       |

## 示例

```bash
type ls                             # 显示 ls 命令类型
type -a ls                          # 显示所有匹配位置
type -p ls                          # 仅显示外部命令路径
type -t cd                          # 仅显示类型名称
type -a echo                        # 显示 echo 所有匹配
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-type.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-command-exec %} | {% post_link Shell内置与杂项/Shell内置与杂项-which-whereis %} | {% post_link Shell内置与杂项/Shell内置与杂项-source %}
