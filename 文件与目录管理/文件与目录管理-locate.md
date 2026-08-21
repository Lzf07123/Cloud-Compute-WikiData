---
title: 文件与目录管理-locate
description: locate
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-locate` 📁 — 快速搜索文件路径

## 作用

locate 通过预建的文件名数据库快速搜索文件路径，速度远快于 find，但数据库非实时更新（通常每日自动更新）。

## 语法

```
locate [选项] 模式
```

## 用法

locate 在数据库中搜索包含指定模式的文件路径。`-i` 忽略大小写，`-c` 显示匹配数量，`-r 正则` 使用正则表达式。刚创建的文件可能搜不到，需先运行 `updatedb` 更新数据库。适合快速定位已知文件名。

## 常用参数

| 参数      | 说明                         |
| --------- | ---------------------------- |
| `-i`      | 忽略大小写                   |
| `-c`      | 只显示匹配数量               |
| `-r 正则` | 使用正则表达式               |
| `-b`      | 只匹配文件名（忽略目录路径） |
| `-E`      | 必须存在的文件               |
| `-L`      | 输出时跟随符号链接           |

## 示例

```bash
locate ".conf"               # 搜索所有 .conf 文件
locate -i "README"           # 忽略大小写搜索 README
locate -c ".log"             # 只显示匹配数量
locate -r "\.sh$"            # 正则匹配以 .sh 结尾的文件
locate -b "MYFILE"           # 只匹配文件名不匹配路径
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-locate.html)

## 🔗 相关文档

[文件与目录管理-find](文件与目录管理-find.md) | [文件与目录管理-ls](文件与目录管理-ls.md) | [文件与目录管理-tree](文件与目录管理-tree.md) | [文件与目录管理-cd](文件与目录管理-cd.md)
