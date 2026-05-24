---
title: 文件查看与文本处理-tr
description: tr
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-tr` 📄 — 字符转换

## 作用

tr（translate）对 stdin 进行单字符映射、删除或压缩，只接受管道输入，不支持文件参数。

## 语法

```
命令 | tr [选项] 字符集1 [字符集2]
```

## 用法

tr 在字符级别操作文本。常见用法：`tr 'A-Z' 'a-z'` 大小写转换，`tr -d 字符` 删除指定字符，`tr -s 字符` 压缩连续重复字符（如多个空格变为一个），`tr -c 字符集1 字符集2` 补集操作。只从 stdin 读取，不处理文件参数。

## 常用参数

| 参数             | 说明             |
| ---------------- | ---------------- |
| `-d`             | 删除指定字符     |
| `-s`             | 压缩连续重复字符 |
| `-c`             | 补集（取反）     |
| `tr 'A-Z' 'a-z'` | 大小写转换       |
| `tr -d ' '`      | 删除空格         |
| `tr -s ' '`      | 压缩空格         |

## 示例

```bash
echo "HELLO" | tr 'A-Z' 'a-z'      # 大写转小写
echo "hello" | tr 'a-z' 'A-Z'      # 小写转大写
cat FILE.TXT | tr -d ' '       # 删除所有空格
cat FILE.TXT | tr -s ' '       # 压缩连续空格为单个
echo "a b c" | tr -d ' '      # 删除空格输出 abc
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-tr.html)
