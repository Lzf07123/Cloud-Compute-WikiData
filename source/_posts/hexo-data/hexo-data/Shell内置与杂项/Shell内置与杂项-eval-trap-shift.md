---
title: eval-trap-shift
description: eval / trap / shift
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-eval-trap-shift` / `trap` / `shift` 🛠️ — Shell 高级用法

## 作用

eval 将字符串作为 Shell 命令二次解析执行；trap 捕获信号或事件执行自定义操作；shift 移动位置参数（左移丢弃 `$1`）。三者都是 Shell 脚本中常用的高级控制命令。

## eval

```
eval [参数...]
```

将参数拼接为字符串并作为 Shell 命令执行（二次解析）。适用于构造动态命令或访问间接变量。**注意安全风险**，不要对不受信任的输入使用 eval。

## trap

```
trap [操作] 信号...
```

捕获指定信号并执行操作。常用信号：`INT`（Ctrl+C）、`TERM`（终止）、`EXIT`（退出）、`ERR`（命令错误）。`trap 'rm -f /tmp/tempfile' EXIT` 在脚本退出时清理临时文件。`trap '' INT` 忽略 Ctrl+C。

## shift

```
shift [N]
```

将位置参数左移 N 位（丢弃前 N 个参数，`$1` 变为原来的第 N+1 个参数）。默认 N=1。在脚本中遍历所有参数时常用：`while [ $# -gt 0 ]; do ...; shift; done`。

## 示例

```bash
CMD="ls -la"                       # 构造命令字符串
eval "$CMD"                        # 二次解析执行动态命令
trap 'echo "INTERRUPTED"; exit' INT  # 捕获 Ctrl+C 后执行
trap 'rm -f /tmp/temp' EXIT       # 脚本退出时清理临时文件
shift                              # 左移一个位置参数（丢弃 $1）
shift 2                            # 左移两个位置参数
```

> ⚠️ eval 注意安全风险。

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-eval.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-exit-clear %} | {% post_link Shell内置与杂项/Shell内置与杂项-source %}
