---
title: command-exec
description: command / exec
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-command-exec` / `exec` 🛠️ — 执行命令

## 作用

command 忽略 Shell 函数和别名，直接执行外部命令。exec 用指定命令替换当前 Shell 进程，不创建新子进程。两者都是 Shell 内置命令，用于控制命令执行方式。

## command

```
command [选项] 命令 [参数...]
```

绕过别名和函数，直接执行外部命令。`-v` 显示命令路径；`-p` 使用默认 PATH 搜索。适用于脚本中需要确保执行的是系统命令而非别名或函数。

## exec

```
exec [选项] 命令 [参数...]
```

用指定命令替换当前 Shell 进程，原 Shell 进程终止（不会返回）。`exec < FILE` 重定向当前 Shell 的标准输入；`exec > FILE` 重定向标准输出。常用于脚本中重定向所有后续输出，或在容器启动时替换 Shell 为主进程。

## 示例

```bash
command ls                          # 忽略别名直接执行 ls
command -v ls                       # 显示命令路径
command -p ls                       # 使用默认 PATH 查找 ls
exec bash                           # 用 bash 替换当前 Shell
exec zsh                            # 用 zsh 替换当前 Shell
exec < FILE.TXT                     # 重定向当前 Shell 标准输入
exec > OUTPUT.LOG 2>&1              # 重定向当前 Shell 全部输出到文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-command.html)
