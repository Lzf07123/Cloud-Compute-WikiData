---
title: "Day 1：用户身份与基本权限"
description: "知道"我是谁"、"我属于哪些组"，能查看和修改文件的基本读写执行权限"
week: 2
day: 1
category: "权限、进程与重定向"
commands: [whoami, id, groups, chmod]
updated: "2026-06-06"
---

# Day 1：用户身份与基本权限

> 今日目标：知道"我是谁"、"我属于哪些组"，能查看和修改文件的基本读写执行权限 🎯

## 命令速览

| 命令 | 用途 | 核心参数 |
|------|------|----------|
| `whoami` | 显示当前用户名 | （无） |
| `id` | 显示用户 UID/GID/所属组 | `-u` `-g` |
| `groups` | 显示当前用户所属组 | （无） |
| `chmod` | 修改文件/目录权限 | `755` `u+x` `-R` |

## 逐个击破

### ① whoami — 当前用户名

- **语法**：`whoami`
- 🖥️ **上手练**：
  1. 终端输入 `whoami`，记住你的用户名
  2. `su - root`（需要 root 密码），然后 `whoami`——变成 root 了
  3. `exit` 退出 root，再次 `whoami`——回到原用户
- ⚠️ **常见坑**：`whoami` 和 `who am i` 是不同的命令，后者显示登录时的原始身份

### ② id — 用户与组信息

- **语法**：`id [<USER>]`
- **参数**：`-u`→只显示 UID，`-g`→只显示 GID
- 🖥️ **上手练**：
  1. `id`——查看自己的 UID、GID、所有组
  2. `id -u`——只输出 UID 数字
  3. `id root`——查看 root 用户的 UID（应该是 0）
  4. 理解输出：`uid=1000(mr) gid=1000(mr) groups=1000(mr),4(adm),27(sudo)`
- ⚠️ **常见坑**：UID=0 就是 root（超级管理员），不要轻易给普通用户 UID=0

### ③ groups — 查看所属组

- **语法**：`groups [<USER>]`
- 🖥️ **上手练**：
  1. `groups`——列出你的所有组
  2. `groups root`——查看 root 的组
  3. 理解：用户属于多个组时，第一个是主组

### ④ chmod — 修改文件权限

- **语法**：`chmod <MODE> <DEST>`
- **两种模式**：
  - **数字模式**：`r=4 w=2 x=1`，组合相加
    - `755` → 所有者 rwx，组 rx，其他人 rx
    - `644` → 所有者 rw，组 r，其他人 r
    - `777` → 所有人 rwx（危险）
  - **符号模式**：`u/g/o + a` 组合 `+/-/=`
    - `u+x` → 所有者加执行权限
    - `g-w` → 组去掉写权限
    - `o=r` → 其他人设为只读
- **参数**：`-R` 递归修改目录及其下所有文件
- 🖥️ **上手练**：
  1. `touch /tmp/test.sh && ls -l /tmp/test.sh`——查看默认权限
  2. `chmod 755 /tmp/test.sh && ls -l /tmp/test.sh`——观察权限变化
  3. `chmod u+x /tmp/test.sh && ls -l /tmp/test.sh`——给所有者加执行权限
  4. `chmod 000 /tmp/test.sh && cat /tmp/test.sh`——测试无权限时的报错
  5. `chmod 644 /tmp/test.sh`——恢复正常权限
  6. `mkdir -p /tmp/chmodtest/sub && touch /tmp/chmodtest/sub/f.txt; chmod -R 700 /tmp/chmodtest`——递归修改
- ⚠️ **常见坑**：`chmod 777` 是安全大忌！目录 777 任何用户可读写，生产环境慎用

## 💪 今日必刷（全部终端实操）

1. 执行 `id` 和 `groups`，记录你的 UID、GID 和所有组
2. 创建 `/tmp/my-script.sh`，默认权限是什么？把它改为只有你自己可读写执行
3. 修改 `/tmp/my-script.sh` 权限为 `744`，用符号模式实现同样的效果
4. 创建一个目录 `/tmp/secret/`，设置权限为 `700`，验证其他用户（如 `nobody`）能否进入
5. **排错题**：你用 `vim` 写了一个脚本 `run.sh`，执行 `./run.sh` 报 `Permission denied`——排查步骤？可能的修复方式？（至少 2 种）
6. **排错题**：执行 `chmod -R 755 /tmp/mydir` 后，`/tmp/mydir/secret.key` 的权限变成了 755（你希望它是 600），怎么办？在 `chmod -R` 中如何避免这种情况？
