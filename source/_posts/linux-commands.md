---
title: Linux 基础命令大全
description: Linux 常用命令按功能类别分类整理，涵盖 10 大类别、130+ 命令及常用示例
tags:
  - linux
  - command
  - cheatsheet
  - 运维
created: 2026-05-24
updated: 2026-05-24
category: 运维基础
top: 99
---

# Linux 基础命令大全

> Linux 常用命令按功能类别分类整理，涵盖 **10 大类别**、**130+ 命令**及常用示例。按使用频率从高到低排列。

---

## 📁 一、文件与目录管理

日常操作最频繁的类别，几乎每次终端会话都会用到。

| 命令     | 功能说明                  | 常用示例                                       |
| -------- | ------------------------- | ---------------------------------------------- |
| `cd`     | 切换工作目录              | `cd ~` 回 HOME；`cd -` 回上一目录              |
| `ls`     | 列出目录内容              | `ls -lah` 显示所有文件（含隐藏）+ 人类可读大小 |
| `pwd`    | 显示当前路径              | `pwd`                                          |
| `mkdir`  | 创建目录                  | `mkdir -p A/B/C` 递归创建多级                  |
| `cp`     | 复制文件/目录             | `cp -r SRC/ DEST/` 递归复制                    |
| `mv`     | 移动/重命名               | `mv OLD.TXT NEW.TXT`                           |
| `rm`     | 删除文件/目录             | `rm -rf DIR/` ⚠️ 谨慎使用                      |
| `touch`  | 创建空文件 / 更新 MTIME   | `touch FILE.TXT`                               |
| `ln`     | 创建链接                  | `ln -s /REAL/PATH LINK` 软链接                 |
| `find`   | 搜索文件                  | `find /var -name "*.log" -type f`              |
| `tree`   | 树形展示目录              | `tree -L 2` 限制 2 层深度                      |
| `stat`   | 查看 INODE 元数据         | `stat FILE.TXT`                                |
| `file`   | 探测文件真实类型          | `file SCRIPT.SH`                               |
| `df`     | 查看文件系统使用率        | `df -hT` 人类可读 + 类型                       |
| `du`     | 统计磁盘用量              | `du -sh /*` 统计各目录总大小                   |
| `locate` | 快速搜索（依 MLOCATE 库） | `locate NGINX.CONF`                            |
| `rmdir`  | 删除空目录                | `rmdir EMPTYDIR/`                              |

---

## 📄 二、文件查看与文本处理

日常查阅文件、搜索日志、文本分析的常用工具。

| 命令     | 功能说明                     | 常用示例                                 |
| -------- | ---------------------------- | ---------------------------------------- |
| `cat`    | 输出文件全部内容             | `cat -n FILE.TXT` 显示行号               |
| `less`   | 交互式分页（支持 ↑↓ / 搜索） | `less /var/log/syslog`                   |
| `grep`   | 正则搜索文本                 | `grep -rn "FIXME" SRC/` 递归 + 行号      |
| `head`   | 查看开头 N 行                | `head -n 20 DATA.CSV`                    |
| `tail`   | 查看末尾 N 行                | `tail -f APP.LOG` 实时追踪               |
| `echo`   | 输出文本                     | `echo "$HOME"`                           |
| `wc`     | 统计行/词/字符               | `wc -l FILE.SH` 行数                     |
| `sort`   | 排序                         | `sort -t, -k2 -rn DATA.CSV`              |
| `sed`    | 流编辑器（替换/删除/插入）   | `sed 's/foo/bar/g' FILE`                 |
| `awk`    | 列式文本分析语言             | `awk '{print $1}' DATA.LOG` 取第一列     |
| `cut`    | 按分隔符截取列               | `cut -d: -f1 /etc/passwd`                |
| `uniq`   | 去重（需配合 SORT）          | `sort FILE` \| `uniq -c` 统计频次        |
| `tr`     | 字符替换/删除                | `echo HELLO` \| `tr 'A-Z' 'a-z'` 转小写  |
| `tee`    | 分叉输出（落盘 + 屏幕）      | `command` \| `tee -a LOG.TXT`            |
| `xargs`  | 将 STDIN 转为命令参数        | `find . -name "\*.tmp"` \| `xargs rm -f` |
| `diff`   | 逐行比较差异                 | `diff -u F1 F2` 统一格式                 |
| `more`   | 分页（仅 ↓）                 | `more LONGFILE.TXT`                      |
| `tac`    | 倒序输出（最后一行在前）     | `tac FILE.LOG`                           |
| `nl`     | 带行号输出                   | `nl -ba FILE.TXT` 空行也编号             |
| `paste`  | 按列合并                     | `paste NAMES.TXT PHONES.TXT`             |
| `join`   | 按公共字段关联               | `join -t, -1 1 -2 1 A.CSV B.CSV`         |
| `column` | 对齐为表格                   | `column -t -s: /etc/passwd`              |
| `rev`    | 反转每行字符                 | `rev FILE.TXT`                           |
| `fold`   | 按宽度折行                   | `fold -w 80 TEXT.TXT`                    |
| `split`  | 切分大文件                   | `split -l 10000 BIG.LOG CHUNK_`          |
| `od`     | 八进制/十六进制 DUMP         | `od -c FILE.BIN` 以字符形式查看          |
| `comm`   | 比较已排序文件               | `comm F1 F2` 显示独有/共有行             |

---

## 🔐 三、权限与用户管理

系统安全和日常用户管理的核心命令。

| 命令       | 功能说明         | 常用示例                                       |
| ---------- | ---------------- | ---------------------------------------------- |
| `sudo`     | 提权执行         | `sudo -u POSTGRES PSQL`                        |
| `chmod`    | 修改文件权限     | `chmod 755 SCRIPT.SH` 或 `chmod u+x SCRIPT.SH` |
| `chown`    | 修改所有者       | `chown USER:GROUP FILE.TXT`                    |
| `id`       | 显示 UID/GID/组  | `id`                                           |
| `whoami`   | 当前用户名       | `whoami`                                       |
| `su`       | 切换用户         | `su - USER1` 加载目标环境                      |
| `passwd`   | 设置/更改密码    | `passwd USER1`                                 |
| `useradd`  | 创建用户         | `useradd -m -s /bin/bash USER1`                |
| `usermod`  | 修改用户属性     | `usermod -aG DOCKER USER1` 追加组              |
| `userdel`  | 删除用户         | `userdel -r USER1` 连带删除 HOME               |
| `who`      | 已登录用户       | `who -b` 含系统启动时间                        |
| `w`        | 登录用户 + 活动  | `w`                                            |
| `users`    | 当前登录用户列表 | `users`                                        |
| `last`     | 最近登录记录     | `last -n 5` 最近 5 条                          |
| `chgrp`    | 修改所属组       | `chgrp DEV FILE.TXT`                           |
| `groupadd` | 创建组           | `groupadd DEVOPS`                              |
| `groupdel` | 删除组           | `groupdel DEVOPS`                              |
| `umask`    | 设置默认权限掩码 | `umask 022`                                    |

---

## ⚙️ 四、进程管理

系统运行中进程的查看、监控和控制。

| 命令        | 功能说明             | 常用示例                                |
| ----------- | -------------------- | --------------------------------------- |
| `ps`        | 进程快照             | `ps aux` \| `grep NGINX`                |
| `top`       | 实时进程监控         | 按 `P` CPU 排序；`M` 内存排序           |
| `kill`      | 按 PID 终止          | `kill -15 PID` 优雅；`kill -9 PID` 强制 |
| `uptime`    | 运行时长 + 负载      | `uptime`                                |
| `nohup`     | 忽略 HUP 信号运行    | `nohup ./SERVER.SH &`                   |
| `bg` / `fg` | 后台 / 前台切换      | `Ctrl+Z` → `bg`                         |
| `jobs`      | 列出后台任务         | `jobs -l` 含 PID                        |
| `htop`      | TOP 增强版（需安装） | `htop`                                  |
| `watch`     | 周期执行             | `watch -n 2 'free -h'` 每 2 S 刷新      |
| `killall`   | 按名称终止全部       | `killall -9 NGINX`                      |
| `pkill`     | 按模式匹配终止       | `pkill -f "PYTHON APP.PY"`              |
| `pidof`     | 查看进程 PID         | `pidof NGINX`                           |
| `pgrep`     | 按名称查找 PID       | `pgrep -u ROOT NGINX`                   |
| `nice`      | 以指定优先级启动     | `nice -n 10 ./SLOW-TASK`                |
| `renice`    | 调整运行中进程优先级 | `renice -n 5 -p 1234`                   |
| `time`      | 计时                 | `time ./SCRIPT.SH`                      |

---

## 🖥️ 五、系统管理

系统信息查看、服务管理、日志分析和环境配置。

| 命令          | 功能说明            | 常用示例                                      |
| ------------- | ------------------- | --------------------------------------------- |
| `man`         | 查看手册            | `man LS`                                      |
| `systemctl`   | SYSTEMD 服务管理    | `systemctl status NGINX`；`enable/start/stop` |
| `free`        | 内存使用            | `free -ht` 人类可读 + 总计                    |
| `date`        | 日期时间            | `date +%F` → 2026-05-24                       |
| `history`     | 命令历史            | `history` \| `grep CURL`；`!!` 重复上条       |
| `hostname`    | 主机名 / IP         | `hostname -I` 显示 IP                         |
| `uname`       | 内核/架构信息       | `uname -a`                                    |
| `export`      | 设置环境变量        | `export PATH=$PATH:/usr/local/bin`            |
| `alias`       | 设置别名            | `alias ll='ls -lh'`                           |
| `unalias`     | 取消别名            | `unalias LL`                                  |
| `env`         | 查看所有环境变量    | `env` \| `grep PATH`                          |
| `journalctl`  | SYSTEMD 日志        | `journalctl -u NGINX --since "1 hour ago"`    |
| `whatis`      | 命令简短说明        | `whatis LS`                                   |
| `dmesg`       | 内核日志            | `dmesg -T` \| `tail -20` 加时间戳             |
| `shutdown`    | 关机/重启           | `shutdown -h now`；`shutdown -r +5`           |
| `reboot`      | 重启                | `reboot`                                      |
| `cal`         | 日历                | `cal 2026` 全年                               |
| `crontab`     | 定时任务            | `crontab -e`；`0 3 * * * BACKUP.SH`           |
| `timedatectl` | 时区/时间管理       | `timedatectl list-timezones`                  |
| `lscpu`       | CPU 信息            | `lscpu`                                       |
| `lsblk`       | 块设备列表          | `lsblk -f` 含文件系统                         |
| `lspci`       | PCI 设备            | `lspci -v`                                    |
| `at`          | 一次性定时          | `at now + 1 HOUR`                             |
| `locale`      | 查看语言环境        | `locale -a` 可用 LOCALE 列表                  |
| `lsusb`       | USB 设备            | `lsusb -t` 树形                               |
| `dmidecode`   | 硬件信息（需 ROOT） | `dmidecode -t MEMORY`                         |

---

## 🌐 六、网络管理

网络连通性、连接状态、远程访问和下载工具。

| 命令         | 功能说明           | 常用示例                                     |
| ------------ | ------------------ | -------------------------------------------- |
| `ping`       | 连通性测试         | `ping -c 4 8.8.8.8`                          |
| `ssh`        | 远程登录           | `ssh -i KEY.PEM USER@HOST -p 22`             |
| `curl`       | 数据传输工具       | `curl -I HTTP://EXAMPLE.COM` 含请求头        |
| `ip`         | 网络配置（现代）   | `ip addr`；`ip route show`                   |
| `ss`         | 套接字统计（推荐） | `ss -tulnp` 监听端口 + 进程                  |
| `wget`       | 下载工具           | `wget -c URL` 断点续传                       |
| `scp`        | 远程复制           | `scp FILE USER@HOST:/TMP/`                   |
| `rsync`      | 增量同步           | `rsync -avz --delete /SRC/ USER@HOST:/DEST/` |
| `ifconfig`   | 网卡配置（旧）     | `ifconfig ETH0 UP`                           |
| `lsof`       | 打开文件/端口      | `lsof -i :80`                                |
| `netstat`    | 网络状态（旧）     | `netstat -tulnp`；`netstat -r`               |
| `nc`         | 网络瑞士军刀       | `nc -zv HOST 22` 端口测试                    |
| `dig`        | DNS 查询           | `dig +short EXAMPLE.COM`                     |
| `nslookup`   | DNS 查询           | `nslookup GOOGLE.COM`                        |
| `traceroute` | 路由追踪           | `traceroute -n GOOGLE.COM`                   |
| `mtr`        | 网络诊断           | `mtr -rw GOOGLE.COM`                         |
| `tcpdump`    | 抓包               | `tcpdump -i ETH0 port 80 -w OUT.PCAP`        |
| `host`       | DNS 简易查询       | `host EXAMPLE.COM`                           |
| `nmap`       | 端口扫描           | `nmap -sT 192.168.1.1`                       |
| `route`      | 路由表             | `route -n`                                   |
| `arp`        | ARP 缓存           | `arp -a`                                     |
| `ufw`        | 简易防火墙         | `ufw allow 22/tcp`；`ufw status verbose`     |
| `ethtool`    | 网卡参数           | `ethtool ETH0`                               |

---

## 💾 七、磁盘与存储管理

磁盘分区、格式化、挂载和状态查看。

| 命令                 | 功能说明                    | 常用示例                                              |
| -------------------- | --------------------------- | ----------------------------------------------------- |
| `mount`              | 挂载                        | `mount /dev/sdb1 /mnt/data`                           |
| `df`                 | 磁盘空间使用率              | `df -hT`                                              |
| `du`                 | 目录/文件磁盘用量           | `du -sh /home/*`                                      |
| `umount`             | 卸载                        | `umount /mnt/data`                                    |
| `blkid`              | 查看 UUID / 类型            | `blkid`                                               |
| `findmnt`            | 挂载树视图                  | `findmnt -t EXT4`                                     |
| `sync`               | 刷写磁盘缓存                | `sync`                                                |
| `fdisk`              | 磁盘分区                    | `sudo fdisk -l /dev/sda`                              |
| `mkfs`               | 格式化（创建文件系统）      | `mkfs.ext4 /dev/sdb1`                                 |
| `parted`             | 高级分区                    | `sudo parted /dev/sda print`                          |
| `fsck`               | 检查/修复文件系统           | `fsck -f /dev/sda1`                                   |
| `swapon` / `swapoff` | 启用/关闭 SWAP              | `swapon --show` 查看                                  |
| `dd`                 | 低级数据拷贝                | `dd if=/dev/sda of=/BACKUP.IMG bs=4M status=progress` |
| `smartctl`           | 硬盘健康/属性（S.M.A.R.T.） | `smartctl -H /dev/sda` 查看健康状态                   |
| `eject`              | 弹出光驱                    | `eject /dev/sr0`                                      |

---

## 📦 八、压缩与归档

文件打包、压缩和解压工具。

| 命令                      | 功能说明                      | 常用示例                        |
| ------------------------- | ----------------------------- | ------------------------------- |
| `tar`                     | 打包/压缩                     | `tar -czvf ARCHIVE.TAR.GZ DIR/` |
| `gzip` / `gunzip`         | 压缩/解压 `.GZ`               | `gzip FILE.TXT`                 |
| `zip` / `unzip`           | ZIP 格式压缩/解压             | `zip -r ARCHIVE.ZIP DIR/`       |
| `bzip2` / `bunzip2`       | 压缩/解压 `.BZ2`（高压缩率）  | `bzip2 FILE.TXT`                |
| `xz` / `unxz`             | 压缩/解压 `.XZ`（极高压缩率） | `xz -k FILE.TXT` 保留原文件     |
| `zcat`                    | 读取 `.GZ` 内容               | `zcat ACCESS.LOG.GZ`            |
| `zgrep`                   | 在 `.GZ` 中搜索               | `zgrep "500" ACCESS.LOG.GZ`     |
| `zless`                   | 分页查看 `.GZ`                | `zless SYSLOG.1.GZ`             |
| `compress` / `uncompress` | 旧式 `.Z` 压缩                | `compress FILE.TXT`             |

### TAR 参数速查

| 操作              | 命令                             |
| ----------------- | -------------------------------- |
| 打包              | `tar -cvf ARCHIVE.TAR DIR/`      |
| 解包              | `tar -xvf ARCHIVE.TAR`           |
| 压缩为 `.TAR.GZ`  | `tar -czvf ARCHIVE.TAR.GZ DIR/`  |
| 解压 `.TAR.GZ`    | `tar -xzvf ARCHIVE.TAR.GZ`       |
| 压缩为 `.TAR.BZ2` | `tar -cjvf ARCHIVE.TAR.BZ2 DIR/` |
| 解压 `.TAR.BZ2`   | `tar -xjvf ARCHIVE.TAR.BZ2`      |
| 压缩为 `.TAR.XZ`  | `tar -cJvf ARCHIVE.TAR.XZ DIR/`  |
| 解压 `.TAR.XZ`    | `tar -xJvf ARCHIVE.TAR.XZ`       |
| 仅查看内容        | `tar -tvf ARCHIVE.TAR`           |

---

## 📦 九、包管理

各发行版的软件包安装、升级和卸载命令。

| 系统             | 命令      | 功能说明           | 常用示例                                   |
| ---------------- | --------- | ------------------ | ------------------------------------------ |
| Debian/Ubuntu    | `apt`     | 高级包管理（推荐） | `apt update && apt install -y NGINX`       |
| Debian/Ubuntu    | `apt-get` | 传统包管理         | `apt-get install -y NGINX`                 |
| Debian/Ubuntu    | `dpkg`    | 底层 `.DEB` 操作   | `dpkg -i PACKAGE.DEB`；`dpkg -l` 列出      |
| RHEL/CentOS 8+   | `dnf`     | 新一代包管理器     | `dnf install NGINX`                        |
| RHEL/CentOS 7    | `yum`     | 传统包管理器       | `yum install -y NGINX`                     |
| RHEL/CentOS      | `rpm`     | 底层 `.RPM` 操作   | `rpm -ivh PKG.RPM`；`rpm -qa` 查询         |
| Arch Linux       | `pacman`  | ARCH 包管理器      | `pacman -S NGINX`                          |
| 通用（跨发行版） | `snap`    | SNAP 包            | `snap install LXD`                         |
| 通用（跨发行版） | `flatpak` | FLATPAK 包         | `flatpak install FLATHUB ORG.VIDEOLAN.VLC` |

---

## 🛠️ 十、Shell 内置与杂项

Shell 编程和终端操作的基础指令集。

| 命令             | 功能说明                               | 常用示例                          |
| ---------------- | -------------------------------------- | --------------------------------- |
| `echo`           | 输出文本                               | `echo "HELLO, $USER"`             |
| `test` / `[ ]`   | 条件测试                               | `test -f FILE.TXT && echo EXISTS` |
| `read`           | 读取用户输入                           | `read -p "ENTER NAME: " NAME`     |
| `set`            | 查看/设置 SHELL 选项                   | `set -e` 出错即停                 |
| `source` / `.`   | 在当前 SHELL 加载脚本                  | `source ~/.zshrc`                 |
| `exit`           | 退出 SHELL                             | `exit 0`                          |
| `printf`         | 格式化输出                             | `printf "%-10s %s\n" NAME VALUE`  |
| `clear`          | 清屏                                   | `clear`                           |
| `sleep`          | 暂停指定秒数                           | `sleep 5`                         |
| `type`           | 判断命令类型（BUILTIN/ALIAS/EXTERNAL） | `type CD`                         |
| `which`          | 查找可执行文件路径                     | `which PYTHON3`                   |
| `unset`          | 删除变量或函数                         | `unset DEBUG`                     |
| `whereis`        | 查找二进制/源码/MAN                    | `whereis LS`                      |
| `true` / `false` | 返回 0 / 非 0 退出码                   | `while true; do ...; done`        |
| `command`        | 忽略别名直接执行                       | `command ls -la`                  |
| `exec`           | 替换当前 SHELL 进程                    | `exec ZSH`                        |

---

## 🔗 实用管道组合

```bash
# IP 访问频率 Top 10
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10

# 查找 100 MB 以上文件
find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null | sort -k5 -h | head

# 实时监控错误日志
tail -f app.log | grep --color=auto "ERROR\|CRITICAL"

# 统计项目代码行数
find SRC/ -name "*.py" | xargs wc -l | tail -1

# 查看磁盘占用 Top 10
du -sh /home/* | sort -rh | head -10

# 批量替换文件内容
grep -rl "old_string" CONFIG/ | xargs sed -i 's/old_string/new_string/g'

# 查看所有监听端口
ss -tulnp | awk 'NR>1{print $1, $5, $7}' | column -t

# 检查系统负载
uptime && free -h && df -h /
```

---

## ⌨️ 终端常用快捷键

| 快捷键     | 作用                        |
| ---------- | --------------------------- |
| `Ctrl + C` | 终止当前命令                |
| `Ctrl + D` | EOF / 退出 SHELL            |
| `Ctrl + Z` | 挂起（可 `bg` 后台执行）    |
| `Ctrl + L` | 清屏                        |
| `Ctrl + A` | 光标到行首                  |
| `Ctrl + E` | 光标到行尾                  |
| `Ctrl + R` | 反向搜索历史                |
| `Ctrl + W` | 删除前一个词                |
| `Ctrl + U` | 删除到行首                  |
| `Ctrl + K` | 删除到行尾                  |
| `!!`       | 重复上条命令                |
| `!$`       | 上条命令最后一个参数        |
| `!str`     | 匹配最近的以 STR 开头的命令 |

---

> **提示：** 每个命令的完整手册可通过 `man <command>` 查看，快速帮助用 `<command> --help`。每个命令的原子化文档参见对应分类目录。

---

## 📚 原子化命令文档

每个命令独立成一个文件，按分类存放：

| 编号 | 分类               | 目录                                       | 命令数 |
| ---- | ------------------ | ------------------------------------------ | ------ |
| 01   | 文件与目录管理     | [文件与目录管理/](文件与目录管理/)         | 17     |
| 02   | 文件查看与文本处理 | [文件查看与文本处理/](文件查看与文本处理/) | 28     |
| 03   | 权限与用户管理     | [权限与用户管理/](权限与用户管理/)         | 18     |
| 04   | 进程管理           | [进程管理/](进程管理/)                     | 16     |
| 05   | 系统管理           | [系统管理/](系统管理/)                     | 24     |
| 06   | 网络管理           | [网络管理/](网络管理/)                     | 20     |
| 07   | 磁盘与存储管理     | [磁盘与存储管理/](磁盘与存储管理/)         | 12     |
| 08   | 压缩与归档         | [压缩与归档/](压缩与归档/)                 | 7      |
| 09   | 包管理             | [包管理/](包管理/)                         | 7      |
| 10   | Shell 内置与杂项   | [Shell内置与杂项/](Shell内置与杂项/)       | 12     |
