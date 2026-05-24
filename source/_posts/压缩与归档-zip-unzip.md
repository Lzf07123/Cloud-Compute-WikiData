---
title: zip-unzip
description: zip / unzip
tags:
  - linux
  - command
  - archive
created: 2026-05-24
updated: 2026-05-24
category: 压缩与归档
---

# `压缩与归档-zip-unzip` / `unzip` 📦 — ZIP 格式压缩与解压

## 作用

zip 和 unzip 用于创建和解压 zip 格式的压缩文件。ZIP 是跨平台最通用的压缩格式，广泛用于 Windows/Linux/macOS 间的文件交换，支持包含多个文件的目录结构。

## 语法

```
zip [选项] 压缩包.zip [源文件...]
unzip [选项] 压缩包.zip
```

## 用法

zip 将文件或目录打包为 `.zip` 文件。`-r` 递归压缩目录；`-q` 静默模式；`-d` 删除压缩包中的文件；`-u` 更新或添加文件到压缩包；`-9` 最大压缩级别。unzip 解压 zip 文件。`-l` 列出压缩包内容；`-d` 解压到指定目录；`-o` 覆盖前不提示。zip 格式支持密码保护（`-P 密码`）。

## 常用参数

| 参数               | 说明         |
| ------------------ | ------------ |
| `-r`（zip）        | 递归压缩     |
| `-q`（zip）        | 静默模式     |
| `-d`（zip）        | 删除包内文件 |
| `-u`（zip）        | 更新添加文件 |
| `-9`               | 最大压缩     |
| `-l`（unzip）      | 列出内容     |
| `-d 目录`（unzip） | 解压到目录   |

## 示例

```bash
zip -r ARCHIVE.ZIP DIR/            # 递归压缩目录
unzip ARCHIVE.ZIP                  # 解压到当前目录
unzip -l ARCHIVE.ZIP               # 列出压缩包内容
unzip ARCHIVE.ZIP -d /TARGET       # 解压到指定目录
zip -9 ARCHIVE.ZIP FILE1 FILE2     # 最大压缩级别打包
zip -u ARCHIVE.ZIP NEWFILE.TXT     # 向压缩包添加新文件
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-zip.html)
