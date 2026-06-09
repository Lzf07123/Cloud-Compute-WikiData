---
title: "OpenStack Heat（编排）详解"
description: "全面介绍OpenStack Heat编排服务的核心概念、HOT模板结构、Stack/Resource/Parameter管理及与AWS CloudFormation的兼容性"
tags: [OpenStack, Heat, 编排, HOT模板, CloudFormation]
categories: [技术文档]
date: 2026-05-16
aliases: ["Heat编排服务", "OpenStack编排详解"]
---

# OpenStack Heat（编排）详解

### 一、核心思想 🌟

Heat 是 OpenStack 的编排服务（Orchestration Service），核心思想是**"基础设施即代码"（Infrastructure as Code）**，通过 HOT 模板定义一组云资源，实现一键式创建、更新、删除。

| 概念                  | 说明                                                              |
| --------------------- | ----------------------------------------------------------------- |
| **Stack（栈）**       | 一组被统一管理的云资源的集合，是 Heat 编排的最小部署单元          |
| **Resource（资源）**  | 栈中的单个云资源（如虚拟机、网络、磁盘），每个资源有唯一的 `type` |
| **Parameter（参数）** | 模板的输入变量，允许用户自定义配置而不修改模板本身                |

### 二、HOT 模板结构 📄

HOT（Heat Orchestration Template）使用 **YAML** 格式，包含四个核心段：

#### 1. 模板版本

```yaml
heat_template_version: 2015-04-30
```

| 版本         | 对应 OpenStack 版本 | 主要特性                 |
| ------------ | ------------------- | ------------------------ |
| `2014-10-16` | Juno                | 初始稳定版               |
| `2015-04-30` | Kilo                | 常用稳定版，支持条件函数 |
| `2016-10-14` | Newton              | 增强 `if` 函数           |
| `2018-08-31` | Rocky               | 新增 `yaql` 支持         |
| `2021-04-16` | Wallaby             | `if` 两参数变体          |

#### 2. 参数定义

```yaml
parameters:
  key_name:
    type: string
    label: Key Name
    description: SSH 密钥对名称
    default: my_key
    hidden: false
    constraints:
      - allowed_values: [m1.small, m1.medium, m1.large]
        description: 规格必须为可选值之一
      - length: { min: 6, max: 8 }
        description: 密码长度 6-8 位
```

**参数类型**：`string`、`number`、`json`、`comma_delimited_list`、`boolean`

**常用修饰符**：

- `default` — 设置默认值
- `hidden: true` — 隐藏输入（如密码）
- `immutable: true` — 创建后不可修改
- `constraints` — 约束条件

#### 3. 资源定义 🧱

```yaml
resources:
  my_instance:
    type: OS::Nova::Server
    properties:
      name: MyServer
      image: { get_param: image_id }
      flavor: { get_param: flavor }
      key_name: { get_param: key_name }
    depends_on: [my_volume]
```

**常用资源类型速查**：

| 资源类型                       | OpenStack 服务 | 说明         |
| ------------------------------ | -------------- | ------------ |
| `OS::Nova::Server`             | Nova           | 计算实例     |
| `OS::Neutron::Net`             | Neutron        | 网络         |
| `OS::Neutron::Subnet`          | Neutron        | 子网         |
| `OS::Neutron::Port`            | Neutron        | 端口         |
| `OS::Neutron::Router`          | Neutron        | 路由器       |
| `OS::Cinder::Volume`           | Cinder         | 云硬盘       |
| `OS::Cinder::VolumeAttachment` | Cinder         | 磁盘挂载     |
| `OS::Heat::AutoScalingGroup`   | Heat           | 自动伸缩组   |
| `OS::Heat::RandomString`       | Heat           | 随机字符串   |
| `OS::Keystone::User`           | Keystone       | 用户         |
| `OS::Swift::Container`         | Swift          | 对象存储容器 |

#### 4. 输出定义

```yaml
outputs:
  instance_ip:
    description: 实例 IP 地址
    value: { get_attr: [my_instance, first_address] }
  instance_id:
    description: 实例 UUID
    value: { get_resource: my_instance }
```

### 三、内置函数 🔧

| 函数           | 用途                    | 类比 CloudFormation |
| -------------- | ----------------------- | ------------------- |
| `get_param`    | 获取参数值              | `Ref`               |
| `get_attr`     | 获取资源属性            | `Fn::GetAtt`        |
| `get_resource` | 获取资源引用            | `Ref`（资源本身）   |
| `get_file`     | 嵌入外部文件            | —                   |
| `str_replace`  | 字符串模板替换          | `Fn::Sub`           |
| `str_split`    | 字符串分割              | `Fn::Split`         |
| `digest`       | 哈希计算                | —                   |
| `repeat`       | 循环生成值              | —                   |
| `list_join`    | 列表拼接                | `Fn::Join`          |
| `yaql`         | YAML 查询语言（v2018+） | —                   |

**条件函数示例**：

```yaml
conditions:
  create_volume: { equals: [{ get_param: volume_size }, "0"] }
  large_flavor: { not: { equals: [{ get_param: flavor }, "m1.small"] } }

resources:
  my_instance:
    type: OS::Nova::Server
    condition: large_flavor
```

### 四、AWS CloudFormation 兼容性 🔄

Heat 在设计上明确兼容 AWS CloudFormation 模板格式：

| 对比项       | HOT（Heat 原生）        | CFN（CloudFormation 兼容） |
| ------------ | ----------------------- | -------------------------- |
| 格式         | YAML（.yaml）           | JSON（.json）              |
| 资源类型前缀 | `OS::*`                 | `AWS::*`                   |
| 函数风格     | `get_param`, `get_attr` | `Ref`, `Fn::GetAtt`        |
| API 端点     | `heat-api`              | `heat-api-cfn`             |

- 通过 `heat-api-cfn` 组件提供 CloudFormation 兼容 API
- 已有的 AWS CloudFormation 模板可直接提交给 Heat 执行
- 支持资源类型映射：`AWS::EC2::Instance` → `OS::Nova::Server`

### 五、Stack 操作生命周期 🔄

```
创建（Create）→ 创建中 → 创建完成
  ↓
更新（Update）→ 更新中 → 更新完成
  ↓
回滚（Rollback）→ 回滚中 → 回滚完成
  ↓
删除（Delete）→ 删除中 → 删除完成
```

**常用 CLI 命令** 🖥️

```bash
openstack stack create -t TEMPLATE.YAML -e ENV.YAML MY_STACK
openstack stack list
openstack stack resource list MY_STACK
openstack stack resource show MY_STACK MY_INSTANCE
openstack stack update -t TEMPLATE.YAML --parameter "FLAVOR=M1.MEDIUM" MY_STACK
openstack stack event list MY_STACK
openstack stack delete MY_STACK
```

**参数覆盖创建**：

```bash
openstack stack create -t TEMPLATE.YAML \
  --parameter "KEY_NAME=MY_KEY;IMAGE=CIRROS;FLAVOR=M1.SMALL" \
  MY_STACK
```

### 六、环境文件（Environment File）📁

环境文件用于分离配置与模板：

```yaml
resource_registry:
  "OS::Nova::Server::MyServer": myserver.yaml

parameter_defaults:
  NetworkName: my_network

parameters:
  MyIP: 192.168.0.1
```

> ⚠️ `parameter_defaults` 应用到所有嵌套栈，`parameters` 仅应用到顶级栈

### 七、模板组合（Template Composition）🧩

将大型模板拆分为可复用的子模板：

```yaml
resources:
  my_server:
    type: my_nova.yaml
    properties:
      key_name: { get_param: key_name }
```

**嵌套栈属性访问**：

```yaml
outputs:
  test_out:
    value: { get_attr: [my_server, resource.server, first_address] }
```

### 八、完整编排流程图 🎯

```
用户输入 HOT 模板（YAML）
        │
        ▼
   Heat API（heat-api / heat-api-cfn）
        │
        ▼
   Heat Engine（引擎核心）
        │
        ├── 解析模板（参数绑定、条件判断）
        ├── 依赖排序（depends_on / 隐式依赖）
        ├── 逐个调用 OpenStack 服务 API
        │    ├── Nova → 创建虚拟机
        │    ├── Neutron → 创建网络/子网
        │    ├── Cinder → 创建云硬盘
        │    └── ...
        ├── 监控资源创建状态
        └── 返回结果（资源 ID、属性等）
```

### 九、完整模板示例 📝

```yaml
heat_template_version: 2015-04-30

description: 完整的 Heat 编排模板示例

parameters:
  key_name:
    type: string
    label: Key Name
    description: 密钥对名称
    default: my_key
  image:
    type: string
    description: 镜像名称或 ID
    default: cirros
  flavor:
    type: string
    description: 实例规格
    default: m1.small
    constraints:
      - allowed_values: [m1.small, m1.medium, m1.large]

resources:
  my_instance:
    type: OS::Nova::Server
    properties:
      name: My Cirros Instance
      image: { get_param: image }
      flavor: { get_param: flavor }
      key_name: { get_param: key_name }

  my_volume:
    type: OS::Cinder::Volume
    properties:
      size: 1
      name: my_volume

  volume_attachment:
    type: OS::Cinder::VolumeAttachment
    properties:
      volume_id: { get_resource: my_volume }
      instance_uuid: { get_resource: my_instance }
      mountpoint: /dev/vdb

outputs:
  instance_name:
    description: 获取实例名称
    value: { get_attr: [my_instance, name] }
  instance_ip:
    description: 获取实例 IP 地址
    value: { get_attr: [my_instance, first_address] }
```

### 十、最佳实践 💡

1. 使用**环境文件**分离配置与模板，避免硬编码参数
2. 合理设置 `depends_on` 确保资源创建顺序
3. 使用 `parameter_defaults` 统一管理嵌套栈参数
4. 为栈开启**回滚**（Rollback）功能，防止失败时产生残留资源
5. 利用 `stack event list` 排查创建失败原因
6. 将大型模板拆分为**可复用的子模板**，提升维护性
7. 使用 `constraints` 对参数进行校验，提前发现错误
8. 生产环境建议使用 **TOSCA** 模板或结合 `tripleo-heat-templates` 进行大规模部署

## 🔗 相关文档

{% post_link OpenStack/Heat编排服务概念 %} | {% post_link OpenStack/OpenStack-Nova计算服务详解 %} | {% post_link OpenStack/OpenStack-Neutron网络服务详解 %}
