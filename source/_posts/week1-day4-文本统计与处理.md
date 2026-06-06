---
title: "Day 4：文本统计与处理"
description: "统计行数/单词数、排序、去重——用管道串起来处理文本数据"
week: 1
day: 4
category: "文件系统与文本基础"
commands: [wc, sort, uniq]
updated: "2026-06-06"
---

# Day 4：文本统计与处理

> 今日目标：统计行数/单词数、排序、去重——用管道串起来处理文本数据 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|------|
| `wc` | 统计行数/单词数/字节数 | `-l` `-w` `-c` |
| `sort` | 排序 | `-r` (逆序) / `-n` (按数值) |
| `uniq` | 去重/统计重复次数 | `-c` (计数) |

## 逐个击破

### ① wc — 字数统计

- **语法**：`wc [OPTIONS] <FILE>`
- **参数**：
  - `-l` → 只统计行数
  - `-w` → 只统计单词数
  - `-c` → 只统计字节数
- 🖥️ **上手练**：
  1. `wc /etc/passwd`——观察三个数字分别是什么（行/词/字节）
  2. `wc -l /etc/passwd`——系统有多少个用户账号？
  3. `ls -l /etc | wc -l`——/etc 下有多少文件？（注意：macOS 的 `/etc` 是软链接，`ls -l /etc` 只显示链接本身，须用 `ls -l /etc/` 列出目录内容。两种系统 `ls -l` 列目录时首行都有 `total N` 汇总行，结果会比实际文件数多 1，修正方式：`ls -l /etc/ | tail -n +2 | wc -l`）
- ⚠️ **常见坑**：`wc -l` 统计的是换行符个数，最后一行如果没有换行符不会被计入

### ② sort — 排序

- **语法**：`sort [OPTIONS] <FILE>`
- **参数**：
  - `-r` → 逆序（从大到小）
  - `-n` → 按数值排序（默认按字典序，`10 < 2` 字典序成立但数值序不成立）
  - `-t` + `-k` → 指定分隔符和排序列（后续 `awk` 课程会用到）
- 🖥️ **上手练**：
  1. `sort /etc/passwd`——默认按字典序排列所有行
  2. `sort -r /etc/passwd`——逆序排列
  3. `printf "2\n10\n1\n" | sort` vs `printf "2\n10\n1\n" | sort -n`——观察字典序和数值序的区别
  4. `ls -l /etc | sort -k5 -n | head`——按文件大小排序列出 /etc 下文件（macOS 用户注意：须用 `ls -l /etc/` 代替 `ls -l /etc`，因为 macOS 的 `/etc` 是软链接）
- ⚠️ **常见坑**：不加 `-n` 时 `sort` 按字典序排列数字，`10` 排在 `2` 前面

### ③ uniq — 去重

- **语法**：`uniq [OPTIONS] <FILE>`
- **参数**：`-c` → 在每行前显示重复次数
- 🖥️ **上手练**：
  1. `printf "a\na\nb\nb\nb\nc\n" | uniq`——相邻重复行被合并
  2. `printf "a\na\nb\nb\nb\nc\n" | uniq -c`——显示每行出现次数
  3. `printf "a\nb\na\n" | uniq`——为什么两个 `a` 没被合并？（不相邻！）
  4. `printf "a\nb\na\n" | sort | uniq -c`——先排序再去重，正确统计
- ⚠️ **致命坑**：`uniq` 只合并**相邻**重复行，去重前通常要 `sort | uniq` 或 `sort -u`

## 💪 今日必刷（全部终端实操）

1. 统计 `/etc/passwd` 有多少行，然后只输出这个行数（不显示文件名）
2. 将 `/etc/passwd` 按逆序排列，取前 5 行
3. 统计 `/etc/passwd` 中每种 shell 各被多少人使用（提示：`awk -F: '{print $NF}' /etc/passwd | sort | uniq -c | sort -rn`）
4. 用 `ls -l /etc | sort -k5 -rn | head -5` 找出 /etc 下最大的 5 个文件（macOS 用户须改用 `ls -l /etc/` 加尾部斜杠，因为 macOS 的 `/etc` 本身是软链接）
5. **排错题**：执行 `cat /tmp/data.txt | uniq` 后发现大量重复行还在——为什么？正确的去重方式是什么？
6. **排错题**：`sort -n /tmp/scores.txt` 排序后 `2` 排在 `10` 前面正确，但 `100` 排在 `20` 前面——哪里错了？排查思路：先确认文件内容格式，再确认参数
