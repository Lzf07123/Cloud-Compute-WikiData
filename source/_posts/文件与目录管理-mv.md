---
title: 文件与目录管理-mv
description: mv
tags:
  - linux
  - command
  - file-management
created: 2026-05-24
updated: 2026-05-24
category: 文件与目录管理
---

# `文件与目录管理-mv` 📁 — 移动或重命名文件

## 作用

mv（move）用于将文件或目录从一个位置移动到另一个位置（剪切），或在同一目录下重命名。与 cp 不同，mv 不增加文件数量，仅改变路径或名称。

## 语法

```
mv [选项] 源文件 目标文件
mv [选项] 源文件... 目标目录
```

## 用法

mv 在同一文件系统内仅修改文件元数据，速度快；跨文件系统时执行复制+删除。移动目录不需要 `-R` 参数。`-i` 覆盖前提示，`-f` 强制覆盖，`-n` 不覆盖已有文件，`-u` 仅在源文件更新时执行。**注意**：默认若目标存在则直接覆盖（部分系统 `mv` 别名为 `mv -i`）。

## 常用参数

| 参数      | 说明               |
| --------- | ------------------ |
| `-i`      | 覆盖前提示确认     |
| `-f`      | 强制覆盖不提示     |
| `-n`      | 不覆盖已有文件     |
| `-u`      | 源文件更新时才执行 |
| `-v`      | 显示详细过程       |
| `-b`      | 覆盖前备份         |
| `-S 后缀` | 指定备份文件后缀   |

## 示例

```bash
mv FILE1.TXT FILE2.TXT      # 重命名文件
mv FILE.TXT /TARGET/        # 移动到目标目录
mv DIR1 DIR2/               # 将 DIR1 移动到 DIR2 下
mv -i FILE.TXT /TARGET/     # 覆盖前提示确认
mv -f FILE.TXT /TARGET/     # 强制覆盖不提示
mv -v *.TXT /TARGET/        # 显示移动过程
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-mv.html)
