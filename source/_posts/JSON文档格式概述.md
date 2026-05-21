---
title: JSON 文档格式概述
description: JSON 是一种轻量级的数据交换格式，本文介绍其语法规则、数据类型、嵌套结构及编写规范
author: Claude
date: 2026-05-21
tags:
  - JSON
  - 数据交换
  - 配置文件
  - 序列化
category: 技术文档
status: 已完成
---

# JSON 文档格式概述

> JSON（JavaScript Object Notation）是一种轻量级的文本数据格式，设计目标是 **简洁易读、跨语言通用**。广泛用于前后端数据交互、API 接口、配置文件等场景。

---

## 📋 基础信息

| 项目       | 说明                            |
| ---------- | ------------------------------- |
| 文件扩展名 | `.json`                         |
| 标准版本   | RFC 8259 / ECMA-404（当前标准） |
| 核心哲学   | 轻量、语言无关、键值对驱动      |

---

## 🔤 语法规则

### 一条完整的 JSON

```json
{
  "name": "张三",
  "age": 25,
  "is_student": false,
  "hobbies": ["读书", "跑步"],
  "address": {
    "city": "北京",
    "district": "海淀"
  },
  "score": 95.5,
  "remark": null
}
```

**六大铁律：**

| 规则         | 说明                                          |
| ------------ | --------------------------------------------- |
| 键名双引号   | 所有键名必须用 `"` 包裹，无引号或单引号均非法 |
| 字符串双引号 | 字符串值也必须用 `"` 包裹                     |
| 禁止尾随逗号 | 最后一个元素后面**不能有逗号**                |
| 布尔值小写   | 只能用 `true` / `false`                       |
| 空值用 null  | 只能用 `null`，不能用 `undefined`             |
| 不支持注释   | 原生 JSON 没有注释语法                        |

---

## 🏗️ 数据结构

### 6 种数据类型

```json
{
  "string": "hello world",
  "number": 42,
  "float": 3.14,
  "boolean": true,
  "null_value": null,
  "array": [1, 2, 3],
  "object": { "key": "value" }
}
```

| 类型   | 说明                   | 示例             |
| ------ | ---------------------- | ---------------- |
| 字符串 | 双引号包裹的文本       | `"你好"`         |
| 数字   | 整数和浮点数           | `42`、`3.14`     |
| 布尔值 | 真 / 假                | `true` / `false` |
| 空值   | 空 / 无                | `null`           |
| 数组   | 有序集合，方括号包裹   | `[1, "a", true]` |
| 对象   | 键值对集合，花括号包裹 | `{"k": "v"}`     |

> **注意**：JSON 不支持 `undefined`、`Function`、`Date`、`NaN` 等类型。

### 嵌套结构

#### 对象嵌套对象

```json
{
  "user": {
    "profile": {
      "name": "张三",
      "address": {
        "city": "北京",
        "street": "建国路"
      }
    }
  }
}
```

#### 对象嵌套数组

```json
{
  "students": [
    { "name": "小明", "score": 95 },
    { "name": "小红", "score": 88 }
  ]
}
```

#### 多维数组

```json
{
  "matrix": [
    [1, 2, 3],
    [4, 5, 6]
  ]
}
```

> **建议**：嵌套深度不超过 **5 层**，过深的结构难以阅读和维护。

---

## 📝 字符串转义

特殊字符需要用反斜杠转义：

| 写法     | 含义         |
| -------- | ------------ |
| `\"`     | 双引号       |
| `\\`     | 反斜杠       |
| `\n`     | 换行         |
| `\t`     | 制表符       |
| `\r`     | 回车         |
| `\uXXXX` | Unicode 字符 |

```json
{
  "message": "他说：\"你好！\"\n欢迎来到北京。"
}
```

---

## ⚡ JSON 与 JavaScript 对象的区别

| 特性     | JSON                   | JavaScript 对象          |
| -------- | ---------------------- | ------------------------ |
| 键名引号 | **必须**双引号         | 可省略 / 单引号 / 双引号 |
| 值类型   | 仅 6 种基础类型        | 支持函数、Date 等        |
| 注释     | 不支持                 | 支持                     |
| 尾随逗号 | 禁止                   | 允许                     |
| 本质     | **字符串**（文本格式） | **内存中的数据结构**     |

---

## ✅ 编写规范与最佳实践

### 键名命名风格

```json
{
  "first_name": "张三",
  "last_name": "李",
  "is_active": true,
  "max_count": 100
}
```

| 风格            | 推荐场景                     |
| --------------- | ---------------------------- |
| **snake_case**  | Python 项目（推荐）          |
| **camelCase**   | JavaScript / Java / REST API |
| 避免 kebab-case | 不方便作为对象属性访问       |

### 类型使用规范

```json
# ❌ 错误：该用数字却用字符串
{ "count": "42", "active": "true" }

# ✅ 正确：使用原生类型
{ "count": 42, "active": true }
```

### null 与缺失字段

```json
# null：字段存在但值为空
{ "middle_name": null }

# 缺失：字段不存在
{}
```

### 布尔值命名

```json
{
  "is_vip": true,
  "has_permission": false,
  "enable_notification": true
}
```

### 结构组织原则

1. 相关字段按逻辑分组排列，用空行分隔
2. 控制嵌套深度 ≤ **3–4 层**
3. **扁平化优先**，降低解析复杂度
4. 数组内元素类型保持一致

---

## 📡 API 响应格式规范

### 统一响应结构

```json
{
  "code": 200,
  "message": "success",
  "data": {},
  "timestamp": 1710000000000
}
```

### 分页格式

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

### 错误响应

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "邮箱格式不正确",
    "field": "email",
    "status": 422
  }
}
```

---

## ⚠️ 常见错误

```json
# ❌ 错误 1：键名没有双引号
{ name: "张三" }

# ❌ 错误 2：单引号包裹
{'name': '张三'}

# ❌ 错误 3：尾随逗号
{"a": 1, "b": 2,}

# ❌ 错误 4：布尔值大写
{"active": True}

# ❌ 错误 5：前导零
{"phone": 0123456}

# ❌ 错误 6：包含 undefined
{"value": undefined}
```

---

## 📐 JSON Schema 校验

JSON Schema 用于定义和验证 JSON 数据的结构规范：

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "username": {
      "type": "string",
      "minLength": 3,
      "maxLength": 20
    },
    "age": {
      "type": "number",
      "minimum": 0,
      "maximum": 150
    },
    "email": {
      "type": "string",
      "format": "email"
    }
  },
  "required": ["username", "age", "email"]
}
```

---

## 🛡️ 安全规范

| 项目         | 要求                                |
| ------------ | ----------------------------------- |
| JSON 注入    | 禁止手动拼接 JSON，使用官方序列化库 |
| 原型污染     | 过滤 `__proto__` 等危险键名         |
| 超大 payload | 限制解析深度和请求体大小            |
| 数字精度     | 超大整数（> 2⁵³）转为字符串传输     |

---

## 🔧 工具链

| 用途       | 工具                                       |
| ---------- | ------------------------------------------ |
| 语法校验   | JSONLint、AJV                              |
| 格式化     | Prettier、VSCode 原生格式化                |
| 命令行处理 | jq（查询/过滤/转换）                       |
| 类型生成   | json2ts（TypeScript）、quicktype（多语言） |
| 接口调试   | Postman、Apifox                            |

---

## 📌 速查卡（Golden Rules）

```
 1. ✅ 键名和字符串必须用双引号
 2. ✅ 禁止尾随逗号
 3. ✅ 布尔值用 true / false（小写）
 4. ✅ 空值用 null
 5. ✅ UTF-8 编码
 6. ✅ 嵌套不超过 5 层
 7. ✅ 键名风格全项目统一
 8. ✅ 数组元素类型保持一致
 9. ✅ 使用 JSON Schema 校验结构
10. ✅ 不手动拼接 JSON 字符串
```

---

> **总结**：JSON 凭借简洁的语法和跨语言通用性，已成为当今最常见的数据交换格式。严格遵循双引号规则、统一命名风格、合理控制嵌套深度，是高效使用 JSON 的关键。
