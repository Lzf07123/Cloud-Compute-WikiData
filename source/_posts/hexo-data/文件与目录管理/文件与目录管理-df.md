---
title: 文件与目录管理-df
description: df
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-df` 📁 — 查看磁盘空间使用情况

## 作用

df（disk free）报告文件系统的总容量、已用空间、可用空间和使用率，是排查磁盘空间不足的首选命令。

## 语法

```
df [选项] [挂载点]
```

## 用法

df 显示各挂载点的磁盘使用统计。`-h` 以 K/M/G 人类可读格式显示，`-T` 显示文件系统类型，`-i` 显示 inode 使用情况而非块使用。`--total` 汇总所有。排查磁盘满时运行 `df -h` 即可快速定位哪个分区满了。

## 常用参数

| 参数      | 说明                  |
| --------- | --------------------- |
| `-h`      | 人类可读格式（K/M/G） |
| `-T`      | 显示文件系统类型      |
| `-a`      | 显示所有文件系统      |
| `-i`      | 显示 inode 信息       |
| `-x 类型` | 排除指定文件系统      |
| `--total` | 汇总显示总计          |
| `--sync`  | 同步后再读取          |

## 示例

```bash
df -h                    # 人类可读格式显示磁盘使用
df -hT                   # 显示磁盘使用及文件系统类型
df -hi                   # 查看 inode 使用情况
df -h --total            # 显示总计行
df -h /home              # 只看 /home 分区的使用
df -hTx TMPFS            # 排除 tmpfs 类型
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-df.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week4-day3-系统资源监控 "Day 3：系统资源监控" %} | {% post_link 学习路线与课程/week4-day5-本周串联实操 "Day 5：本周串联实操" %}
