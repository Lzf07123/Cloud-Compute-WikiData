---
title: "ConfigMap配置与密文管理详解"
description: "详细介绍Kubernetes中ConfigMap和Secret的使用方法，包括创建方式、注入机制和实际应用示例"
tags: [Kubernetes, ConfigMap, Secret, 配置管理, 安全]
categories: [容器技术]
date: 2026-05-14
aliases: ["K8s配置管理", "容器配置存储"]
---

# ConfigMap配置管理 🔧

## ConfigMap概述 💡

- **概念**: ConfigMap是Kubernetes中用于存储非敏感配置数据的API对象，以键值对形式存储配置信息
- **实现**: 提供了向Pod注入配置数据的标准方法，实现镜像与配置的解耦，提升应用的可移植性和可复用性

## ConfigMap典型使用场景 🎯

- **概念**: ConfigMap在实际应用中的主要用途
- **实现**:
  - 填充环境变量的值
  - 设置容器内的命令行参数
  - 填充卷的配置文件

## ConfigMap创建方式 🛠️

### 1. 使用字面值创建

```bash
kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

### 2. 使用文件创建

```bash
kubectl create configmap my-config --from-file=path/to/bar
```

### 3. 使用目录创建

```bash
kubectl create configmap my-config --from-file=path/to/bar
```

### 4. 编写YAML文件创建

通过定义完整的YAML清单文件来创建ConfigMap对象

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: ConfigMap # 资源类型，声明为ConfigMap对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: my-config # ConfigMap的名称
  namespace: default # 所属命名空间
data: # 配置数据，以键值对形式存储
  key1: config1 # 配置键值对1
  key2: config2 # 配置键值对2
```

## ConfigMap注入方式 🚀

### 1. 环境变量方式

通过`spec.env`或`spec.envFrom`进行引用配置数据

### 2. 卷挂载方式

通过`spec.volumes`引用，将ConfigMap挂载到容器内部的文件或目录

## 环境变量注入示例 📝

### 使用spec.env方式

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: cm-pod-test001 # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: cm-test # 容器名称
      image: tomcat:8 # 容器镜像
      command: ["/bin/sh", "-c", "env | grep APP"] # 容器启动命令
      env: # 环境变量配置
        - name: APPCONF01 # 环境变量名称
          valueFrom: # 值来源配置
            configMapKeyRef: # ConfigMap键引用
              name: cm-test01 # ConfigMap名称
              key: appconf01 # ConfigMap中的键名
        - name: APPCONF02 # 环境变量名称
          valueFrom: # 值来源配置
            configMapKeyRef: # ConfigMap键引用
              name: cm-test01 # ConfigMap名称
              key: appconf02 # ConfigMap中的键名
```

### 使用spec.envFrom方式

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: cm-pod-test002 # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: cm-test2 # 容器名称
      image: tomcat:8 # 容器镜像
      command: ["/bin/sh", "-c", "env"] # 容器启动命令
      envFrom: # 环境变量来源配置
        - configMapRef: # ConfigMap引用
            name: cm-test01 # ConfigMap名称，所有键值对都会作为环境变量注入
```

## Volume挂载示例 📁

### 指定items的方式

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: cm-pod-test003 # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: cm-test3 # 容器名称
      image: tomcat:8 # 容器镜像
      volumeMounts: # 卷挂载配置
        - name: vm-01-1 # 卷名称，与volumes中的name对应
          mountPath: /conf # 挂载路径，容器内的目录路径
  volumes: # 卷定义列表
    - name: vm-01-1 # 卷名称
      configMap: # ConfigMap卷配置
        name: cm-test-file # ConfigMap名称
        items: # 指定要挂载的键值对
          - key: key-testproperties # ConfigMap中的键名
            path: test.properties # 挂载后的文件名
```

### 不指定items的方式

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: cm-pod-test004 # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: cm-test4 # 容器名称
      image: tomcat:8 # 容器镜像
      volumeMounts: # 卷挂载配置
        - name: vm-02-2 # 卷名称，与volumes中的name对应
          mountPath: /conf # 挂载路径，容器内的目录路径
  volumes: # 卷定义列表
    - name: vm-02-2 # 卷名称
      configMap: # ConfigMap卷配置
        name: cm-test-file # ConfigMap名称，所有键值对都会被挂载
```

# Secret密文管理 🔒

## Secret概述 🔐

- **概念**: Secret对象与ConfigMap对象类似，专门用于存储敏感信息
- **实现**: 主要存储密码、OAuth token和SSH key等敏感数据，比直接存储在Pod定义或Docker镜像中更加安全和灵活

## Secret类型 🏷️

### 1. Opaque类型

- **特点**: 使用base64编码存储信息
- **安全性**: 可以通过`base64 --decode`解码获得原始数据，安全性相对较弱

### 2. kubernetes.io/dockerconfigjson类型

- **用途**: 专门用于存储docker registry的认证信息

### 3. kubernetes.io/service-account-token类型

- **用途**: 用于被serviceaccount引用的服务账号令牌

## Secret创建方式 🛠️

### 1. 从文件中创建Secret

```bash
echo -n "admin" > ./username.txt
echo -n "123456" > ./password.txt
kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
```

### 2. 使用描述文件创建Secret

首先进行base64编码：

```bash
echo -n 'name' | base64
echo -n '123456' | base64
```

然后创建secret.yaml文件：

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Secret # 资源类型，声明为Secret对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: mysecret # Secret的名称
type: Opaque # Secret类型，Opaque表示通用类型
data: # 敏感数据，以base64编码的键值对形式存储
  username: bmFtZQ== # 用户名，base64编码后的值
  password: MTIzNDU2 # 密码，base64编码后的值
```

## Secret注入方式 🚀

### 1. 将Secret挂载到Volume中

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: mypod # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: mypod # 容器名称
      image: redis # 容器镜像
      volumeMounts: # 卷挂载配置
        - name: foo # 卷名称，与volumes中的name对应
          mountPath: "/etc/foo" # 挂载路径，容器内的目录路径
          readOnly: true # 只读挂载，增强安全性
  volumes: # 卷定义列表
    - name: foo # 卷名称
      secret: # Secret卷配置
        secretName: mysecret # Secret名称
```

### 2. 将Secret设置为环境变量

```yaml
apiVersion: v1 # API版本，指定使用的Kubernetes API版本
kind: Pod # 资源类型，声明为Pod对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: secret-env-pod # Pod的名称
spec: # Pod规格，定义Pod的配置
  containers: # 容器列表
    - name: mycontainer # 容器名称
      image: redis # 容器镜像
      env: # 环境变量配置
        - name: SECRET_USERNAME # 环境变量名称
          valueFrom: # 值来源配置
            secretKeyRef: # Secret键引用
              name: mysecret # Secret名称
              key: username # Secret中的键名
        - name: SECRET_PASSWORD # 环境变量名称
          valueFrom: # 值来源配置
            secretKeyRef: # Secret键引用
              name: mysecret # Secret名称
              key: password # Secret中的键名
```

# ConfigMap热更新机制 🔄

## 更新特性说明 ⚡

- **Volume挂载方式**: 可以感知配置变化（延迟几秒到一分钟左右）
- **环境变量方式**: 不会自动更新，需要重启Pod才能生效

## 使用建议 💡

- **频繁变化的配置**: 推荐使用Volume挂载方式
- **固定配置**: 可以使用环境变量注入方式

# 实际应用示例 🎯

## MySQL密码注入Pod示例 🗄️

### 1. 创建Secret

```bash
kubectl create secret generic test --from-literal=MYSQL_ROOT_PASSWORD=1234567
```

### 2. 创建Deployment

```yaml
apiVersion: apps/v1 # API版本，指定使用的Kubernetes API版本
kind: Deployment # 资源类型，声明为Deployment对象
metadata: # 元数据，包含资源的名称、命名空间等信息
  name: myapp-demo # Deployment的名称
  namespace: default # 所属命名空间
spec: # Deployment规格，定义Deployment的配置
  replicas: 1 # 副本数量
  selector: # 选择器，用于匹配Pod
    matchLabels: # 标签匹配规则
      app: myapp # 匹配标签为app=myapp的Pod
  template: # Pod模板，定义Pod的配置
    metadata: # Pod元数据
      labels: # Pod标签
        app: myapp # Pod的标签
    spec: # Pod规格
      containers: # 容器列表
        - name: myapp # 容器名称
          image: ikubernetes/myapp:v1 # 容器镜像
          imagePullPolicy: IfNotPresent # 镜像拉取策略
          ports: # 端口配置
            - name: http # 端口名称
              containerPort: 80 # 容器端口
          volumeMounts: # 卷挂载配置
            - name: mysql # 卷名称，与volumes中的name对应
              mountPath: /test/ # 挂载路径，容器内的目录路径
      volumes: # 卷定义列表
        - name: mysql # 卷名称
          secret: # Secret卷配置
            secretName: test # Secret名称
```

# 注意事项 ⚠️

- **安全性**: Secret虽然使用base64编码，但并非真正的加密，敏感信息仍需谨慎处理
- **性能**: ConfigMap和Secret都有大小限制，不宜存储过大的配置文件
- **更新策略**: 根据配置变化频率选择合适的注入方式
- **权限管理**: 合理设置RBAC权限，控制ConfigMap和Secret的访问范围
  通过合理使用ConfigMap和Secret，可以实现Kubernetes应用的配置与镜像分离，提升应用的可维护性和安全性 🚀
