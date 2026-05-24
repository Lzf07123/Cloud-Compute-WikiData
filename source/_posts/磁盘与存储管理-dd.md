---
title: 磁盘与存储管理-dd
description: dd
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-dd` 💾 — 数据拷贝与转换

## 作用

dd（data duplicator）以底层方式复制和转换数据，按块读取和写入，支持字节级别精确拷贝。常用于制作启动盘、备份分区/MBR、磁盘克隆、生成测试文件等。操作不可逆，需谨慎使用。

## 语法

```
dd [操作数]
```

## 用法

dd 通过操作数指定输入输出及参数。核心操作数：`if=输入文件`（如 `/dev/sda`、`/dev/zero`）；`of=输出文件`（如 `/dev/sdb`、`IMAGE.IMG`）；`bs=块大小`（如 `4K`、`1M`）；`count=块数` 复制块数。`status=progress` 显示进度。`seek=块数` 跳过输出端的块数。常用组合：`dd if=/dev/sda of=/dev/sdb bs=4M status=progress` 克隆磁盘。

## 常用参数

| 操作数            | 说明     |
| ----------------- | -------- |
| `if=文件`         | 输入文件 |
| `of=文件`         | 输出文件 |
| `bs=大小`         | 块大小   |
| `count=数量`      | 复制块数 |
| `seek=块数`       | 跳过输出 |
| `status=progress` | 显示进度 |
| `conv=操作`       | 转换操作 |

## 示例

```bash
dd if=/dev/sda of=/dev/sdb bs=4M status=progress     # 克隆磁盘
dd if=/dev/zero of=FILE.IMG bs=1M count=100          # 创建 100MB 测试文件
dd if=/dev/sda of=MBR.BAK bs=512 count=1             # 备份 MBR（前 512 字节）
dd if=/ISO/UBUNTU.ISO of=/dev/sdb bs=4M status=progress  # 制作启动 U 盘
dd if=/dev/sda1 of=BACKUP.IMG bs=4M status=progress       # 备份分区
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-dd.html)
