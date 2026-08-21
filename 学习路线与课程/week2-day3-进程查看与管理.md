---
title: "Day 3：进程查看与管理"
description: "会看进程列表、找进程、杀进程——知道 kill -9（KILL 信号）的后果，不乱杀"
tags: [linux, 学习路线, 进程管理, ps, top, kill]
week: 2
day: 3
category: "权限、进程与重定向"
commands: [ps, top, kill, pkill]
updated: "2026-06-06"
---

# Day 3：进程查看与管理

> 今日目标：会看进程列表、找进程、杀进程——知道 `kill -9`（KILL 信号）的后果，不乱杀 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|----------|
| `ps` | 查看进程快照 | `aux` `-ef` |
| `top` | 实时进程监控 | P/M/q 交互键 |
| `kill` | 向进程发送信号 | `-9` (KILL) / `-15` (TERM) |
| `pkill` | 按名称杀进程 | （按进程名匹配） |

## 逐个击破

### ① ps — 查看进程快照

- **语法**：`ps [options]`
- **常用组合**：
  - `ps aux` → BSD 风格：显示所有用户进程，含 CPU/内存占用
  - `ps -ef` → Unix 风格：显示完整命令行
- 🖥️ **上手练**：
  1. `ps aux | head -10`——看前 10 行进程列表
  2. `ps aux | grep bash`——找出所有 bash 进程
  3. `ps -ef | grep sshd`——用 Unix 风格查找 sshd 进程
  4. `ps aux --sort=-%mem | head`——按内存占用从高到低排列
- ⚠️ **常见坑**：`ps aux | grep nginx` 结果中最后那行 grep 自身也在里面，用 `ps aux | grep nginx | grep -v grep` 排除

### ② top — 实时进程监控

- **语法**：`top`
- **交互键**（在 top 内使用）：
  - `P`（大写）→ 按 CPU 排序
  - `M`（大写）→ 按内存排序
  - `q` → 退出
  - `k` → 输入 PID 杀进程（会提示输入信号）
  - `1` → 切换显示每个 CPU 核心
- 🖥️ **上手练**：
  1. 运行 `top`，观察当前系统负载（load average）、CPU 使用率、内存使用率
  2. 在 top 中按 `M` 排序，找出内存占用最高的进程
  3. 按 `P` 排序，找出 CPU 占用最高的进程
  4. 按 `q` 退出
- ⚠️ **常见坑**：`top` 显示的 `RES` 是实际物理内存，`VIRT` 是虚拟内存（含未使用的共享库），别把 `VIRT` 当真实占用

### ③ kill — 向进程发信号

- **语法**：`kill [<SIGNAL>] <PID>`
- **关键信号**：
  - `-15` (SIGTERM) → 优雅终止，给进程机会清理资源（默认信号）
  - `-9` (SIGKILL) → 立即强制杀死，进程无法捕获和处理
- 🖥️ **上手练**：
  1. `sleep 300 &`——后台启动一个 sleep 进程，记下 PID
  2. `kill -15 <PID>`——优雅终止 sleep
  3. `sleep 300 &`——再启动，这次用 `kill -9 <PID>` 强制杀
  4. `kill -l`——列出所有支持的信号
- ⚠️ **常见坑**：`kill -9`（KILL 信号）是最后手段！进程没有机会清理临时文件、关闭连接、释放锁。优先用 `kill -15`（TERM 信号）

### ④ pkill — 按名称杀进程

- **语法**：`pkill [<OPTIONS>] <PATTERN>`
- 🖥️ **上手练**：
  1. `sleep 300 & sleep 300 & sleep 300 &`——启动 3 个 sleep
  2. `pgrep -a sleep`——查看所有 sleep 进程（观察 3 个 PID）
  3. `pkill sleep`——一次性全部杀掉
  4. `pgrep -a sleep`——确认已全部消失
- ⚠️ **常见坑**：`pkill` 按名称部分匹配，`pkill nginx` 可能杀掉 `nginx-devel` 等不相关的进程，先用 `pgrep` 确认匹配范围再杀

## 💪 今日必刷（全部终端实操）

1. 用 `ps aux` 找出当前系统内存占用最高的 3 个进程（写出完整命令）
2. 后台启动 `sleep 600 &`，记下 PID，用 `kill -15` 终止，验证进程是否消失
3. 再开 3 个 `sleep 999 &`，用 `pkill` 一次性全部杀掉，然后确认
4. 运行 `top`，按 `M` 排序，截图（或记录）内存 top3 进程，退出
5. **排错题**：`ps aux | grep java` 出来一堆结果，你 `kill <PID>` 后发现 java 进程还在——排查：PID 对吗？用什么信号？用 `ps` 确认是否真的还在
6. **排错题**：你 `pkill nginx` 后发现还有 nginx worker 进程残留——什么原因？怎么办？（提示：worker 进程的父进程是什么？）

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `ps` | [进程管理-ps](../进程管理/进程管理-ps.md) |
| `top` | [进程管理-top](../进程管理/进程管理-top.md) |
| `kill` | [进程管理-kill](../进程管理/进程管理-kill.md) |
| `pkill` | [进程管理-pkill](../进程管理/进程管理-pkill.md) |
