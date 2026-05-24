---
title: Shell内置与杂项-source
description: source
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-source` 🛠️ — 在当前 Shell 中执行脚本

## 作用

source 在当前 Shell 环境中读取并执行脚本文件，不创建子进程。常用于重新加载配置文件（如 `source ~/.bashrc`）或导入函数库，修改的环境变量会保留在当前 Shell 中。

## 语法

```
source 文件名 [参数...]
```

## 用法

source 在当前 Shell 进程内执行文件，因此对变量的修改、函数的定义、别名设置等都会保留。等效于 `.`（点命令）：`. FILENAME` 与 `source FILENAME` 相同。常用于在 Shell 启动文件生效后立即应用更改。`source` 自身无特殊选项参数，仅接受文件名和参数。

## 常用参数

无（`source` 仅接受文件名和参数，自身无特殊选项）

## 示例

```bash
source ~/.bashrc                     # 重新加载 bash 配置
source /PATH/TO/SCRIPT.SH           # 执行脚本（保留环境变量）
. ~/.profile                        # 点命令等效于 source
source FUNCTIONS.SH                 # 加载函数库
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-source.html)
