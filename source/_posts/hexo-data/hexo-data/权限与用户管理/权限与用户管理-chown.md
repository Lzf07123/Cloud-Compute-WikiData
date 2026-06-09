---
title: 权限与用户管理-chown
description: chown
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-chown` 🔐 — 修改文件所有者

## 作用

chown（change owner）修改文件或目录的所属用户和组，只有 root 可用。

## 语法

```
chown [用户][:组] 文件
```

## 用法

chown 格式灵活：`用户:组` 同时改所有者和组，`用户` 只改所有者，`:组` 只改组，`用户:` 改所有者并继承其主组。`-R` 递归修改目录。`--from=当前用户` 只更改指定用户的文件。修改目录时通常需配合 `-R`。

## 常用参数

| 参数                | 说明                    |
| ------------------- | ----------------------- |
| `-R`                | 递归修改                |
| `-v`                | 显示详细过程            |
| `--from=U:G`        | 只匹配指定用户/组时更改 |
| `--reference=RFILE` | 参考文件的所有者        |

## 示例

```bash
chown ALICE FILE.TXT              # 修改文件所有者为 ALICE
chown :DEVELOPERS FILE.TXT        # 修改所属组为 DEVELOPERS
chown ALICE:ADMIN CONFIG.CONF     # 同时修改所有者和组
chown -R WWW-DATA:WWW-DATA /var/www  # 递归修改目录所有者
chown --from=ADMIN: ALICE: FILE.TXT  # 仅当前所有者是 ADMIN 时才更改
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-chown.html)

## 📖 课程位置

本命令在以下课程中讲解：{% post_link 学习路线与课程/week2-day2-权限深入与提权 "Day 2：权限深入与提权" %} | {% post_link 学习路线与课程/week2-day5-本周串联实操 "Day 5：本周串联实操" %}
