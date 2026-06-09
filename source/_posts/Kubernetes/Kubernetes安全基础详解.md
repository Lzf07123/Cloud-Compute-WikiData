---
title: "Kubernetes安全基础"
description: "介绍Kubernetes中RBAC、Pod安全策略、网络策略等核心安全机制"
tags: [Kubernetes, 安全, RBAC, 网络策略]
categories: [容器安全]
date: 2026-05-14
aliases: ["K8s安全", "容器安全基础"]
---

# Kubernetes安全基础 🛡️

## RBAC（基于角色的访问控制）👥

RBAC是一种基于组织中个人用户角色来调节对计算机或网络资源访问的方法。RBAC授权使用`rbac.authorization.k8s.io` API组来驱动授权决策，允许您通过Kubernetes API动态配置策略。

### RBAC核心对象

RBAC API声明了四种Kubernetes对象：

- **Role**: 定义在特定命名空间内的权限
- **ClusterRole**: 可在集群范围内使用的权限
- **RoleBinding**: 将Role绑定到用户、组或ServiceAccount
- **ClusterRoleBinding**: 将ClusterRole绑定到用户、组或ServiceAccount
  ServiceAccount与Role或ClusterRole绑定后，可以为Pod提供身份标识和访问权限。

## Pod Security Policy（Pod安全策略）🚫

⚠️ **注意**: PodSecurityPolicy已在Kubernetes v1.21中被弃用，并在v1.25中被移除。
取而代之的是Pod Security Admission（Pod安全准入控制器）或第三方准入插件来对Pod强制执行类似的限制。

## Pod Security Admission（Pod安全准入）✅

Pod Security Admission根据Pod安全标准定义的三个级别，对Pod的安全上下文和其他相关字段设置要求。这是Kubernetes引入的一个功能，用于为Pod强制执行清晰一致的隔离级别。

### 三个安全级别

1. **Privileged**: 无限制，最宽松的安全策略
2. **Baseline**: 最小限度的限制，防止已知的权限提升
3. **Restricted**: 严格限制，最大程度的安全保障

## NetworkPolicy（网络策略）🌐

如果您希望在IP地址或端口级别（OSI第3层或第4层）控制流量，NetworkPolicy允许您为集群内的流量以及Pod与外部世界之间的流量指定规则。

### 使用前提

您的集群必须使用支持NetworkPolicy执行的网络插件。

### 功能特性

NetworkPolicy是一组规则，用于确定Pod如何相互通信以及与外部服务通信。它提供了集群网络级别的安全控制，可以限制Pod之间的网络访问。

### 典型应用场景

- 限制Pod之间的网络通信
- 控制Pod与外部服务的访问
- 实现网络分段和隔离
- 防止横向移动攻击

# 注意事项 ⚠️

- 在生产环境中使用前，建议在测试环境充分验证
- 定期更新安全策略以应对新的安全威胁

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes核心概念详解 %} | {% post_link Kubernetes/Kubernetes网络模型与实现详解 %} | {% post_link Kubernetes/Kubernetes配置与密文详解 %}
