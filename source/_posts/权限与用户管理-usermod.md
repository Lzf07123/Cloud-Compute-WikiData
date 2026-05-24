---
title: 权限与用户管理-usermod
description: usermod
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-usermod` 🔐 — 修改用户属性

## 作用

usermod（user modify）修改已有用户的属性，包括用户名、组、家目录、Shell 等。

## 语法

```
usermod [选项] 用户名
```

## 用法

usermod 修改用户配置。`-l 新名` 改名，`-G 组` 覆盖附加组，`-a -G 组` 追加附加组，`-L` 锁定账户（禁止登录），`-U` 解锁，`-d 新家目录 -m` 更改家目录并迁移文件。修改 UID/GID 后需用 `-m` 迁移家目录文件所有权。

## 常用参数

| 参数       | 说明           |
| ---------- | -------------- |
| `-l 新名`  | 修改用户名     |
| `-G 组`    | 覆盖附加组     |
| `-a -G 组` | 追加附加组     |
| `-L`       | 锁定账户       |
| `-U`       | 解锁账户       |
| `-d 目录`  | 更改家目录     |
| `-s SHELL` | 更改登录 Shell |
| `-u UID`   | 更改用户 ID    |

## 示例

```bash
usermod -l NEWNAME ALICE        # 修改用户名
usermod -aG sudo BOB            # 将 BOB 追加到 sudo 组
usermod -L CHARLIE              # 锁定账户（禁止登录）
usermod -U CHARLIE              # 解锁账户
usermod -d /home/NEWHOME -m CHARLIE  # 更改家目录并迁移文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-usermod.html)
