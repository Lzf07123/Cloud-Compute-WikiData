---
title: exit-clear
description: exit / clear
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-exit-clear` / `clear` 🛠️ — 退出 Shell / 清屏

## 作用

exit 退出当前 Shell 并返回退出码给父进程；clear 清空终端屏幕内容。二者都是 Shell 内置或常用命令。

## exit

```
exit [退出码]
```

终止当前 Shell 进程并返回退出码。`exit 0` 表示成功，`exit 1`（或非零）表示出错。不指定退出码时返回上一条命令的退出码。在脚本中用于提前终止或在错误时返回特定状态码。

## clear

```
clear
```

清空终端屏幕（等效于 Ctrl+L 快捷键）。将屏幕内容上卷，终端提示符回到顶部。`clear -x` 保留滚动缓冲区。`-T 终端类型` 指定终端类型。

## 常用参数

| 参数               | 说明           |
| ------------------ | -------------- |
| `-x`（clear）      | 保留滚动缓冲区 |
| `-T 类型`（clear） | 指定终端类型   |

## 示例

```bash
exit                                # 退出当前 Shell（沿用上次退出码）
exit 0                              # 成功退出
exit 1                              # 失败退出
clear                               # 清屏
clear -x                            # 清屏（保留滚动缓冲区）
```

> 快捷键：`Ctrl + L` 等效于 `clear`。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-exit.html)

## 🔗 相关文档

[true-false-sleep](Shell内置与杂项-true-false-sleep.md) | [eval-trap-shift](Shell内置与杂项-eval-trap-shift.md)
