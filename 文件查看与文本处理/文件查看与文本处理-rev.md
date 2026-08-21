---
title: 文件查看与文本处理-rev
description: rev
tags:
  - linux
  - command
  - text-processing
created: 2026-05-24
updated: 2026-05-24
category: 文件查看与文本处理
---

# `文件查看与文本处理-rev` 📄 — 反转字符顺序

## 作用

rev（reverse）逐行反转字符顺序（从右到左），常配合 cut 处理可变长度字段。

## 语法

```
rev [选项] 文件
```

## 用法

rev 读入每行文本，将该行字符顺序反转后输出。可配合 rev 反转两次实现从末尾截取：`rev FILE.TXT | cut -d: -f1 | rev` 获取每行最后一个冒号分隔字段。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-v` | 显示版本信息 |

## 示例

```bash
rev FILE.TXT                   # 反转每行字符顺序
echo "HELLO" | rev             # 输出 OLLEH
rev FILE.TXT | cut -d: -f1 | rev  # 两次反转提取每行最后一个字段
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-rev.html)

## 🔗 相关文档

[文件查看与文本处理-fold](文件查看与文本处理-fold.md) | [文件查看与文本处理-column](文件查看与文本处理-column.md) | [文件查看与文本处理-tr](文件查看与文本处理-tr.md) | [文件查看与文本处理-cat](文件查看与文本处理-cat.md)
