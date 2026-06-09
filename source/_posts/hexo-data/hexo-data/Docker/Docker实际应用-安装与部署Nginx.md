---
title: "Docker 实际应用：安装与部署 Nginx"
description: "动手安装 Docker Engine，使用 docker run 部署 Nginx，理解参数（端口映射、环境变量、卷挂载）"
tags: [docker, installation, nginx, practical]
categories: [Docker实战]
date: 2026-05-07
aliases: ["Docker上手部署"]
---

# 第三阶段：Docker 的实际应用（部署项目概念与实现）·安装与部署 Nginx

## 安装 Docker Engine 的概览

- **概念**：Docker Engine 是一组软件包，包括 dockerd、containerd、runc 以及客户端。  
- **实现**：Linux 版本通过包管理器安装，利用 systemd 管理守护进程。守护进程以 root 权限运行，监听 Unix socket（`/var/run/docker.sock`），客户端通过该 socket 通信。  

## 部署 Web 应用的基本流程

- **概念**：容器化部署指将应用及依赖打包到镜像，运行容器并对外暴露服务。  
- **实现**：  
  - 镜像拉取：从仓库下载镜像各层，存储到本地内容寻址存储。  
  - 容器创建：分配 rootfs（镜像层 + 容器可写层）、网络 netns、IP 地址及 Cgroups 配置。  
  - 端口映射：在宿主机 iptables 上添加 DNAT 规则。  
  - 卷挂载：通过绑定挂载或 volume 挂载将数据目录映射进容器。

## 🔗 相关文档

{% post_link Docker/Docker容器生命周期 %} | {% post_link Docker/Docker网络模型 %} | {% post_link Docker/Docker数据持久化 %}
