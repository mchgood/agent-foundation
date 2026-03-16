# Function Calling 学习笔记

## 资料来源

- OpenAI 官方文档：<https://platform.openai.com/docs/guides/function-calling>

## 学习目标

- 理解 Function Calling 的本质和完整调用闭环
- 掌握函数定义中的 `name`、`description`、`parameters`、`strict` 的作用
- 明白模型、工具、应用代码三者的职责边界
- 能把 Function Calling 与 Agent 行为设计联系起来

## 笔记概要

Function Calling 的核心不是让模型“自己执行函数”，而是让模型在合适的时候输出结构化行动意图，由你的程序去执行工具，再把结果回传给模型。  
它是 Agent 系统中最关键的桥梁之一：让模型从“只会生成文本”升级为“能进入外部行动链路”。

## 详细笔记

### 1. 先纠正一个最常见误解

模型不会真的执行你的 Python 函数、数据库查询或 HTTP 请求。  
它只会返回一个结构化结果，告诉你的应用：

- 我认为现在应该调用哪个工具
- 这个工具应该传什么参数

然后由应用代码真正执行。  
这说明 Function Calling 从第一天开始就带有明确的工程边界：

- 模型负责决策
- 程序负责执行
- 程序负责把执行结果回传

### 2. 一条完整调用链长什么样

Function Calling 的标准闭环可以理解为五步：

1. 应用定义可用工具给模型
2. 模型决定是否调用工具
3. 模型返回 tool call 或 function call 结构
4. 应用执行真实函数
5. 应用把结果返回给模型，模型继续生成最终回答

这个流程非常重要，因为很多人只学到了第 2 步，以为“模型返回函数名”就结束了。  
实际上，真正的系统价值来自完整闭环。

### 3. 工具定义为什么是稳定性的核心

在 OpenAI 文档中，函数定义至少包括：

- `name`
- `description`
- `parameters`
- `strict`

其中最容易被低估的是 `description` 和 `parameters`。

#### 3.1 `description` 的作用

它不是注释，而是给模型看的“使用说明书”。  
描述不清楚，模型就很难判断：

- 什么时候该用这个函数
- 什么时候不该用
- 它和其他函数有什么区别

#### 3.2 `parameters` 的作用

参数定义使用 JSON Schema。  
这意味着函数调用不是模糊语言行为，而是结构化协议行为。

参数 schema 写得越清晰，模型传参就越稳。

### 4. `strict: true` 和严格参数约束

文档推荐开启 `strict: true`，原因非常直接：

- 减少参数漂移
- 降低字段缺失风险
- 避免类型错误
- 提高工具调用的可执行性

官方还明确建议：

- object 字段用 `additionalProperties: false`
- 所有声明字段都写进 `required`
- 可选值通过 `null` 表达

这说明 Function Calling 不只是“让模型决定行动”，而是“让模型按严格合同发起行动”。

### 5. 工程上最值得记住的几条建议

#### 5.1 工具数量不要太多

文档提到，工具数量过多会降低准确率。  
这是因为模型面对的选择空间变大了，误选和混淆会更多。

这和软件设计也一致：  
接口暴露得越多，调用复杂度越高。

#### 5.2 已知参数不要让模型重复生成

如果某些值应用本来就知道，比如用户 ID、订单 ID、当前会话上下文，就不应该让模型猜。  
这属于非常典型的“把确定性交给不确定系统处理”的反模式。

更好的做法是：

- 模型只生成它必须决定的参数
- 应用补齐你已知的上下文参数

#### 5.3 要假设可能有多个 tool call

一个响应里不一定只有一个函数调用。  
随着 Agent 场景变复杂，多调用是正常情况。

因此系统设计不能只覆盖“单调用 happy path”，而要考虑：

- 0 次调用
- 1 次调用
- 多次调用
- 调用失败后的回退

### 6. Function Calling 对 Agent 的意义

如果没有 Function Calling，模型最多只能告诉你：

- 你可以去搜索资料
- 你可以查数据库
- 你可以调用天气接口

它只能“建议行动”。  
而有了 Function Calling，模型就能输出：

- 调用 `search_notes`
- 调用 `get_weather`
- 调用 `lookup_order_status`

这意味着模型开始成为流程中的决策节点，而不只是文本生成器。

这就是为什么它被视为 Agent 基础设施，而不是附属特性。

### 7. 和 Structured Outputs 的关系

你应该把这两者区分为：

- Structured Outputs：结果结构化
- Function Calling：行为结构化

前者解决“最终输出给谁消费”；  
后者解决“下一步动作该怎么发起”。

但它们共享相同的工程哲学：  
自然语言能力需要被 schema 约束，才能稳定进入系统链路。

## 关键术语

- `function calling`：模型输出函数调用意图和参数的机制
- `tool call`：模型请求外部工具执行某个动作的结构化表示
- `description`：给模型看的工具使用说明
- `parameters`：函数参数的 JSON Schema 定义
- `strict: true`：要求函数参数严格遵守 schema 的设置

## 练习题

1. 为什么说模型并不会真的执行函数？
2. Function Calling 的 5 步闭环分别是什么？
3. 工具 `description` 为什么非常关键？
4. 为什么已知参数不应该让模型填写？
5. 什么情况下系统必须考虑多个 tool call？
6. 请你为一个 `search_notes` 工具写出函数用途说明和参数设计思路。
7. Structured Outputs 和 Function Calling 的差异是什么？

## 学完后的自测标准

- 你能完整讲出 Function Calling 的标准闭环
- 你能为一个工具写出清晰的 description 和参数 schema 思路
- 你能解释它为什么是 Agent 的基础设施

