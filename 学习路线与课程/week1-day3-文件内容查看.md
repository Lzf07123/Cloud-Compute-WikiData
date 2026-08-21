---
title: "Day 3：文件内容查看"
description: "不用编辑器也能快速查看文件——全貌、头部、尾部、实时追踪、分页浏览"
tags: [linux, 学习路线, 文件查看, cat, head, tail]
week: 1
day: 3
category: "文件系统与文本基础"
commands: [cat, head, tail, less]
updated: "2026-06-06"
---

# Day 3：文件内容查看

> 今日目标：不用编辑器也能快速查看文件——全貌、头部、尾部、实时追踪、分页浏览 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|------|
| `cat` | 输出文件全部内容 | `-n` 显示行号 |
| `head` | 查看文件开头 N 行 | `-n N` |
| `tail` | 查看文件末尾 N 行 / 实时追踪 | `-n N` / `-f` |
| `less` | 分页浏览大文件 | 空格/b//搜索/q |

## 逐个击破

### ① cat — 输出文件全部内容

- **语法**：`cat <FILE>` 或 `cat <FILE1> <FILE2>`（拼接多文件）
- **参数**：`-n` 给每一行加行号
- 🖥️ **上手练**：
  1. `cat /etc/hostname`——查看本机主机名
  2. `cat -n /etc/hosts`——带行号查看 hosts 文件
  3. `cat /etc/hosts /etc/hostname`——两个文件内容拼接输出
- ⚠️ **常见坑**：`cat` 大文件（如日志几百MB）会刷屏，应该用 `less` 分页查看

### ② head — 查看文件开头

- **语法**：`head -n <N> <FILE>`
- 🖥️ **上手练**：
  1. `head -n 5 /etc/passwd`——看系统账号文件前 5 行
  2. `head -n 3 /etc/hosts`——看 hosts 前 3 行
  3. 不用 `-n`：`head /etc/passwd`——默认输出前 10 行
- ⚠️ **常见坑**：参数是 `-n 5` 而不是 `-5`（旧写法可能兼容但不推荐）

### ③ tail — 查看文件末尾 / 实时追踪

- **语法**：`tail -n <N> <FILE>` / `tail -f <FILE>`
- **参数**：
  - `-n N` → 末尾 N 行
  - `-f` → 持续追踪文件新写入的内容（Ctrl+C 退出）
- 🖥️ **上手练**：
  1. `tail -n 5 /var/log/syslog`——查看最近 5 条日志
  2. `tail -f /var/log/syslog`，打开另一个终端做一些操作，观察该终端日志变化
  3. `tail -n 20 /etc/passwd`——查看 passwd 最后 20 行
- ⚠️ **常见坑**：`tail -f` 会阻塞终端，别忘了 Ctrl+C 退出

### ④ less — 分页浏览大文件

- **语法**：`less <FILE>`
- **操作键**（在 less 内）：
  - `空格` → 向下翻一页
  - `b` → 向上回翻一页
  - `/关键词` 回车 → 向下搜索，`n` 跳下一个匹配，`N` 跳上一个
  - `q` → 退出
- 🖥️ **上手练**：
  1. `less /var/log/syslog`，用空格翻页、`b` 回翻、`q` 退出
  2. 在 less 中搜索 `error`（输入 `/error` 回车），用 `n` 查看所有匹配
  3. `less /etc/services`——这是个大文件，体验分页浏览比 `cat` 好在哪里
- ⚠️ **常见坑**：在 less 里按 `q` 退出后终端内容消失（less 不污染终端），想保留内容用 `more` 或用重定向

## 💪 今日必刷（全部终端实操）

1. 查看 `/etc/passwd` 的第 1 行和第 50 行（提示：管道组合 `head` + `tail`）
2. 用 `less` 打开 `/var/log/syslog`，搜索 `error`，找到 3 个匹配项后退出
3. 用一条命令统计 `/etc/passwd` 的前 10 行（先 `head` 取前 10 行，再用 `wc -l` 确认是 10 行）
4. 先 `touch /tmp/watch.log` 创建文件（不创建的话 `tail -f` 会立即报错退出），然后打开两个终端窗口：终端A 执行 `tail -f /tmp/watch.log`；终端B 执行 `echo "hello" >> /tmp/watch.log`，观察终端A 的变化
5. **排错题**：执行 `cat /var/log/syslog` 终端疯狂刷屏看不清内容——如何优雅查看？
6. **排错题**：`tail -f /var/log/nginx/access.log` 报 `No such file or directory`——可能原因是什么？如何排查日志文件实际位置？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `cat` | [文件查看与文本处理-cat](../文件查看与文本处理/文件查看与文本处理-cat.md) |
| `head` | [文件查看与文本处理-head](../文件查看与文本处理/文件查看与文本处理-head.md) |
| `tail` | [文件查看与文本处理-tail](../文件查看与文本处理/文件查看与文本处理-tail.md) |
| `less` | [文件查看与文本处理-less](../文件查看与文本处理/文件查看与文本处理-less.md) |
