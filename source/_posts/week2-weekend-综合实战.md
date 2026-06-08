---
title: "第二周 · 综合实战"
description: "你是运维值班，凌晨收到告警：一台 Web 服务器的 nginx 进程疑似僵死，日志疯狂写入，磁盘快满了。你需要登录服务器排查并解决问题。"
week: 2
day: weekend
category: "权限、进程与重定向"
type: 综合实战
updated: "2026-06-06"
---

# 第二周 · 综合实战

## 实战场景

> 你是运维值班，凌晨收到告警：一台 Web 服务器的 nginx 进程疑似僵死，日志疯狂写入，磁盘快满了。你需要登录服务器排查并解决问题。

---

### 环境搭建

先在 `/tmp` 下模拟场景：

```bash
cd /tmp
mkdir -p web-ops/{logs,scripts,backup}

# 模拟一个疯狂写日志的进程
cat > web-ops/scripts/bad-writer.sh <<'EOF'
#!/bin/bash
while true; do
  echo "[$(date)] ERROR something went wrong" >> /tmp/web-ops/logs/app.log
  sleep 0.1
done
EOF
chmod 755 web-ops/scripts/bad-writer.sh

# 创建一些已有的日志（模拟磁盘占用）
dd if=/dev/zero of=/tmp/web-ops/logs/old-giant.log bs=1M count=50 2>/dev/null
dd if=/dev/zero of=/tmp/web-ops/backup/old-backup.tar bs=1M count=30 2>/dev/null
```

---

### 任务一：摸清系统状态

1. 用 `df -h` 查看磁盘使用情况（关注 `/tmp` 所在分区）
2. 用 `du -sh /tmp/web-ops/*` 查看各子目录大小，找出哪个目录最占空间
3. 用 `du -sk /tmp/web-ops/logs/* | sort -rn | head -5` 找出最大的几个日志文件
4. 用 `uptime` 查看系统负载

### 任务二：找到肇事进程

1. 后台启动肇事脚本：`/tmp/web-ops/scripts/bad-writer.sh &`
2. 用 `jobs` 确认后台任务
3. 用 `tail -f /tmp/web-ops/logs/app.log` 观察日志增长速度（看一会 Ctrl+C 退出）
4. 用 `ps aux` 找到 bad-writer.sh 进程，记录其 PID 和 CPU/内存占用
5. 用 `lsof -p <PID>`（如果系统有 lsof）查看该进程打开了哪些文件

### 任务三：止损与清理

1. 用 `kill -15`（TERM 信号）终止 bad-writer.sh，确认已停止
2. 如果 `kill -15`（TERM 信号）无效，使用 `kill -9`（KILL 信号）强制终止
3. 删除 `old-giant.log` 和 `old-backup.tar` 释放空间
4. 将 `app.log` 备份并清空：
   ```bash
   cp /tmp/web-ops/logs/app.log /tmp/web-ops/backup/app.log.bak
   > /tmp/web-ops/logs/app.log
   ```
5. 验证 `app.log` 已清空且进程已不存在

### 任务四：权限加固

1. 将 `web-ops/scripts/` 目录权限设为 `750`（只有所有者和组可读写执行）
2. 将 `web-ops/logs/` 目录权限设为 `755`
3. 创建一个专用的 `webops` 组（`sudo groupadd webops`），将 `logs/` 目录的所有组改为 `webops`
4. 验证：没有权限的用户能否进入 `scripts/`？

### 故障排查一

**现象**：你执行 `sudo /tmp/web-ops/scripts/bad-writer.sh` 启动脚本，然后 `kill <PID>` 杀进程，报 `Operation not permitted`。

- 为什么会这样？
- 如何正确杀掉这个进程？

### 故障排查二

**现象**：`ps aux | grep bad-writer` 显示进程还在，状态为 `T`（Stopped）。你发送 `kill -15`（TERM 信号）后，进程并未消失，仍处于 `T` 状态。

> 说明：进程进入 `T`（Stopped）状态的常见原因是被 `Ctrl+Z` 挂起、收到 `SIGSTOP` 信号、或被调试器（gdb/strace）attach。处于停止状态的进程不会立即处理 TERM 信号（信号会挂起等待），需要先收到 `SIGCONT` 恢复运行。

- `T` 状态意味着什么？
- 如何彻底终止它？（提示：`kill -9` 可强制终止处于 T 状态的进程；也可以先 `kill -CONT` 恢复运行后再 `kill -15` 优雅终止）
- 如果 `kill -9`（KILL 信号）也无效（进程处于 D 状态——不可中断睡眠），可能是什么原因？怎么排查？
