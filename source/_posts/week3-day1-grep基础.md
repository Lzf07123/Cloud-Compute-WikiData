---
title: "Day 1：grep 基础"
description: "用 grep 在文件中快速过滤内容——大小写、反向匹配、递归搜索、显示行号、只输出匹配部分"
week: 3
day: 1
category: "文本三剑客与打包"
commands: [grep]
updated: "2026-06-06"
---

# Day 1：grep 基础

> 今日目标：用 grep 在文件中快速过滤内容——大小写、反向匹配、递归搜索、显示行号、只输出匹配部分 🎯

## 命令速览

| 参数 | 用途 |
|------|------|
| `grep "pattern"` | 基本搜索 |
| `-i` | 忽略大小写 |
| `-v` | 反向匹配（排除匹配行） |
| `-r` | 递归搜索目录 |
| `-n` | 显示行号 |
| `-o` | 只输出匹配的部分（不输出整行） |

## 逐个击破

### ① grep 基本搜索

- **语法**：`grep [options] "pattern" <FILE>`
- 🖥️ **上手练**：
  1. `grep root /etc/passwd`——找出包含 root 的行
  2. `grep /bin/bash /etc/passwd`——找出使用 bash 的用户（注意 `/` 不是特殊字符）
  3. `grep "server" /etc/hosts /etc/hostname`——同时在多个文件中搜索

### ② -i 忽略大小写

- 🖥️ **上手练**：
  1. `echo -e "Error\nERROR\nerror\nInfo" > /tmp/grep-test.txt`
  2. `grep error /tmp/grep-test.txt`——只匹配小写
  3. `grep -i error /tmp/grep-test.txt`——全匹配，3 行都出来
- ⚠️ **常见坑**：Linux 下大小写敏感，日志里 `Error` 和 `ERROR` 是不同的，加 `-i` 统一匹配

### ③ -v 反向匹配

- 🖥️ **上手练**：
  1. `grep -v root /etc/passwd`——排除 root，看其他用户
  2. `grep -v "^#" /etc/ssh/sshd_config`——排除注释行（`^#` 表示以 `#` 开头）
  3. `grep -v "^#" /etc/ssh/sshd_config | grep -v "^$"`——同时排除空行（`^$` 是空行）
- ⚠️ **常见坑**：`-v` 排除后还要排除空行，否则输出里全是没有实际内容的空行

### ④ -r 递归搜索

- 🖥️ **上手练**：
  1. `grep -r "127.0.0.1" /etc/`——在 /etc 整个目录下搜索（可能需要 sudo 处理部分无权限文件）
  2. `grep -r "localhost" /etc/ 2>/dev/null`——忽略权限错误
  3. `grep -rl "localhost" /etc/ 2>/dev/null`——只显示包含匹配的文件名，不显示内容
- ⚠️ **常见坑**：递归搜索大目录（如 `/`）会很慢，先限制目录范围；`2>/dev/null` 屏蔽权限错误

### ⑤ -n 显示行号

- 🖥️ **上手练**：
  1. `grep -n root /etc/passwd`——每行前面带行号
  2. `grep -rn "127.0.0.1" /etc/ 2>/dev/null`——递归 + 行号，方便定位

### ⑥ -o 只输出匹配部分

- 🖥️ **上手练**：
  1. `echo "IP=192.168.1.1 PORT=8080" | grep -o "[0-9.]*"`——只提取 IP 地址
  2. `grep -o "root" /etc/passwd`——每行 root 出现几次就输出几次
  3. `grep -o "[a-zA-Z]*" /tmp/grep-test.txt | sort | uniq -c | sort -rn`——统计词频

## 💪 今日必刷（全部终端实操）

1. 在 `/etc/passwd` 中查找所有使用 `/sbin/nologin` 的用户行，显示行号
2. 统计 `/etc/ssh/sshd_config` 中有效配置行（排除注释和空行）的数量
3. 递归搜索 `/etc/` 下所有包含 `root` 的文件（只显示文件名），忽略权限错误
4. 从 `ps aux` 的输出中提取所有以 `/usr` 开头的进程路径（用 `grep -o`）
5. **排错题**：`grep error /var/log/syslog` 只匹配到小写 error，但日志里还有 `ERROR` 和 `Error`——修正命令
6. **排错题**：`grep -r "password" /etc/` 报了大量 `Permission denied`，想忽略这些权限错误怎么办？
