---
title: "OpenStack Horizon Web界面概念"
description: "全面介绍OpenStack Horizon Web界面服务的核心概念、界面架构、功能模块及管理操作"
tags: [OpenStack, Horizon, Dashboard, Web界面]
categories: [技术文档, 云计算]
date: 2026-05-16
aliases: ["Horizon仪表板", "OpenStack Web管理界面", "Dashboard组件"]
---

# OpenStack Horizon Web界面概念 🖥️

Horizon 是 OpenStack 平台的**官方 Web 管理界面组件**（Dashboard），基于 Django 框架构建，为管理员和租户用户提供图形化的云资源管理入口，无需记忆命令行即可完成绝大部分日常操作🚀。

核心设计理念：将底层各 OpenStack 组件的 API 调用封装为**直观的 Web 操作界面**，降低使用门槛，提升运维效率。

---

## 一、核心架构

Horizon 采用 Django MTV（Model-Template-View）架构模式🏗️：

| 层级           | 组件                              | 说明                                |
| -------------- | --------------------------------- | ----------------------------------- |
| **API 层**     | `openstack_dashboard/api/`        | 封装对各 OpenStack 组件的 REST 调用 |
| **视图层**     | `openstack_dashboard/views`       | 处理请求逻辑与数据上下文            |
| **模板层**     | `openstack_dashboard/templates/`  | Django 模板引擎，渲染 Web 页面      |
| **静态资源层** | `openstack_dashboard/static/`     | CSS / JS / 图片等前端资源           |
| **配置层**     | `openstack_dashboard/settings.py` | 全局配置、功能开关、后端对接        |

### 通信架构

```
浏览器（用户） ➔ Apache/nginx（WSGI） ➔ Horizon（Django App）
    ➔ REST API ➔ Keystone（认证） / Nova（计算） / Neutron（网络）
    ➔ Cinder（存储） / Glance（镜像） / Swift（对象存储）
```

---

## 二、界面结构

### 1. Dashboard（仪表板）与 Panel（面板）

Horizon 的界面按**用户角色**划分为两大 Dashboard📊：

| Dashboard   | 适用对象 | 主要面板                                  |
| ----------- | -------- | ----------------------------------------- |
| **Admin**   | 云管理员 | 项目/用户/镜像/网络/卷/实例/服务/配额管理 |
| **Project** | 普通租户 | 实例/卷/网络/安全组/密钥对/镜像/负载均衡  |

### 2. UI 核心组件

Horizon 提供多种标准 UI 组件，确保界面一致性与可扩展性🎨：

- **DataTables（数据表）**：展示资源列表，支持排序、过滤、分页、批量操作
- **Tabs & TabGroups（标签页）**：将关联数据分组展示（如实例详情/控制台/日志）
- **Workflows（工作流）**：多步骤导向型操作，如创建实例（选择镜像 ➔ 选择规格 ➔ 网络配置 ➔ 确认）
- **Forms（表单）**：参数填写与配置提交
- **Wizards（向导）**：复杂操作的引导式流程

### 3. 用户设置

用户可在设置面板中个性化配置⚙️：

- **时区选择**（Timezone）：控制时间显示
- **语言偏好**（Language）：支持多语言切换
- **页面大小**（Items Per Page）：列表每页显示条目数
- **HTTP 日志**（Logs）：查看 API 调用历史

---

## 三、核心功能

### 1. 计算管理（Nova）💻

| 功能         | 操作说明                         |
| ------------ | -------------------------------- |
| 实例生命周期 | 创建/启动/停止/软硬重启/删除实例 |
| 实例控制台   | 通过 VNC/SPICE 在浏览器内访问    |
| 实例快照     | 基于运行中实例创建镜像           |
| 规格管理     | 查看和管理 Flavor 配置           |
| 密钥对管理   | SSH 密钥对的导入/创建/删除       |

### 2. 网络管理（Neutron）🌐

| 功能         | 操作说明                           |
| ------------ | ---------------------------------- |
| 网络创建     | 创建 Provider / Self-Service 网络  |
| 子网管理     | 配置 CIDR / DHCP 启用/网关设置     |
| 路由器管理   | 创建路由器、添加接口、设置外部网关 |
| 浮动 IP 管理 | 绑定/解绑浮动 IP                   |
| 安全组规则   | 自定义入站/出站规则                |
| 负载均衡     | 创建监听器、后端池、健康检查       |

### 3. 存储管理（Cinder/Glance/Swift）💾

| 功能     | 操作说明                         |
| -------- | -------------------------------- |
| 卷管理   | 创建/挂载/卸载/扩展/删除卷       |
| 卷快照   | 创建/删除快照，从快照创建新卷    |
| 镜像管理 | 上传/下载/编辑/删除/共享镜像     |
| 对象存储 | 容器创建、对象上传/下载（Swift） |

### 4. 项目与用户管理（Keystone）👥

| 功能     | 操作说明                           |
| -------- | ---------------------------------- |
| 项目管理 | 创建/编辑/禁用/删除项目            |
| 用户管理 | 创建/编辑/启用/禁用/删除用户       |
| 角色分配 | 为用户分配项目角色                 |
| 配额管理 | 设置项目级别计算/存储/网络资源配额 |

---

## 四、扩展与定制 🔧

Horizon 支持丰富的定制与扩展能力：

### 1. Dashboard 注册机制

开发者的自定义功能通过 Django `dashboard.py` 文件注册到 Horizon：

```python
# 将自定义面板注册到 Dashboard
dashboard.default_panels.append('MyCustomPanel')
```

### 2. 主题与品牌定制

- 修改 `openstack_dashboard/settings.py` 中的 `AVAILABLE_THEMES` 切换主题
- 支持自定义 Logo、品牌色、页脚、样式表
- 内置 Bootstrap 主题引擎，支持响应式布局

### 3. 功能模块扩展

- 采用 Django App 机制，各功能独立模块化
- Panel（面板）是基本功能单元，可自由增删
- 支持第三方插件（如 Heat / Mistral / Sahara 等扩展组件）

### 4. 常见扩展方式

| 扩展方式     | 说明                                       |
| ------------ | ------------------------------------------ |
| **继承重写** | 继承 Horizon 基类，覆盖默认视图/模板       |
| **插件注册** | 通过 horizon.py 注册自定义 Dashboard/Panel |
| **模板覆写** | 利用 Django 模板继承机制替换默认模板       |
| **静态资源** | 在 static/ 目录添加自定义 JS/CSS/图片      |

---

## 五、部署模式

| 模式            | 说明                                    |
| --------------- | --------------------------------------- |
| **单节点**      | Horizon 与控制节点部署在一起，测试/POCC |
| **多节点 + HA** | Horizon 独立部署，前端 + Apache 集群    |
| **容器化部署**  | 通过 Kolla-Ansible 以容器方式运行       |
| **HTTPS + SSO** | 配置 SSL 证书 + LDAP / SAML 认证集成    |

### 生产环境部署建议⚠️

```bash
# 安装 Horizon
apt install openstack-dashboard

# 配置 Apache
a2ensite 000-default
systemctl restart apache2

# HTTPS 配置（签发有效期检查）
openstack-config --set /etc/openstack-dashboard/local_settings.py DEFAULT_ENABLE_HTTPS True
```

---

## 六、监控与排查

| 问题             | 排查方法                                           |
| ---------------- | -------------------------------------------------- |
| 页面加载失败     | 检查 Apache/nginx 状态：`systemctl status apache2` |
| API 调用报错     | 查看 `/var/log/horizon/horizon.log`                |
| 登录失败         | 检查 Keystone 服务状态及网络连通性                 |
| 界面显示异常     | 浏览器 F12 查看 Console/Network 错误               |
| 静态资源加载失败 | `python manage.py collectstatic` 重新收集静态文件  |

---

## 七、Horizon SDN 管理思想

Horizon 的核心设计哲学：**通过抽象化资源视图，屏蔽底层组件差异**。

| 传统操作方式       | Horizon 实现                |
| ------------------ | --------------------------- |
| 命令行管理各组件   | 统一 Web 界面，跨组件集成   |
| 手动记录资源信息   | DataTables 实时展示资源状态 |
| 手工处理多步骤操作 | Workflows 引导式流程        |
| 逐组件排查问题     | 集成式资源拓扑与日志视图    |

### 关键技术注解 💡

- **Django MTV 架构**: Horizon 的基础框架，Model（数据模型）→ Template（模板渲染）→ View（业务逻辑处理）
- **REST API 封装**: Horizon 内部通过 `python-openstackclient` 和各组件 SDK 与后端服务通信，用户操作最终转化为 API 调用
- **Workflows 工作流**: Horizon 独创的多步骤操作模式，将创建实例等复杂操作分解为有引导的步骤，每步可独立验证
- **Panel 面板机制**: 最细粒度的功能单元，支持按角色/权限动态加载显示，实现功能级 RBAC 控制
