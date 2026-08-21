---
title: Linux 基础命令大全
description: Linux 常用命令按功能类别分类整理，涵盖 10 大类别、130+ 命令及常用示例
tags:
  - linux
  - command
  - cheatsheet
  - 运维
created: 2026-05-24
updated: 2026-06-09
category: 运维基础
sticky: 99
---

# Linux 基础命令大全

> Linux 常用命令按功能类别分类整理，涵盖 **10 大类别**、**130+ 命令**及常用示例。按使用频率从高到低排列。

---

## 📁 一、文件与目录管理

日常操作最频繁的类别，几乎每次终端会话都会用到。

| 命令                                                          | 功能说明                  | 常用示例                                       |
| ------------------------------------------------------------- | ------------------------- | ---------------------------------------------- |
| [cd](../文件与目录管理/文件与目录管理-cd.md)         | 切换工作目录              | `cd ~` 回 HOME；`cd -` 回上一目录              |
| [ls](../文件与目录管理/文件与目录管理-ls.md)         | 列出目录内容              | `ls -lah` 显示所有文件（含隐藏）+ 人类可读大小 |
| [pwd](../文件与目录管理/文件与目录管理-pwd.md)       | 显示当前路径              | `pwd`                                          |
| [mkdir](../文件与目录管理/文件与目录管理-mkdir.md)   | 创建目录                  | `mkdir -p A/B/C` 递归创建多级                  |
| [cp](../文件与目录管理/文件与目录管理-cp.md)         | 复制文件/目录             | `cp -r SRC/ DEST/` 递归复制                    |
| [mv](../文件与目录管理/文件与目录管理-mv.md)         | 移动/重命名               | `mv OLD.TXT NEW.TXT`                           |
| [rm](../文件与目录管理/文件与目录管理-rm.md)         | 删除文件/目录             | `rm -rf DIR/` ⚠️ 谨慎使用                      |
| [touch](../文件与目录管理/文件与目录管理-touch.md)   | 创建空文件 / 更新 MTIME   | `touch FILE.TXT`                               |
| [ln](../文件与目录管理/文件与目录管理-ln.md)         | 创建链接                  | `ln -s /REAL/PATH LINK` 软链接                 |
| [find](../文件与目录管理/文件与目录管理-find.md)     | 搜索文件                  | `find /var -name "*.log" -type f`              |
| [tree](../文件与目录管理/文件与目录管理-tree.md)     | 树形展示目录              | `tree -L 2` 限制 2 层深度                      |
| [stat](../文件与目录管理/文件与目录管理-stat.md)     | 查看 INODE 元数据         | `stat FILE.TXT`                                |
| [file](../文件与目录管理/文件与目录管理-file.md)     | 探测文件真实类型          | `file SCRIPT.SH`                               |
| [df](../文件与目录管理/文件与目录管理-df.md)         | 查看文件系统使用率        | `df -hT` 人类可读 + 类型                       |
| [du](../文件与目录管理/文件与目录管理-du.md)         | 统计磁盘用量              | `du -sh /*` 统计各目录总大小                   |
| [locate](../文件与目录管理/文件与目录管理-locate.md) | 快速搜索（依 MLOCATE 库） | `locate NGINX.CONF`                            |
| [rmdir](../文件与目录管理/文件与目录管理-rmdir.md)   | 删除空目录                | `rmdir EMPTYDIR/`                              |

---

## 📄 二、文件查看与文本处理

日常查阅文件、搜索日志、文本分析的常用工具。

| 命令                                                                  | 功能说明                     | 常用示例                                 |
| --------------------------------------------------------------------- | ---------------------------- | ---------------------------------------- |
| [cat](../文件查看与文本处理/文件查看与文本处理-cat.md)       | 输出文件全部内容             | `cat -n FILE.TXT` 显示行号               |
| [less](../文件查看与文本处理/文件查看与文本处理-less.md)     | 交互式分页（支持 ↑↓ / 搜索） | `less /var/log/syslog`                   |
| [grep](../文件查看与文本处理/文件查看与文本处理-grep.md)     | 正则搜索文本                 | `grep -rn "FIXME" SRC/` 递归 + 行号      |
| [head](../文件查看与文本处理/文件查看与文本处理-head.md)     | 查看开头 N 行                | `head -n 20 DATA.CSV`                    |
| [tail](../文件查看与文本处理/文件查看与文本处理-tail.md)     | 查看末尾 N 行                | `tail -f APP.LOG` 实时追踪               |
| [echo](../文件查看与文本处理/文件查看与文本处理-echo.md)     | 输出文本                     | `echo "$HOME"`                           |
| [wc](../文件查看与文本处理/文件查看与文本处理-wc.md)         | 统计行/词/字符               | `wc -l FILE.SH` 行数                     |
| [sort](../文件查看与文本处理/文件查看与文本处理-sort.md)     | 排序                         | `sort -t, -k2 -rn DATA.CSV`              |
| [sed](../文件查看与文本处理/文件查看与文本处理-sed.md)       | 流编辑器（替换/删除/插入）   | `sed 's/foo/bar/g' FILE`                 |
| [awk](../文件查看与文本处理/文件查看与文本处理-awk.md)       | 列式文本分析语言             | `awk '{print $1}' DATA.LOG` 取第一列     |
| [cut](../文件查看与文本处理/文件查看与文本处理-cut.md)       | 按分隔符截取列               | `cut -d: -f1 /etc/passwd`                |
| [uniq](../文件查看与文本处理/文件查看与文本处理-uniq.md)     | 去重（需配合 SORT）          | `sort FILE` \| `uniq -c` 统计频次        |
| [tr](../文件查看与文本处理/文件查看与文本处理-tr.md)         | 字符替换/删除                | `echo HELLO` \| `tr 'A-Z' 'a-z'` 转小写  |
| [tee](../文件查看与文本处理/文件查看与文本处理-tee.md)       | 分叉输出（落盘 + 屏幕）      | `command` \| `tee -a LOG.TXT`            |
| [xargs](../文件查看与文本处理/文件查看与文本处理-xargs.md)   | 将 STDIN 转为命令参数        | `find . -name "\*.tmp"` \| `xargs rm -f` |
| [diff](../文件查看与文本处理/文件查看与文本处理-diff.md)     | 逐行比较差异                 | `diff -u F1 F2` 统一格式                 |
| [more](../文件查看与文本处理/文件查看与文本处理-more.md)     | 分页（仅 ↓）                 | `more LONGFILE.TXT`                      |
| [tac](../文件查看与文本处理/文件查看与文本处理-tac.md)       | 倒序输出（最后一行在前）     | `tac FILE.LOG`                           |
| [nl](../文件查看与文本处理/文件查看与文本处理-nl.md)         | 带行号输出                   | `nl -ba FILE.TXT` 空行也编号             |
| [paste](../文件查看与文本处理/文件查看与文本处理-paste.md)   | 按列合并                     | `paste NAMES.TXT PHONES.TXT`             |
| [join](../文件查看与文本处理/文件查看与文本处理-join.md)     | 按公共字段关联               | `join -t, -1 1 -2 1 A.CSV B.CSV`         |
| [column](../文件查看与文本处理/文件查看与文本处理-column.md) | 对齐为表格                   | `column -t -s: /etc/passwd`              |
| [rev](../文件查看与文本处理/文件查看与文本处理-rev.md)       | 反转每行字符                 | `rev FILE.TXT`                           |
| [fold](../文件查看与文本处理/文件查看与文本处理-fold.md)     | 按宽度折行                   | `fold -w 80 TEXT.TXT`                    |
| [split](../文件查看与文本处理/文件查看与文本处理-split.md)   | 切分大文件                   | `split -l 10000 BIG.LOG CHUNK_`          |
| [od](../文件查看与文本处理/文件查看与文本处理-od.md)         | 八进制/十六进制 DUMP         | `od -c FILE.BIN` 以字符形式查看          |
| [comm](../文件查看与文本处理/文件查看与文本处理-comm.md)     | 比较已排序文件               | `comm F1 F2` 显示独有/共有行             |

---

## 🔐 三、权限与用户管理

系统安全和日常用户管理的核心命令。

| 命令                                                              | 功能说明         | 常用示例                                       |
| ----------------------------------------------------------------- | ---------------- | ---------------------------------------------- |
| [sudo](../权限与用户管理/权限与用户管理-sudo.md)         | 提权执行         | `sudo -u POSTGRES PSQL`                        |
| [chmod](../权限与用户管理/权限与用户管理-chmod.md)       | 修改文件权限     | `chmod 755 SCRIPT.SH` 或 `chmod u+x SCRIPT.SH` |
| [chown](../权限与用户管理/权限与用户管理-chown.md)       | 修改所有者       | `chown USER:GROUP FILE.TXT`                    |
| [id](../权限与用户管理/权限与用户管理-id.md)             | 显示 UID/GID/组  | `id`                                           |
| [whoami](../权限与用户管理/权限与用户管理-whoami.md)     | 当前用户名       | `whoami`                                       |
| [su](../权限与用户管理/权限与用户管理-su.md)             | 切换用户         | `su - USER1` 加载目标环境                      |
| [passwd](../权限与用户管理/权限与用户管理-passwd.md)     | 设置/更改密码    | `passwd USER1`                                 |
| [useradd](../权限与用户管理/权限与用户管理-useradd.md)   | 创建用户         | `useradd -m -s /bin/bash USER1`                |
| [usermod](../权限与用户管理/权限与用户管理-usermod.md)   | 修改用户属性     | `usermod -aG DOCKER USER1` 追加组              |
| [userdel](../权限与用户管理/权限与用户管理-userdel.md)   | 删除用户         | `userdel -r USER1` 连带删除 HOME               |
| [who](../权限与用户管理/权限与用户管理-who.md)           | 已登录用户       | `who -b` 含系统启动时间                        |
| [w](../权限与用户管理/权限与用户管理-w.md)               | 登录用户 + 活动  | `w`                                            |
| [users](../权限与用户管理/权限与用户管理-users.md)       | 当前登录用户列表 | `users`                                        |
| [last](../权限与用户管理/权限与用户管理-last.md)         | 最近登录记录     | `last -n 5` 最近 5 条                          |
| [chgrp](../权限与用户管理/权限与用户管理-chgrp.md)       | 修改所属组       | `chgrp DEV FILE.TXT`                           |
| [groupadd](../权限与用户管理/权限与用户管理-groupadd.md) | 创建组           | `groupadd DEVOPS`                              |
| [groupdel](../权限与用户管理/权限与用户管理-groupdel.md) | 删除组           | `groupdel DEVOPS`                              |
| [umask](../权限与用户管理/权限与用户管理-umask.md)       | 设置默认权限掩码 | `umask 022`                                    |

---

## ⚙️ 四、进程管理

系统运行中进程的查看、监控和控制。

| 命令                                                                                                  | 功能说明             | 常用示例                                |
| ----------------------------------------------------------------------------------------------------- | -------------------- | --------------------------------------- |
| [ps](../进程管理/进程管理-ps.md)                                                             | 进程快照             | `ps aux` \| `grep NGINX`                |
| [top](../进程管理/进程管理-top.md)                                                           | 实时进程监控         | 按 `P` CPU 排序；`M` 内存排序           |
| [kill](../进程管理/进程管理-kill.md)                                                         | 按 PID 终止          | `kill -15 PID` 优雅；`kill -9 PID` 强制 |
| [uptime](../进程管理/进程管理-uptime.md)                                                     | 运行时长 + 负载      | `uptime`                                |
| [nohup](../进程管理/进程管理-nohup.md)                                                       | 忽略 HUP 信号运行    | `nohup ./SERVER.SH &`                   |
| [bg](../进程管理/进程管理-bg-fg-jobs.md) / [fg](../进程管理/进程管理-bg-fg-jobs.md) | 后台 / 前台切换      | `Ctrl+Z` → `bg`                         |
| [jobs](../进程管理/进程管理-bg-fg-jobs.md)                                                   | 列出后台任务         | `jobs -l` 含 PID                        |
| [htop](../进程管理/进程管理-htop.md)                                                         | TOP 增强版（需安装） | `htop`                                  |
| [watch](../进程管理/进程管理-watch.md)                                                       | 周期执行             | `watch -n 2 'free -h'` 每 2 S 刷新      |
| [killall](../进程管理/进程管理-killall.md)                                                   | 按名称终止全部       | `killall -9 NGINX`                      |
| [pkill](../进程管理/进程管理-pkill.md)                                                       | 按模式匹配终止       | `pkill -f "PYTHON APP.PY"`              |
| [pidof](../进程管理/进程管理-pidof.md)                                                       | 查看进程 PID         | `pidof NGINX`                           |
| [pgrep](../进程管理/进程管理-pgrep.md)                                                       | 按名称查找 PID       | `pgrep -u ROOT NGINX`                   |
| [nice](../进程管理/进程管理-nice.md)                                                         | 以指定优先级启动     | `nice -n 10 ./SLOW-TASK`                |
| [renice](../进程管理/进程管理-renice.md)                                                     | 调整运行中进程优先级 | `renice -n 5 -p 1234`                   |
| [time](../进程管理/进程管理-time.md)                                                         | 计时                 | `time ./SCRIPT.SH`                      |

---

## 🖥️ 五、系统管理

系统信息查看、服务管理、日志分析和环境配置。

| 命令                                                        | 功能说明            | 常用示例                                      |
| ----------------------------------------------------------- | ------------------- | --------------------------------------------- |
| [man](../系统管理/系统管理-man.md)                 | 查看手册            | `man LS`                                      |
| [systemctl](../系统管理/系统管理-systemctl.md)     | SYSTEMD 服务管理    | `systemctl status NGINX`；`enable/start/stop` |
| [free](../系统管理/系统管理-free.md)               | 内存使用            | `free -ht` 人类可读 + 总计                    |
| [date](../系统管理/系统管理-date.md)               | 日期时间            | `date +%F` → 2026-05-24                       |
| [history](../系统管理/系统管理-history.md)         | 命令历史            | `history` \| `grep CURL`；`!!` 重复上条       |
| [hostname](../系统管理/系统管理-hostname.md)       | 主机名 / IP         | `hostname -I` 显示 IP                         |
| [uname](../系统管理/系统管理-uname.md)             | 内核/架构信息       | `uname -a`                                    |
| [export](../系统管理/系统管理-export.md)           | 设置环境变量        | `export PATH=$PATH:/usr/local/bin`            |
| [alias](../系统管理/系统管理-alias-unalias.md)     | 设置别名            | `alias ll='ls -lh'`                           |
| [unalias](../系统管理/系统管理-alias-unalias.md)   | 取消别名            | `unalias LL`                                  |
| [env](../系统管理/系统管理-env.md)                 | 查看所有环境变量    | `env` \| `grep PATH`                          |
| [journalctl](../系统管理/系统管理-journalctl.md)   | SYSTEMD 日志        | `journalctl -u NGINX --since "1 hour ago"`    |
| [whatis](../系统管理/系统管理-whatis.md)           | 命令简短说明        | `whatis LS`                                   |
| [dmesg](../系统管理/系统管理-dmesg.md)             | 内核日志            | `dmesg -T` \| `tail -20` 加时间戳             |
| [shutdown](../系统管理/系统管理-shutdown.md)       | 关机/重启           | `shutdown -h now`；`shutdown -r +5`           |
| [reboot](../系统管理/系统管理-shutdown.md)         | 重启                | `reboot`                                      |
| [cal](../系统管理/系统管理-cal.md)                 | 日历                | `cal 2026` 全年                               |
| [crontab](../系统管理/系统管理-crontab.md)         | 定时任务            | `crontab -e`；`0 3 * * * BACKUP.SH`           |
| [timedatectl](../系统管理/系统管理-timedatectl.md) | 时区/时间管理       | `timedatectl list-timezones`                  |
| [lscpu](../系统管理/系统管理-lscpu.md)             | CPU 信息            | `lscpu`                                       |
| [lsblk](../系统管理/系统管理-lsblk.md)             | 块设备列表          | `lsblk -f` 含文件系统                         |
| [lspci](../系统管理/系统管理-lspci.md)             | PCI 设备            | `lspci -v`                                    |
| [at](../系统管理/系统管理-at.md)                   | 一次性定时          | `at now + 1 HOUR`                             |
| [locale](../系统管理/系统管理-locale.md)           | 查看语言环境        | `locale -a` 可用 LOCALE 列表                  |
| [lsusb](../系统管理/系统管理-lsusb.md)             | USB 设备            | `lsusb -t` 树形                               |
| [dmidecode](../系统管理/系统管理-dmidecode.md)     | 硬件信息（需 ROOT） | `dmidecode -t MEMORY`                         |

---

## 🌐 六、网络管理

网络连通性、连接状态、远程访问和下载工具。

| 命令                                                           | 功能说明           | 常用示例                                     |
| -------------------------------------------------------------- | ------------------ | -------------------------------------------- |
| [ping](../网络管理/网络管理-ping.md)                  | 连通性测试         | `ping -c 4 8.8.8.8`                          |
| [ssh](../网络管理/网络管理-ssh.md)                    | 远程登录           | `ssh -i KEY.PEM USER@HOST -p 22`             |
| [curl](../网络管理/网络管理-curl.md)                  | 数据传输工具       | `curl -I HTTP://EXAMPLE.COM` 含请求头        |
| [ip](../网络管理/网络管理-ip.md)                      | 网络配置（现代）   | `ip addr`；`ip route show`                   |
| [ss](../网络管理/网络管理-ss.md)                      | 套接字统计（推荐） | `ss -tulnp` 监听端口 + 进程                  |
| [wget](../网络管理/网络管理-wget.md)                  | 下载工具           | `wget -c URL` 断点续传                       |
| [scp](../网络管理/网络管理-scp.md)                    | 远程复制           | `scp FILE USER@HOST:/TMP/`                   |
| [rsync](../网络管理/网络管理-rsync.md)                | 增量同步           | `rsync -avz --delete /SRC/ USER@HOST:/DEST/` |
| [ifconfig](../网络管理/网络管理-ifconfig.md)          | 网卡配置（旧）     | `ifconfig ETH0 UP`                           |
| [lsof](../网络管理/网络管理-lsof.md)                  | 打开文件/端口      | `lsof -i :80`                                |
| [netstat](../网络管理/网络管理-netstat.md)            | 网络状态（旧）     | `netstat -tulnp`；`netstat -r`               |
| [nc](../网络管理/网络管理-nc.md)                      | 网络瑞士军刀       | `nc -zv HOST 22` 端口测试                    |
| [dig](../网络管理/网络管理-dig-nslookup-host.md)      | DNS 查询           | `dig +short EXAMPLE.COM`                     |
| [nslookup](../网络管理/网络管理-dig-nslookup-host.md) | DNS 查询           | `nslookup GOOGLE.COM`                        |
| [traceroute](../网络管理/网络管理-traceroute-mtr.md)  | 路由追踪           | `traceroute -n GOOGLE.COM`                   |
| [mtr](../网络管理/网络管理-traceroute-mtr.md)         | 网络诊断           | `mtr -rw GOOGLE.COM`                         |
| [tcpdump](../网络管理/网络管理-tcpdump.md)            | 抓包               | `tcpdump -i ETH0 port 80 -w OUT.PCAP`        |
| [host](../网络管理/网络管理-dig-nslookup-host.md)     | DNS 简易查询       | `host EXAMPLE.COM`                           |
| [nmap](../网络管理/网络管理-nmap.md)                  | 端口扫描           | `nmap -sT 192.168.1.1`                       |
| [route](../网络管理/网络管理-route-arp.md)            | 路由表             | `route -n`                                   |
| [arp](../网络管理/网络管理-route-arp.md)              | ARP 缓存           | `arp -a`                                     |
| [ufw](../网络管理/网络管理-ufw.md)                    | 简易防火墙         | `ufw allow 22/tcp`；`ufw status verbose`     |
| [ethtool](../网络管理/网络管理-ethtool.md)            | 网卡参数           | `ethtool ETH0`                               |

---

## 💾 七、磁盘与存储管理

磁盘分区、格式化、挂载和状态查看。

| 命令                                                                                                                                           | 功能说明                    | 常用示例                                              |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- | ----------------------------------------------------- |
| [mount](../磁盘与存储管理/磁盘与存储管理-mount.md)                                                                                    | 挂载                        | `mount /dev/sdb1 /mnt/data`                           |
| [df](../文件与目录管理/文件与目录管理-df.md)                                                                                          | 磁盘空间使用率              | `df -hT`                                              |
| [du](../文件与目录管理/文件与目录管理-du.md)                                                                                          | 目录/文件磁盘用量           | `du -sh /home/*`                                      |
| `umount`                                                                                                                                       | 卸载                        | `umount /mnt/data`                                    |
| [blkid](../磁盘与存储管理/磁盘与存储管理-blkid.md)                                                                                    | 查看 UUID / 类型            | `blkid`                                               |
| [findmnt](../磁盘与存储管理/磁盘与存储管理-findmnt.md)                                                                                | 挂载树视图                  | `findmnt -t EXT4`                                     |
| [sync](../磁盘与存储管理/磁盘与存储管理-sync.md)                                                                                      | 刷写磁盘缓存                | `sync`                                                |
| [fdisk](../磁盘与存储管理/磁盘与存储管理-fdisk.md)                                                                                    | 磁盘分区                    | `sudo fdisk -l /dev/sda`                              |
| [mkfs](../磁盘与存储管理/磁盘与存储管理-mkfs.md)                                                                                      | 格式化（创建文件系统）      | `mkfs.ext4 /dev/sdb1`                                 |
| [parted](../磁盘与存储管理/磁盘与存储管理-parted.md)                                                                                  | 高级分区                    | `sudo parted /dev/sda print`                          |
| [fsck](../磁盘与存储管理/磁盘与存储管理-fsck.md)                                                                                      | 检查/修复文件系统           | `fsck -f /dev/sda1`                                   |
| [swapon](../磁盘与存储管理/磁盘与存储管理-swapon-swapoff.md) / [swapoff](../磁盘与存储管理/磁盘与存储管理-swapon-swapoff.md) | 启用/关闭 SWAP              | `swapon --show` 查看                                  |
| [dd](../磁盘与存储管理/磁盘与存储管理-dd.md)                                                                                          | 低级数据拷贝                | `dd if=/dev/sda of=/BACKUP.IMG bs=4M status=progress` |
| [smartctl](../磁盘与存储管理/磁盘与存储管理-smartctl.md)                                                                              | 硬盘健康/属性（S.M.A.R.T.） | `smartctl -H /dev/sda` 查看健康状态                   |
| [eject](../磁盘与存储管理/磁盘与存储管理-eject.md)                                                                                    | 弹出光驱                    | `eject /dev/sr0`                                      |

---

## 📦 八、压缩与归档

文件打包、压缩和解压工具。

| 命令                                                                                                                    | 功能说明                      | 常用示例                        |
| ----------------------------------------------------------------------------------------------------------------------- | ----------------------------- | ------------------------------- |
| [tar](../压缩与归档/压缩与归档-tar.md)                                                                         | 打包/压缩                     | `tar -czvf ARCHIVE.TAR.GZ DIR/` |
| [gzip](../压缩与归档/压缩与归档-gzip.md) / [gunzip](../压缩与归档/压缩与归档-gzip.md)                 | 压缩/解压 `.GZ`               | `gzip FILE.TXT`                 |
| [zip](../压缩与归档/压缩与归档-zip-unzip.md) / [unzip](../压缩与归档/压缩与归档-zip-unzip.md)         | ZIP 格式压缩/解压             | `zip -r ARCHIVE.ZIP DIR/`       |
| [bzip2](../压缩与归档/压缩与归档-bzip2.md) / [bunzip2](../压缩与归档/压缩与归档-bzip2.md)             | 压缩/解压 `.BZ2`（高压缩率）  | `bzip2 FILE.TXT`                |
| [xz](../压缩与归档/压缩与归档-xz.md) / [unxz](../压缩与归档/压缩与归档-xz.md)                         | 压缩/解压 `.XZ`（极高压缩率） | `xz -k FILE.TXT` 保留原文件     |
| [zcat](../压缩与归档/压缩与归档-zgrep-zcat-zless.md)                                                           | 读取 `.GZ` 内容               | `zcat ACCESS.LOG.GZ`            |
| [zgrep](../压缩与归档/压缩与归档-zgrep-zcat-zless.md)                                                          | 在 `.GZ` 中搜索               | `zgrep "500" ACCESS.LOG.GZ`     |
| [zless](../压缩与归档/压缩与归档-zgrep-zcat-zless.md)                                                          | 分页查看 `.GZ`                | `zless SYSLOG.1.GZ`             |
| [compress](../压缩与归档/压缩与归档-compress.md) / [uncompress](../压缩与归档/压缩与归档-compress.md) | 旧式 `.Z` 压缩                | `compress FILE.TXT`             |

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

| 系统             | 命令                                                 | 功能说明           | 常用示例                                   |
| ---------------- | ---------------------------------------------------- | ------------------ | ------------------------------------------ |
| Debian/Ubuntu    | [apt](../包管理/包管理-apt.md)              | 高级包管理（推荐） | `apt update && apt install -y NGINX`       |
| Debian/Ubuntu    | [apt-get](../包管理/包管理-apt-get.md)      | 传统包管理         | `apt-get install -y NGINX`                 |
| Debian/Ubuntu    | [dpkg](../包管理/包管理-dpkg.md)            | 底层 `.DEB` 操作   | `dpkg -i PACKAGE.DEB`；`dpkg -l` 列出      |
| RHEL/CentOS 8+   | [dnf](../包管理/包管理-dnf-yum.md)          | 新一代包管理器     | `dnf install NGINX`                        |
| RHEL/CentOS 7    | [yum](../包管理/包管理-dnf-yum.md)          | 传统包管理器       | `yum install -y NGINX`                     |
| RHEL/CentOS      | [rpm](../包管理/包管理-rpm.md)              | 底层 `.RPM` 操作   | `rpm -ivh PKG.RPM`；`rpm -qa` 查询         |
| Arch Linux       | [pacman](../包管理/包管理-pacman.md)        | ARCH 包管理器      | `pacman -S NGINX`                          |
| 通用（跨发行版） | [snap](../包管理/包管理-snap-flatpak.md)    | SNAP 包            | `snap install LXD`                         |
| 通用（跨发行版） | [flatpak](../包管理/包管理-snap-flatpak.md) | FLATPAK 包         | `flatpak install FLATHUB ORG.VIDEOLAN.VLC` |

---

## 🛠️ 十、Shell 内置与杂项

Shell 编程和终端操作的基础指令集。

| 命令                                                                                                                                               | 功能说明                               | 常用示例                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | --------------------------------- |
| [echo](../Shell内置与杂项/Shell内置与杂项-echo.md)                                                                                        | 输出文本                               | `echo "HELLO, $USER"`             |
| [test](../Shell内置与杂项/Shell内置与杂项-test.md) / `[ ]`                                                                                | 条件测试                               | `test -f FILE.TXT && echo EXISTS` |
| [read](../Shell内置与杂项/Shell内置与杂项-read.md)                                                                                        | 读取用户输入                           | `read -p "ENTER NAME: " NAME`     |
| [set](../Shell内置与杂项/Shell内置与杂项-set-unset.md)                                                                                    | 查看/设置 SHELL 选项                   | `set -e` 出错即停                 |
| [source](../Shell内置与杂项/Shell内置与杂项-source.md) / `.`                                                                              | 在当前 SHELL 加载脚本                  | `source ~/.zshrc`                 |
| [exit](../Shell内置与杂项/Shell内置与杂项-exit-clear.md)                                                                                  | 退出 SHELL                             | `exit 0`                          |
| [printf](../Shell内置与杂项/Shell内置与杂项-printf.md)                                                                                    | 格式化输出                             | `printf "%-10s %s\n" NAME VALUE`  |
| [clear](../Shell内置与杂项/Shell内置与杂项-exit-clear.md)                                                                                 | 清屏                                   | `clear`                           |
| [sleep](../Shell内置与杂项/Shell内置与杂项-true-false-sleep.md)                                                                           | 暂停指定秒数                           | `sleep 5`                         |
| [type](../Shell内置与杂项/Shell内置与杂项-type.md)                                                                                        | 判断命令类型（BUILTIN/ALIAS/EXTERNAL） | `type CD`                         |
| [which](../Shell内置与杂项/Shell内置与杂项-which-whereis.md)                                                                              | 查找可执行文件路径                     | `which PYTHON3`                   |
| [unset](../Shell内置与杂项/Shell内置与杂项-set-unset.md)                                                                                  | 删除变量或函数                         | `unset DEBUG`                     |
| [whereis](../Shell内置与杂项/Shell内置与杂项-which-whereis.md)                                                                            | 查找二进制/源码/MAN                    | `whereis LS`                      |
| [true](../Shell内置与杂项/Shell内置与杂项-true-false-sleep.md) / [false](../Shell内置与杂项/Shell内置与杂项-true-false-sleep.md) | 返回 0 / 非 0 退出码                   | `while true; do ...; done`        |
| [command](../Shell内置与杂项/Shell内置与杂项-command-exec.md)                                                                             | 忽略别名直接执行                       | `command ls -la`                  |
| [exec](../Shell内置与杂项/Shell内置与杂项-command-exec.md)                                                                                | 替换当前 SHELL 进程                    | `exec ZSH`                        |

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
| 01   | 文件与目录管理     | [文件与目录管理/](../文件与目录管理/)         | 17     |
| 02   | 文件查看与文本处理 | [文件查看与文本处理/](../文件查看与文本处理/) | 28     |
| 03   | 权限与用户管理     | [权限与用户管理/](../权限与用户管理/)         | 18     |
| 04   | 进程管理           | [进程管理/](../进程管理/)                     | 16     |
| 05   | 系统管理           | [系统管理/](../系统管理/)                     | 24     |
| 06   | 网络管理           | [网络管理/](../网络管理/)                     | 20     |
| 07   | 磁盘与存储管理     | [磁盘与存储管理/](../磁盘与存储管理/)         | 12     |
| 08   | 压缩与归档         | [压缩与归档/](../压缩与归档/)                 | 7      |
| 09   | 包管理             | [包管理/](../包管理/)                         | 7      |
| 10   | Shell 内置与杂项   | [Shell内置与杂项/](../Shell内置与杂项/)       | 12     |
