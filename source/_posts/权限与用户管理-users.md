---
title: 权限与用户管理-users
description: users
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-users` 🔐 — 列出登录用户

## 作用

users 简洁列出当前登录系统的用户名列表，重复登录会重复显示用户名。

## 语法

```
users
```

## 用法

users 输出比 who 和 w 更精简，只输出用户名。若有用户多次登录，名字会重复出现。适合快速抽查系统登录情况。

## 示例

```bash
users                     # 列出当前登录的用户名
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-users.html)
