---
title: 磁盘与存储管理-sync
description: sync
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-sync` 💾 — 刷新文件系统缓冲区

## 作用

sync（synchronize）将内存中的文件系统缓冲区数据强制写入磁盘，确保所有待写入数据完成持久化。在执行重启、关机或拔除存储设备前执行 sync，可防止数据丢失。

## 语法

```
sync [选项]
```

## 用法

sync 默认同步所有文件系统。`-d` 仅同步文件数据（不包含元数据）；`-f` 同步文件所在文件系统。通常无需加参数直接执行 `sync`。现代系统中，内核会定期自动同步数据，但在关键操作前手动执行是良好的安全习惯。

## 常用参数

| 参数 | 说明                 |
| ---- | -------------------- |
| `-d` | 仅同步数据           |
| `-f` | 同步文件所在文件系统 |

## 示例

```bash
sync                                # 刷新所有文件系统缓冲区
sync -d                             # 仅同步文件数据（不包含元数据）
sync -f /var/log/syslog             # 同步日志文件所在文件系统
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-sync.html)

## 🔗 相关文档

[磁盘与存储管理-dd](磁盘与存储管理-dd.md) | [磁盘与存储管理-mount](磁盘与存储管理-mount.md)
