---
title: 文件查看与文本处理-less
description: less
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-less` 📄 — 分页查看文件

## 作用

less 是 more 的增强版分页查看器，支持上下翻页、双向搜索、跳转等操作，查看大文件时只加载可见部分，内存占用低。

## 语法

```
less [选项] 文件
```

## 用法

less 交互式操作：方向键/PageUp/PageDown 翻页，`/模式` 向下搜索，`?模式` 向上搜索，`G` 跳到底部，`g` 跳到顶部，`Q` 退出。`-N` 显示行号，`-S` 不折行（横向滚动），`-I` 搜索忽略大小写。支持同时打开多个文件用 `:N` `:P` 切换。

## 常用参数

| 参数      | 说明                     |
| --------- | ------------------------ |
| `-N`      | 显示行号                 |
| `-S`      | 不折行（横向滚动）       |
| `-I`      | 搜索忽略大小写           |
| `-G`      | 不高亮搜索结果           |
| `-M`      | 显示详细状态行           |
| `+F`      | 跟踪模式（类似 tail -F） |
| `-p 模式` | 从匹配行开始显示         |

## 示例

```bash
less FILE.TXT                    # 分页查看文件
less -N FILE.TXT                 # 显示行号
less -S FILE.TXT                 # 不折行，横向滚动
less +F /var/log/syslog          # 跟踪模式，实时查看日志
less -p "ERROR" LOG.TXT          # 从第一个 ERROR 处开始显示
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-less.html)

## 📖 课程位置

本命令在以下课程中讲解：[Day 3：文件内容查看](../学习路线与课程/week1-day3-文件内容查看.md) | [Day 5：本周串联实操](../学习路线与课程/week1-day5-本周串联实操.md)

## 🔗 相关文档

[文件查看与文本处理-more](文件查看与文本处理-more.md) | [文件查看与文本处理-head](文件查看与文本处理-head.md) | [文件查看与文本处理-tail](文件查看与文本处理-tail.md) | [文件查看与文本处理-cat](文件查看与文本处理-cat.md)
