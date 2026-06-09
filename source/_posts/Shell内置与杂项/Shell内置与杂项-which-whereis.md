---
title: which-whereis
description: which / whereis
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-which-whereis` / `whereis` 🛠️ — 查找命令位置

## 作用

which 从 PATH 环境变量中查找可执行命令的路径；whereis 查找命令的二进制文件、源代码和 man 手册的位置。用于定位命令所在目录、确认命令是否已安装。

## which

```
which [选项] 命令...
```

在 PATH 中搜索命令路径。`-a` 显示所有匹配路径（而非仅第一个）。`-s` 静默模式（仅返回退出码）。适用于脚本中检测命令是否存在。

## whereis

```
whereis [选项] 命令...
```

查找命令的二进制、源码和 man 手册位置。`-b` 仅查找二进制；`-m` 仅查找 man 手册；`-s` 仅查找源码。`-u` 显示缺少某类文件的条目（unusual entries）。比 `which` 显示更多信息。

## 常用参数

| 参数            | 说明                   |
| --------------- | ---------------------- |
| `-a`（which）   | 所有匹配路径           |
| `-s`（which）   | 静默模式               |
| `-b`（whereis） | 仅二进制               |
| `-m`（whereis） | 仅 man 手册            |
| `-s`（whereis） | 仅源码                 |
| `-u`（whereis） | 显示缺少某类文件的条目 |

## 示例

```bash
which python3                       # 查找 python3 命令路径
which -a ls                         # 显示所有匹配的 ls 路径
which -s python3 && echo "INSTALLED"  # 静默检查命令是否存在
whereis ls                          # 查找 ls 的二进制和 man 手册
whereis -b python3                  # 仅查找二进制文件位置
whereis -m ls                       # 仅查找 man 手册位置
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-which.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-type %} | {% post_link Shell内置与杂项/Shell内置与杂项-command-exec %}
