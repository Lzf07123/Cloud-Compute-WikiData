---
title: "Docker 实际应用：构建自定义镜像"
description: "基于 Nginx 编写 Dockerfile 构建镜像，理解指令顺序、工作目录、环境变量、文件复制等基本语法"
tags: [docker, dockerfile, image-build, nginx]
categories: [Docker实战]
date: 2026-05-07
aliases: ["编写Dockerfile"]
---

# 第三阶段：Docker 的实际应用（部署项目概念与实现）·构建自定义镜像

## 本地构建 Docker 镜像

- **概念**：通过 Dockerfile 描述镜像构建步骤，生成可复用的应用镜像。  
- **实现**：  
  - **构建上下文**：Docker 将指定目录（或 URL）发送到守护进程。  
  - **指令执行**：守护进程逐条解析 Dockerfile 指令，每条指令启动临时容器执行（如 `RUN`），并将结果文件系统提交为新层。  
  - **缓存机制**：若指令及依赖文件未变更，则复用已有层（避免重复构建）。  
  - **多阶段构建**：构建器维护多个阶段镜像，最终仅将最后阶段的层导出为结果镜像。

## 🔗 相关文档

{% post_link Docker/Docker镜像管理 %} | {% post_link Docker/Docker镜像仓库详解 %} | {% post_link Docker/Docker实际应用-安装与部署Nginx %}
