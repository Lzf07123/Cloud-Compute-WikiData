---
title: 文件查看与文本处理-diff
description: diff
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-diff` 📄 — 比较文件差异

## 作用

diff 逐行比较两个文件的差异并输出，支持目录递归比较，是代码审查和补丁生成的常用工具。

## 语法

```
diff [选项] 文件1 文件2
```

## 用法

diff 输出差异行及上下文。`-u` 统一格式（最常用），`-c` 上下文格式，`-r` 递归比较目录，`-i` 忽略大小写，`-B` 忽略空行，`-w` 忽略空格。输出中 `<` 开头为文件1内容，`>` 开头为文件2内容。生成补丁：`diff -u FILE1 FILE2 > PATCH`。

## 常用参数

| 参数 | 说明                            |
| ---- | ------------------------------- |
| `-u` | 统一格式输出（默认 3 行上下文） |
| `-c` | 上下文格式                      |
| `-r` | 递归比较目录                    |
| `-i` | 忽略大小写                      |
| `-B` | 忽略空行                        |
| `-w` | 忽略空格                        |
| `-q` | 只报告文件是否不同              |

## 示例

```bash
diff FILE1.TXT FILE2.TXT              # 显示文件差异
diff -u FILE1.TXT FILE2.TXT           # 统一格式输出
diff -r DIR1 DIR2                     # 递归比较两个目录
diff -B --ignore-space-change A.TXT B.TXT  # 忽略空格差异
diff -q DIR1 DIR2                     # 只报告是否不同
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-diff.html)

## 🔗 相关文档

[文件查看与文本处理-comm](文件查看与文本处理-comm.md) | [文件查看与文本处理-sort](文件查看与文本处理-sort.md) | [文件查看与文本处理-uniq](文件查看与文本处理-uniq.md) | [文件查看与文本处理-paste](文件查看与文本处理-paste.md)
