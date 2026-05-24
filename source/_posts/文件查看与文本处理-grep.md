---
title: 文件查看与文本处理-grep
description: grep
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-grep` 📄 — 文本搜索

## 作用

grep（global regular expression print）在文件中按模式搜索文本行并输出匹配结果，支持正则表达式，是文本处理的必备工具。

## 语法

```
grep [选项] 模式 文件
```

## 用法

grep 逐行扫描文件，输出包含匹配模式的行。`-i` 忽略大小写，`-r` 递归搜索目录，`-C N` 显示匹配行前后各 N 行，`-L` 只输出匹配的文件名，`-v` 反向匹配（输出不包含模式的行），`-E` 扩展正则，`-w` 精确匹配单词，`-o` 只输出匹配部分。日志分析必备。

## 常用参数

| 参数   | 说明           |
| ------ | -------------- |
| `-i`   | 忽略大小写     |
| `-n`   | 显示行号       |
| `-v`   | 反向匹配       |
| `-C N` | 匹配行数       |
| `-r`   | 递归搜索       |
| `-A N` | 匹配后 N 行    |
| `-B N` | 匹配前 N 行    |
| `-C N` | 前后各 N 行    |
| `-L`   | 只输出文件名   |
| `-E`   | 扩展正则       |
| `-w`   | 精确匹配单词   |
| `-o`   | 只输出匹配部分 |

## 示例

```bash
grep "ERROR" APP.LOG              # 搜索包含 ERROR 的行
grep -n "ERROR" APP.LOG           # 显示匹配行及行号
grep -ri "TODO" SRC/              # 递归搜索源码中所有 TODO
grep -C 3 "PANIC" CRASH.LOG       # 显示匹配行及前后各 3 行
grep "^root" /etc/passwd          # 正则搜索以 root 开头的行
grep -E '^[0-9]+$' FILE           # 扩展正则，纯数字行
grep -L "TODO" *.py               # 只输出不含 TODO 的文件名
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-grep.html)
