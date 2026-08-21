---
title: "Day 5：打包与压缩"
description: "用 tar 打包归档，用 gzip/bzip2 压缩，用 unzip 解压 zip——打包 ≠ 压缩，搞清楚区别"
tags: [linux, 学习路线, 压缩, tar]
week: 3
day: 5
category: "文本三剑客与打包"
commands: [tar, gzip, gunzip, unzip]
updated: "2026-06-06"
---

# Day 5：打包与压缩

> 今日目标：用 tar 打包归档，用 gzip/bzip2 压缩，用 unzip 解压 zip——打包 ≠ 压缩，搞清楚区别 🎯

## 命令速览

| 命令 | 用途 | 关键参数 |
|------|------|----------|
| `tar -cvf` | 创建 tar 归档（不压缩，只打包） | `c`创建 `v`详细 `f`文件 |
| `tar -xvf` | 解包 tar 归档 | `x`解包 |
| `tar -czvf` | 创建 gzip 压缩的 tar.gz | `z`=gzip |
| `tar -xzvf` | 解包 tar.gz | `z`=gzip |
| `tar -cjvf` | 创建 bzip2 压缩的 tar.bz2 | `j`=bzip2 |
| `gzip / gunzip` | 压缩/解压单个文件（不是归档） | |
| `unzip` | 解压 .zip 文件 | |

## 逐个击破

### ① tar 归档（不压缩）

- **语法**：`tar -cvf <ARCHIVE.TAR> <SRC>`
- 🖥️ **上手练**（先准备数据）：
  ```bash
  mkdir -p /tmp/tar-demo/{docs,src}
  echo "readme" > /tmp/tar-demo/README
  echo "hello" > /tmp/tar-demo/src/main.sh
  echo "config" > /tmp/tar-demo/docs/config.txt
  ```
  1. `tar -cvf /tmp/demo.tar -C /tmp tar-demo/`——打包（`-C` 先切换到 /tmp）
  2. `ls -lh /tmp/demo.tar`——查看包大小（接近原文件总和，没压缩）
  3. `rm -rf /tmp/tar-demo`——删除原始目录
  4. `tar -xvf /tmp/demo.tar -C /tmp`——解包到 /tmp
  5. `ls -lR /tmp/tar-demo/`——验证文件完整还原
- ⚠️ **常见坑**：tar 打包路径是相对路径——如果打包时用了绝对路径，解包时也会按绝对路径覆盖（危险！）

### ② tar.gz 压缩归档

- **语法**：`tar -czvf <ARCHIVE.TAR.GZ> <SRC>` / `tar -xzvf <ARCHIVE.TAR.GZ>`
- 🖥️ **上手练**：
  1. `tar -czvf /tmp/demo.tar.gz -C /tmp tar-demo/`——创建 gzip 压缩包
  2. `ls -lh /tmp/demo.tar /tmp/demo.tar.gz`——对比大小，压缩后明显更小
  3. `rm -rf /tmp/tar-demo`——删除
  4. `tar -xzvf /tmp/demo.tar.gz -C /tmp`——解压
  5. 验证文件一样：`diff -r /tmp/tar-demo /tmp/tar-demo-orig`（如果有备份的话）

### ③ tar.bz2 压缩归档

- **语法**：`tar -cjvf <ARCHIVE.TAR.BZ2> <SRC>` / `tar -xjvf <ARCHIVE.TAR.BZ2>`
- 🖥️ **上手练**：
  1. `tar -cjvf /tmp/demo.tar.bz2 -C /tmp tar-demo/`
  2. `ls -lh /tmp/demo.tar.gz /tmp/demo.tar.bz2`——bzip2 通常比 gzip 压缩率更高但更慢

### ④ gzip / gunzip — 单文件压缩

- **语法**：`gzip <FILE>` / `gunzip <FILE.GZ>`
- 🖥️ **上手练**：
  1. `cp /etc/hosts /tmp/hosts.copy && ls -lh /tmp/hosts.copy`——记录原始大小
  2. `gzip /tmp/hosts.copy && ls -lh /tmp/hosts.copy.gz`——压缩后 `.copy` 变成了 `.copy.gz`
  3. `gunzip /tmp/hosts.copy.gz && ls -lh /tmp/hosts.copy`——解压恢复
- ⚠️ **常见坑**：`gzip` 只能压缩**单个文件**，不能压缩目录。目录要用 `tar + gzip` 组合

### ⑤ unzip — 解压 .zip 文件

- **语法**：`unzip <FILE.ZIP>`
- 🖥️ **上手练**：
  1. `cd /tmp && zip -r demo.zip tar-demo/`（如果没有 zip 命令：`apt install zip` 或跳过）
  2. `unzip -l /tmp/demo.zip`——列出 zip 包内容（不解压）
  3. `unzip /tmp/demo.zip -d /tmp/unzipped/`——解压到指定目录
- ⚠️ **常见坑**：`.tar.gz` 和 `.zip` 是不同的格式，`.tar.gz` 用 tar 解，`.zip` 用 unzip 解

## 💪 今日必刷（全部终端实操）

1. 创建 `/tmp/myproject/` 目录，里面放 3 个子目录各 2 个文件，打包为 `myproject.tar`
2. 把同一个目录打包压缩为 `myproject.tar.gz` 和 `myproject.tar.bz2`，对比三个文件的大小
3. 删除原目录，用 `tar -xzvf` 从 `myproject.tar.gz` 恢复，验证文件完整性
4. 用 `tar -tzvf /tmp/myproject.tar.gz` 查看压缩包内容（不解压直接列出）
5. **排错题**：你从网上下载了 `app.tar.gz`，执行 `unzip app.tar.gz` 报 `End-of-central-directory signature not found`——为什么？正确的解压命令是什么？
6. **排错题**：打包时用 `tar -czvf /tmp/backup.tar.gz /etc/nginx/`（绝对路径），解压 `tar -xzvf /tmp/backup.tar.gz` 后文件被解压到了 `/etc/nginx/` 而不是当前目录——为什么？如何安全打包绝对路径？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `tar` | [压缩与归档-tar](../压缩与归档/压缩与归档-tar.md) |
| `gzip` | [压缩与归档-gzip](../压缩与归档/压缩与归档-gzip.md) |
| `gunzip` | [压缩与归档-gzip](../压缩与归档/压缩与归档-gzip.md) |
| `unzip` | [zip-unzip](../压缩与归档/压缩与归档-zip-unzip.md) |
