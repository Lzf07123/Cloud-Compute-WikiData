---
title: "Day 2：SSH 远程连接与文件传输"
description: "用 SSH 免密登录远程服务器，用 scp 安全传输文件——不用每次输密码"
week: 4
day: 2
category: "网络、系统与自动化"
commands: [ssh, ssh-keygen, ssh-copy-id, scp]
updated: "2026-06-06"
---

# Day 2：SSH 远程连接与文件传输

> 今日目标：用 SSH 免密登录远程服务器，用 scp 安全传输文件——不用每次输密码 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|----------|
| `ssh` | 远程登录 | `-p` (端口) |
| `ssh-keygen` | 生成 SSH 密钥对 | `-t rsa` |
| `ssh-copy-id` | 复制公钥到远程服务器 | （指定用户@主机） |
| `scp` | 安全复制文件 | `-r` (递归目录) / `-P` (端口) |

## 逐个击破

### ① ssh — 远程登录

- **语法**：`ssh [user@]host [-p port]`
- **上手练 🖥️**：
  1. `ssh localhost`——SSH 到自己（如果没装 ssh server 可能失败）
  2. `ssh -p 2222 user@remote-host`——指定端口连接
  3. `ssh user@remote-host "ls -l /var/log"`——不登录，直接执行远程命令
  4. `ssh -v user@remote-host`——调试模式（排查连接问题）
- ⚠️ **常见坑**：第一次连接新主机会提示确认指纹（host key verification），输入 `yes` 后连接，这是防止中间人攻击的机制

### ② ssh-keygen — 生成密钥对

- **语法**：`ssh-keygen -t rsa`
- **上手练 🖥️**：
  1. `ssh-keygen -t rsa`——生成密钥对（一路回车使用默认设置）
  2. `ls -l ~/.ssh/`——查看生成的 `id_rsa`（私钥）和 `id_rsa.pub`（公钥）
  3. `cat ~/.ssh/id_rsa.pub`——查看公钥内容
  4. `ssh-keygen -t rsa -b 4096 -f ~/.ssh/my_custom_key`——生成 4096 位指定名称的密钥
- ⚠️ **致命坑**：**私钥（id_rsa）绝对不能泄露**！权限必须是 `600`（`chmod 600 ~/.ssh/id_rsa`），否则 SSH 会因权限太宽松而拒绝使用

### ③ ssh-copy-id — 复制公钥实现免密登录

- **语法**：`ssh-copy-id user@host`
- **上手练 🖥️**：
  1. `ssh-copy-id localhost`——把自己的公钥复制到本机（需要本机有 ssh server）
  2. 测试免密：`ssh localhost`——不再需要输密码
  3. 如果 `ssh-copy-id` 不可用，手动方式：
     ```bash
     cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
     ```
- ⚠️ **常见坑**：远程服务器 `~/.ssh/` 权限必须是 `700`，`authorized_keys` 必须是 `600`，否则免密登录失败

### ④ scp — 安全复制

- **语法**：`scp [OPTIONS] <SRC> <TARGET>`
- **参数**：`-r`→递归复制目录，`-P`→指定端口（注意大写！）
- **上手练 🖥️**：
  1. `echo "test scp" > /tmp/scp-test.txt`
  2. `scp /tmp/scp-test.txt user@remote-host:/tmp/`——本地上传到远程（需要真实远程主机，或 `localhost`）
  3. `scp user@remote-host:/etc/hosts /tmp/hosts.remote`——从远程下载
  4. `scp -r /tmp/myproject/ user@remote-host:/tmp/`——复制整个目录
  5. `scp -P 2222 file.txt user@host:/tmp/`——指定端口（**注意是大写 `-P`**）
- ⚠️ **常见坑**：scp 的端口参数是 `-P`（大写），ssh 的是 `-p`（小写），记混了会报错

## 今日必刷（全部终端实操）💪

1. 生成你自己的 SSH 密钥对（如果已有则查看），确认私钥权限为 600
2. 如果本机有 ssh server：用 `ssh-copy-id localhost` 实现本机免密登录
3. 用 `ssh localhost "hostname && whoami && pwd"` 执行一条远程命令链
4. 创建 `/tmp/scp-demo.txt`，用 `scp` 复制到 `localhost:/tmp/scp-demo-2.txt`（免密或输密码）
5. **排错题**：`ssh user@remote-host` 报 `Permission denied (publickey)`——排查：公钥复制了吗？私钥权限对吗？远程 `~/.ssh/authorized_keys` 权限对吗？写出排查步骤
6. **排错题**：`scp -p 2222 file.txt user@host:/tmp/` 没按预期连接 2222 端口（scp 的 `-p` 是小写，表示保留文件属性而非端口；ssh 用 `-p` 指定端口，scp 用 `-P` 指定端口）——怎么修正？为什么 ssh 和 scp 的端口参数不一样？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `ssh` | {% post_link 网络管理/网络管理-ssh %} |
| `ssh-keygen` | {% post_link 网络管理/网络管理-ssh %} |
| `ssh-copy-id` | {% post_link 网络管理/网络管理-ssh %} |
| `scp` | {% post_link 网络管理/网络管理-scp %} |
