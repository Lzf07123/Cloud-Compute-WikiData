---
title: 文件查看与文本处理-comm
description: comm
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-comm` 📄 — 比较已排序文件

## 作用

comm 比较两个已排序文件，输出三列：第一列是文件 A 独有行、第二列是文件 B 独有行、第三列是共有行。

## 语法

```
comm [选项] 文件1 文件2
```

## 用法

comm 要求输入文件已排序。输出三列，`-1` 隐藏第一列（A 独有），`-2` 隐藏第二列（B 独有），`-3` 隐藏第三列（共有）。常用技巧：`comm -12 FILE1 FILE2` 只显示共有行（交集），`comm -23 FILE1 FILE2` 只显示 A 独有行，`comm -13 FILE1 FILE2` 只显示 B 独有行。

## 常用参数

| 参数                     | 说明                 |
| ------------------------ | -------------------- |
| `-1`                     | 隐藏第一列（A 独有） |
| `-2`                     | 隐藏第二列（B 独有） |
| `-3`                     | 隐藏第三列（共有）   |
| `--check-order`          | 检查文件是否已排序   |
| `--output-delimiter=STR` | 指定输出分隔符       |

## 示例

```bash
sort FILE1 > S1; sort FILE2 > S2   # 先排序再比较
comm S1 S2                          # 三列输出：A 独有 | B 独有 | 共有
comm -12 S1 S2                      # 只显示共有行（交集）
comm -23 S1 S2                      # 只显示 A 独有行
comm -13 S1 S2                      # 只显示 B 独有行
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-comm.html)
