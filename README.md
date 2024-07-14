# AskAnything
基于大模型从0到1建设智能客服，包括：FAQ、多轮问答和文档问答三类任务。项目衍生于在浙江大华的0到1的智能客服项目经验
## 业务数据特点
由于企业内容数据的私有化属性，只从宏观角度介绍数据概况和处理时遇到的困难
## 技术选型
* 根据数据特点，明确业务场景需要实现的目标，规划FAQ和文档问答的建设
* 调研传统智能客服框架和基于大模型的智能客服框架的区别：
  1.[RASA](https://github.com/RasaHQ/rasa)
  2.[Qanything](https://github.com/netease-youdao/qanything)
  3.[MaxKB](https://github.com/1panel-dev/MaxKB)
* 调研[LangChain](https://github.com/langchain-ai/langchain)、[LlamaIndex](https://github.com/run-llama/llama_index)等技术框架
* 调研RAG、Embedding模型、Rerank等技术的原理，在搜索时的具体的应用场景和作用
## 产品框架
* 技术框架
* 用户流程
## 模型测试
* 测试BGE/BCE/M3E等模型在企业内部数据集上进行问答的效果
* 回答准确率提升
## 示例
## 未来规划探讨
* 如何与意图识别结合，比如意图分类、query改写等过程 
