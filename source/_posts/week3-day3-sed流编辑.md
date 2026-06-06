---
title: "Day 3：sed 流编辑"
description: "用 sed 做替换、删除行、提取行——不改文件时预览，确认无误后直接修改"
week: 3
day: 3
category: "文本三剑客与打包"
commands: [sed]
updated: "2026-06-06"
---

# Day 3：sed 流编辑

> 今日目标：用 sed 做替换、删除行、提取行——不改文件时预览，确认无误后直接修改 🎯

## 命令速览

| 操作 | 语法 | 用途 |
|------|------|------|
| 替换 | `sed 's/old/new/g'` | 全局替换 |
| 就地修改 | `sed -i 's/old/new/g'` | 直接修改文件（危险但常用） |
| 删除行 | `sed '3,5d'` | 删除第 3~5 行 |
| 打印行 | `sed -n '10p'` | 只打印第 10 行 |
| 行范围操作 | `sed '/start/,/end/d'` | 删除从 start 到 end 的行块 |

## 逐个击破

### ① 替换 — s/old/new/g

- **语法**：`sed 's/查找/替换/标志' <FILE>`
- **标志**：`g`→全局替换（每行所有匹配），不加 `g` 只替换每行第一个匹配
- 🖥️ **上手练**（先准备数据）：
  ```bash
  cat > /tmp/sed-test.txt <<EOF
  hello world
  hello linux
  hello world hello again
  goodbye world
  EOF
  ```
  1. `sed 's/hello/hi/' /tmp/sed-test.txt`——只替换每行第一个 hello
  2. `sed 's/hello/hi/g' /tmp/sed-test.txt`——全局替换所有 hello
  3. `sed 's/world/earth/' /tmp/sed-test.txt`——替换 world
  4. `sed 's/hello/hi/g; s/world/earth/g' /tmp/sed-test.txt`——同时做多个替换（分号分隔）
- ⚠️ **常见坑**：sed 默认输出到终端，**不会修改文件**，加 `-i` 才直接修改

### ② 就地修改 — sed -i

- **语法**：`sed -i 's/old/new/g' <FILE>`
- 🖥️ **上手练**：
  1. `cp /tmp/sed-test.txt /tmp/sed-test.bak`——先备份
  2. `sed -i 's/hello/hi/g' /tmp/sed-test.txt && cat /tmp/sed-test.txt`——直接修改了
  3. 恢复：`cp /tmp/sed-test.bak /tmp/sed-test.txt`
  4. `sed -i.bak 's/hello/hi/' /tmp/sed-test.txt`——`-i.bak` 生成 .bak 备份文件
- ⚠️ **致命坑**：`sed -i` 不可逆！没有 git 时先备份

### ③ 按行删除 — d

- **语法**：`sed '<N或RANGE>d' <FILE>`
- 🖥️ **上手练**（创建测试数据）：
  ```bash
  seq 1 10 > /tmp/sed-num.txt
  ```
  1. `sed '3d' /tmp/sed-num.txt`——删除第 3 行
  2. `sed '3,5d' /tmp/sed-num.txt`——删除第 3~5 行
  3. `sed '1d' /tmp/sed-num.txt`——删除第 1 行
  4. `sed '/^5/,/^8/d' /tmp/sed-num.txt`——删除从 "5" 开头到 "8" 开头的行块
  5. `sed '/^#/d' /etc/ssh/sshd_config`——删除所有注释行

### ④ 打印指定行 — p

- **语法**：`sed -n '<N>p' <FILE>`（`-n` 抑制默认输出，配合 `p` 只打印指定行）
- 🖥️ **上手练**：
  1. `sed -n '1p' /tmp/sed-num.txt`——打印第 1 行
  2. `sed -n '5,8p' /tmp/sed-num.txt`——打印第 5~8 行
  3. `sed -n '$p' /tmp/sed-num.txt`——打印最后一行（`$` 在 sed 中表示最后一行）

### ⑤ 行范围操作

- 🖥️ **上手练**：
  1. `sed -n '/^2/,/^6/p' /tmp/sed-num.txt`——打印从 "2" 开头到 "6" 开头的行范围
  2. `sed '/^3/,/^7/s/.*/REPLACED/' /tmp/sed-num.txt`——在 3~7 行范围内做替换
  3. 实战：删除 C 风格注释块 `/* ... */`（略）

## 💪 今日必刷（全部终端实操）

1. 将 `/etc/ssh/sshd_config` 中所有 `yes` 替换为 `no`（先不要用 `-i`，只输出看效果）
2. 用 sed 删除 `/etc/ssh/sshd_config` 的所有注释行和空行，输出有效配置（管道也行）
3. 打印 `/etc/passwd` 的第 1 行和第 10 行（用 sed 实现，不用 head+tail）
4. 用 sed 批量替换——将 `/tmp/sed-test.txt` 中 `hello` 改为 `hi`、`world` 改为 `earth`（一条命令）
5. 实际修改：用 `sed -i` 将 `/tmp/sed-num.txt` 中的奇数行注释掉（每行前加 `#`）
6. **排错题**：执行 `sed -i 's/a/b/' file` 报 `invalid command code`——可能原因是什么？如何排查？
7. **排错题**：执行 `sed 's/\//-/g' /tmp/path.txt` 发现把所有 `/` 都替换了，但你只想替换行首的 `/`——如何限制替换范围？
