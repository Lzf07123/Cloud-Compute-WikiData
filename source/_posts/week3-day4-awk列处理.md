---
title: "Day 4：awk 列处理"
description: "用 awk 按列处理数据——提取列、条件过滤、数值计算、统计汇总"
week: 3
day: 4
category: "文本三剑客与打包"
commands: [awk]
updated: "2026-06-06"
---

# Day 4：awk 列处理

> 今日目标：用 awk 按列处理数据——提取列、条件过滤、数值计算、统计汇总 🎯

## 命令速览

| 功能 | 语法 | 用途 |
|------|------|------|
| 提取列 | `awk '{print $1, $NF}'` | 打印第 1 列和最后一列 |
| 指定分隔符 | `awk -F:` | 以冒号分隔（默认空格/Tab） |
| 条件过滤 | `awk '$3 > 10'` | 第 3 列大于 10 才输出 |
| 求和 | `awk '{sum+=$1} END{print sum}'` | 累加第 1 列 |
| 行数 | `awk 'END{print NR}'` | 总行数 |

## 逐个击破

### ① 列提取 — $1 / $NF / $0

- **语法**：`awk '{print $N}'`，`$1`=第一列，`$2`=第二列，`$NF`=最后一列，`$0`=整行
- 🖥️ **上手练**：
  1. `echo "alice 25 engineer" | awk '{print $1}'`——打印第 1 列 alice
  2. `echo "alice 25 engineer" | awk '{print $NF}'`——打印最后一列 engineer
  3. `echo "alice 25 engineer" | awk '{print $1, $3}'`——打印第 1、3 列
  4. `awk '{print $1}' /etc/passwd`——提取所有用户名（默认 `:` 不是分隔符！所以打印整行）
  5. `awk -F: '{print $1}' /etc/passwd`——用 `:` 分隔，正确提取用户名

### ② 指定分隔符 — -F

- **语法**：`awk -F'<DELIM>'`
- 🖥️ **上手练**：
  1. `awk -F: '{print $1, $7}' /etc/passwd`——用户名和 shell
  2. `awk -F: '{print $1, $3}' /etc/passwd`——用户名和 UID
  3. `awk -F, '{print $2}' /tmp/data.csv`——CSV 文件的第二列
  4. `awk -F'[ :]' '{print $1, $NF}' /tmp/mixed.txt`——多个分隔符（空格或冒号）
  5. `echo "192.168.1.1 - - [10/Jan/2024:08:30:00]" | awk -F'[][]' '{print $2}'`——提取方括号内的时间戳

### ③ 条件过滤

- **语法**：`awk '条件 {动作}'`
- 🖥️ **上手练**：
  1. `awk -F: '$3 > 1000' /etc/passwd`——UID 大于 1000 的用户（通常是普通用户）
  2. `awk -F: '$3 == 0' /etc/passwd`——UID 为 0 的用户（root）
  3. `awk -F: '$7 == "/bin/bash"' /etc/passwd`——使用 bash 的用户
  4. `awk -F: '$1 ~ /^a/' /etc/passwd`——用户名以 a 开头的（`~` 是正则匹配）
  5. `awk -F: 'length($1) < 5' /etc/passwd`——用户名长度小于 5 的

### ④ 数值计算

- **语法**：`awk '{sum+=$1} END{print sum}'`
- 🖥️ **上手练**（创建测试数据）：
  ```bash
  cat > /tmp/awk-scores.txt <<EOF
  alice 85
  bob 92
  charlie 78
  diana 95
  eve 88
  EOF
  ```
  1. `awk '{sum+=$2} END{print sum}' /tmp/awk-scores.txt`——总分
  2. `awk '{sum+=$2} END{print sum/NR}' /tmp/awk-scores.txt`——平均分
  3. `awk '$2 > 85' /tmp/awk-scores.txt`——成绩大于 85 的人
  4. `awk '{if($2>90) print $1,"excellent"; else if($2>80) print $1,"good"; else print $1,"pass"}' /tmp/awk-scores.txt`——评级
  5. `awk 'END{print NR}' /tmp/awk-scores.txt`——总行数（等价于 `wc -l`）

### ⑤ BEGIN/END 块

- **语法**：`awk 'BEGIN{初始化} {每行动作} END{汇总输出}'`
- 🖥️ **上手练**：
  1. `awk -F: 'BEGIN{print "用户名\tUID"} {print $1"\t"$3} END{print "总共 " NR " 个用户"}' /etc/passwd`——带表头和汇总
  2. `awk 'BEGIN{max=0} {if($2>max) max=$2} END{print "最高分:", max}' /tmp/awk-scores.txt`——找最大值

## 💪 今日必刷（全部终端实操）

1. 从 `/etc/passwd` 中提取所有用户名和对应的 shell，用 `:` 隔开输出（如 `root:/bin/bash`）
2. 统计 `/etc/passwd` 中 UID 大于等于 1000 的用户数量（提示：用累加计数器变量，END 输出计数值；注意 END 块的 NR 永远是总行数而非过滤后行数）
3. 计算 `/tmp/awk-scores.txt` 中所有人的平均分、最高分、最低分（一条 awk 命令）
4. 模拟 nginx 日志分析：统计每个 HTTP 状态码的出现次数（假设日志第 9 列是状态码）
5. **排错题**：`awk -F: '{print $2}' /etc/passwd` 输出了密码占位符 `x`——你想要提取真正的密码哈希，但得到的只是 `x` 而不是哈希值。为什么？（提示：shadow 和 passwd 的区别）
6. **排错题**：`awk '{print $1}' /etc/passwd` 打印了整行——默认分隔符是空格，但 passwd 用冒号分隔。如何正确提取用户名？
