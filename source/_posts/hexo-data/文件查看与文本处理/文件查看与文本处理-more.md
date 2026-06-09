---
title: 文件查看与文本处理-more
description: more
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-more` 📄 — 分页查看文件

## 作用

more 是早期的分页查看器，空格翻页、回车下滚一行、Q 退出，功能较 less 简单但足以满足基本查看需求。

## 语法

```
more [选项] 文件
```

## 用法

more 以百分比形式显示阅读进度。空格键向下翻一屏，回车键下滚一行，`B` 向上翻屏，`/模式` 搜索，`Q` 退出。`-d` 显示提示信息，`-s` 压缩空行，`+N` 从第 N 行开始显示。查看完自动退出（less 不自动退出）。

## 常用参数

| 参数     | 说明               |
| -------- | ------------------ |
| `-d`     | 显示提示信息       |
| `-s`     | 压缩连续空行为一行 |
| `+N`     | 从第 N 行开始      |
| `+/模式` | 从匹配模式处开始   |
| `-c`     | 翻页时清屏         |

## 示例

```bash
more FILE.TXT                  # 分页查看文件
more -d FILE.TXT               # 显示提示信息
more +10 FILE.TXT              # 从第 10 行开始显示
more +/"ERROR" LOG.TXT         # 从第一个 ERROR 处开始显示
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-more.html)
