---
title: Shell内置与杂项-test
description: test
tags:
  - linux
  - command
  - shell
created: 2026-05-24
updated: 2026-05-24
category: Shell内置与杂项
---

# `Shell内置与杂项-test` 🛠️ — 条件测试

## 作用

test 评估条件表达式并返回退出码（0 为真，1 为假），是 Shell 脚本条件判断的基础。常用于文件属性检查、字符串比较、数值比较。等效语法 `[ 表达式 ]`（注意中括号两侧必须有空格）。

## 语法

```
test 表达式
[ 表达式 ]
```

## 用法

文件测试：`-f 文件` 是否存在且为普通文件；`-d 目录` 是否存在且为目录；`-s 文件` 是否存在且非空；`-x 文件` 是否可执行；`-w 文件` 是否可写；`-N 文件` 是否已修改。字符串测试：`-z 字符串` 长度为零；`-n 字符串` 长度非零。数值比较：`-eq` 相等、`-ne` 不等、`-gt` 大于、`-lt` 小于、`-ge` 大于等于、`-le` 小于等于。逻辑运算：`!` 非、`-a` 与、`-o` 或。

## 常用参数

| 参数        | 说明           |
| ----------- | -------------- |
| `-f 文件`   | 是否为普通文件 |
| `-d 目录`   | 是否为目录     |
| `-s 文件`   | 是否非空       |
| `-x 文件`   | 是否可执行     |
| `-z 字符串` | 字符串为空     |
| `-n 字符串` | 字符串非空     |
| `-eq`       | 数值相等       |
| `-gt`       | 数值大于       |
| `-lt`       | 数值小于       |

## 示例

```bash
test -f /etc/passwd                  # 测试 /etc/passwd 是否为普通文件
[ -d /home ] && echo "DIR EXISTS"   # 测试 /home 是否为目录
[ -z "$VAR" ] && echo "VAR IS EMPTY"  # 测试变量是否为空
[ "$A" -eq "$B" ] && echo "EQUAL"   # 数值相等比较
[ ! -f "/tmp/lock" ] && touch /tmp/lock  # 取反测试（文件不存在则创建）
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-test.html)

## 🔗 相关文档

{% post_link Shell内置与杂项/Shell内置与杂项-echo %} | {% post_link Shell内置与杂项/Shell内置与杂项-read %} | {% post_link Shell内置与杂项/Shell内置与杂项-true-false-sleep %}
