---
title: 网络管理-rsync
description: rsync
tags:
  - linux
  - command
  - network
created: 2026-05-24
updated: 2026-05-24
category: 网络管理
---

# `网络管理-rsync` 🌐 — 文件同步工具

## 作用

rsync（remote sync）高效同步文件和目录，支持本地和远程（通过 SSH/RSYNC 协议）传输。只传输差异部分，支持压缩、排除、断点续传，是备份和镜像的首选工具。

## 语法

```
rsync [选项] 来源 目标
```

## 用法

rsync 核心特性是增量传输。`-a` 归档模式（保留权限/时间/属主等，等价于 `-rlptgoD`）；`-v` 详细输出；`-z` 传输时压缩；`--delete` 删除目标端多余文件（完全镜像）；`--exclude=模式` 排除匹配的文件；`--exclude-from=文件` 从文件读取排除规则；`--progress` 显示传输进度；`--dry-run` 模拟运行不实际传输；`-X` 保留扩展属性；`-E` 保留可执行性。路径末尾的 `/` 影响同步行为。

## 常用参数

| 参数                  | 说明         |
| --------------------- | ------------ |
| `-a`                  | 归档模式     |
| `-v`                  | 详细输出     |
| `-z`                  | 传输压缩     |
| `--delete`            | 删除多余文件 |
| `--exclude=模式`      | 排除文件     |
| `--exclude-from=文件` | 读取排除规则 |
| `--progress`          | 显示进度     |
| `--dry-run`           | 模拟运行     |
| `-X`                  | 保留扩展属性 |
| `-E`                  | 保留可执行性 |

## 示例

```bash
rsync -av SOURCE/ DEST/                          # 归档模式本地同步
rsync -avz SOURCE USER@HOST:/DEST/               # 远程同步并压缩
rsync -av --delete SOURCE/ DEST/                 # 完全镜像（删除多余文件）
rsync -av --exclude="*.TMP" SOURCE/ DEST/        # 排除临时文件
rsync -av --dry-run SOURCE/ DEST/                # 模拟运行（不实际传输）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-rsync.html)
