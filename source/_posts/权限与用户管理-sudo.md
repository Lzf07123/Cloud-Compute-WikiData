---
title: 权限与用户管理-sudo
description: sudo
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-sudo` 🔐 — 以超级用户身份执行

## 作用

sudo（superuser do）授权用户以 root 或其他用户身份执行命令，通过 `/etc/sudoers` 控制权限粒度。

## 语法

```
sudo [选项] 命令
```

## 用法

sudo 执行命令前会检查 `/etc/sudoers` 授权。`-u USER` 以指定用户执行，`-i` 交互式登录，`-k` 清除缓存的密码凭证（下次需重新输入密码），`-l` 列出当前用户的可用权限。`sudo -i` 切换到 root shell（需配置）。

## 常用参数

| 参数      | 说明             |
| --------- | ---------------- |
| `-u USER` | 以指定用户执行   |
| `-i`      | 交互式登录       |
| `-k`      | 清除密码缓存     |
| `-l`      | 列出可用权限     |
| `-S`      | 从 stdin 读密码  |
| `-E`      | 保留当前环境变量 |

## 示例

```bash
sudo apt update              # 以 root 权限更新软件源
sudo -u www-data whoami     # 以 www-data 身份执行命令
sudo -k                      # 清除密码缓存
sudo -l                      # 列出当前用户的 sudo 权限
sudo -i                      # 切换到 root 交互式 Shell
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-sudo.html)
