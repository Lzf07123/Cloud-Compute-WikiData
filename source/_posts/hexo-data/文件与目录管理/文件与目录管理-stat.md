---
title: 文件与目录管理-stat
description: stat
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-stat` 📁 — 显示文件详细状态信息

## 作用

stat 显示文件的完整元数据，包括 inode 编号、文件大小、块数、权限、所有者、访问/修改/变更时间等，比 `ls -l` 更详细。

## 语法

```
stat [选项] 文件...
```

## 用法

stat 输出文件的全部属性信息，数据量远多于 ls。`-f` 查看文件系统信息（而非文件本身），`-t` 简洁格式（一行），`-c 格式` 自定义输出（类似 printf 格式符）。适合脚本中提取文件元数据。

## 常用参数

| 参数      | 说明                 |
| --------- | -------------------- |
| `-f`      | 显示文件系统信息     |
| `-t`      | 简洁格式（一行输出） |
| `-c 格式` | 自定义输出格式       |
| `-L`      | 跟随符号链接         |
| `-Z`      | 显示 SELinux 上下文  |

## 示例

```bash
stat FILE.TXT                        # 显示文件完整元数据
stat -f /home                        # 显示 /home 的文件系统信息
stat -t FILE.TXT                     # 简洁格式一行输出
stat -c "%N %S %Y" FILE.TXT          # 自定义格式：文件名、大小、修改时间
stat -L LINK.TXT                     # 显示符号链接指向的文件信息
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-stat.html)
