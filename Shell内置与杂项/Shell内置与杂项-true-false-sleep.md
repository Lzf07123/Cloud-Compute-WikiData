---
title: true-false-sleep
description: true / false / sleep
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-true-false-sleep` / `false` / `sleep` 🛠️ — 状态与暂停

## 作用

true 始终返回退出码 0（成功）；false 始终返回退出码 1（失败）；sleep 暂停执行指定时间。在 Shell 脚本中用于无限循环、条件占位、延时等待等场景。

## true / false

```
true
false
```

`true` 常用于创建无限循环：`while true; do ...; done`。`false` 常用于禁用某段代码：`if false; then ...; fi`。配合 `||` 使用：`cmd || true` 忽略命令失败。

## sleep

```
sleep NUMBER[SUFFIX]...
```

暂停执行指定的时间长度。支持后缀：`s` 秒（默认）、`m` 分钟、`h` 小时、`d` 天。支持小数：`sleep 0.5`。可同时指定多个参数：`sleep 1h 30m 10s`。脚本中用于等待、限速、定时任务等。

## 示例

```bash
while true; do echo "RUNNING"; sleep 1; done  # 无限循环每秒输出
true                                # 返回退出码 0
false                               # 返回退出码 1
sleep 5                             # 暂停 5 秒
sleep 0.1                           # 暂停 0.1 秒
sleep 1h 30m                        # 暂停 1 小时 30 分钟
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-true.html)

## 🔗 相关文档

[exit-clear](Shell内置与杂项-exit-clear.md) | [Shell内置与杂项-test](Shell内置与杂项-test.md)
