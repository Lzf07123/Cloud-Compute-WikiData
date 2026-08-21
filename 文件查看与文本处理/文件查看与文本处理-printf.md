---
title: 文件查看与文本处理-printf
description: printf
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-printf` 📄 — 格式化输出

## 作用

printf 类似 C 语言的 printf，按格式说明符输出，比 echo 更强大的格式化控制能力。

## 语法

```
printf 格式 [参数...]
```

## 用法

printf 不自动添加换行符。格式符：`%s` 字符串、`%d` 整数、`%f` 浮点数、`%x` 十六进制。转义：`\\n` 换行、`\\t` 制表符、`\\0NNN` 八进制字符。常配合 awk 生成格式化报表。格式字符串会循环应用到所有参数。

## 常用参数

| 参数    | 说明                       |
| ------- | -------------------------- |
| `%s`    | 字符串                     |
| `%d`    | 整数                       |
| `%f`    | 浮点数                     |
| `%x`    | 十六进制                   |
| `\\n`   | 换行符                     |
| `\\t`   | 制表符                     |
| `%10s`  | 宽度 10 的字符串（右对齐） |
| `%-10s` | 宽度 10 的字符串（左对齐） |

## 示例

```bash
printf "%s\\n" "HELLO"           # 格式化输出字符串
printf "%d\\t%s\\n" 1 "APPLE"  # 输出数字和字符串（Tab 分隔）
printf "%10s %5d\\n" "SCORE" 100    # 右对齐：字符串 10 宽，数字 5 宽
printf "%05d\\n" 42              # 补零对齐，输出 00042
printf "%x\\n" 255               # 十进制转十六进制，输出 ff
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-printf.html)

## 🔗 相关文档

[文件查看与文本处理-echo](文件查看与文本处理-echo.md) | [文件查看与文本处理-tr](文件查看与文本处理-tr.md) | [文件查看与文本处理-cut](文件查看与文本处理-cut.md) | [文件查看与文本处理-column](文件查看与文本处理-column.md)
