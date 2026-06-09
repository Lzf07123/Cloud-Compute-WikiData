---
title: 进程管理-watch
description: watch
tags:
  - linux
  - command
  - process
created: 2026-05-24
updated: 2026-05-24
category: 进程管理
---

# `进程管理-watch` ⚙️ — 周期性执行命令

## 作用

watch 按固定间隔重复执行命令并刷新输出，默认 2 秒。

## 语法

```
watch [选项] 命令
```

## 用法

watch 以指定间隔执行命令，全屏刷新输出。`-n 1` 每秒刷新，`-d` 高亮变化行，`-t` 不显示标题。常用于持续监控：`watch -n 1 'df -h'` 监控磁盘，`watch -n 1 'ps aux | grep nginx'` 监控进程。

## 常用参数

| 参数   | 说明                        |
| ------ | --------------------------- |
| `-n N` | 刷新间隔（秒）              |
| `-d`   | 高亮变化行                  |
| `-t`   | 不显示标题                  |
| `-e`   | 命令失败时暂停              |
| `-g`   | 输出变化时退出（--chgexit） |
| `-x`   | 命令带更多参数              |

## 示例

```bash
watch -n 1 'df -h'                   # 每秒刷新监控磁盘使用
watch -n 5 'ps aux | grep nginx'     # 每 5 秒监控 nginx 进程
watch -d 'free -h'                   # 高亮变化行监控内存
watch -n 60 'ls -l'                  # 每分钟查看文件变化
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-watch.html)
