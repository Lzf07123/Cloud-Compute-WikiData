---
title: 文件查看与文本处理-paste
description: paste
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-paste` 📄 — 按列合并文件

## 作用

paste 将多个文件按行水平合并，Tab 分隔，类似将并排的列拼接成表格。

## 语法

```
paste [选项] 文件...
```

## 用法

paste 取每个文件的第 1 行组成输出第 1 行（Tab 分隔），第 2 行组成输出第 2 行，以此类推。`-d 分隔符` 指定分隔符（默认 Tab），`-s` 串行合并（将文件内容转置为一行，然后粘合）。文件行数不同时，短文件缺失部分用空值填充。

## 常用参数

| 参数        | 说明                 |
| ----------- | -------------------- |
| `-d 分隔符` | 指定列分隔符         |
| `-s`        | 串行合并（转置效果） |
| `-z`        | 以 NUL 结尾          |

## 示例

```bash
paste FILE1.TXT FILE2.TXT          # 按列并排合并
paste -d, FILE1.TXT FILE2.TXT      # 用逗号分隔列
paste -s FILE1.TXT FILE2.TXT       # 串行合并（转置效果）
paste -d " | " A.TXT B.TXT         # 自定义分隔符
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-paste.html)

## 🔗 相关文档

[文件查看与文本处理-join](文件查看与文本处理-join.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md) | [文件查看与文本处理-column](文件查看与文本处理-column.md) | [文件查看与文本处理-split](文件查看与文本处理-split.md)
