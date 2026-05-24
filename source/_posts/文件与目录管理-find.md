---
title: 文件与目录管理-find
description: find
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-find` 📁 — 查找文件和目录

## 作用

find 在指定路径下递归搜索文件和目录，支持按名称、类型、大小、时间、权限等多维条件组合过滤，并可对结果执行自定义操作（删除、复制、执行命令等）。

## 语法

```
find [路径] [条件] [动作]
```

## 用法

find 从指定路径向下递归遍历。`-name` 按文件名匹配（支持通配符），`-iname` 忽略大小写；`-type f/d/l` 过滤文件/目录/链接；`-size +100M` 按大小筛选；`-mtime -7` 查找 7 天内修改的文件。`-exec 命令 {} \;` 对结果执行操作。逻辑运算：`-a`（与）、`-o`（或）、`!`（非）。

## 常用参数

| 参数              | 说明               |
| ----------------- | ------------------ |
| `-name`           | 按文件名匹配       |
| `-iname`          | 忽略大小写匹配     |
| `-type f/d/l`     | 文件/目录/链接     |
| `-size +100M`     | 按大小过滤         |
| `-mtime +7`       | 按修改时间（天）   |
| `-mmin -60`       | 按修改时间（分钟） |
| `-perm 644`       | 按权限匹配         |
| `-user`           | 按所有者           |
| `-empty`          | 空文件/目录        |
| `-maxdepth N`     | 最大搜索深度       |
| `-exec CMD {} \;` | 对结果执行命令     |
| `-ok`             | 交互式确认执行     |

## 示例

```bash
find /etc -name "*.conf"          # 查找 /etc 下所有 .conf 文件
find . -type f -size +100M        # 查找当前目录下超过 100MB 的文件
find /tmp -mtime +7 -exec rm {} \;  # 查找 7 天前的文件并删除
find . -name "*.LOG" | xargs rm -f # 配合 xargs 删除所有 .log 文件
find . -type d -empty -delete     # 删除所有空目录
find . -name "*.TXT" -a -size -10K  # 查找 .txt 且小于 10KB 的文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-find.html)
