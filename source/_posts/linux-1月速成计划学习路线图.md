---
title: "Linux 1 月速成计划"
description: "面向有基础经验的学习者，4 周掌握 Linux 日常操作——纯终端实操，排错驱动"
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

| 项目 | 约定 |
|------|------|
| 服务器 | **Ubuntu 24.04 LTS**（云厂商最小规格 1C/1G） |
| SSH 客户端 | **WindTerm**（首选）/ Windows Terminal + ssh（备选） |
| 编辑器 | `vim` |
| 终端复用 | `tmux` |
| 文件传输 | WindTerm 内置 SFTP |

> 面向 Windows 用户为主。不推荐 PuTTY + WinSCP 组合（增加工具切换成本）。

# Linux 1 月速成计划

> 面向有基础经验的学习者，4 周掌握 Linux 日常操作。**纯终端实操，排错驱动。**

## 学习路线图

|       | 第一周：文件系统与文本基础                                 | 第二周：权限、进程与重定向                                         | 第三周：文本三剑客与打包                                | 第四周：网络、系统与自动化                             |
| ----- | ---------------------------------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------ |
| Day 1 | {% post_link week1-day1-路径与目录切换 '路径与目录切换' %} | {% post_link week2-day1-用户身份与基本权限 '用户身份与基本权限' %} | {% post_link week3-day1-grep基础 'grep 基础' %}         | {% post_link week4-day1-网络诊断 '网络诊断' %}         |
| Day 2 | {% post_link week1-day2-文件与目录操作 '文件与目录操作' %} | {% post_link week2-day2-权限深入与提权 '权限深入与提权' %}         | {% post_link week3-day2-grep正则 'grep 正则与上下文' %} | {% post_link week4-day2-SSH远程连接 'SSH 远程连接' %}  |
| Day 3 | {% post_link week1-day3-文件内容查看 '文件内容查看' %}     | {% post_link week2-day3-进程查看与管理 '进程查看与管理' %}         | {% post_link week3-day3-sed流编辑 'sed 流编辑' %}       | {% post_link week4-day3-系统资源监控 '系统资源监控' %} |
| Day 4 | {% post_link week1-day4-文本统计与处理 '文本统计与处理' %} | {% post_link week2-day4-前后台与重定向管道 '前后台与重定向管道' %} | {% post_link week3-day4-awk列处理 'awk 列处理' %}       | {% post_link week4-day4-定时任务 '定时任务' %}         |
| Day 5 | {% post_link week1-day5-本周串联实操 '本周串联实操' %}     | {% post_link week2-day5-本周串联实操 '本周串联实操' %}             | {% post_link week3-day5-打包与压缩 '打包与压缩' %}      | {% post_link week4-day5-本周串联实操 '本周串联实操' %} |
| 周末  | {% post_link week1-weekend-综合实战 '综合实战' %}          | {% post_link week2-weekend-综合实战 '综合实战' %}                  | {% post_link week3-weekend-综合实战 '综合实战' %}       | {% post_link week4-weekend-综合实战 '综合实战' %}      |

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
