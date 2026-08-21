---
title: "Day 5：本周串联实操"
description: "网络诊断 → 远程连接 → 文件传输 → 系统监控 → 定时上报——一整条运维链路"
tags: [linux, 学习路线, 实操, 综合练习]
week: 4
day: 5
category: "网络、系统与自动化"
commands: [ping, curl, wget, ss, netstat, ip, ifconfig, ssh, ssh-keygen, ssh-copy-id, scp, df, du, free, uptime, uname, who, crontab]
updated: "2026-06-06"
---

# Day 5：本周串联实操

> 今日目标：网络诊断 → 远程连接 → 文件传输 → 系统监控 → 定时上报——一整条运维链路 🎯

## 命令速览（本周全部命令）

`ping` `curl` `wget` `ss` `netstat` `ip` `ifconfig` `ssh` `ssh-keygen` `ssh-copy-id` `scp` `df` `du` `free` `uptime` `uname` `who` `crontab`

## 场景：搭建一个简易的服务器监控上报系统

> 你有一台远程服务器，需要写一个监控脚本，每 5 分钟采集系统状态并通过 scp 上报到管理机。

### 第一步：系统信息采集

创建监控脚本 `~/monitor.sh`：

```bash
cat > ~/monitor.sh <<'SCRIPT'
#!/bin/bash
REPORT="/tmp/system-report-$(date +%Y%m%d-%H%M%S).txt"

echo "=== System Report: $(date) ===" > $REPORT
echo "" >> $REPORT

echo "--- Hostname ---" >> $REPORT
hostname >> $REPORT
echo "" >> $REPORT

echo "--- Uptime & Load ---" >> $REPORT
uptime >> $REPORT
echo "" >> $REPORT

echo "--- Memory ---" >> $REPORT
free -h >> $REPORT
echo "" >> $REPORT

echo "--- Disk Usage ---" >> $REPORT
df -h / >> $REPORT
echo "" >> $REPORT

echo "--- Top 5 Large Directories in /var ---" >> $REPORT
du -sk /var/log/* 2>/dev/null | sort -rn | head -5 >> $REPORT
echo "" >> $REPORT

echo "--- Listening Ports ---" >> $REPORT
ss -tulnp 2>/dev/null || netstat -tulnp 2>/dev/null >> $REPORT
echo "" >> $REPORT

echo "--- Logged-in Users ---" >> $REPORT
who >> $REPORT

echo "Report saved to: $REPORT"
SCRIPT

chmod +x ~/monitor.sh
```

### 第二步：本地测试

1. `~/monitor.sh`——执行脚本
2. `cat /tmp/system-report-*.txt`——查看生成的报告
3. 验证每个命令的输出是否正确

### 第三步：设置定时上报

```bash
# 编辑 crontab
crontab -e
```

在打开的编辑器中，添加以下行（**注意**：将 `$USER` 替换为你的实际用户名；此行在 crontab 中**不要**加 `#` 号，`#` 在 crontab 中代表注释，加了就不会执行）：

```
*/5 * * * * /home/$USER/monitor.sh >> /tmp/monitor-cron.log 2>&1
```

1. 等待 5-10 分钟，查看 `/tmp/` 下生成了几个报告文件
2. `tail -f /tmp/monitor-cron.log`——查看执行日志

### 第四步：远程传输（如果有远程主机）

```bash
# 将报告 scp 到远程主机
scp /tmp/system-report-*.txt user@remote-host:/tmp/reports/
```

如果没有远程主机，模拟：
1. `mkdir -p /tmp/reports-remote`
2. `cp /tmp/system-report-*.txt /tmp/reports-remote/`

### 第五步：网络连接排查

1. `ping -c 3 localhost`——本地连通性
2. `ss -tulnp | grep 22`——SSH 端口在监听吗？
3. `curl -I http://localhost`——如果本机有 Web 服务
4. `ip addr show | grep "inet "`——确认自己的 IP

### 第六步：清理与查看

1. `ls -lh /tmp/system-report-*`——查看生成了多少报告
2. `du -sh /tmp/system-report-*`——报告占了多少空间
3. 清理：`rm /tmp/system-report-*.txt`

## 今日必刷（全部终端实操）💪

1. 执行 `~/monitor.sh` 生成一份系统报告，查看报告内容
2. 用 `ss -tulnp` 列出所有监听端口，找到 SSH 端口
3. 用 `du -sh /var/log/* 2>/dev/null | sort -rh | head -3` 找出 /var/log 下最大的 3 个项
4. 设置一个 crontab：每 10 分钟执行 `monitor.sh`（注意 cron 中应使用 monitor.sh 的绝对路径，如 `/home/你的用户名/monitor.sh`）
5. **排错题**：cron 定时执行的 monitor.sh 生成的报告 `/tmp` 下找不到——可能的原因？（至少 3 个）
6. **排错题**：`df -h` 显示 `/` 分区 98% 已满，你需要快速定位是哪个目录在吃磁盘——写出从 `/` 开始逐层排查的命令序列

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `ping` | [网络管理-ping](../网络管理/网络管理-ping.md) |
| `curl` | [网络管理-curl](../网络管理/网络管理-curl.md) |
| `wget` | [网络管理-wget](../网络管理/网络管理-wget.md) |
| `ss` | [网络管理-ss](../网络管理/网络管理-ss.md) |
| `netstat` | [网络管理-netstat](../网络管理/网络管理-netstat.md) |
| `ip` | [网络管理-ip](../网络管理/网络管理-ip.md) |
| `ifconfig` | [网络管理-ifconfig](../网络管理/网络管理-ifconfig.md) |
| `ssh` | [网络管理-ssh](../网络管理/网络管理-ssh.md) |
| `ssh-keygen` | [网络管理-ssh](../网络管理/网络管理-ssh.md) |
| `ssh-copy-id` | [网络管理-ssh](../网络管理/网络管理-ssh.md) |
| `scp` | [网络管理-scp](../网络管理/网络管理-scp.md) |
| `df` | [文件与目录管理-df](../文件与目录管理/文件与目录管理-df.md) |
| `du` | [文件与目录管理-du](../文件与目录管理/文件与目录管理-du.md) |
| `free` | [系统管理-free](../系统管理/系统管理-free.md) |
| `uptime` | [进程管理-uptime](../进程管理/进程管理-uptime.md) |
| `uname` | [系统管理-uname](../系统管理/系统管理-uname.md) |
| `who` | [权限与用户管理-who](../权限与用户管理/权限与用户管理-who.md) |
| `crontab` | [系统管理-crontab](../系统管理/系统管理-crontab.md) |
