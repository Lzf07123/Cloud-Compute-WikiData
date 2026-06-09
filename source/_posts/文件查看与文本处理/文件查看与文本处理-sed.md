---
title: 文件查看与文本处理-sed
description: sed
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-sed` 📄 — 流式编辑器

## 作用

sed（stream editor）按行读取文本并执行编辑操作（替换、删除、插入、打印等），支持正则表达式和原地修改，是文本批处理的瑞士军刀。

## 语法

```
sed [选项] '脚本' 文件
```

## 用法

sed 逐行处理文本，不修改原文件默认输出到标准输出。`-i` 原地修改文件。核心指令：`s/旧/新/g` 全局替换，`/模式/d` 删除匹配行，`/模式/p` 打印匹配行，`N d` 删除第 N 行，`N,Mp` 打印 N 到 M 行。脚本中常结合管道使用。

## 常用参数

| 参数        | 说明                        |
| ----------- | --------------------------- |
| `-i`        | 原地修改文件                |
| `-n`        | 关闭自动打印（配合 p 使用） |
| `-E`        | 扩展正则                    |
| `s/旧/新/g` | 全局替换                    |
| `/模式/d`   | 删除匹配行                  |
| `/模式/p`   | 打印匹配行                  |
| `N d`       | 删除第 N 行                 |
| `N,Mp`      | 打印 N 到 M 行              |

## 示例

```bash
sed 's/OLD/NEW/g' FILE.TXT         # 全局替换 OLD 为 NEW（输出到屏幕）
sed -i 's/OLD/NEW/g' FILE.TXT     # 原地修改文件
sed -n '/ERROR/p' LOG.TXT          # 只打印含 ERROR 的行
sed '5,10d' FILE.TXT               # 删除第 5 到 10 行
sed 's/^/PREFIX/' FILE.TXT         # 在每行开头添加 PREFIX
sed '/^$/d' FILE.TXT               # 删除所有空行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-sed.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week3-day3-sed流编辑 "Day 3：sed 流编辑" %}
