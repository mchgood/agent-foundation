# FastAPI Request Body 学习笔记

## 资料来源

- FastAPI 官方文档：<https://fastapi.tiangolo.com/tutorial/body/>

## 学习目标

- 理解 FastAPI 如何通过 Pydantic 模型定义请求体
- 掌握 request body、query parameter、path parameter 的基本区分
- 认识 Pydantic 在类型校验、JSON Schema、OpenAPI 文档中的作用
- 能把模型输出结构和后端输入输出结构连接起来

## 笔记概要

这篇文档讲的是 FastAPI 中最基础、也最关键的一件事：  
如何用 Pydantic 模型来定义和校验请求体。  
对 AI 应用来说，它的重要性在于：前面你学的是“让模型稳定地产出结构”，这一篇教的是“后端怎样稳定地接住这些结构”。

## 详细笔记

### 1. Request Body 是什么

HTTP 请求除了 URL 和查询参数外，还可以携带请求体。  
在 FastAPI 中，如果你在接口函数参数中使用 Pydantic 模型，FastAPI 就会把它视为 request body。

例如，一个创建对象的接口可能需要：

- name
- description
- price
- tax

这些就很适合放在请求体中，而不是塞进 URL。

### 2. 为什么 FastAPI 要用 Pydantic

FastAPI 使用 Pydantic 的核心价值不只是“写法优雅”，而是它能自动完成很多关键工作：

- 将 JSON 解析为 Python 对象
- 根据类型注解做校验
- 返回结构化错误信息
- 自动生成 JSON Schema
- 自动生成 OpenAPI 文档

这意味着你不是在手写解析逻辑，而是在声明接口协议。

### 3. `BaseModel` 的作用

文档最基础的例子就是继承 `BaseModel`：

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
```

这段代码本质上做了三件事：

1. 定义接口字段
2. 定义字段类型
3. 定义哪些字段必填、哪些字段可选

也就是说，Pydantic 模型就是接口输入合同。

### 4. 必填字段和可选字段怎么区分

FastAPI/Pydantic 的判断规则很重要：

- 没有默认值的字段：必填
- 有默认值或 `None` 的字段：可选

你在学习这部分时，要把它和 JSON Schema 的 `required` 思维联系起来。  
两边其实在做同一件事：定义稳定接口边界。

### 5. Request Body、Path、Query 的区分

FastAPI 会根据参数类型和位置推断来源：

- 路径中声明的变量是 path parameter
- 普通基本类型参数通常会被当作 query parameter
- Pydantic 模型参数会被当作 request body

这个规则非常值得记住，因为你以后写 AI 接口时，常常会同时用到三类输入。

例如：

- `POST /extract` 的正文是用户 query
- `GET /items/{item_id}` 的 `item_id` 是路径参数
- `?limit=10` 这类是查询参数

### 6. 自动文档为什么对学习项目也重要

FastAPI 会把 Pydantic 模型自动转成 JSON Schema，并在 OpenAPI 文档中展示。  
这意味着你写的 Python 类型不是“自己知道就行”，而是会直接生成接口说明。

这对 AI 项目尤其好，因为它让你的输入输出定义可以被：

- 开发者理解
- 前端理解
- 测试理解
- 文档系统理解

这其实和 Structured Outputs 的目标是一致的：  
都在把“不确定文本约定”变成“明确结构约定”。

### 7. 为什么它和 AI 项目密切相关

如果你后面做一个 `/extract` 接口，通常流程是：

1. 用户发来自然语言 query
2. 后端把 query 交给模型
3. 模型输出结构化结果
4. 后端再把结果返回给前端或其他系统

在这个流程里：

- Request model 负责规范输入
- Response model 负责规范输出
- 模型只是中间的结构转换器

这样你的系统才是完整接口，而不是“收一段字符串，回一段不稳定字符串”。

### 8. GET 请求为什么通常不该有 body

FastAPI 文档也专门提醒，虽然技术上某些情况下可以给 GET 带 body，但这不符合常见 HTTP 习惯，也不利于文档与代理兼容。

这提醒你一点：  
AI 项目也不能因为有模型就忽略基本 Web 规范。  
接口设计依然要遵守传统后端的可维护性原则。

### 9. 把它和前面学的内容串起来

这一篇和前面的学习内容其实可以形成一条很清晰的链：

- Prompt：让模型理解任务
- Consistency：让模型更稳定
- Structured Outputs：让结果结构可控
- Function Calling：让模型发起行动
- FastAPI/Pydantic：让应用接口也保持结构清晰

你现在学到这里，应该开始形成一个完整意识：

**AI 应用不是只有模型层需要结构化，API 层也必须结构化。**

## 关键术语

- `request body`：HTTP 请求中承载主要输入数据的正文
- `BaseModel`：Pydantic 中定义结构化数据模型的基类
- `path parameter`：直接出现在 URL 路径中的参数
- `query parameter`：通过 URL 查询字符串传递的参数
- `JSON Schema`：描述 JSON 结构的规范
- `OpenAPI`：自动化 API 文档标准

## 练习题

1. 为什么 FastAPI 推荐用 Pydantic 模型定义 request body？
2. `BaseModel` 帮你自动完成了哪些事情？
3. 什么情况下字段是必填，什么情况下是可选？
4. path parameter、query parameter、request body 有什么区别？
5. 为什么 AI 项目里也要重视 FastAPI 的结构化输入输出？
6. 请你为 `/extract` 接口设计一个最小 Request Model 和 Response Model。

## 学完后的自测标准

- 你能写出一个最小的 Pydantic 请求模型
- 你能区分 body、query、path 三种输入来源
- 你能说明 Pydantic 和 Structured Outputs 在接口思维上的相通点

