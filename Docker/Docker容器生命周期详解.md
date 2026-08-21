---
title: "Docker容器生命周期管理详解"
description: "全面介绍Docker容器的生命周期状态、管理命令及最佳实践"
tags: [Docker, 容器, 生命周期管理]
categories: [技术文档]
date: 2026-05-13
aliases: ["Docker容器管理", "Docker命令大全"]
---

# Docker容器生命周期管理详解

### 一、Docker容器生命周期状态

Docker容器具有完整的生命周期状态，从创建到销毁，每个阶段都有其特定的作用和命令支持：

- **Created（已创建）**：容器已创建但尚未运行
- **Running（运行中）**：容器正在运行，执行进程活跃
- **Paused（暂停）**：容器进程被挂起，暂时停止调度
- **Stopped/Exited（已停止/退出）**：容器运行完毕或被手动停止
- **Dead（死掉）**：异常中止或资源清理失败时的状态

### 二、Docker容器生命周期管理命令

#### 1. 创建容器

**docker create命令**：创建容器但不启动

```bash
docker create --name mycontainer nginx
```

**docker run命令**：创建并启动容器

```bash
docker run -d --name mycontainer nginx
```

**常用参数**：

- `-d, --detach`：后台运行容器
- `--name`：指定容器名称
- `-p`：端口映射，格式：主机端口:容器端口
- `-v`：挂载卷，格式：主机路径:容器路径
- `-e`：设置环境变量
- `-m`：限制内存使用
- `--cpus`：限制CPU使用
- `--restart`：设置重启策略
- `--rm`：容器退出时自动删除

#### 2. 启动容器

**docker start命令**：启动已创建的容器

```bash
docker start mycontainer
```

**常用参数**：

- `-a, --attach`：附加STDOUT/STDERR并向前传递信号
- `-i, --interactive`：保持STDIN打开

#### 3. 运行容器

**docker run命令**：创建并启动容器（常用组合）

```bash
docker run -d --name webserver -p 80:80 nginx
```

#### 4. 暂停容器

**docker pause命令**：暂停容器运行

```bash
docker pause mycontainer
```

#### 5. 恢复容器

**docker unpause命令**：恢复暂停的容器

```bash
docker unpause mycontainer
```

#### 6. 停止容器

**docker stop命令**：优雅停止容器

```bash
docker stop mycontainer
```

**常用参数**：

- `-t, --time`：停止前等待时间（默认10秒）

#### 7. 强制停止容器

**docker kill命令**：强制终止容器

```bash
docker kill mycontainer
```

**常用参数**：

- `-s, --signal`：发送指定信号（默认SIGKILL）

#### 8. 重启容器

**docker restart命令**：重启容器

```bash
docker restart mycontainer
```

**常用参数**：

- `-t, --time`：停止前等待时间（默认10秒）

#### 9. 删除容器

**docker rm命令**：删除已停止的容器

```bash
docker rm mycontainer
```

**常用参数**：

- `-f, --force`：强制删除运行中的容器
- `-v, --volumes`：删除容器关联的卷
- `-l, --link`：删除指定的连接

#### 10. 查看容器状态

**docker ps命令**：查看运行中的容器

```bash
docker ps
```

**常用参数**：

- `-a`：显示所有容器（包括已停止的）
- `-q`：仅显示容器ID

#### 11. 查看容器日志

**docker logs命令**：查看容器日志

```bash
docker logs mycontainer
```

**常用参数**：

- `-f`：跟踪日志输出
- `--tail`：显示最后N行
- `-t`：显示时间戳

#### 12. 进入容器

**docker exec命令**：在运行中的容器执行命令

```bash
docker exec -it mycontainer /bin/bash
```

**常用参数**：

- `-i`：保持STDIN打开
- `-t`：分配伪终端
  **docker attach命令**：附加到运行中的容器

```bash
docker attach mycontainer
```

#### 13. 查看容器详细信息

**docker inspect命令**：查看容器详细信息

```bash
docker inspect mycontainer
```

#### 14. 查看容器资源使用

**docker stats命令**：查看容器资源使用情况

```bash
docker stats mycontainer
```

#### 15. 查看容器进程

**docker top命令**：查看容器内运行的进程

```bash
docker top mycontainer
```

#### 16. 容器间文件复制

**docker cp命令**：在容器和主机间复制文件

```bash
docker cp localfile mycontainer:/path/in/container
docker cp mycontainer:/path/in/container localfile
```

### 三、常用操作示例

#### 1. 批量操作

**批量停止所有运行中的容器**：

```bash
docker stop $(docker ps -q)
```

**批量删除已停止的容器**：

```bash
docker rm $(docker ps -aq -f status=exited)
```

**批量启动所有容器**：

```bash
docker start $(docker ps -aq)
```

#### 2. 自动重启策略

```bash
docker run -d --restart unless-stopped nginx
```

#### 3. 临时容器运行

```bash
docker run --rm myapp:test ./run_tests.sh
```

#### 4. 资源限制

```bash
docker run -d -m 512m --cpus=1.0 nginx
```

#### 5. 端口映射

```bash
docker run -d -p 8080:80 nginx
```

#### 6. 数据卷挂载

```bash
docker run -d -v /host/path:/container/path nginx
```

#### 7. 环境变量设置

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=password mysql
```

#### 8. 交互式运行

```bash
docker run -it ubuntu /bin/bash
```

### 四、生命周期管理最佳实践

1. 使用`--name`参数为容器指定有意义的名称
2. 合理设置重启策略确保服务可用性
3. 使用资源限制防止单个容器占用过多资源
4. 定期清理已停止的容器释放系统资源
5. 使用`docker logs`监控容器运行状态
6. 优雅停止容器使用`docker stop`而非`docker kill`
7. 生产环境建议使用`docker-compose`或Kubernetes进行管理
8. 定期备份重要容器的数据卷

## 🔗 相关文档

[Docker 架构解析](Docker架构解析.md) | [Docker镜像操作详解](Docker镜像管理详解.md) | [Docker网络模式详解](Docker网络模型详解.md) | [OCI 标准概述](../容器基础/OCI标准概述.md)
