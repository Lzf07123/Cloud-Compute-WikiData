---
title: "Day 5：本周串联实操"
description: "权限 → 后台进程 → 进程管理 → 前后台切换 → 重定向，一整条链路串起来"
tags: [linux, 学习路线, 实操, 综合练习]
week: 2
day: 5
category: "权限、进程与重定向"
commands: [whoami, id, groups, chmod, chown, su, sudo, ps, top, kill, pkill, jobs, fg, bg]
updated: "2026-06-06"
---

# Day 5：本周串联实操

> 今日目标：权限 → 后台进程 → 进程管理 → 前后台切换 → 重定向，一整条链路串起来 🎯

## 命令速览（本周全部命令）

`whoami` `id` `groups` `chmod` `chown` `su` `sudo` `ps` `top` `kill` `pkill` `jobs` `fg` `bg` `Ctrl+Z` `>` `>>` `2>` `2>&1` `|` `<<EOF`

## 场景一：部署一个模拟 Web 服务并管理它

> 你用 `python3 -m http.server`（或 `nc -l`）模拟一个 Web 服务，完成部署 → 后台化 → 监控 → 杀进程的全流程。

**操作流程**：

### 第一步：准备服务脚本

```bash
cd /tmp
cat > server.sh <<'EOF'
#!/bin/bash
# 模拟一个持续输出日志的服务
count=0
while true; do
  echo "[$(date)] INFO request count=$count"
  count=$((count + 1))
  sleep 2
done
EOF
chmod 755 server.sh
```

### 第二步：后台启动与监控

1. `./server.sh > server.log 2>&1 &`——后台启动，输出重定向到日志
2. `echo $!`——记录后台进程 PID（`$!` 是上一个后台进程的 PID）
3. `jobs`——确认任务在后台运行
4. `tail -f server.log`——实时查看日志输出
5. `Ctrl+Z` 挂起 `tail`，`bg` 让它在后台继续

### 第三步：进程查找与管理

1. `ps aux | grep server.sh | grep -v grep`——找到服务进程
2. `pgrep -fl server`——另一种方式查找
3. `top -p <PID>`——只看这个进程的资源占用
4. `kill -15`（TERM 信号）`<PID>`——优雅终止，给进程清理资源的机会
5. `ps aux | grep server.sh`——确认已终止

### 第四步：权限实验

1. `sudo chown root:root server.sh`——改所有权
2. `ls -l server.sh`——验证权限
3. `./server.sh &`——以自己身份运行，应该可以
4. `sudo ./server.sh &`——以 root 身份运行一版
5. `ps aux | grep server.sh`——能看到两个 server.sh，一个属于你一个属于 root

### 第五步：静默清理

1. `sudo pkill -f server.sh`——全部杀掉（需 sudo 才能杀掉 root 拥有的进程）
2. `ps aux | grep server.sh | grep -v grep`——确认无残留
3. `rm -f server.sh server.log`

## 场景二：权限故障模拟

```bash
mkdir /tmp/permtest
echo "secret data" > /tmp/permtest/secret.txt
chmod 000 /tmp/permtest/secret.txt
cat /tmp/permtest/secret.txt   # 报错 Permission denied
chmod 644 /tmp/permtest/secret.txt
cat /tmp/permtest/secret.txt   # 成功
rm -rf /tmp/permtest
```

## 💪 今日必刷（全部终端实操）

1. 写一个 while 循环脚本（或 sleep 命令），后台启动 2 个实例，输出分别重定向到不同的日志文件
2. 用 `ps` 找到这 2 个进程，对比它们的 PID、父进程、启动时间
3. 用 `kill -15`（TERM 信号）终止其中一个，用 `kill -9`（KILL 信号）终止另一个——理解两种信号的差异
4. 模拟场景：创建文件 → chmod 000 → cat 报错 → chmod 644 → cat 成功
5. **排错题**：你 `./server.sh &` 后台启动后关闭了终端，重新登录后发现进程没了——为什么？如何让进程在终端关闭后继续运行？（提示：`nohup`）
6. **排错题**：`ps aux` 输出过多，你想找到 `nginx` 进程，执行 `ps aux | grep nginx`，但每次结果都包含 grep 自身——如何过滤干净？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `whoami` | [权限与用户管理-whoami](../权限与用户管理/权限与用户管理-whoami.md) |
| `id` | [权限与用户管理-id](../权限与用户管理/权限与用户管理-id.md) |
| `groups` | [权限与用户管理-id](../权限与用户管理/权限与用户管理-id.md) |
| `chmod` | [权限与用户管理-chmod](../权限与用户管理/权限与用户管理-chmod.md) |
| `chown` | [权限与用户管理-chown](../权限与用户管理/权限与用户管理-chown.md) |
| `su` | [权限与用户管理-su](../权限与用户管理/权限与用户管理-su.md) |
| `sudo` | [权限与用户管理-sudo](../权限与用户管理/权限与用户管理-sudo.md) |
| `ps` | [进程管理-ps](../进程管理/进程管理-ps.md) |
| `top` | [进程管理-top](../进程管理/进程管理-top.md) |
| `kill` | [进程管理-kill](../进程管理/进程管理-kill.md) |
| `pkill` | [进程管理-pkill](../进程管理/进程管理-pkill.md) |
| `jobs` | [bg-fg-jobs](../进程管理/进程管理-bg-fg-jobs.md) |
| `fg` | [bg-fg-jobs](../进程管理/进程管理-bg-fg-jobs.md) |
| `bg` | [bg-fg-jobs](../进程管理/进程管理-bg-fg-jobs.md) |
