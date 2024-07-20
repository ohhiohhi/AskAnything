# AskAnything
项目源自在某大型互联网公司从0到1搭建知识问答系统的经验。包括：FAQ、文档问答和任务型对话三类任务。（已脱敏）
## 业务数据特点
由于企业内容数据的私有化属性，只从宏观角度介绍数据概况和处理时遇到的困难
## 产品规划
根据数据特点，和业务需要实现的目标，规划FAQ、文档问答、任务型对话和RPA的建设
**任务**
根据业务目标，调研实现不同任务需要的功能
![image](https://github.com/user-attachments/assets/dbeeeff9-d0ef-4d50-8729-1f037c4da89f)


**MVP**
为保证系统快速上线，功能能够满足用户的基本需求，同时保证上线后的问答效果，提出MVP方案。功能设计需要保证后续迭代的可以实现正常的功能叠加，减少二次开发过程
* 功能
![image](https://github.com/user-attachments/assets/10ba8c6e-1934-4d1e-a110-ef66bff09d98)

**分类规划**
* FAQ
  ** 由于
* 文档问答
* 任务型对话
* RPA规划
## 技术选型
**客服框架调研**
* 调研传统智能客服框架和基于大模型的智能客服框架的区别：1.[RASA](https://github.com/RasaHQ/rasa)2.[Qanything](https://github.com/netease-youdao/qanything) 3.[MaxKB](https://github.com/1panel-dev/MaxKB) 4.[智谱Ai](https://zhuanlan.zhihu.com/p/704828374?utm_psn=1797042432065552385)

|   |  原理（流程图）  |  支持任务  |  功能点  |  优点  |  缺点  |
| --- | --- | --- | --- | --- | --- |
|  RASA  |  Rasa基于意图的问答 ![rasa-流程图.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/MAeqxYDQEpgdO8j9/img/24130173-e09b-4319-9d0d-5725da474ab9.png) Rasa基于LLM和RAG的意图分类 ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/MAeqxYDQEpgdO8j9/img/7678f410-c217-4129-afac-2e4cd68b494d.png)  |  FAQ问答  |  意图识别 实体抽取 上下文记忆 自定义动作  |  *   支持纯本地部署      *   具有模块化框架      *   可以外接知识图谱      *   可自定义回复动作       |  功能缺点： *   不能进行文档问答      模型缺点： *   组件的ML和DL模型组件过时      *   每次更新示例时需要重新训练       |
|  [QAnything](https://qanything.ai/docs/introduce)  |  基于RAG的两阶段检索机制，结合信息检索和文本生成![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/MAeqxYDQEpgdO8j9/img/3312fb3c-66cd-4a09-89bf-1b6873e351d1.png) 参数设置： *   检索：不设置阈值，返回TOP K      *   重排：阈值0.35       |  文档问答 FAQ问答  |  文档解析 混合检索（BM25+向量检索） 联网检索 业务场景定制 对话日志  |  *   支持纯本地部署      *   无文档数量上限      *   问答准确率高      *   支持多语言       |  数据侧： *   处理非结构化文档存在困难      *   从图片、pdf等非文本格式提取信息      *   文档数量增加，检索速度和生成效率      模型侧： *   大模型计算资源消耗大      *   对特定领域数据的依赖等问题       |
|  [MaxKB](https://maxkb.cn/docs/system_arch/)  |  ![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/MAeqxYDQEpgdO8j9/img/b396642d-0f50-4cca-afae-c3b818282e2b.png)  |   |  1.  知识库：      多格式文档导入、自动分段向量化；命中检测 2.  应用：      Agent工作流；嵌入第三方；对话日志；访问限制；用量统计 1.  系统管理       |  *   开箱即用      多格式多类型文档，在线/离线，文本自动拆分、向量化、RAG *   无缝嵌入      可以快速嵌入到第三方系统 *   灵活编排      工作流配置 *   模型中立      可自行接入其他大模型  |  模型： *   召回相似度Top N问题       |
|  智谱AI  |  ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/Mp7ldVyDdEEvqBQN/img/193b8094-eea9-413c-8a32-faf9c5ed03aa.jpg) 参数设置： *   重排：文本得分+向量得分，权重分别为0.3和0.7       |   |  query改写 路由 多路召回 文本检索+向量检索（ES混合检索）  |   |   |

**大模型技术框架调研**
* 调研[LangChain](https://github.com/langchain-ai/langchain)、[LlamaIndex](https://github.com/run-llama/llama_index)等技术框架

|   |  功能模块  |  具体功能  |  应用场景  |  缺点  |
| --- | --- | --- | --- | --- |
|  Langchain （适合复杂的对话系统，如聊天机器人，智能助手）  |  模板 Agent Chain 记忆 提示工程 模型接口 文档QA  |  *   从文档生成QA对      *   多轮对话记忆      *   根据输入内容路由到指定的链       |  *   更适合构建面向企业的、可以产生收入的产品。这些产品需要支持多种模型接口、提供丰富的功能和优化的性能。      *   适用于需要构建更加复杂的对话系统的场景，如聊天机器人、智能助手等。       |  *   抽象程度较高，可能增加代码复杂性和学习成本。      *   在某些情况下可能不够灵活，限制了底层代码的编写和调试。       |
|  LlamaIndex  |  数据连接 索引构建 查询接口  |   |  *   更适合用于构建RAG系统，提升大型语言模型在处理长文本或大量数据时的查询效率。      *   提供简化的数据索引和查询能力。      *   适用于需要处理大量数据、对查询性能有较高要求的场景，如搜索引擎、推荐系统等。       |  *   功能相对单一，主要侧重于检索和索引，不如LangChain功能丰富。      *   对模型接口的支持较少，可能限制某些特定需求的实现。       |

**大模型技术调研**
* 调研RAG、Function Calling、RAGFlow等技术的原理，以及在搜索时的具体应用场景和作用
|   |  原理  |  核心组件  |  适用场景  |  优点  |  缺点  |
| --- | --- | --- | --- | --- | --- |
|  RAG  |  外部知识库中检索相关信息来增强语言模型的回答能力  |  信息检索 文本生成  |  需要处理大量数据和复杂查询的场景，如知识问答、文档检索等  |  *   解决大语言模型的知识过期和细分领域的幻觉问题。      *   增强模型生成能力，提供更准确、更丰富的回答       |  *   需要预先构建和维护知识库，增加系统复杂性      *   对于实时性和非公开数据的处理能力有限       |
|  RAGFlow  |  ![image](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/Mp7ldVyDdEEvqBQN/img/d5cc8ac6-1b7f-48ca-baeb-682f30f831a4.jpg)  |  多路召回  |   |   |   |
|  Function Calling  |  将用户的自然语言请求转换为可执行的函数调用，并生成符合预定义函数签名的结构化输出，如JSON对象  |  *   系统接收用户问题并检查是否有可用函数      *   系统生成工具调用请求（ToolCall），应用程序执行请求的函数，并返回结果。      *   系统返回函数的响应（ToolCallResponse），生成最终的用户响应       |  需要实时数据或特定服务的场景，如天气预报、股票信息查询等  |  *   灵活的方式来扩展语言模型的能力，允许模型调用外部API或服务      *   可以获取实时数据，解决大模型知识滞后的问题       |  *   开发者定义和维护外部函数，增加开发和维护的复杂性      *   函数调用的准确性和可靠性依赖于外部服务的稳定性和响应时间       |


## MVP产品框架
**系统架构**
**FAQ框架**
**用户流程**
**功能需求**

## 问答测试
### 测试指标
**离线自动指标**
**离线人工指标**
**上线后业务指标**
测试开源模型在企业内部数据集上进行问答的效果

### 离线自动测试方案
**方法**
**测试集**
**示例代码**
### 离线人工测试方案


## 未来规划
* 意图识别结合，比如意图分类、query改写等过程