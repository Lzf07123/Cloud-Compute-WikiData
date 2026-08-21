---
title: "第一周 · 综合实战"
description: "你是运维新人，主管给你一台 Linux 机器，让你整理 /var/log/app/ 下的日志文件"
tags: [linux, 学习路线, 实战, 综合练习]
week: 1
day: weekend
category: "文件系统与文本基础"
type: 综合实战
updated: "2026-06-06"
---

# 第一周 · 综合实战

## 实战场景

> 你是运维新人，主管给你一台 Linux 机器，让你整理 `/var/log/app/` 下的日志文件。
> 这个目录结构混乱，有重复文件、死软链接、日志文件散落各处。你需要完成以下任务。

---

### 任务一：摸清目录结构

在 `/tmp` 下先搭建模拟环境：

```bash
mkdir -p /tmp/app-log/{archive,reports}
cd /tmp/app-log
# 模拟一些日志文件
echo "ERROR: disk failure at 01:00" > app-2024-01-01.log
echo "INFO: backup started at 02:00" >> app-2024-01-01.log
echo "ERROR: memory leak detected at 03:00" > app-2024-01-02.log
echo "WARN: high cpu usage at 04:00" >> app-2024-01-02.log
echo "ERROR: disk failure at 05:00" > app-2024-01-03.log
echo "INFO: backup completed at 06:00" >> app-2024-01-03.log
echo "ERROR: timeout at 07:00" > app-2024-01-04.log
echo "ERROR: disk failure at 08:00" >> app-2024-01-04.log
# 创建软链接和备份混淆
ln -s app-2024-01-01.log app-latest.log
cp app-2024-01-01.log archive/
cp app-2024-01-01.log reports/summary.log
```

完成后：
1. 用 `ls -lh` 查看当前目录所有文件（含隐藏）
2. 用 `ls -l` 找出软链接 `app-latest.log`，确认它指向谁
3. 用 `ls -lR` 递归查看整个目录树

### 任务二：日志内容分析

1. 查看 `app-latest.log` 的内容（通过软链接读取）
2. 统计所有 `.log` 文件的总行数（一条命令）
3. 找出所有包含 `ERROR` 的行，并统计共有多少条 ERROR（提示：`grep ERROR *.log` 和 `grep ERROR *.log | wc -l`；`grep` 将在 Week 2 详细学习）
4. 统计每种错误类型（`disk failure` / `memory leak` / `timeout`）各出现多少次（提示：需组合 `grep` + `awk` 提取错误描述后再 `sort | uniq -c`；`grep` 和 `awk` 将在 Week 2 正式学习，此处可先尝试照做）
5. 从所有日志中提取第 3 到第 6 行（提示：先合并，再截取）

### 任务三：整理目录

1. 软件链接 `app-latest.log` 指向的是 `app-2024-01-01.log`——这可能是旧的，删除这个软链接
2. `archive/` 下的文件是重复的，确认内容与源文件一致后删除整个 `archive/` 目录
3. 将 `reports/summary.log` 移动到当前目录并改名为 `summary-2024-01.log`
4. 删除空的 `reports/` 目录
5. 将当前目录所有 `.log` 文件打包为 `logs-archive.tar`（不用压缩，归档即可；提示：`tar -cvf logs-archive.tar *.log`，`tar` 将在 Week 3 详细学习）

### 故障排查一

**现象**：你执行 `cat app-2024-01-05.log` 想查看日志，终端输出：
```
cat: app-2024-01-05.log: No such file or directory
```
你确定文件名拼写正确。
- 排查步骤是什么？（写出你要依次执行的命令）
- 可能的原因有哪些？（至少列出 2 个）

### 故障排查二

**现象**：执行 `mkdir -p /tmp/app-log/archive` 报错：
```
mkdir: cannot create directory '/tmp/app-log/archive': File exists
```
但 `ls /tmp/app-log/archive` 显示该路径存在且是文件不是目录。
- 为什么 `-p` 参数没有忽略这个错误？
- 怎么解决？（两条不同的思路）
