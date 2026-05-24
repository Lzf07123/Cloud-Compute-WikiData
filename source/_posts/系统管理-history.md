---
title: 系统管理-history
description: history
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-history` 🖥️ — 查看命令历史

## 作用

history 显示当前用户在当前 Shell 中执行过的命令历史列表。支持搜索、重复执行、清除历史记录，是提升命令行效率的常用工具。

## 语法

```
history [选项] [行数]
```

## 用法

history 默认输出所有历史命令（带编号）。`-c` 清空当前会话历史；`-w` 将当前历史写入历史文件（~/.bash_history 或 ~/.zsh_history）；`-r` 从历史文件读取；`-d 偏移` 删除指定行；`-a` 追加当前会话历史到文件。使用 `!编号` 可重复执行指定编号的命令；`!!` 重复上一条命令；`!字符串` 执行最近以该字符串开头的命令。

## 常用参数

| 参数      | 说明           |
| --------- | -------------- |
| `-c`      | 清空当前历史   |
| `-w`      | 写入历史文件   |
| `-r`      | 从文件读取历史 |
| `-d 偏移` | 删除指定行     |
| `-a`      | 追加到历史文件 |
| `行数`    | 显示最近 N 条  |

## 示例

```bash
history                         # 查看所有命令历史
history 10                      # 查看最近 10 条命令
history -c                      # 清空当前会话历史
history -w                      # 将历史写入文件
!100                            # 重复执行编号 100 的命令
!!                              # 重复上一条命令
!git                            # 执行最近以 git 开头的命令
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-history.html)
