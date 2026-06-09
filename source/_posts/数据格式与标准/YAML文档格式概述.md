---
title: YAML 文档格式概述
description: YAML 是一种用于写配置的文本格式，本文介绍其语法、数据结构和编写规范
author: Claude
date: 2026-05-21
tags:
  - YAML
  - 配置文件
  - 数据序列化
  - DevOps
  - 云原生
category: 技术文档
status: 已完成
---

# YAML 文档格式概述

> YAML（YAML Ain't Markup Language）是一种用文本写配置的格式，设计目标是 **让人能轻松看懂**。广泛用于各类工具和平台的配置文件。

---

## 📋 基础信息

| 项目       | 说明                                               |
| ---------- | -------------------------------------------------- |
| 文件扩展名 | `.yaml` 或 `.yml`（推荐 `.yaml`）                  |
| 标准版本   | YAML 1.2（当前主流），YAML-LD 1.0（2026 W3C 草案） |
| 核心哲学   | 数据驱动、人类可读、语言无关                       |

---

## 🔤 核心语法规则

### 基础规则

```yaml
# 大小写敏感 | 空格缩进 | 2 空格标准
server:
  host: localhost
  port: 8080
  ssl: true
```

**四大铁律：**

| 规则       | 说明                            |
| ---------- | ------------------------------- |
| 大小写敏感 | `port` 与 `Port` 是两个不同的键 |
| 空格缩进   | **严格禁止 TAB**，仅使用空格    |
| 同级对齐   | 同层级的缩进量必须一致          |
| 2 空格标准 | 每级缩进 2 个空格，行业共识     |

### 注释

```yaml
# 这是行首注释，说明下文意图
server:
  host: localhost # 行尾注释，代码后至少空 2 格
```

> **注意**：YAML 不支持多行注释，需逐行使用 `#`。

---

## 🏗️ 数据结构

### 标量（Scalars）

```yaml
string: "hello world"
integer: 42
float: 3.14
boolean: true # ✅ 仅用 true/false
null: ~ # 或留空 / null
date: 2025-12-25
datetime: 2025-12-25T00:00:00+08:00
```

### 映射（Mappings）

```yaml
# ✅ 块式（推荐）
database:
  host: localhost
  port: 3306
  name: myapp

# ⚠️ 流式（仅简单结构）
database: { host: localhost, port: 3306 }
```

### 序列（Sequences）

```yaml
# ✅ 块式（推荐）
services:
  - web
  - database
  - cache

# ⚠️ 流式
services: [web, database, cache]
```

### 复合结构

```yaml
instances:
  - name: node-1
    region: us-east
    cpu: 4
  - name: node-2
    region: us-west
    cpu: 8
```

---

## 📝 字符串处理

### 多行字符串

```yaml
# | 保留换行符
script: |
  #!/bin/bash
  echo "hello"
  exit 0
# 输出: "#!/bin/bash\necho "hello"\nexit 0\n"
# > 折叠换行（换行转空格）
description: >
  这是一个很长的段落，
  会被合并为一行。
# 输出: "这是一个很长的段落，会被合并为一行。\n"
# |- 保留换行但去掉末尾换行（最常用）
config: |-
  line1
  line2

# 输出: "line1\nline2"
```

### 引号规则

| 格式       | 说明                     | 场景                    |
| ---------- | ------------------------ | ----------------------- |
| **无引号** | 默认，不含特殊字符时使用 | `name: nginx`           |
| **单引号** | 原样保留，不处理转义     | `'https://example.com'` |
| **双引号** | 支持转义序列             | `"line1\nline2"`        |

> **建议**：仅在包含 `: # @ { } [ ] , & * ! > |` 等特殊字符时加引号，优先使用单引号。

---

## ⚡ 高级特性

### 锚点与引用（配置复用）

```yaml
defaults: &defaults
  timeout: 30
  retries: 3
  log_level: info

service:
  <<: *defaults # 合并默认配置
  name: auth-service # 新增字段
  timeout: 60 # 覆盖默认值
```

| 符号 | 含义                  |
| ---- | --------------------- |
| `&`  | 定义锚点（命名位置）  |
| `*`  | 引用锚点（引用值）    |
| `<<` | 合并映射（Merge Key） |

### 类型显式声明

```yaml
port_str: !!str 8080 # 强制转换为字符串
checksum: !!binary | # Base64 编码
  R0lGODlhDAAMAIQAAP//
created: !!timestamp 2025-08-23T12:00:00Z
```

> **安全警告**：`!python/object` 等危险标签存在代码注入风险，生产环境应使用 `safeloader`。

### 多文档流

```yaml
---
# 文档 1：开发环境
env: development
db_url: jdbc:mysql://dev-db:3306
---
# 文档 2：生产环境
env: production
db_url: jdbc:mysql://prod-db:3306
...
```

---

## ✅ 编写规范与最佳实践

### 格式规范速查

| 类别     | 规范要求                                |
| -------- | --------------------------------------- |
| 缩进     | **2 个空格**，禁止使用 TAB              |
| 编码     | **UTF-8**                               |
| 行尾     | **LF**（Unix），文件末尾保留一个换行符  |
| 行长度   | 不超过 **80–120 字符**                  |
| 尾随空格 | **必须清除**                            |
| 空行控制 | 段落间最多连续 2 个空行，文件首尾无空行 |

### 命名风格

```yaml
# ✅ 推荐：小写 + 下划线（snake_case）
max_connections: 100
db_host: localhost

# ❌ 避免：含义不明的缩写
ph: /usr/local # → 应改为 program_home
```

### 布尔值规范

```yaml
# ✅ YAML 1.2 标准
enabled: true
disabled: false

# ❌ 旧版兼容，不推荐
active: yes # 可能被解析为布尔值
inactive: no
turned_on: on
turned_off: off
```

### 结构组织原则

1. 相关配置分组为逻辑段落，用空行分隔
2. 控制嵌套深度 ≤ **3–4 层**
3. **块式优先**于流式（可读性更佳）
4. 大型配置文件拆分为子文件，合并引入

---

## 🛡️ 安全规范

| 项目     | 要求                                              |
| -------- | ------------------------------------------------- |
| 敏感信息 | 不硬编码密码/密钥，使用环境变量或 SOPS/Vault 加密 |
| 解析器   | 使用 `safeloader`，禁用危险标签                   |
| 远程配置 | 加载需签名验证                                    |
| 版本控制 | 所有 YAML 文件纳入 Git 管理                       |

---

## 🔧 质量工具链

| 工具             | 用途                             |
| ---------------- | -------------------------------- |
| **yamllint**     | 语法校验（缩进、重复键、行长度） |
| **prettier**     | 自动格式化                       |
| **VSCode YAML**  | 语法高亮、自动补全、校验         |
| **redocly cli**  | OpenAPI/AsyncAPI YAML 校验       |
| **json schema**  | 配置结构正确性验证               |
| **editorconfig** | 团队统一缩进/编码配置            |

---

## ⚠️ 常见陷阱

```yaml
# ❌ 陷阱 1：TAB 混入（YAML 绝对禁止）
server:
	host: localhost   # 此处使用了 TAB，解析报错

# ❌ 陷阱 2：类型自动推断
phone: 0123456       # 可能被解析为八进制（YAML 1.1）
# ✅ 应写为
phone: "0123456"     # 加引号明确为字符串

# ❌ 陷阱 3：布尔值误判
active: yes          # YAML 1.1 中解析为 true
# ✅ 应写为
active: true

# ❌ 陷阱 4：重复键
key: value1
key: value2          # 后者覆盖前者，静默错误
```

---

## 📌 速查卡（Golden Rules）

```
 1. ✅ 2 空格缩进，只用空格
 2. ✅ UTF-8 编码，LF 行尾
 3. ✅ 文件末尾保留一个换行符
 4. ✅ 特殊字符使用引号，优先单引号
 5. ✅ 布尔值只用 true / false
 6. ✅ 块式优先于流式
 7. ✅ 注释写在上一行，说明"为什么"
 8. ✅ 使用 yamllint 校验语法
 9. ✅ 敏感信息不硬编码
10. ✅ 控制嵌套深度不超过 4 层
```

---

> **总结**：YAML 凭借简洁、易读的语法，已成为目前最流行的配置格式之一。统一缩进、善用锚点复用配置、配合检查工具保证质量，是写好 YAML 的关键。

## 🔗 相关文档

{% post_link Kubernetes/Kubernetes配置与密文详解 %} | {% post_link Kubernetes/Kubernetes核心概念详解 %} | {% post_link Docker/Docker-Compose多容器编排入门 %}
