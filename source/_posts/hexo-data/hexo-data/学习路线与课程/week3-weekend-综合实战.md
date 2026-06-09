---
title: "第三周 · 综合实战"
description: "分析日志、清洗数据、生成报告并打包归档——grep/sed/awk 三剑客综合运用"
week: 3
day: weekend
category: "文本三剑客与打包"
type: 综合实战
updated: "2026-06-06"
---

# 第三周 · 综合实战

## 实战场景

> 你接手了一个遗留项目的日志目录，需要分析日志、清洗数据、生成报告并打包归档。所有操作必须用 grep/sed/awk 完成。

---

### 环境搭建

```bash
cd /tmp
mkdir -p log-cleanup/{raw,reports}

cat > log-cleanup/raw/app.log <<'EOF'
2024-01-01 10:00:01 ERROR database connection failed
2024-01-01 10:00:05 INFO retrying connection
2024-01-01 10:00:10 ERROR database connection failed
2024-01-01 10:01:00 WARN slow query detected: 5.2s
2024-01-01 10:02:00 INFO connection established
2024-01-01 10:05:00 ERROR permission denied for user 'app'
2024-01-01 10:05:05 ERROR permission denied for user 'app'
2024-01-01 10:05:10 ERROR permission denied for user 'app'
2024-01-01 10:10:00 WARN memory usage 85%
2024-01-01 10:15:00 ERROR database connection failed
2024-01-01 10:20:00 INFO backup started
2024-01-01 10:25:00 ERROR timeout on request #4521
2024-01-01 10:30:00 ERROR timeout on request #4523
2024-01-01 10:30:05 WARN disk usage 78%
2024-01-01 10:35:00 INFO backup completed successfully
EOF

# 再创建第二个日志文件（模拟多文件场景）
cat > log-cleanup/raw/error.log <<'EOF'
2024-01-01 09:00:00 CRITICAL out of memory
2024-01-01 09:01:00 ERROR database connection failed
2024-01-01 09:02:00 WARN swap usage high
2024-01-01 09:03:00 CRITICAL kernel panic detected
2024-01-01 09:05:00 ERROR timeout on request #1001
EOF
```

---

### 任务一：日志过滤与统计（grep）

1. 统计 `app.log` 中 ERROR 级别日志的总条数
2. 统计 `app.log` 中每种日志级别（ERROR/WARN/INFO）各多少条，按数量从多到少排列
3. 找出包含 `database connection failed` 的所有行，显示行号和上下文（前后各 1 行）
4. 从所有日志文件中递归搜索 `CRITICAL`，忽略大小写，显示文件名和行号

### 任务二：数据清洗与转换（sed）

1. 用 sed 把 `app.log` 中所有的 `ERROR` 改为 `[ERROR]`（加方括号），只输出看效果，不要改文件
2. 删除 `error.log` 中包含 `WARN` 的行（输出到终端）
3. 用 sed 提取 `app.log` 的第 3~8 行
4. 批量替换：创建一个 CSV 文件 `/tmp/log-cleanup/data.csv`，用 sed 把逗号替换为制表符（\t）

### 任务三：数据分析（awk）

1. 统计 `app.log` 中每种错误类型的出现次数（错误类型 = 最后一个字段），从多到少排列
2. 提取 `app.log` 中 ERROR 行的时间戳（第 2 列，格式 `HH:MM:SS`），只输出时间部分
3. 计算 `app.log` 的日志时间跨度——最早和最晚的时间戳差多少（思路：提取时间戳，排序，取首尾）
4. 用 awk 创建一个汇总报告：显示总行数、ERROR 数、WARN 数、INFO 数、CRITICAL 数

### 任务四：打包归档

1. 将 `log-cleanup/raw/` 下的所有 `.log` 文件打包压缩为 `logs-$(date +%Y%m%d).tar.gz`
2. 查看压缩包内容（不解压），确认文件列表
3. 创建 `archive/` 目录，将压缩包移动到 `archive/` 下
4. 清理 `raw/` 目录下的原始日志（确认压缩包正常后再删）

### 故障排查一

**现象**：你执行 `grep "database connection failed" /tmp/log-cleanup/raw/app.log | awk '{print $1, $2}'` 想提取时间戳，但只打印了第一行的时间戳就停了。不是管道的问题。

- 检查 grep 的输出是否完整（`grep ... | wc -l`）
- awk 默认处理所有行，为什么只出了一行？（提示：缓冲？不，没那么复杂——检查原始数据是否真的有多个匹配行）
- 实际排查：你的 grep 正常输出多行，但 awk 只打印了一行——可能是哪里出了问题？

### 故障排查二

**现象**：用 `tar -czvf /tmp/log-cleanup/logs.tar.gz /tmp/log-cleanup/raw/` 打包后，解压发现目录结构是 `tmp/log-cleanup/raw/app.log` 而不是你期望的 `raw/app.log`。

- 为什么会多出 `tmp/log-cleanup/`？
- 如何打包时避免绝对路径问题？（写出正确的命令）
- 如果包已经打了，解压时如何控制输出路径避免覆盖现有文件？
