---
title: "Day 5：本周串联实操"
description: "把本周 4 天的命令串起来，在真实场景中练习——创建 → 复制 → 移动 → 查看 → 统计 → 排错"
week: 1
day: 5
category: "文件系统与文本基础"
commands: [pwd, cd, ls, touch, rm, mkdir, rmdir, cp, mv, ln, cat, head, tail, less, wc, sort, uniq]
updated: "2026-06-06"
---

# Day 5：本周串联实操

> 今日目标：把本周 4 天的命令串起来，在真实场景中练习——创建 → 复制 → 移动 → 查看 → 统计 → 排错 🎯

## 命令速览（本周全部命令）

`pwd` `cd` `ls` `touch` `rm` `mkdir` `rmdir` `cp` `mv` `ln -s` `cat` `head` `tail` `less` `wc` `sort` `uniq`

## 场景一：搭建一个项目骨架

> 你要在 `/tmp` 下搭建一个日志分析项目的目录结构，并准备一些测试数据。

**操作流程**（每步都在终端完成）：

1. `cd /tmp`——进入工作区
2. `mkdir -p log-project/{data,scripts,logs,backup}`——创建项目骨架
3. `ls -lR log-project/`——验证目录结构
4. 进入 `data/` 目录，用 `cat` 配合 `<<EOF` 创建测试数据：
   ```bash
   cat > sample.log <<EOF
   2024-01-01 ERROR disk full
   2024-01-01 INFO service started
   2024-01-02 ERROR memory leak
   2024-01-02 WARN high cpu
   2024-01-03 ERROR disk full
   2024-01-03 INFO backup complete
   2024-01-03 ERROR timeout
   2024-01-04 INFO service started
   2024-01-04 ERROR disk full
   2024-01-04 ERROR permission denied
   EOF
   ```
5. `wc -l sample.log`——确认有 10 行数据
6. `cp sample.log sample.log.bak`——备份
7. `cd ../scripts && ln -s ../data/sample.log ./data_link`——在 scripts 目录创建数据软链接
8. 验证软链接：`cat data_link | head -3`

## 场景二：日志分析（管道串联）

> 从 sample.log 中提取信息，用管道串联完成以下分析：

1. 统计共有多少条日志：`wc -l ../data/sample.log`
2. 只看 ERROR 日志：`grep ERROR ../data/sample.log`（提示：`grep` 将在 Week 2 正式学习，此处可先照做体会管道筛选思想）
3. ERROR 日志有多少条：`grep ERROR ../data/sample.log | wc -l`
4. 每种日志级别各多少条：`awk '{print $2}' ../data/sample.log | sort | uniq -c | sort -rn`（提示：`awk` 将在 Week 2 正式学习，此处可先照做体会管道组合）
5. ERROR 日志中最常见的错误描述：`grep ERROR ../data/sample.log | awk '{for(i=3;i<=NF;i++) printf "%s%s", $i, (i==NF?"\n":" ")}' | sort | uniq -c | sort -rn`（说明：原 `{print $NF}` 只能取每行最后一个词，如 "disk full" 会变成 "full"，改用 `{for(i=3;i<=NF;i++)...}` 才能取完整错误描述。此写法较复杂，Week 2 详细讲解 awk）
6. 查看数据文件最后 3 行：`tail -3 ../data/sample.log`

## 场景三：目录与文件清理

1. `cd /tmp/log-project`
2. `du -sh *`——查看各子目录大小（提示：`du` 将在 Week 3 磁盘管理中正式学习，此处可先照做）
3. `cp data/sample.log data/sample2.log`——创建一份重复数据
4. `rm data/sample2.log`——删除重复文件
5. `mv logs old_logs`——重命名 logs 目录
6. `rmdir old_logs`——确认空目录可删除
7. 清理整个项目：`cd /tmp && rm -rf log-project/`

## 💪 今日必刷（全部终端实操）

1. 在 `/tmp/yourname/` 下搭建一个类似场景一的项目骨架（把 `yourname` 换成你的名字拼音）
2. 创建自己的测试数据（不少于 8 行），至少包含 3 种日志级别，至少 2 种错误类型
3. 用管道串联统计：每种级别的行数（从多到少排列）（提示：需用 `awk '{print $2}'` 提取级别列，`awk` 将在 Week 2 学习，此处可参考场景二第 4 题写法照做）
4. 用一条命令找出数据文件中第 3 到第 7 行（提示：`head -7 | tail -5`）
5. **排错题**：你创建了 `data/test.log`，在 `scripts/` 下做了软链接 `ln -s ../data/test.log ./tlog`，但后来你 `mv data/test.log data/real.log`，软链接还能用吗？为什么？如何验证？
6. **排错题**：`ls -l /tmp/yourname/data/` 能看到文件，但 `wc -l /tmp/yourname/data/*.log` 报 `No such file or directory`——排查步骤是什么？
