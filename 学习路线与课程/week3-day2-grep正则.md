---
title: "Day 2：grep 正则与上下文"
description: "用扩展正则精确匹配复杂模式，用上下文参数看匹配行前后内容"
tags: [linux, 学习路线, grep, 正则表达式]
week: 3
day: 2
category: "文本三剑客与打包"
commands: [grep]
updated: "2026-06-06"
---

# Day 2：grep 正则与上下文

> 今日目标：用扩展正则精确匹配复杂模式，用上下文参数看匹配行前后内容 🎯

## 命令速览

| 参数 | 用途 |
|------|------|
| `-E` | 扩展正则（支持 `+` `?` `{}` `()` `|`） |
| `-A N` | 显示匹配行及后面 N 行（After） |
| `-B N` | 显示匹配行及前面 N 行（Before） |
| `-C N` | 显示匹配行及前后各 N 行（Context） |

## 正则速查

| 符号 | 含义 | 示例 |
|------|------|------|
| `^` | 行首 | `^Error` 以 Error 开头 |
| `$` | 行尾 | `;$` 以分号结尾 |
| `.` | 任意单个字符 | `h.t` 匹配 hat/hot/hit |
| `*` | 前一个字符出现 0 次或多次 | `ab*c` 匹配 ac/abc/abbc |
| `[]` | 字符集 | `[Ee]rror` 匹配 Error 或 error |
| `[^]` | 排除字符集 | `[^a-z]` 非小写字母 |
| `+` | 前一个字符出现 1 次或多次（需 -E） | `ab+c` → abc/abbc |
| `?` | 前一个字符出现 0 次或 1 次（需 -E） | `colou?r` → color/colour |
| `{n,m}` | 前一个字符出现 n~m 次（需 -E） | `[0-9]{2,4}` 2-4 位数字 |
| `()` | 分组（需 -E） | `(error|warn)` error 或 warn |
| `|` | 或（需 -E） | `ERROR|WARN` ERROR 或 WARN |

## 逐个击破

### ① 扩展正则 — grep -E

先准备测试数据：
```bash
cat > /tmp/grep-re.txt <<EOF
Error: connection refused
ERROR: timeout after 30s
WARN: retry attempt 1
error: disk full
INFO: all systems operational
DEBUG: processing request 12345
user@host connected from 192.168.1.100
2024-01-15 08:30:22 app[1234]: started
colour
color
abc
abbc
abbbc
EOF
```

🖥️ **上手练**：

1. `grep -E "^(Error|ERROR)" /tmp/grep-re.txt`——匹配 Error 或 ERROR 开头的行
2. `grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" /tmp/grep-re.txt`——提取 IP 地址
3. `grep "colou*r" /tmp/grep-re.txt`——基础正则可匹配 colour（`u*` = u 出现 0 次或多次）
4. `grep -E "colou?r" /tmp/grep-re.txt`——扩展正则匹配 color/colour（`u?` = u 出现 0/1 次）
5. `grep -E "ab+c" /tmp/grep-re.txt`——匹配 abc/abbc/abbbc（b 至少 1 次）
6. `grep -E "ab{2}c" /tmp/grep-re.txt`——只匹配 abbc（b 恰好 2 次）
7. `grep -E "(error|warn)" /tmp/grep-re.txt -i`——匹配 error 或 warn（`-i` 忽略大小写）

### ② 上下文参数 — -A / -B / -C

🖥️ **上手练**：

1. `grep -A3 "ERROR" /tmp/grep-re.txt`——显示 ERROR 行及其后面 3 行
2. `grep -B2 "started" /tmp/grep-re.txt`——显示 started 行及其前面 2 行
3. `grep -C2 "timeout" /tmp/grep-re.txt`——显示 timeout 行及其前后各 2 行
4. 实战：查看某条日志前后发生了什么：
   ```bash
   grep -C5 "error" /var/log/syslog 2>/dev/null | head -30
   ```

### ③ 正则组合实战

1. 匹配所有以 `192.168` 开头的 IP：`grep -E "192\.168\.[0-9]{1,3}\.[0-9]{1,3}"`
2. 匹配邮箱地址：`grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"`
3. 匹配时间戳格式 `HH:MM:SS`：`grep -E "[0-9]{2}:[0-9]{2}:[0-9]{2}"`
4. 排除空行和注释行：`grep -vE "^(#|$)" file`

## 💪 今日必刷（全部终端实操）

1. 用 `grep -E` 从 `/tmp/grep-re.txt` 中提取所有 IP 地址（只输出匹配的 IP，不输出整行）
2. 统计 `/tmp/grep-re.txt` 中以 `E` 或 `e` 开头的行数（忽略大小写）
3. 查看 `/etc/ssh/sshd_config` 中包含 `Port` 的行及其前后各 1 行
4. 排除 `/etc/ssh/sshd_config` 中的注释行和空行，只显示有效配置行数
5. **排错题**：`grep "error|warn" /tmp/grep-re.txt` 什么也匹配不到——为什么？修正这条命令
6. **排错题**：你想查看 nginx 日志中某个 IP `10.0.0.5` 的访问记录前后 5 行上下文，于是写了 `grep -C5 "10.0.0.5" /var/log/nginx/access.log`，结果也匹配到了 `10.0.0.50`、`10.0.0.51`——如何精确匹配只匹配 `10.0.0.5` 这个 IP？（提示：IP 在行中的位置特点）

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `grep` | [文件查看与文本处理-grep](../文件查看与文本处理/文件查看与文本处理-grep.md) |
