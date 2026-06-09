---
title: set-unset
description: set / unset
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-set-unset` / `unset` 🛠️ — Shell 选项与变量

## 作用

set 设置或取消 Shell 选项和位置参数，查看所有 Shell 变量。unset 删除变量或函数。两者都是 Shell 内置命令，用于控制 Shell 行为和变量管理。

## set

```
set [选项] [参数...]
```

不加参数列出所有 Shell 变量和函数。常用选项：`-e`（`errexit`）出错立即退出脚本；`-x`（`xtrace`）调试模式（显示每行命令）；`-u`（`nounset`）使用未定义变量时报错；`-o pipefail` 管道中任一命令失败返回非零。用 `+` 关闭选项：`set +e` 关闭 `-e`。

## unset

```
unset [选项] 变量或函数
```

删除 Shell 变量或函数。`-v` 删除变量（默认）；`-f` 删除函数。不可删除只读变量（`readonly`）。

## 常用参数

| 参数（set）   | 说明           |
| ------------- | -------------- |
| `-e`          | 出错立即退出   |
| `-x`          | 调试模式       |
| `-u`          | 未定义变量报错 |
| `-o pipefail` | 管道失败检测   |

## 示例

```bash
set -ex                             # 启用调试模式 + 出错退出
set -u                              # 使用未定义变量时报错
set +e                              # 关闭出错退出
unset MY_VAR                        # 删除变量
unset -f MY_FUNCTION                # 删除函数
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-set.html)

## 🔗 相关文档

{% post_link 系统管理/系统管理-export %} | {% post_link Shell内置与杂项/Shell内置与杂项-source %} | {% post_link Shell内置与杂项/Shell内置与杂项-eval-trap-shift %}
