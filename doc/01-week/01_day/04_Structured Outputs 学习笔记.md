# Structured Outputs 学习笔记

## 资料来源

- OpenAI 官方文档：<https://platform.openai.com/docs/guides/structured-outputs>
- 参考补充：<https://platform.openai.com/docs/guides/migrate-to-responses>

## 学习目标

- 理解 Structured Outputs 与普通 JSON 输出的本质区别
- 掌握 JSON Schema、`strict: true`、`additionalProperties: false` 的作用
- 分清 Structured Outputs 和 Function Calling 在工程中的分工
- 能把模型输出设计成可被程序稳定消费的接口结果

## 笔记概要

这篇文档讲的是如何让模型按照你提供的 schema 输出，而不是“尽量像 JSON”。  
它是把模型输出从“人可读文本”推进到“程序可消费结构”的关键一步。  
对于任何需要解析模型结果的场景，例如提槽、信息抽取、工具参数生成、状态机输出，Structured Outputs 都是最核心的工程能力之一。

## 详细笔记

### 1. 先分清三个层次

学习这一篇时，你必须先区分下面三种情况：

1. 普通文本输出
2. JSON mode
3. Structured Outputs

它们看起来都和“输出结构化内容”有关，但稳定性不是一个级别。

#### 1.1 普通文本输出

你只靠 Prompt 要求模型“请按 JSON 返回”。  
这种方式最不稳定，因为模型仍然在自由生成文本，只是尽量模仿 JSON。

#### 1.2 JSON mode

JSON mode 比普通文本更进一步，可以保证输出大体是合法 JSON。  
但它仍然不能保证：

- 字段是否齐全
- 类型是否正确
- 是否出现额外字段
- 层级是否稳定

#### 1.3 Structured Outputs

Structured Outputs 的目标是让模型遵循你定义的 JSON Schema。  
这意味着你不只是说“给我 JSON”，而是明确规定：

- 根结构是什么
- 每个字段类型是什么
- 哪些字段必填
- 是否允许额外属性

这才是真正适合程序处理的输出方式。

### 2. 为什么 Schema 是核心

Schema 的本质是输出合同。  
在没有 schema 时，输出格式主要依赖模型“理解你想要什么”；有 schema 后，输出变成一个可以验证的接口定义。

你可以把 schema 理解成：

- 给模型看的约束
- 给程序看的验证规则
- 给团队看的接口文档

这和后面 FastAPI/Pydantic 的思路完全一致。

### 3. `strict: true` 是为什么被反复强调

OpenAI 官方把 `strict: true` 放得很重要，因为它决定了模型是否要严格遵循 schema。

如果不开 strict，模型可能只是“大致参考”；  
开了 strict，才更接近真正的结构约束。

学习这一节时你应该明确记住：

- 想要工程稳定，strict 通常不是可选项
- 想要少写兜底解析逻辑，strict 是必要条件之一

### 4. `additionalProperties: false` 为什么重要

很多初学者只定义了字段，却没有限制额外字段。  
结果是模型除了你要的内容，还会多输出一些“看起来合理但程序没准备好处理”的字段。

`additionalProperties: false` 的作用，就是把这种不确定性砍掉。  
它代表一个很重要的工程思想：

**接口应该是封闭定义，而不是开放猜测。**

### 5. 必填字段与可选字段怎么理解

在 schema 里，你要明确区分：

- 必须稳定存在的信息
- 可以缺省的信息

如果一个字段对后续代码逻辑很关键，例如：

- `intent`
- `slots`
- `confidence`

那它通常应该是 required。  
如果某个字段只是补充信息，就可以设计为可选。

这里最重要的不是语法，而是接口思维：  
你必须先想清楚“后续代码最依赖什么”。

### 6. 它和 Function Calling 的关系

Structured Outputs 和 Function Calling 很容易被混淆，所以这里一定要拆开理解。

Structured Outputs 解决的是：

- 最终输出结构是否稳定

Function Calling 解决的是：

- 模型是否应该调用某个函数
- 调哪个函数
- 传什么参数

但两者也不是完全分离。  
OpenAI 的严格 function calling 底层也依赖 schema 约束思路。

你可以这样记：

- Structured Outputs：结果结构化
- Function Calling：行动结构化

### 7. 在 Responses API 中的位置

在 OpenAI 新接口体系中，Structured Outputs 已经成为 Responses API 主线的一部分。  
也就是说，它不是边角能力，而是新一代应用主流程中的标准能力。

这意味着你以后做系统设计时，不应该把它看成“高级特性”，而应该看成默认工具箱之一。

### 8. 它解决的真正问题是什么

这篇文档解决的是一个非常底层但关键的问题：

**模型输出如何从“给人看”变成“给程序接”。**

如果没有这一步，你后面会遇到这些麻烦：

- 提槽接口要写大量字符串解析
- JSON 结果偶尔缺字段导致程序报错
- 模型多输出一层嵌套，前端直接崩
- 多轮任务里中间状态无法稳定保存

而 Structured Outputs 的价值就在于，把这些风险前移为“接口设计问题”。

## 关键术语

- `JSON Schema`：描述 JSON 结构、字段类型和约束规则的标准方式
- `strict: true`：要求模型严格遵守定义 schema 的设置
- `additionalProperties: false`：禁止输出未声明字段的约束
- `required`：标记必填字段的 schema 项
- `Responses API`：OpenAI 新一代统一调用接口

## 练习题

1. 普通 Prompt 输出 JSON、JSON mode、Structured Outputs 三者有什么差别？
2. 为什么说 Schema 是“输出合同”？
3. 如果你没有设置 `strict: true`，会带来什么风险？
4. `additionalProperties: false` 为什么在工程上非常重要？
5. Structured Outputs 和 Function Calling 的区别是什么？
6. 请为“学习笔记提取器”设计一个最小 schema，并说明哪些字段应该 required。

## 学完后的自测标准

- 你能解释 Structured Outputs 为什么比“要求输出 JSON”更可靠
- 你能读懂一个最小 JSON Schema 的关键部分
- 你能为自己的 AI 接口设计一个稳定输出结构

