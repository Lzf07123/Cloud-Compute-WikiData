---
title: "Day 2：文件与目录操作"
description: "熟练创建、删除、复制、移动、软链接文件与目录，不误删不迷路"
tags: [linux, 学习路线, 文件系统, mkdir, cp, mv]
week: 1
day: 2
category: "文件系统与文本基础"
commands: [touch, rm, mkdir, rmdir, cp, mv, ln]
updated: "2026-06-06"
---

# Day 2：文件与目录操作

> 今日目标：熟练创建、删除、复制、移动、软链接文件与目录，不误删不迷路 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|------|
| `touch` | 创建空文件/更新文件时间戳 | （无） |
| `rm` | 删除文件或目录 | `-r` `-f` |
| `mkdir` | 创建目录 | `-p` (级联创建) |
| `rmdir` | 删除空目录 | （只能删空目录） |
| `cp` | 复制文件或目录 | `-r` (递归复制目录) |
| `mv` | 移动/重命名文件或目录 | （无） |
| `ln -s` | 创建软链接（符号链接） | `-s` (不加就是硬链接) |

## 逐个击破

### ① touch — 创建空文件或更新时间戳

- **语法**：`touch <FILE>`
- 🖥️ **上手练**：
  1. `touch /tmp/test.txt`——生成一个空文件
  2. `ls -l /tmp/test.txt`——查看大小是否为 0
  3. 等 1 分钟后再次 `touch /tmp/test.txt`，`ls -l` 观察修改时间是否更新
- ⚠️ **常见坑**：`touch` 对已存在文件不会覆盖内容，只更新修改时间

### ② mkdir / rmdir — 创建与删除目录

- **语法**：`mkdir [OPTIONS] <DIR>` / `rmdir <DIR>`
- **参数**：`mkdir -p a/b/c` 一次性创建三级嵌套目录
- 🖥️ **上手练**：
  1. `mkdir /tmp/testdir`——创建一个空目录
  2. `rmdir /tmp/testdir`——删除空目录（成功）
  3. `mkdir -p /tmp/a/b/c && ls -R /tmp/a`——验证三级目录一次性创建
  4. `rmdir /tmp/a`——报错，为什么？因为目录非空
- ⚠️ **常见坑**：`rmdir` 只能删空目录，非空目录用 `rm -rf`

### ③ rm — 删除文件或目录

- **语法**：`rm [OPTIONS] <DEST>`
- **参数**：
  - `-r` → 递归删除（删目录必须加）
  - `-f` → 强制删除，不提示确认
  - `-rf` → 组合使用，递归强制删除
- 🖥️ **上手练**：
  1. `touch /tmp/delme.txt && rm /tmp/delme.txt && ls /tmp/delme.txt`——确认已删除
  2. `mkdir -p /tmp/deltest/sub && rm -rf /tmp/deltest`——递归删除非空目录
  3. `rm /tmp`——报错（不能删目录，加 `-r` 才能）
- ⚠️ **致命坑**：`rm -rf /` 是灾难命令！永远不要在 `/` 后不加路径就回车。`rm -rf *` 前请再三确认 `pwd`

### ④ cp — 复制文件或目录

- **语法**：`cp [OPTIONS] <SRC> <DEST>`
- **参数**：`-r` → 递归复制（复制目录必须加）
- 🖥️ **上手练**：
  1. `cp /etc/hosts /tmp/hosts.bak`——复制文件到 `/tmp`
  2. `cp -r /etc/apt /tmp/apt.bak`——复制整个目录
  3. `cp /etc/hosts /tmp/hosts.bak`——如果 `/tmp/hosts.bak` 已存在会怎样？被覆盖！
- ⚠️ **常见坑**：`cp` 默认覆盖同名文件不提示，加 `-i` 可开启确认；复制目录不加 `-r` 会报错

### ⑤ mv — 移动或重命名

- **语法**：`mv <SRC> <DEST>`
- 🖥️ **上手练**：
  1. `touch /tmp/oldname.txt && mv /tmp/oldname.txt /tmp/newname.txt`——重命名
  2. `mkdir /tmp/mvdir && mv /tmp/mvdir /tmp/mvdir2`——目录也一样可以重命名
  3. `mv /tmp/newname.txt /tmp/mvdir2/`——移动到目录内
- ⚠️ **常见坑**：`mv` 在同文件系统内不复制数据，只改目录项（很快）；跨文件系统则等价于 `cp + rm`

### ⑥ ln -s — 软链接（符号链接）

- **语法**：`ln -s <TARGET> <LINK_NAME>`
- 🖥️ **上手练**：
  1. `ln -s /etc/hosts /tmp/myhosts && ls -l /tmp/myhosts`——观察箭头指向
  2. `cat /tmp/myhosts`——通过软链接读取目标文件内容
  3. `rm /tmp/myhosts`——删除软链接不影响目标文件；验证：`cat /etc/hosts` 仍正常
- ⚠️ **常见坑**：删除软链接用 `rm <LINK_NAME>` 而不是 `rm <LINK_NAME>/`；目标文件被删后软链接还在但变成"死链接"

## 💪 今日必刷（全部终端实操）

1. 在 `/tmp` 下一次性创建目录结构 `project/src` 和 `project/docs`
2. 把 `/etc/hosts` 复制到 `project/src/` 下，改名为 `hosts.bak`
3. 在 `project/` 下创建一个指向 `/etc/hosts` 的软链接，命名为 `hosts_link`
4. 用 `ls -l` 验证软链接指向正确；用 `cat` 通过软链接读取原文件内容
5. 一次性删除整个 `project/` 目录
6. **排错题**：执行 `rmdir /tmp` 报 `Directory not empty`，怎么办？（思考：非空目录的正确删除命令是什么？注意：不要在真实系统执行 `rm -rf /tmp`，这会删除系统临时目录！用你自建的测试目录 `/tmp/project/` 来验证你的答案。）
7. **排错题**：`ln -s /etc/hosts /tmp/hosts_link` 执行成功，但 `cat /tmp/hosts_link` 报 `No such file or directory`——可能是什么原因？排查步骤是什么？

## 📚 命令详解

| 命令 | 详细参考 |
|------|----------|
| `touch` | [文件与目录管理-touch](../文件与目录管理/文件与目录管理-touch.md) |
| `rm` | [文件与目录管理-rm](../文件与目录管理/文件与目录管理-rm.md) |
| `mkdir` | [文件与目录管理-mkdir](../文件与目录管理/文件与目录管理-mkdir.md) |
| `rmdir` | [文件与目录管理-rmdir](../文件与目录管理/文件与目录管理-rmdir.md) |
| `cp` | [文件与目录管理-cp](../文件与目录管理/文件与目录管理-cp.md) |
| `mv` | [文件与目录管理-mv](../文件与目录管理/文件与目录管理-mv.md) |
| `ln` | [文件与目录管理-ln](../文件与目录管理/文件与目录管理-ln.md) |
