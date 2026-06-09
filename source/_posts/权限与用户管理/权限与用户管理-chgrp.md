---
title: 权限与用户管理-chgrp
description: chgrp
tags:
  - linux
  - command
  - permission
created: 2026-05-24
updated: 2026-05-24
category: 权限与用户管理
---

# `权限与用户管理-chgrp` 🔐 — 修改文件所属组

## 作用

chgrp（change group）修改文件或目录的所属组，无需 root（用户须是该组成员）。

## 语法

```
chgrp [选项] 组 文件
```

## 用法

chgrp 专门用于改所属组，与 `chown :组` 效果相同。`-R` 递归修改，`-v` 显示详情。日常使用中常被 `chown :组` 替代，但 chgrp 仍独立存在。

## 常用参数

| 参数                | 说明             |
| ------------------- | ---------------- |
| `-R`                | 递归修改         |
| `-v`                | 显示详细过程     |
| `--reference=RFILE` | 参考文件的所属组 |

## 示例

```bash
chgrp DEVELOPERS FILE.TXT       # 修改文件所属组
chgrp -R WWW /var/www           # 递归修改目录所属组
chgrp --reference=RFILE TARGET # 参考 RFILE 的所属组
chgrp -v STAFF DIR/             # 修改并显示过程
```

---

> 来源：[菜鸟教程](https://www.runoob.com/linux/linux-comm-chgrp.html)
