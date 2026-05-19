---
title: "OpenStack Horizon（Web控制面板）详解"
description: "全面介绍OpenStack Horizon架构、核心功能、部署配置、主题定制及面板扩展开发"
tags: [OpenStack, Horizon, 仪表板, Django, 云管理]
categories: [技术文档]
date: 2026-05-16
aliases: ["OpenStack Web界面", "Horizon控制面板"]
---

# OpenStack Horizon（Web控制面板）详解

Horizon是OpenStack的官方Web管理界面，基于 **Python Django框架** 开发，为云管理员和租户用户提供图形化的可视化管理入口。

---

### 一、Horizon架构概述

Horizon采用经典的 **Django MTV架构**，通过REST API与各OpenStack核心服务通信：

| 交互组件     | 说明                       |
| ------------ | -------------------------- |
| **Nova**     | 计算服务，管理虚拟机实例   |
| **Neutron**  | 网络服务，管理网络与安全组 |
| **Cinder**   | 块存储服务，管理卷与快照   |
| **Glance**   | 镜像服务，管理虚拟机镜像   |
| **Keystone** | 认证服务，管理用户与项目   |
| **Swift**    | 对象存储服务，管理文件容器 |

#### 设计原则

- **Core Support**：内置Project、Admin、Settings三大面板
- **Extensible**：通过Dashboard+Panel注册机制支持任意扩展
- **Consistent**：提供可复用的模板、表单和表格基类
- **Stable**：核心API保证向后兼容

---

### 二、三大核心面板

#### 1、Project面板（项目面板）

面向 **终端租户用户** 的自助服务门户：

- 💻 实例管理：创建/启动/停止/重启/删除/快照
- 💾 卷管理：创建/挂载/卸载/删除卷及快照
- 🌐 网络管理：网络/子网/路由器/浮动IP/安全组
- 🖼️ 镜像管理：上传/删除/启动镜像
- 🔑 密钥对管理：生成/导入/删除SSH密钥
- 📊 访问与安全：安全组规则、API访问权限

#### 2、Admin面板（管理员面板）

面向 **云平台管理员** 的全局管控入口：

- 📋 资源总览：计算/存储/网络资源的全局用量
- 👥 项目与用户管理：创建/编辑/删除项目、用户及角色分配
- 📏 配额管理：为各项目设置CPU/内存/存储上限
- 🖥️ 主机聚合：管理计算节点与主机聚合（Host Aggregate）
- 🔄 实例迁移：跨计算节点的实例调度与迁移
- 📈 系统信息：服务状态、系统用量报告

#### 3、Settings面板（设置面板）

面向 **所有用户** 的个性化配置：

- 🌍 语言与时区设置
- 🔐 密码修改
- 🔔 仪表板主题切换

---

### 三、部署配置

#### 安装方式

```bash
pip install openstack-dashboard
```

#### 配置文件路径

```
/etc/openstack-dashboard/local_settings.py
```

#### 核心配置项

| 配置项                            | 说明                           | 示例值                           |
| --------------------------------- | ------------------------------ | -------------------------------- |
| `OPENSTACK_HOST`                  | Keystone服务地址               | `"192.168.1.100"`                |
| `OPENSTACK_KEYSTONE_URL`          | Keystone认证URL                | `"http://192.168.1.100:5000/v3"` |
| `OPENSTACK_KEYSTONE_DEFAULT_ROLE` | 默认角色                       | `"member"`                       |
| `ALLOWED_HOSTS`                   | 允许访问的主机列表             | `["*"]`                          |
| `TIME_ZONE`                       | 时区                           | `"Asia/Shanghai"`                |
| `DEBUG`                           | 调试模式开关（生产环境需关闭） | `False`                          |

#### Web服务器部署

```bash
# Apache部署（mod_wsgi）
apt install apache2 libapache2-mod-wsgi-py3

# Nginx + uwsgi部署
pip install uwsgi
```

---

### 四、主题与品牌定制

Horizon支持完整的品牌定制，覆盖Logo、颜色、字体、页脚等视觉元素。

#### 1、基础品牌配置（local_settings.py）

```python
SITE_BRANDING = "My Cloud Platform"
SITE_BRANDING_LINK = "https://cloud.example.com"

AVAILABLE_THEMES = [
    ("default", "Default", "themes/default"),
    ("mytheme", "My Custom Theme", "themes/mytheme"),
]
```

#### 2、创建自定义主题

主题目录结构：

```
themes/mytheme/
├── manifest.py              # 品牌声明文件
├── static/
│   └── img/
│       ├── logo.png         # 导航栏Logo
│       ├── logo-splash.png  # 登录页Logo
│       └── favicon.ico      # 浏览器图标
├── templates/
│   └── auth/
│       └── login.html       # 自定义登录页
└── scss/
    ├── _variables.scss      # 颜色/字体变量
    └── _styles.scss         # 自定义样式
```

#### 3、主题样式变量示例（\_variables.scss）

```scss
$brand-primary: #00a1c9; // 主色调
$navbar-bg-color: #2a4e77; // 导航栏背景
$body-bg-color: #f5f5f5; // 页面背景
$font-family-base: "Microsoft YaHei", sans-serif;
```

#### 4、启用主题

在配置目录下创建 `_12_mytheme_theme.py`：

```python
AVAILABLE_THEMES = [
    ("mytheme", "My Cloud Theme", "themes/mytheme"),
]
```

> **注意**：文件名中的数字前缀控制加载顺序，多个主题文件按文件名升序加载。

---

### 五、面板扩展开发

Horizon通过 **Dashboard + Panel** 机制实现功能扩展。

#### 1、目录结构

```
my_plugin/
├── dashboard.py        # 注册新Dashboard
├── panel.py            # 注册Panel
├── urls.py             # URL路由
├── views.py            # 视图处理
├── tables.py           # 数据表格
├── forms.py            # 表单定义
└── templates/
    └── my_plugin/
        └── index.html  # 模板文件
```

#### 2、注册新Panel示例

**dashboard.py**：

```python
from django.conf import settings
from horizon.dashboards import Dashboard, panels
from openstack_dashboard.dashboards.project import dashboard


class MyPanel(panels.Panel):
    name = "我的插件"
    slug = "my_panel"
    permissions = ("openstack.roles.member",)


dashboard.Project.register(MyPanel)
```

**enabled/\_50_my_plugin.py**：

```python
ADD_INSTALLED_APPS = [
    "my_plugin",
]

ADD_PANEL = {
    "panel_group": "compute",
    "panel": "my_panel",
}
```

#### 3、Views示例

```python
from horizon import tables
from my_plugin import tables as my_tables


class IndexView(tables.DataTableView):
    table_class = my_tables.MyTable
    template_name = "my_plugin/index.html"

    def get_data(self):
        return self.request.user.my_data
```

#### 4、启用/禁用已有面板

```python
# 从Admin面板中移除卷备份面板
REMOVE_PANEL = {
    "openstack_dashboard.dashboards.admin.volumes.panel_volume_backups",
}

# 修改面板显示名称
UPDATE_PANEL = {
    "project.volumes": {
        "name": "云硬盘",
    },
}
```

---

### 六、常用管理命令

#### 静态文件处理

```bash
# 收集静态文件
python manage.py COLLECTSTATIC

# 压缩CSS/JS（修改静态资源后需执行）
python manage.py COMPRESS
```

#### 调试与排障

```bash
# 检查配置正确性
python manage.py CHECK

# 开发服务器启动（仅用于开发调试）
python manage.py RUNSERVER 0.0.0.0:8080

# 查看Horizon版本
python manage.py VERSION
```

---

### 七、安全最佳实践

1. 🔒 生产环境务必关闭 `DEBUG = True`
2. 🛡️ 配置 `ALLOWED_HOSTS` 限制可访问域名
3. 🔐 启用HTTPS，使用SSL证书加密传输
4. 👮 严格遵循最小权限原则分配Keystone角色
5. ⏰ 为JWT Token设置合理过期时间
6. 📝 定期审查安全组规则与API访问日志
7. 🔄 保持Horizon版本与OpenStack各组件版本兼容
