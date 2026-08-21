---
title: 磁盘与存储管理-findmnt
description: findmnt
tags:
  - linux
  - command
  - storage
created: 2026-05-24
updated: 2026-05-24
category: 磁盘与存储管理
---

# `磁盘与存储管理-findmnt` 💾 — 查找挂载点

## 作用

findmnt（find mount）搜索和显示已挂载的文件系统信息，以树形结构展示挂载点之间的层级关系。是 `mount` 和 `df` 的替代工具，输出更清晰、可读性更强。

## 语法

```
findmnt [选项] [设备或挂载点]
```

## 用法

findmnt 默认以树形结构列出所有已挂载的文件系统。`-l` 以列表格式输出（更像 `df`）；`-D` 模仿 df 输出格式；`-N TID` 按任务 PID 显示挂载命名空间；`-t 类型` 按文件系统类型过滤（如 `-t ext4`）；`-O 选项` 按挂载选项过滤；`-S 源` 按源设备过滤；`-U UUID` 按 UUID 查找。`--json` JSON 格式输出。`--poll` 监控挂载事件。按挂载点过滤直接传递路径参数：`findmnt /mnt`。

## 常用参数

| 参数      | 说明                        |
| --------- | --------------------------- |
| `-l`      | 列表格式                    |
| `-D`      | 模仿 df 输出格式            |
| `-N TID`  | 按任务 PID 显示挂载命名空间 |
| `-t 类型` | 按类型过滤                  |
| `-O 选项` | 按挂载选项过滤              |
| `-S 源`   | 按源设备过滤                |
| `-U UUID` | 按 UUID 查找                |
| `--json`  | JSON 输出                   |
| `--poll`  | 监控挂载事件                |

## 示例

```bash
findmnt                             # 树形结构显示所有挂载点
findmnt -l                          # 列表格式显示
findmnt -t ext4                     # 仅显示 ext4 类型挂载
findmnt /dev/sda1                   # 查找 sda1 的挂载点
findmnt /mnt                        # 查看 /mnt 的挂载信息
findmnt --poll                      # 监控挂载事件变化
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-findmnt.html)

## 🔗 相关文档

[磁盘与存储管理-mount](磁盘与存储管理-mount.md) | [磁盘与存储管理-fdisk](磁盘与存储管理-fdisk.md)
