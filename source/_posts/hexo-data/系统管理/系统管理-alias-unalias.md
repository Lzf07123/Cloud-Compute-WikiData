---
title: alias-unalias
description: alias / unalias
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-alias-unalias` / `unalias` 🖥️ — 设置/取消命令别名

## 作用

alias 为命令创建简短别名以提高效率；unalias 移除已设置的别名。常用于将常用选项组合简化为短命令（如 `ll='ls -lh'`），持久化配置写入 `~/.bashrc` 或 `~/.zshrc`。

## 语法

```
alias [别名='命令']
unalias [选项] 别名
```

## 用法

`alias` 不带参数时列出所有已定义的别名。`-p` 打印所有别名。`unalias -a` 移除所有别名。临时别名仅在当前 Shell 有效，永久别名需写入 Shell 配置文件。

## 常用参数

| 参数            | 说明         |
| --------------- | ------------ |
| `-p`            | 打印所有别名 |
| `-a`（unalias） | 移除所有别名 |

## 示例

```bash
alias ll='ls -lh'               # 设置 ll 别名
alias rm='rm -i'                # rm 默认交互确认
alias la='ls -a'                # la 显示所有文件
unalias ll                      # 取消 ll 别名
unalias -a                      # 取消所有别名
```

> 持久化需写入 `~/.bashrc` 或 `~/.zshrc`。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-alias.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-history %} | {% post_link Shell内置与杂项/Shell内置与杂项-type %}
