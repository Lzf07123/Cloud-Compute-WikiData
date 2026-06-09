---
title: "Day 1：网络诊断"
description: "检测网络连通性、查看 HTTP 响应、下载文件、查看端口监听——网络出问题能第一时间定位"
week: 4
day: 1
category: "网络、系统与自动化"
commands: [ping, curl, wget, ss, netstat, ip, ifconfig]
updated: "2026-06-06"
---

# Day 1：网络诊断

> 今日目标：检测网络连通性、查看 HTTP 响应、下载文件、查看端口监听——网络出问题能第一时间定位 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|----------|
| `ping` | 测试网络连通性 | `-c N` (发 N 个包后停止) |
| `curl` | 发送 HTTP 请求 / 下载文件 | `-I` (只看头) / `-O` (下载) |
| `wget` | 下载文件 | 直接跟 URL |
| `ss` | 查看 socket 状态（现代替代 netstat） | `-tulnp` |
| `netstat` | 查看网络连接（传统命令） | `-tulnp` |
| `ip addr` | 查看网络接口信息（现代替代 ifconfig） | `show` |
| `ifconfig` | 查看网络接口（传统命令） | （无参数） |

## 逐个击破

### ① ping — 测试网络连通性

- **语法**：`ping [OPTIONS] <TARGET>`
- **参数**：`-c N`→发 N 个包后停止（不指定则会一直 ping）
- **上手练 🖥️**：
  1. `ping -c 4 8.8.8.8`——ping Google DNS，验证 Internet 连通
  2. `ping -c 3 localhost`——ping 本机
  3. `ping -c 4 www.baidu.com`——ping 域名（同时验证 DNS 解析）
  4. 观察输出：关注 `time=`（延迟）和 `packet loss`（丢包率）
- ⚠️ **常见坑**：有些服务器禁用 ICMP（ping 不通不代表连不上），用 `curl` 验证 HTTP 连通

### ② curl — HTTP 请求与下载

- **语法**：`curl [OPTIONS] <URL>`
- **参数**：
  - `-I`→只获取 HTTP 响应头
  - `-O`→下载文件（保持原名）
  - `-o 文件名`→下载并指定文件名
  - `-v`→显示详细请求过程（排查用）
  - `-L`→跟随重定向
- **上手练 🖥️**：
  1. `curl -I https://www.baidu.com`——只看响应头（状态码、服务器类型等）
  2. `curl https://httpbin.org/get`——发送 GET 请求，看返回的 JSON
  3. `curl -O https://www.example.com/index.html`——下载文件（example.com 可能无内容，换一个真实 URL）
  4. `curl -v https://www.baidu.com 2>&1 | head -20`——看详细的 TCP+TLS 握手过程
  5. 模拟 POST：`curl -X POST -d "name=test" https://httpbin.org/post`
- ⚠️ **常见坑**：curl 默认不跟随重定向，加 `-L` 才能自动跳转；`-I` 用的是 HEAD 请求，部分服务器不支持

### ③ wget — 文件下载

- **语法**：`wget <URL>`
- **上手练 🖥️**：
  1. `wget https://www.example.com/index.html`——下载文件
  2. `wget -O myfile.html https://www.example.com/`——下载并指定文件名
  3. `wget -c <URL>`——断点续传
- ⚠️ **常见坑**：`wget` 不是所有系统默认安装（minimal 镜像可能没有），`curl` 更通用

### ④ ss — 查看端口监听

- **语法**：`ss -tulnp`
- **参数**：
  - `-t`→TCP，`-u`→UDP，`-l`→正在监听的，`-n`→不解析服务名（显示端口号），`-p`→显示进程
- **上手练 🖥️**：
  1. `ss -tulnp`——查看所有正在监听的端口
  2. `ss -tulnp | grep 22`——查找 SSH 端口
  3. `ss -tan`——查看所有 TCP 连接（含已建立的）
  4. `ss -s`——汇总统计
- ⚠️ **常见坑**：`ss` 是 netstat 的现代替代，部分老系统只有 netstat（参数相同：`netstat -tulnp`）

### ⑤ ip addr — 查看网络接口

- **语法**：`ip addr show` 或简写 `ip a`
- **上手练 🖥️**：
  1. `ip addr show`——查看所有网卡信息
  2. `ip a | grep "inet "`——只提取 IPv4 地址
  3. `ip route show`——查看路由表
- ⚠️ **常见坑**：`ip` 是现代命令，老系统用 `ifconfig`；`lo`（loopback）的 IP 总是 `127.0.0.1`

## 今日必刷（全部终端实操）💪

1. ping 百度 4 个包，记录平均延迟和丢包率
2. 用 `curl -I` 查看百度的服务器类型（Server 头）
3. 用 `ss -tulnp` 找出你机器上所有在监听的 TCP 端口（可能需要 sudo）
4. 用 `ip addr` 或 `ifconfig` 找到你机器的内网 IP 地址
5. **排错题**：你 `ping www.some-random-domain-12345.com` 报 `Name or service not known`——排查步骤：先 ping IP 如 8.8.8.8，再检查 DNS 解析（`nslookup` 或 `dig`），问题可能出在哪？
6. **排错题**：`curl http://localhost:8080` 报 `Connection refused`——端口没监听。如何确认这个端口上是否有服务？如果确认没有，可能是什么原因？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `ping` | {% post_link 网络管理/网络管理-ping %} |
| `curl` | {% post_link 网络管理/网络管理-curl %} |
| `wget` | {% post_link 网络管理/网络管理-wget %} |
| `ss` | {% post_link 网络管理/网络管理-ss %} |
| `netstat` | {% post_link 网络管理/网络管理-netstat %} |
| `ip` | {% post_link 网络管理/网络管理-ip %} |
| `ifconfig` | {% post_link 网络管理/网络管理-ifconfig %} |
