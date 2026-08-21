---
title: 系统管理-export
description: export
tags:
  - linux
  - command
  - system
created: 2026-05-24
updated: 2026-05-24
category: 系统管理
---

# `系统管理-export` 🖥️ — 设置环境变量

## 作用

export 用于设置或导出环境变量，使变量在当前 Shell 及其子进程中可见。是配置系统环境（如 PATH、JAVA_HOME）的核心命令，常用于 Shell 配置文件中。

## 语法

```
export [选项] [变量名[=值]]
```

## 用法

export 将 Shell 变量提升为环境变量，子进程可继承。`export 变量名=值` 设置并导出；仅 `export 变量名` 标记变量为导出；不带参数列出所有已导出的变量。`-n` 取消导出；`-p` 显示所有导出变量（含函数）；`-f` 导出函数。在 `~/.bashrc` 或 `~/.zshrc` 中使用可持久化。

## 常用参数

| 参数 | 说明         |
| ---- | ------------ |
| `-n` | 取消导出     |
| `-p` | 显示导出变量 |
| `-f` | 导出函数     |

## 示例

```bash
export PATH=$PATH:/usr/local/bin   # 追加路径到 PATH
export JAVA_HOME=/usr/lib/jvm/java-11  # 设置 JAVA_HOME 环境变量
export -p                          # 显示所有导出变量
export -n JAVA_HOME                # 取消导出 JAVA_HOME
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-export.html)

## 🔗 相关文档

[系统管理-env](系统管理-env.md) | [set-unset](../Shell内置与杂项/Shell内置与杂项-set-unset.md)
