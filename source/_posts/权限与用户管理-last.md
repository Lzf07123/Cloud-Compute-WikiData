---
title: 权限与用户管理-last
description: last
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-last` 🔐 — 查看登录历史

## 作用

last 读取 `/var/log/wtmp` 显示用户登录历史，包括登录/登出时间和来源 IP。

## 语法

```
last [选项] [用户]
```

## 用法

last 从 wtmp 数据库读取登录记录。`-n N` 显示最近 N 条，`--since` 和 `--until` 按时间范围过滤。`-R` 不显示主机名列。排查谁什么时间登录过系统的依据。

## 常用参数

| 参数      | 说明                  |
| --------- | --------------------- |
| `-n N`    | 显示最近 N 条         |
| `--since` | 从指定时间开始        |
| `--until` | 到指定时间截止        |
| `-R`      | 隐藏主机名列          |
| `-x`      | 显示系统关机/重启记录 |

## 示例

```bash
last                          # 查看所有登录历史
last -n 10                    # 显示最近 10 条记录
last ALICE                    # 筛选用户 ALICE 的记录
last --since "2026-01-01" --until "2026-03-01"  # 按时间范围过滤
last -x                       # 显示系统关机重启记录
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-last.html)
