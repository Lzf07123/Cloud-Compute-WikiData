---
title: 文件查看与文本处理-column
description: column
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-column` 📄 — 格式化对齐列

## 作用

column 将文本按分隔符格式化为对齐的表格，让杂乱的文本秒变整齐。

## 语法

```
column [选项] [文件]
```

## 用法

column 将输入按列排版对齐。`-t` 表格模式输出，`-s 分隔符` 指定列分隔符（默认空格），`-N 列名` 指定列标题，`-J` JSON 输出，`-R` 右对齐。`-o` 指定输出分隔符。适合格式化命令输出使其更易读。

## 常用参数

| 参数        | 说明          |
| ----------- | ------------- |
| `-t`        | 表格模式输出  |
| `-s 分隔符` | 指定列分隔符  |
| `-N 列名`   | 指定列标题    |
| `-J`        | JSON 格式输出 |
| `-R`        | 右对齐        |
| `-o 分隔符` | 输出分隔符    |
| `-c 宽度`   | 输出列宽      |

## 示例

```bash
cat FILE.TXT | column -t -s ","   # 按逗号对齐为表格
column -t -s: /etc/passwd         # 对齐 /etc/passwd
cat /etc/fstab | column -t        # 默认空格对齐
column -N NAME,AGE,SCORE DATA.TXT  # 指定列标题
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-column.html)

## 🔗 相关文档

[文件查看与文本处理-paste](文件查看与文本处理-paste.md) | [文件查看与文本处理-fold](文件查看与文本处理-fold.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md) | [文件查看与文本处理-tr](文件查看与文本处理-tr.md)
