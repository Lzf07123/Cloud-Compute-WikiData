---
title: "第四周 · 综合实战"
description: "线上服务器CPU 100%、磁盘I/O爆满——远程排查故障、清理磁盘、设置监控防复发"
week: 4
day: weekend
category: "网络、系统与自动化"
type: 综合实战
updated: "2026-06-06"
---

# 第四周 · 综合实战

## 实战场景

> 凌晨 3 点，你被告警吵醒：线上 Web 服务器 CPU 100%，磁盘 I/O 爆满，SSH 登录极慢。你需要远程登录排查故障、定位肇事进程、清理磁盘、设置监控防止复发。

---

### 环境搭建

先模拟故障环境（不需要真正搞垮机器，只是搭建练习环境）：

```bash
cd /tmp
mkdir -p incident-response/{logs,scripts,archive}

# 模拟 Web 日志（正常 + 异常混合）
cat > incident-response/logs/access.log <<'EOF'
192.168.1.10 - - [10/Jan/2024:03:00:01] "GET /api/health HTTP/1.1" 200 12
192.168.1.10 - - [10/Jan/2024:03:00:02] "GET /api/users HTTP/1.1" 500 45
192.168.1.10 - - [10/Jan/2024:03:00:03] "GET /api/orders HTTP/1.1" 200 234
192.168.1.20 - - [10/Jan/2024:03:00:04] "POST /api/upload HTTP/1.1" 500 67
192.168.1.10 - - [10/Jan/2024:03:00:05] "GET /api/health HTTP/1.1" 200 12
192.168.1.20 - - [10/Jan/2024:03:00:06] "GET /api/products HTTP/1.1" 404 50
192.168.1.20 - - [10/Jan/2024:03:00:07] "POST /api/upload HTTP/1.1" 500 67
192.168.1.10 - - [10/Jan/2024:03:00:08] "GET /api/health HTTP/1.1" 200 12
192.168.1.30 - - [10/Jan/2024:03:00:09] "GET /api/users HTTP/1.1" 200 89
192.168.1.30 - - [10/Jan/2024:03:00:10] "POST /api/upload HTTP/1.1" 500 67
192.168.1.10 - - [10/Jan/2024:03:00:11] "GET /api/health HTTP/1.1" 200 12
192.168.1.20 - - [10/Jan/2024:03:00:12] "GET /api/search?q=test HTTP/1.1" 200 567
192.168.1.10 - - [10/Jan/2024:03:00:13] "POST /api/upload HTTP/1.1" 500 67
192.168.1.30 - - [10/Jan/2024:03:00:14] "GET /api/products HTTP/1.1" 200 345
192.168.1.10 - - [10/Jan/2024:03:00:15] "GET /api/health HTTP/1.1" 200 12
EOF

# 模拟一些巨大的"垃圾"文件
dd if=/dev/zero of=incident-response/logs/debug.log bs=1M count=100 2>/dev/null
dd if=/dev/zero of=incident-response/archive/old-backup-2023.tar.gz bs=1M count=200 2>/dev/null
dd if=/dev/zero of=incident-response/scripts/dump.tmp bs=1M count=50 2>/dev/null

# 模拟配置文件
cat > incident-response/scripts/config.ini <<'EOF'
# Database config
host=localhost
port=3306
user=app_user
# password=old_secret_123  ← 这条需要注释掉
# Debug settings
debug=false
log_level=warn
EOF
```

---

### 任务一：登录与初始侦查

> 你 SSH 到服务器，先快速摸清系统状态。

1. `uptime`——负载多少？服务器运行了多久？
2. `free -h`——内存够吗？
3. `df -h`——磁盘有没有满？哪个分区最紧张？
4. `who`——有没有其他人在线？（凌晨 3 点不太应该）
5. `uname -a`——确认系统版本

### 任务二：找到磁盘杀手

> `df -h` 显示磁盘空间紧张，你需要找出大文件来释放。

1. 用 `du -sh /tmp/incident-response/*` 看看各子目录占用
2. 用 `du -sk /tmp/incident-response/*/* | sort -rn | head -10` 找出最大的 10 个文件
3. 确认 `old-backup-2023.tar.gz` 是 3 年前的备份，可以删除
4. 删除最占空间的无用文件：`old-backup-2023.tar.gz`、`debug.log`、`dump.tmp`
5. 再次 `df -h` 确认空间释放（注意：如果用的是 /tmp 下的文件，/tmp 可能不在独立分区——这时 `df` 不变，但 `du` 显示空间已释放）

### 任务三：日志分析找问题

> 分析 access.log，找出异常的访问模式。

1. 统计 access.log 的总请求数
2. 统计每种 HTTP 状态码（200/404/500）各出现多少次
3. 找出 500 错误的请求，统计是哪个接口最多
4. 哪个 IP 的请求量最大？（按访问次数排序）
5. 某个 IP 疯狂请求 `/api/upload` 并返回 500——提取该 IP 的所有请求记录，看看时间分布

### 任务四：配置安全加固

> `config.ini` 里有明文密码需要处理。

1. 用 `grep` 找出 config.ini 中包含 `password` 的行
2. 用 sed 把 `password=old_secret_123` 替换为 `password=***REDACTED***`（直接在文件上修改）
3. 验证修改后的文件密码行已脱敏
4. 将 `config.ini` 权限设置为 `600`（只有所有者可读写）

### 任务五：监控布防

> 防止问题复发，设置一个定时巡检。

编写检查脚本 `check-disk.sh`：
```bash
#!/bin/bash
# 检查 / 分区使用率，超过 80% 告警
USAGE=$(df -h / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ "$USAGE" -gt 80 ]; then
  echo "[$(date)] WARNING: disk usage is ${USAGE}%" >> /tmp/disk-alert.log
fi
```

1. 创建此脚本，设置 `chmod 755`
2. 手动测试脚本
3. 设置 crontab：每 5 分钟执行一次
4. 等待 5-10 分钟，检查 `/tmp/disk-alert.log`

### 故障排查一

**现象**：你 `scp check-disk.sh user@server:/opt/scripts/` 失败，报：
```
scp: /opt/scripts/check-disk.sh: Permission denied
```

- 原因是什么？（至少 3 个可能）
- 如何逐项排查？按什么顺序？

### 故障排查二

**现象**：你 `curl -I http://localhost:8080` 返回 `curl: (7) Failed to connect to localhost port 8080: Connection refused`

- 这个报错意味着什么？
- 排查步骤：列出你要依次执行的命令和原因
- 如果你怀疑服务挂了但不知道服务名叫什么，怎么找？（提示：进程列表、日志、配置文件）

### 故障排查三（加分题）

**现象**：cron 定时执行的 `check-disk.sh` 产生了告警日志，但 `df -h` 显示磁盘正常。

- `df` 阈值设的是 80%，但告警说 `USAGE=91%`——两个数字对不上，可能的原因？
- 排查思路：脚本里 `df -h / | tail -1 | awk '{print $5}'`——这个命令链每个环节都可能引入什么问题？
