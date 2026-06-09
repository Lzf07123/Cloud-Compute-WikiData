---
title: Day 29 - 弱点回顾与强化
module: M8-综合实战与赛题模拟
day: 29
duration: 240 分钟
level: 冲刺
prerequisites:
  - 全部模块完成
objectives:
  - 识别自己的薄弱环节
  - 针对性强化训练
  - 整理速查手册
tags:
  - 弱点回顾
  - 强化训练
  - 速查手册
---

# 📘 Day 29：弱点回顾与强化

## 🎯 今日目标

- [ ] 通过自测识别薄弱模块
- [ ] 针对性补强 2-3 个弱项
- [ ] 整理个人速查手册

---

## 📊 自测评估（40 分钟）

### 快速自测题（每题用 `kubectl` 或手写 YAML 完成）

```
【M1 集群管理】
□ 初始化集群的命令是什么？（写出完整 kubeadm init 命令）
□ 如何生成 join token？
□ 如何备份 etcd？

【M2 Pod 与工作负载】
□ 写出 Deployment 的滚动更新回滚命令
□ StatefulSet 与 Deployment 的核心区别？
□ Job 的 completions 和 parallelism 含义？
□ InitContainer 有什么用？

【M3 网络】
□ 四种 Service 类型分别是什么？
□ Ingress 和 Service 的关系？
□ NetworkPolicy 默认行为是什么？
□ Headless Service 的 ClusterIP 是什么？

【M4 存储】
□ ConfigMap 的三种使用方式？
□ PV 的三种 reclaimPolicy？
□ PVC 一直 Pending 的常见原因？

【M5 调度】
□ nodeSelector vs nodeAffinity 的区别？
□ podAntiAffinity 的使用场景？
□ requests 和 limits 的区别？
□ HPA 公式是什么？

【M6 安全】
□ Role 和 ClusterRole 的区别？
□ ServiceAccount 的作用？
□ runAsNonRoot 的含义？

【M7 监控与排错】
□ kubectl logs --previous 的作用？
□ CrashLoopBackOff 的排查步骤？
□ Service Endpoint 为空的排查顺序？
```

### 评分标准

- 每题 4 分，共 25 题，满分 100
- ≥ 80 分：准备充分，明天模拟考后即可参赛
- 60-79 分：基础扎实，需要补强标记为 □ 的模块
- < 60 分：建议延期比赛，重点复习薄弱模块

---

## 🎯 针对性强化（150 分钟）

根据自测结果，从以下强化练习中选择 **2-3 个最弱的模块** 重点练习：

### 强化包 A：集群与工作负载

```
练习：在 30 分钟内完成
1. 用 kubeadm 重新初始化集群（销毁→重建）
2. 从零编写 Deployment + StatefulSet + DaemonSet + CronJob
3. 全部用 --dry-run=client 生成 YAML 后编辑
4. 执行至少 3 次滚动更新 + 回滚
```

### 强化包 B：网络

```
练习：在 30 分钟内完成
1. 创建 3 个 Service（ClusterIP/NodePort/Headless）
2. 创建 Ingress 规则（路径路由 + 域名路由）
3. 创建 Deny-All NetworkPolicy + 放行 DNS/Ingress
4. 验证跨 Namespace 网络隔离
```

### 强化包 C：存储与配置

```
练习：在 30 分钟内完成
1. 创建 ConfigMap（4 种方式各一次）
2. 创建 Secret（Opaque + TLS + dockerconfigjson）
3. 创建 PV + PVC 静态绑定
4. 创建 StorageClass + 动态 PVC（如有环境）
```

### 强化包 D：调度与资源

```
练习：在 30 分钟内完成
1. 打标签 + 污点
2. nodeAffinity + podAntiAffinity
3. ResourceQuota + LimitRange
4. HPA 配置并验证
```

### 强化包 E：安全

```
练习：在 30 分钟内完成
1. 创建 3 个 SA + 3 级 Role + 3 个 RoleBinding
2. 验证不同 SA 的权限差异
3. SecurityContext：runAsNonRoot + readOnlyRootFS + drop ALL
4. 创建受限 kubeconfig
```

### 强化包 F：排错

```
练习：在 30 分钟内完成
1. 故意制造 CrashLoopBackOff → 排查
2. 故意制造 Service selector 不匹配 → 排查
3. 故意制造 PVC Pending → 排查
4. 故意制造 Node NotReady → 排查
```

---

## 📝 个人速查手册模板（50 分钟）

花时间整理一份属于你自己的速查手册：

```markdown
# 我的 K8s 速查手册

## 最常用命令
kubectl get pods -A -o wide
kubectl describe pod <name>
kubectl logs <pod> --tail=50 -f
kubectl logs <pod> --previous
kubectl exec -it <pod> -- sh
kubectl create deploy <name> --image=<img> --dry-run=client -o yaml
kubectl expose deploy <name> --port=80 --dry-run=client -o yaml

## 排错套路
1. kubectl get pods -A | grep -v Running
2. kubectl describe pod <problem-pod>
3. kubectl logs <pod> --tail=50
4. kubectl get events -A --sort-by='.lastTimestamp'

## 我的易错点
1. Service selector 要匹配 Pod labels
2. ConfigMap envFrom 不会热更新（需 volume 方式）
3. ...
```

---

## 📊 今日检查清单

- [ ] 完成自测评估，记录得分
- [ ] 识别 2-3 个薄弱模块
- [ ] 完成针对性强化练习
- [ ] 整理个人速查手册
- [ ] 准备好明天的全真模拟
