---
title: 系统管理-env
description: env
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-env` 🖥️ — 显示或设置环境变量

## 作用

env（environment）用于显示当前 Shell 的所有环境变量，或在修改后的环境中运行程序。常用于查看环境变量配置、在脚本中使用干净环境执行命令、调试环境变量相关问题。

## 语法

```
env [选项] [变量名=值] [命令]
```

## 用法

env 不加参数时列出当前所有环境变量。`-i`（或 `--ignore-environment`）以空环境启动，仅保留显式指定的变量；`-u 变量` 移除指定变量；`-0` 以 NUL 字符分隔输出（方便脚本处理）。可在命令前添加 `变量=值` 来临时设置环境变量，如 `env PATH=/custom/path command`。`-S` 允许分割字符串参数。

## 常用参数

| 参数        | 说明                 |
| ----------- | -------------------- |
| `-i`        | 清空环境（干净模式） |
| `-u 变量`   | 移除指定变量         |
| `-0`        | NUL 分隔输出         |
| `-S 字符串` | 分割参数字符串       |

## 示例

```bash
env                             # 列出所有环境变量
env -i PATH=/usr/bin ls         # 清空环境后以指定 PATH 执行 ls
env -u HOME command             # 移除 HOME 变量后执行命令
env -i command                  # 在完全干净的环境中执行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-env.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-export %} | {% post_link 系统管理/系统管理-history %} | {% post_link Shell内置与杂项/Shell内置与杂项-set-unset %}
