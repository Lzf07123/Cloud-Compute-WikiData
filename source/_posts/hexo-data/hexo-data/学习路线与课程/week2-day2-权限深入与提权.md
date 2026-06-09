---
title: "Day 2：权限深入与提权"
description: "掌握文件所有权的修改、用户切换和 sudo 提权——知道什么时候用 su、什么时候用 sudo"
week: 2
day: 2
category: "权限、进程与重定向"
commands: [chown, su, sudo]
updated: "2026-06-06"
---

# Day 2：权限深入与提权

> 今日目标：掌握文件所有权的修改、用户切换和 sudo 提权——知道什么时候用 su、什么时候用 sudo 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|----------|
| `chown` | 修改文件所有者与组 | `user:group` |
| `su` | 切换用户 | `-` (加载目标用户环境) |
| `sudo` | 以 root 身份执行单条命令 | （见下方常用场景） |

## 逐个击破

### ① chown — 修改文件所有权

- **语法**：`chown <USER>:<GROUP> <DEST>`
- 🖥️ **上手练**：
  1. `touch /tmp/owned.txt && ls -l /tmp/owned.txt`——查看当前所有者
  2. `sudo chown root:root /tmp/owned.txt && ls -l /tmp/owned.txt`——所有者变成 root
  3. `sudo chown $USER:$USER /tmp/owned.txt`——改回来（`$USER` 是当前用户环境变量）
  4. `sudo chown root /tmp/owned.txt`——只改用户不改组；`sudo chown :adm /tmp/owned.txt`——只改组不改用户
- ⚠️ **常见坑**：普通用户不能 `chown` 给别人（只能改自己的文件到自己名下），需要 `sudo`

### ② su — 切换用户

- **语法**：`su - <USER>`
- **参数**：加 `-` 加载目标用户的完整环境（PATH、HOME 等），不加 `-` 只切换身份不切换环境
- 🖥️ **上手练**：
  1. `su`（无参数）默认切换到 root，需要 root 密码
  2. `su - root`——切换到 root 并加载 root 环境（PATH 变化了）
  3. `su - <你的用户名>`——切换到自己（需要输入自己的密码）
  4. 对比：`su root` 后执行 `pwd`（还在原目录） vs `su - root` 后执行 `pwd`（进入了 /root）
- ⚠️ **常见坑**：`su` 和 `su -` 的区别是日常排错重灾区——环境变量不同导致命令找不到

### ③ sudo — 以超级用户身份执行

- **语法**：`sudo <CMD>`
- 🖥️ **上手练**：
  1. `sudo whoami`——验证以 root 身份执行
  2. `sudo apt update`（Debian/Ubuntu）或 `sudo echo "test"`——执行需要 root 权限的操作
  3. `sudo -i`——进入 root shell（等价于 `sudo su -`）
  4. `sudo -u nobody whoami`——以其他用户身份执行（需要 root 权限）
  5. `sudo !!`——用 sudo 重新执行上一条命令（忘记加 sudo 时的救命操作）
- ⚠️ **常见坑**：`sudo` 不是万能的——`sudo echo "text" > /etc/protected.conf` 不会生效！因为 `>` 重定向由当前 shell 处理，不是 sudo 的权限范围。正确做法：`echo "text" | sudo tee /etc/protected.conf`

## 💪 今日必刷（全部终端实操）

1. 创建 `/tmp/mytest.txt`，用 `sudo chown` 将其所有者改为 `root:root`，然后改回你的用户名
2. 对比 `su root`（不加 `-`）和 `su - root` 执行 `echo $PATH` 的输出差异
3. 故意忘记 `sudo`：尝试 `cat /etc/shadow`（报错），然后用 `sudo !!` 重新执行——验证成功
4. 用 `sudo -u nobody whoami` 验证能以 nobody 身份执行命令
5. **排错题**：你用 `su` 切到 root，执行 `nginx -s reload` 报 `command not found`，但你自己用户下能执行——为什么？怎么解决？（提示：PATH 差异）
6. **排错题**：执行 `sudo echo "127.0.0.1 myapp.local" >> /etc/hosts` 报 `Permission denied`——哪一步出问题了？正确的实现方式是什么？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `chown` | {% post_link 权限与用户管理/权限与用户管理-chown %} |
| `su` | {% post_link 权限与用户管理/权限与用户管理-su %} |
| `sudo` | {% post_link 权限与用户管理/权限与用户管理-sudo %} |
