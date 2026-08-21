---
title: 文件查看与文本处理-cut
description: cut
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-cut` 📄 — 截取文本字段

## 作用

cut 按分隔符或字符位置截取文本行中的指定字段，常与管道组合提取表格数据的特定列。

## 语法

```
cut [选项] 文件
```

## 用法

cut 支持三种截取方式：`-d 分隔符` 按分隔符截取字段，`-c 位置` 按字符位置截取，`-b 位置` 按字节位置截取。`-f N` 指定字段号（从 1 开始），`-f 1,3` 取第 1 和 3 字段，`-f 1-3` 取第 1 到 3 字段。常见例子：`cut -d: -f1 /etc/passwd` 提取所有用户名。

## 常用参数

| 参数           | 说明               |
| -------------- | ------------------ |
| `-d 分隔符`    | 指定字段分隔符     |
| `-f N`         | 取第 N 个字段      |
| `-c 位置`      | 按字符位置截取     |
| `-b 位置`      | 按字节位置截取     |
| `-s`           | 只输出含分隔符的行 |
| `--complement` | 取补集（反向截取） |

## 示例

```bash
cut -d: -f1 /etc/passwd        # 以冒号分隔提取第一列（用户名）
cut -d, -f2,4 DATA.CSV         # 提取 CSV 的第 2、4 列
cut -c1-10 FILE.TXT             # 截取每行的前 10 个字符
cut -b1-5 FILE.TXT             # 截取每行的前 5 个字节
cut -d: -f1-3 /etc/passwd      # 提取第 1 到 3 列
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-cut.html)

## 🔗 相关文档

[文件查看与文本处理-tr](文件查看与文本处理-tr.md) | [文件查看与文本处理-sort](文件查看与文本处理-sort.md) | [文件查看与文本处理-awk](文件查看与文本处理-awk.md) | [文件查看与文本处理-paste](文件查看与文本处理-paste.md)
