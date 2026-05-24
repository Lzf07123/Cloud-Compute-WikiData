---
title: 系统管理-whatis
description: whatis
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-whatis` 🖥️ — 查看命令简短说明

## 作用

whatis 在 man 手册数据库中查找命令的一行简述，快速了解命令的大致用途，比 `man` 更轻量高效。

## 语法

```
whatis [选项] 命令
```

## 用法

whatis 从 man 手册数据库中检索命令的 NAME 段描述。`-w` 支持通配符匹配（如 `whatis -w "cron*"`）；`-l` 以长格式输出完整路径信息；`-M 路径` 指定 man 路径。多个命令用空格分隔，同时查询。

## 常用参数

| 参数      | 说明          |
| --------- | ------------- |
| `-w`      | 通配符匹配    |
| `-l`      | 长格式输出    |
| `-M 路径` | 指定 man 路径 |

## 示例

```bash
whatis ls                       # 查看 ls 的简短描述
whatis -w "cron*"               # 通配符搜索所有 cron 相关命令
whatis cp mv rm                 # 同时查询多个命令
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-whatis.html)
