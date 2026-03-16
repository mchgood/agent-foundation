我去把这 12 周课程表压到**岗位导向**上：只保留对 **AI-Agent 开发 JD** 直接有用的内容，再用一组还在维护的官方资料做主线，避免学成知识盆景。

我已经把资料线收紧了：主干就用 **Anthropic / OpenAI / LangGraph / MCP / Dify / Hugging Face** 这几套仍在维护的官方资料。这样课程会更像岗位训练，而不是读一圈学术目录。

Thought for 1m 51s

好，下面这版是**按 AI-Agent 开发岗反推**出来的 12 周课程表，不走“传统 AI 大课全家桶”路线。默认你每周投入 **10–12 小时**，目标是在第 12 周结束时，拿到 **2 个能讲、能演示、能写进简历的项目**，同时把脑子里的知识从“草莽经验”整理成一套可复用的方法论。

这套课只围绕 6 个能力块展开：**LLM 交互契约、RAG、工作流/Agent 编排、MCP/工具接入、评测与可观测性、平台化交付**。之所以这么排，是因为官方文档现在都把这些当作 Agent 开发主轴：Anthropic 的提示工程文档覆盖了输出控制、工具使用和 agentic systems；OpenAI 文档把 Structured Outputs、Function Calling、Responses API、Agents、Tools、Evals 放在同一主干里；LangGraph 明确区分了 workflows 和 agents，并突出 durable execution、memory、human-in-the-loop；MCP 把“连接外部系统”定义成标准协议；Dify 则把 agentic workflows、knowledge、retrieval、knowledge pipeline 直接做成产品能力。

## 学习节奏

每周固定 4 件事：

- **读**：2–3 小时，只读官方文档和一小部分课程
- **做**：5–6 小时，必须写代码
- **记**：1–2 小时，写一页自己的方法笔记
- **讲**：1 小时，把本周成果整理成 README / 架构图 / 面试话术

------

# 12 周课程表

## 第 1 周：把“大模型调用”学成“交互契约”

**学什么**

- message 结构
- system prompt 的职责
- structured output
- function/tool calling 的基本范式
- prompt 为什么要像接口协议，而不是像聊天

**看什么**

- Anthropic `Prompting best practices`
- OpenAI `Responses API`、`Structured output`、`Function calling`
- Hugging Face LLM Course 第 1 章导论与 LLM 基本概念。

**做什么**

- 用 `FastAPI` 写两个接口：
  - `/chat`
  - `/extract`（固定返回 JSON）
- 对你现在已有的“提槽”能力，重写一版**结构化输出协议**

**本周产出**

- 一个最小可运行 AI 服务
- 一篇 1 页笔记：`Prompt 不是文案，是接口合同`

------

## 第 2 周：把 Prompt 从经验活，变成可复用模板

**学什么**

- few-shot 什么时候有用
- XML / 标签化上下文
- 输出约束
- 工具说明怎么写
- prompt 失败的常见模式

**看什么**

- Anthropic 的 prompt engineering 文档，重点看清晰指令、示例、XML structuring、tool use
- OpenAI 文档里与 structured outputs / function calling 对应的章节。

**做什么**

- 给你的“提槽 + 搜索 + 过滤 + 生成”链路，补一套：
  - system prompt 模板
  - tool schema
  - 错误兜底输出格式
- 做 20 条用例，手工观察稳定性

**本周产出**

- `prompts/` 目录
- `schemas/` 目录
- 第一版提示词规范文档

------

## 第 3 周：RAG 入门，但只学 Agent 真会用到的部分

**学什么**

- chunking
- embedding
- top-k retrieval
- rerank 是干嘛的
- 为什么“长上下文”不等于“不需要检索”

**看什么**

- Dify 的 Knowledge、Knowledge Retrieval、Knowledge Pipeline 文档
- Hugging Face LLM Course 中和 Transformers / 表征相关的入门部分。

**做什么**

- 做一个最小知识库：
  - 导入 Markdown / PDF / txt
  - 切分
  - 检索
  - 拼装上下文回答
- 先不追求炫技，先把链路走通

**本周产出**

- `rag-demo` 项目初版
- 一页图：`文档 -> chunk -> embedding -> retrieval -> answer`

------

## 第 4 周：把 RAG 做到能讲，不只是“能跑”

**学什么**

- 检索失败 vs 生成失败
- 拒答策略
- 引用返回
- 检索参数怎么影响结果
- 文档预处理为什么重要

**看什么**

- Dify Knowledge Retrieval 与 Knowledge Pipeline 编排说明
- OpenAI 文档中的 file search / retrieval / tools 总入口
- LangGraph 的 graph / state 概念先扫一遍。

**做什么**

- 给第 3 周的知识库补：
  - 引用来源
  - 基础拒答
  - 检索日志
  - 错误样本集
- 选 30 个问题做小评测

**本周产出**

- 一个能演示的 RAG 小项目
- 一份评测表：命中 / 错答 / 幻觉 / 拒答

------

## 第 5 周：正式进入 Agent 编排，先把 workflow 和 agent 分清

**学什么**

- workflow 和 agent 的边界
- 静态路径 vs 动态决策
- state 是什么
- memory 是什么，不是什么

**看什么**

- LangGraph `Overview`
- `Workflows and agents`
- `Graph API` / `Functional API`
- `Memory overview`。LangGraph 文档明确把 workflow 定义为预定代码路径，把 agent 定义为动态决定流程和工具使用；同时提供 persistence、memory、human-in-the-loop。

**做什么**

- 用 LangGraph 重画你现有项目：
  - Query 理解
  - 槽位抽取
  - 工具路由
  - 搜索
  - 过滤
  - 响应生成
- 先不接真实所有能力，先做 mock

**本周产出**

- Agent 架构图
- 第一版 State 设计
- 你自己的结论：哪些地方该规则，哪些地方该模型

------

## 第 6 周：让 Agent 真的“会调工具”

**学什么**

- tool schema 设计
- 什么时候用 tool，什么时候不用
- tool 结果怎么回写状态
- 多工具路由
- agent loop 的边界

**看什么**

- Anthropic `Tool use`
- OpenAI tools / function calling 主线
- LangGraph quickstart。

**做什么**

- 给 Agent 接 3 个真实工具：
  - 搜索工具
  - 知识库工具
  - 规则过滤工具
- 每个工具都写：
  - 入参 schema
  - 出参 schema
  - 错误处理策略

**本周产出**

- Agent 的 tool registry
- 一份《工具设计规范》

------

## 第 7 周：学 MCP，不是为了赶时髦，是为了把“工具接入”标准化

**学什么**

- MCP 的 client / server / host 概念
- MCP 适合什么，不适合什么
- 如何把内部能力暴露成标准工具
- 权限与安全边界

**看什么**

- MCP `What is MCP?`
- `Architecture overview`
- `Understanding MCP servers`
- `Build an MCP server`
- `Authorization` / `Security Best Practices`。MCP 官方把它定义为连接 AI 应用与外部数据源、工具、工作流的开放标准，并单独给了 server、client、authorization 和 security 文档。

**做什么**

- 把你现有项目里一个工具能力，封成最小 MCP server
- 例如：
  - 媒体搜索
  - 曲库查询
  - 规则过滤

**本周产出**

- 一个能跑的 MCP server
- 一页文档：`什么时候直接写 function，什么时候上 MCP`

------

## 第 8 周：把 Agent 做成“可恢复、可中断、可人工接管”的系统

**学什么**

- durable execution
- checkpoints
- human-in-the-loop
- interrupts
- time travel / replay
- 为什么生产 Agent 不能只靠一条 while loop 硬怼

**看什么**

- LangGraph `Durable execution`
- `Persistence`
- `Interrupts`
- `Thinking in LangGraph`
- 需要的话扫一眼前端 graph execution 展示。

**做什么**

- 给你的 Agent 加：
  - checkpoint
  - 人工审核点
  - 执行日志
  - 失败后恢复
- 至少演示一次“半路暂停 -> 恢复继续”

**本周产出**

- 一个真正像样的 Agent runtime demo
- 架构说明：状态、检查点、人工介入点

------

## 第 9 周：做评测，不然 Agent 永远靠感觉

**学什么**

- task eval
- tool selection eval
- retrieval eval
- trace 观察什么
- 怎么定义成功与失败
- 怎么做稳定性回归

**看什么**

- OpenAI docs 里 agents / evals / trace grading 的主线入口
- Anthropic 提示工程中关于通过 prompt 修正失败标准的部分。OpenAI 现在把 agents、tools、evals、trace grading 放在同一开发路径下；Anthropic 也强调不是所有失败都该靠改 prompt，有些问题是模型、成本、时延或工具链问题。

**做什么**

- 给你的两个项目分别建 20–30 条评测样本
- 至少做 3 类指标：
  - 结构化输出正确率
  - 工具命中率
  - 检索相关性 / 引用正确性

**本周产出**

- `evals/` 目录
- 一份评测报告
- 一份“最常见失败模式清单”

------

## 第 10 周：做平台化，不然你还是“会搭 demo 的人”

**学什么**

- AI 网关 / 服务层设计
- 配置中心
- 日志、trace、成本统计
- retry / timeout / fallback
- Java 主业务服务 + Python AI 服务怎么协作

**看什么**

- OpenAI docs 中 tools / Responses / context management 总入口
- LangGraph persistence / runtime 相关说明
- Dify 的平台与知识 API 文档。

**做什么**

- 统一你的项目结构：
  - `gateway`
  - `agent-service`
  - `knowledge-service`
  - `frontend`
- 至少做 1 个成本统计面板
- 接一套最基础的日志链路

**本周产出**

- 平台化目录结构
- 部署脚本
- 系统模块图

------

## 第 11 周：补前端工作台，让项目能被“看见”

**学什么**

- 最小可用 AI 工作台设计
- prompt / tool / knowledge / trace 的可视化
- 前后端联调
- 演示路径设计

**看什么**

- Dify 的应用、知识、检索相关页面逻辑
- LangGraph 前端概念页，理解 graph execution 如何映射到 UI。

**做什么**

- 用 Next.js 或 React 做一个工作台：
  - 输入请求
  - 查看工具调用
  - 查看检索结果
  - 查看最终输出
  - 查看 trace
- 不追求审美，追求可解释性

**本周产出**

- 一个能录屏的全栈 demo
- 5 张项目截图

------

## 第 12 周：求职封装周

**学什么**

- 项目怎么讲成 JD 语言
- 简历怎么从“Java 后端”变成“AI-Agent 开发”
- 面试中怎么解释：
  - workflow vs agent
  - function calling vs MCP
  - RAG 为什么会幻觉
  - 为什么要评测、checkpoint、HITL

**看什么**

- 回顾 Anthropic / OpenAI / LangGraph / MCP 这几条主线，整理成自己的知识图谱。

**做什么**

- 把项目整理成 3 份材料：
  - 简历项目描述
  - README
  - 面试口述稿
- 录 1 次 10 分钟项目讲解
- 模拟回答 20 个高频问题

**本周产出**

- 最终简历 v1
- GitHub 项目页
- 面试题答案库

------

# 12 周结束后的验收标准

到第 12 周，你至少应该达到这 8 条：

1. **能清楚讲出 workflow 和 agent 的区别**
2. **能自己实现结构化输出和 tool calling**
3. **能独立做一个最小 RAG 系统**
4. **能把工具能力封成普通 function 或 MCP server**
5. **能用 LangGraph 做状态、持久化、中断恢复**
6. **能做基础 eval，而不是只看“感觉还行”**
7. **能做一个有前端工作台的全栈 demo**
8. **能把项目讲成岗位语言，而不是只说“我写了点 prompt”**

------

# 你这 12 周最该避免的坑

## 不要把主线时间砸在这些地方

- 大而全传统 ML 课程
- 从零啃 PyTorch 训练体系
- LoRA / 微调 / 分布式训练
- 各种 Agent 框架横向试吃

这些不是完全没用，但对你当前目标，它们不是主战场。

## 主线只抓 6 个词

**Prompt 合同、RAG、Tool、State、Eval、Platform**

这 6 个词抓稳了，你的课程就不会跑偏。

------

# 我给你的执行建议

你现在最适合的组合是：

- **Python**：AI 编排、LangGraph、MCP、RAG
- **Java**：主业务服务、平台接口、工程化
- **React / Next.js**：工作台与演示层

这不是“技术信仰”，这是为了让你在 12 周后看起来像一个**能独立交付 AI-Agent 系统的工程师**，而不是一个会调几句 prompt 的人。

下一步最合理的动作，是把这 12 周计划再压成一份 **每周 10 小时的日历版安排**，直接细到“周一看什么、周三做什么、周末交付什么”。

好的，继续输出吧

我把它收紧成 **每周 10 小时的日历版安排**，直接按“学什么、哪天做什么、周末交付什么”来排。这样你不用再自己把 12 周大纲翻译成执行动作。

Thought for 18s

下面给你一版 **AI-Agent 开发岗 12 周日历版安排**。

我按你目前的情况来设计：

- 背景：**5+ 年后端，主力 Java，会 Python/Go**
- 目标：**武汉，20k+，AI 应用开发 / Agent 工程师**
- 当前状态：**能独立搭 Agent，但 AI 知识体系不成网**
- 每周投入：**10 小时**
- 核心原则：**只学对 JD 有直接帮助的内容**

------

# 总体时间分配

每周固定 4 个时间块，尽量不要改：

| 时间块   | 时长 | 用途                        |
| -------- | ---- | --------------------------- |
| 周二晚上 | 2h   | 学习主线资料                |
| 周四晚上 | 2h   | 跟着资料写最小 demo         |
| 周六上午 | 3h   | 做本周主功能                |
| 周日上午 | 3h   | 整理笔记、补 README、做验收 |

这样每周正好 **10 小时**。
 如果你某周工作忙，至少保住 **周六 + 周日** 这两个大块。

------

# 12 周日历版安排

------

## 第 1 周：建立 LLM 应用开发的基本骨架

## 本周目标

把“调模型”这件事，先变成一个**有协议的后端服务**。

## 学习重点

- message 结构
- system prompt 的职责
- structured output
- function calling 基本形式
- FastAPI + Pydantic 基础接口封装

## 周二晚上（2h）

读：

- OpenAI / Anthropic 文档中关于：
  - messages
  - system prompt
  - structured output
  - function calling

记笔记：

- 什么是“提示词合同”
- 为什么 AI 接口不能只返回自然语言

## 周四晚上（2h）

编码：

- 新建项目 `agent-foundation`
- 用 FastAPI 写两个接口：
  - `/chat`
  - `/extract`

要求：

- `/extract` 必须固定输出 JSON
- 用 Pydantic 定义 schema

## 周六上午（3h）

把你过去“提槽”的经验抽象成结构化接口：

例如：

- intent
- slots
- confidence
- raw_query

要求：

- 输入一句自然语言
- 输出统一 JSON，不允许自由散文

## 周日上午（3h）

整理：

- README 初版
- 一页笔记：《Prompt 不是文案，是接口协议》
- 补 10 条测试样例

## 本周交付

- 一个能运行的最小 AI 服务
- 一个结构化提槽接口
- 一篇方法笔记

------

## 第 2 周：把 Prompt 从手感活变成模板系统

## 本周目标

让你写 Prompt 不再靠“临场发挥”。

## 学习重点

- few-shot
- 标签化上下文
- XML / section 化输入
- 输出约束
- 提示词版本管理

## 周二晚上（2h）

读：

- Anthropic prompt engineering 文档
- 重点看：
  - 清晰指令
  - 示例
  - XML / 标签化结构
  - 工具描述

记笔记：

- Prompt 为什么要分区
- 示例什么时候会提升稳定性

## 周四晚上（2h）

编码：

- 建 `prompts/` 目录
- 建 `schemas/` 目录
- 把提槽 prompt 改造成模板化结构：

示例结构：

- role
- task
- constraints
- output_schema
- examples

## 周六上午（3h）

做 20 条真实测试语料，覆盖：

- 正常 query
- 模糊 query
- 多意图 query
- 噪音 query
- 错误输入

比较：

- 不加示例
- 加示例
- 不加 schema
- 加 schema

## 周日上午（3h）

整理：

- 输出一份《Prompt 编写规范 v1》
- 写一页总结：
  - 哪些改动最影响稳定性
  - 结构化输出的收益是什么

## 本周交付

- Prompt 模板目录
- 提槽 prompt 规范化版本
- 20 条测试语料与结果对比

------

## 第 3 周：RAG 最小链路搭建

## 本周目标

第一次把 **知识库 -> 检索 -> 回答** 跑通。

## 学习重点

- chunking
- embedding
- retrieval
- top-k
- 基础知识库接入

## 周二晚上（2h）

读：

- Dify 的 Knowledge / Retrieval 相关资料
- 补一点 embedding、chunking 基础概念

记笔记：

- 为什么文档不能整篇塞给模型
- chunk 切太大和切太小分别有什么问题

## 周四晚上（2h）

编码：

- 新建项目 `rag-assistant`
- 实现：
  - 文档导入
  - 文档切分
  - embedding
  - top-k retrieval

这里不用追求复杂，先通路。

## 周六上午（3h）

接入一个简单问答接口：

- 用户提问
- 先检索
- 再组装上下文
- 再让模型回答

要求：

- 返回检索到的片段
- 回答里保留引用片段编号

## 周日上午（3h）

整理：

- README
- 架构图
- 一页总结：《RAG 链路最小闭环》

## 本周交付

- 一个最小 RAG 项目
- 支持文档导入和问答
- 带引用编号返回

------

## 第 4 周：把 RAG 做到“能讲”

## 本周目标

不是只会跑，而是知道它哪里会错。

## 学习重点

- 检索失败 vs 生成失败
- 拒答
- 引用溯源
- 简单评测

## 周二晚上（2h）

读：

- retrieval / file search / 引用相关资料
- 看几个 RAG 错误案例

记笔记：

- 幻觉可能来自哪里
- 为什么有时不是模型错，是检索错

## 周四晚上（2h）

编码：

- 给 `rag-assistant` 加：
  - 引用来源
  - 无法回答时拒答
  - 检索日志打印

## 周六上午（3h）

自己造 30 条问题：

- 文档里明确有答案
- 文档里模糊有答案
- 文档里没有答案
- 需要跨段拼接答案

记录：

- 命中
- 错答
- 幻觉
- 拒答

## 周日上午（3h）

整理：

- 一份小评测表
- 一页笔记：《RAG 出错时先查哪一层》

## 本周交付

- RAG 项目可演示版
- 30 条测试集
- 基础评测结果

------

## 第 5 周：把 Workflow 和 Agent 分清

## 本周目标

你要开始形成 Agent 的系统观，而不是“多调几个节点”。

## 学习重点

- workflow vs agent
- state
- memory
- 静态流程 vs 动态决策

## 周二晚上（2h）

读：

- LangGraph overview
- workflows and agents
- state / memory 基础概念

记笔记：

- 什么场景适合 workflow
- 什么场景才需要 agent

## 周四晚上（2h）

画图：
 把你过去的“多媒体随心搜”拆成 6 层：

- query understanding
- slot extraction
- route planning
- search/retrieval
- result filtering
- response generation

## 周六上午（3h）

编码：

- 用 LangGraph 或你熟悉的方式
- 做一个 mock 版 Agent 流程
- 每个节点先只输出模拟数据也可以

目标：

- 把状态流转跑通

## 周日上午（3h）

整理：

- 画状态图
- 写一页说明：
  - 哪些节点必须规则化
  - 哪些节点可以交给模型

## 本周交付

- Agent 状态图
- 多媒体项目的标准分层架构图
- workflow/agent 边界说明

------

## 第 6 周：Tool Calling 与多工具路由

## 本周目标

让 Agent 真正具备“调工具”的能力。

## 学习重点

- tool schema
- tool registry
- 多工具路由
- 工具错误处理
- 工具结果回写状态

## 周二晚上（2h）

读：

- tool use / function calling 文档
- 看工具描述与 schema 示例

记笔记：

- 好 tool schema 的标准是什么
- 为什么工具名、参数名会影响调用效果

## 周四晚上（2h）

编码：

- 设计 3 个工具：
  - search_tool
  - kb_tool
  - filter_tool

每个工具都写：

- 输入 schema
- 输出 schema
- error schema

## 周六上午（3h）

把 3 个工具接进 Agent 流程：

- 根据意图选工具
- 执行后写回 state
- 出错时 fallback

## 周日上午（3h）

整理：

- 一份《工具设计规范》
- 一页笔记：《工具不是函数堆，是 agent 的外部能力边界》

## 本周交付

- Agent 工具注册表
- 3 个真实工具
- 多工具路由链路

------

## 第 7 周：MCP 入门与标准化接入

## 本周目标

理解 MCP 在你项目里的位置，不盲目神化，也不绕开它。

## 学习重点

- MCP 基本概念
- client / server / host
- 什么能力适合封 MCP
- 权限与安全边界

## 周二晚上（2h）

读：

- MCP intro
- architecture
- server / client 基本概念

记笔记：

- MCP 相比普通 function calling 多解决了什么问题

## 周四晚上（2h）

编码：

- 选一个已有能力封成最小 MCP server
- 推荐选：
  - 曲库搜索
  - 内容过滤
  - 媒体检索

## 周六上午（3h）

让你的 Agent 能调用这个 MCP server

要求：

- 输入输出明确
- 能打印调用日志
- 能处理调用失败

## 周日上午（3h）

整理：

- 一页总结：《什么时候直接 function calling，什么时候 MCP 更合适》
- README 补一节 MCP 接入说明

## 本周交付

- 一个最小 MCP server
- Agent 调 MCP 的 demo
- MCP 选型说明

------

## 第 8 周：Checkpoint、恢复与人工接管

## 本周目标

把 Agent 从 demo 升级成更像生产系统的形态。

## 学习重点

- durable execution
- checkpoint
- interrupt
- human-in-the-loop
- replay / recovery

## 周二晚上（2h）

读：

- LangGraph durable execution
- persistence
- interrupts

记笔记：

- 为什么生产 Agent 需要检查点
- 哪些地方该放人工介入点

## 周四晚上（2h）

编码：

- 给你的 Agent 加 checkpoint
- 加一个人工审核节点

## 周六上午（3h）

演示两种场景：

- 中途失败 -> 恢复执行
- 中途暂停 -> 人工确认 -> 继续执行

## 周日上午（3h）

整理：

- 画运行时架构图
- 写一页总结：《Agent 为什么不能只靠 while loop》

## 本周交付

- 可中断恢复的 Agent
- 人工审核点
- 运行时说明文档

------

## 第 9 周：做 Evals，不再靠感觉判断“挺准的”

## 本周目标

建立最小评测体系。

## 学习重点

- eval case
- success criteria
- structured output 正确率
- tool selection 正确率
- retrieval 相关性

## 周二晚上（2h）

读：

- agents / evals / trace grading 相关资料

记笔记：

- 什么叫一个“可评测”的 Agent 任务
- 为什么要把“效果”拆成多个子指标

## 周四晚上（2h）

编码：

- 建 `evals/` 目录
- 为提槽任务建立 20 条样本
- 为 RAG 问答建立 20 条样本
- 为 tool route 建立 20 条样本

## 周六上午（3h）

跑评测：

- 提槽字段正确率
- 工具调用命中率
- RAG 引用正确率
- 拒答合理性

## 周日上午（3h）

整理：

- 输出一份小报告
- 列 5 类最常见失败模式

## 本周交付

- 最小评测集
- 一份评测报告
- 失败模式清单

------

## 第 10 周：平台化重构

## 本周目标

让你的项目看起来像“系统”，不是几个脚本。

## 学习重点

- 服务拆分
- gateway
- config
- logging
- tracing
- retry / timeout / fallback
- Java + Python 协作边界

## 周二晚上（2h）

设计：
 统一项目目录结构，例如：

- `gateway/`
- `agent-service/`
- `knowledge-service/`
- `frontend/`
- `evals/`
- `docs/`

## 周四晚上（2h）

编码：

- 加配置文件
- 加日志中间件
- 加统一错误返回

## 周六上午（3h）

做两个工程化能力：

- 超时与重试
- fallback 策略

例如：

- 检索失败 -> 直接拒答
- 工具失败 -> 换备用工具 / 进入人工节点

## 周日上午（3h）

整理：

- 系统模块图
- 请求链路图
- 一页总结：《AI 应用平台最小工程化清单》

## 本周交付

- 平台化目录结构
- 统一配置 / 日志 / 错误处理
- 模块图与链路图

------

## 第 11 周：前端工作台

## 本周目标

把系统“展示出来”。

## 学习重点

- React / Next 最小页面组织
- trace 可视化
- prompt / tool / retrieval 结果展示
- demo 路径设计

## 周二晚上（2h）

设计页面：

- 输入区
- 工具调用区
- 检索结果区
- 最终输出区
- trace 区

## 周四晚上（2h）

编码：

- 搭一个最小前端壳子
- 接 Agent API
- 接 RAG API

## 周六上午（3h）

继续编码：

- 展示工具调用日志
- 展示引用片段
- 展示最终响应
- 展示失败原因

## 周日上午（3h）

整理：

- 截图
- README 补“使用说明”
- 录一个 3 分钟演示视频

## 本周交付

- 一个可演示的前端工作台
- 5 张截图
- 1 个短演示视频

------

## 第 12 周：求职封装

## 本周目标

把过去 11 周的成果，转成简历和面试资产。

## 学习重点

- 项目表达
- JD 关键词映射
- 面试讲述结构
- 高概率问题准备

## 周二晚上（2h）

写简历项目描述，至少整理 2 个项目：

### 项目 1

多媒体随心搜 Agent 系统

### 项目 2

企业知识库 RAG 助手 / Agent 工作台

## 周四晚上（2h）

整理 README：

- 背景
- 架构
- 核心能力
- 关键取舍
- 失败恢复
- 评测方法

## 周六上午（3h）

写面试口述稿：
 每个项目准备 5 分钟版本和 10 分钟版本。

结构统一：

- 业务背景
- 目标
- 架构
- 难点
- 如何评测
- 如何优化
- 最终结果

## 周日上午（3h）

准备高频问题答案，至少写 20 题，例如：

- workflow 和 agent 区别
- function calling 和 MCP 区别
- RAG 为什么会幻觉
- embedding 和 rerank 的区别
- 为什么要 checkpoint
- 如何做 tool schema
- 如何评测 agent
- 如何处理失败恢复
- Java 和 Python 在系统里的分工

## 本周交付

- 简历 v1
- 两个项目 README
- 面试题答案库
- 项目讲述稿

------

# 每周统一验收模板

每周周日固定做这 5 件事：

## 1. 代码验收

这周功能是否真的跑通。

## 2. 文档验收

README 是否更新。

## 3. 笔记验收

是否写了一页自己的理解，不是摘抄。

## 4. 演示验收

是否能录一个 1–3 分钟演示。

## 5. 求职验收

这周的内容能不能写进简历一句话。

------

# 你这 12 周的两个核心项目

## 项目 1：多媒体随心搜 Agent 系统

你已有基础，适合做成主项目。

### 应该突出

- Query understanding
- Slot extraction
- Tool routing
- Search / retrieval
- Result filtering
- Prompted generation
- Trace / fallback / eval

### 简历价值

这是你最有业务真实感的项目。

------

## 项目 2：企业知识库 RAG + Agent 工作台

这是补齐 JD 高频关键词用的。

### 应该突出

- Knowledge ingestion
- Chunking / retrieval / rerank
- Citation
- Refusal
- Eval
- Frontend console
- Observability

### 简历价值

这是你对齐 **RAG / 知识库 / 平台化** 岗位要求的关键项目。

------

# 这 12 周里，哪些内容不要扩张

不要中途分心去重学一堆东西：

- 不要系统啃传统机器学习大课
- 不要深挖 LoRA / 微调 / 训练
- 不要横向试十几个 Agent 框架
- 不要把时间耗在 UI 美化
- 不要把项目做成玩具聊天机器人

你现在要的是 **岗位对齐**，不是知识收藏。

------

# 最后给你一个执行准则

每周都用这句话检查自己有没有跑偏：

> 这周学的内容，能不能直接提升我在 AI-Agent 开发 JD 里的竞争力？

如果答案是否定的，就收回来。
 这个阶段最怕的不是学不够，而是学散了，像把发动机零件扔进洗衣机里一起转，热闹但不出车。

下面这一句是你 12 周后的目标定位：

> **资深后端出身，具备 LLM 应用、RAG、Agent 编排、MCP 接入、评测与平台化交付能力的 AI-Agent 开发工程师。**