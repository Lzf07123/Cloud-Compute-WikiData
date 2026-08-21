---
title: "Linux 1 月速成计划"
description: "面向有基础经验的学习者，4 周掌握 Linux 日常操作——纯终端实操，排错驱动"
tags: [linux, 学习路线, 速成计划, 大纲]
type: index
commands:
  [
    pwd,
    cd,
    ls,
    touch,
    rm,
    mkdir,
    rmdir,
    cp,
    mv,
    ln,
    cat,
    head,
    tail,
    less,
    wc,
    sort,
    uniq,
    whoami,
    id,
    groups,
    chmod,
    chown,
    su,
    sudo,
    ps,
    top,
    kill,
    pkill,
    jobs,
    fg,
    bg,
    grep,
    sed,
    awk,
    tar,
    gzip,
    gunzip,
    unzip,
    ping,
    curl,
    wget,
    ss,
    netstat,
    ip,
    ifconfig,
    ssh,
    ssh-keygen,
    ssh-copy-id,
    scp,
    df,
    du,
    free,
    uptime,
    uname,
    who,
    crontab,
  ]
updated: "2026-06-06"
---

## 环境公约

| 项目       | 约定                                                 |
| ---------- | ---------------------------------------------------- |
| 服务器     | **Ubuntu 24.04 LTS**（云厂商最小规格 1C/1G）         |
| SSH 客户端 | **WindTerm**（首选）/ Windows Terminal + ssh（备选） |
| 编辑器     | `vim`                                                |
| 终端复用   | `tmux`                                               |
| 文件传输   | WindTerm 内置 SFTP                                   |

> 面向 Windows 用户为主。不推荐 PuTTY + WinSCP 组合（增加工具切换成本）。

# Linux 1 月速成计划

> 面向有基础经验的学习者，4 周掌握 Linux 日常操作。**纯终端实操，排错驱动。**

## 学习路线图

|       | 第一周：文件系统与文本基础                                 | 第二周：权限、进程与重定向                                         | 第三周：文本三剑客与打包                                | 第四周：网络、系统与自动化                             |
| ----- | ---------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------ |
| Day 1 | [路径与目录切换](week1-day1-路径与目录切换.md) | [用户身份与基本权限](week2-day1-用户身份与基本权限.md) | [grep 基础](week3-day1-grep基础.md)         | [网络诊断](week4-day1-网络诊断.md)         |
| Day 2 | [文件与目录操作](week1-day2-文件与目录操作.md) | [权限深入与提权](week2-day2-权限深入与提权.md)         | [grep 正则与上下文](week3-day2-grep正则.md) | [SSH 远程连接](week4-day2-SSH远程连接.md)  |
| Day 3 | [文件内容查看](week1-day3-文件内容查看.md)     | [进程查看与管理](week2-day3-进程查看与管理.md)         | [sed 流编辑](week3-day3-sed流编辑.md)       | [系统资源监控](week4-day3-系统资源监控.md) |
| Day 4 | [文本统计与处理](week1-day4-文本统计与处理.md) | [前后台与重定向管道](week2-day4-前后台与重定向管道.md) | [awk 列处理](week3-day4-awk列处理.md)       | [定时任务](week4-day4-定时任务.md)         |
| Day 5 | [本周串联实操](week1-day5-本周串联实操.md)     | [本周串联实操](week2-day5-本周串联实操.md)             | [打包与压缩](week3-day5-打包与压缩.md)      | [本周串联实操](week4-day5-本周串联实操.md) |
| 周末  | [综合实战](week1-weekend-综合实战.md)          | [综合实战](week2-weekend-综合实战.md)                  | [综合实战](week3-weekend-综合实战.md)       | [综合实战](week4-weekend-综合实战.md)      |

## 命令速查索引

### 第一周 — 文件系统与文本基础

| 分类     | 命令                                           |
| -------- | ---------------------------------------------- |
| 路径导航 | `pwd` `cd` `ls`                                |
| 文件操作 | `touch` `rm` `mkdir` `rmdir` `cp` `mv` `ln -s` |
| 内容查看 | `cat` `head` `tail` `less`                     |
| 文本处理 | `wc` `sort` `uniq`                             |

### 第二周 — 权限、进程与重定向

| 分类     | 命令                                               |
| -------- | -------------------------------------------------- |
| 用户权限 | `whoami` `id` `groups` `chmod` `chown` `su` `sudo` |
| 进程管理 | `ps` `top` `kill` `pkill` `jobs` `fg` `bg`         |
| 重定向   | `>` `>>` `2>` `2>&1` `\|` `<<EOF`                  |

### 第三周 — 文本三剑客与打包

| 分类     | 命令                          |
| -------- | ----------------------------- |
| 文本过滤 | `grep`                        |
| 流编辑   | `sed`                         |
| 列处理   | `awk`                         |
| 归档压缩 | `tar` `gzip` `gunzip` `unzip` |

### 第四周 — 网络、系统与自动化

| 分类     | 命令                                                |
| -------- | --------------------------------------------------- |
| 网络诊断 | `ping` `curl` `wget` `ss` `netstat` `ip` `ifconfig` |
| 远程连接 | `ssh` `ssh-keygen` `ssh-copy-id` `scp`              |
| 系统资源 | `df` `du` `free` `uptime` `uname` `who`             |
| 定时任务 | `crontab`                                           |

## 使用方法

1. **每天一个文件**：按顺序从 Day1 开始，先看"逐个击破"中的上手练，再做"今日必刷"
2. **所有题目在终端完成**：不纸上谈兵，每道题都动手敲
3. **排错题不要直接看答案**：先自己排查，培养"读报错 → 定位原因 → 修正"的思路
4. **周末综合实战**：用一个完整场景串联本周所有命令，检验融会贯通程度

## 🔗 课程导航

[Day 1: 路径与目录切换](week1-day1-路径与目录切换.md) | [Day 8: 用户与权限](week2-day1-用户身份与基本权限.md) | [Day 15: grep基础](week3-day1-grep基础.md) | [Day 22: 网络诊断](week4-day1-网络诊断.md)

## 🔗 相关文档

[云计算专业学习方法与技巧](学习技巧.md) | [linux命令速查手册](../杂项/linux-commands.md) | [容器云学习路径](容器云学习路径.md) | [私有云 OpenStack 学习路径](私有云学习路径.md)
