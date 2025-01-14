# Llama，Llama，Llama：3 个简单步骤，使用你的内容进行本地 RAG

> 原文：[`www.kdnuggets.com/3-simple-steps-to-local-rag-with-your-content`](https://www.kdnuggets.com/3-simple-steps-to-local-rag-with-your-content)

![3 个简单步骤，使用你的内容进行本地 RAG](img/c90e6a1c2995d52c9c4f9fc3fc7dc6dd.png)

作者提供的图片 | Midjourney & Canva

你想要本地 RAG 且麻烦最少吗？你是否有一堆文档，想把它们当作知识库来增强语言模型？想要构建一个了解你所需内容的聊天机器人？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

好吧，这里有一个可以说是最简单的方法。

我可能不是最优化的推理速度、向量精度或存储系统，但它超级简单。如果需要，可以进行调整，但即使没有，短时间的教程也应该能让你的本地 RAG 系统完全运行起来。由于我们将使用 Llama 3，我们也可以期待一些很好的结果。

我们今天使用了什么工具？3 只 Llama：Ollama 用于模型管理，Llama 3 作为我们的语言模型，LlamaIndex 作为我们的 RAG 框架。Llama，Llama，Llama。

让我们开始吧。

## 第一步：Ollama，用于模型管理

Ollama 可以用于管理和与语言模型交互。今天我们将使用它进行模型管理，并且由于 LlamaIndex 能够直接与 Ollama 管理的模型交互，因此间接用于交互。这将使我们的整体过程更加简便。

我们可以通过遵循应用程序的[GitHub 仓库](https://github.com/ollama/ollama)上的系统特定说明来安装 Ollama。

一旦安装完成，我们可以从终端启动 Ollama 并指定我们希望使用的模型。

## 第二步：Llama 3，语言模型

一旦 Ollama 安装并正常运行，我们可以从其 GitHub 仓库下载列出的任何模型，或者从其他现有语言模型实现中创建自己的 Ollama 兼容模型。使用 Ollama 运行命令将下载指定的模型，如果系统中未存在该模型，因此可以通过以下命令下载 Llama 3 8B：

```py
ollama run llama3
```

只需确保你有足够的本地存储来容纳 4.7 GB 的下载。

一旦 Ollama 终端应用程序以 Llama 3 模型作为后台启动，你可以继续并最小化它。我们将使用 LlamaIndex 从自己的脚本与之进行交互。

## 第三步：LlamaIndex，RAG 框架

这个难题的最后一部分是[ LlamaIndex](https://www.llamaindex.ai/)，我们的 RAG 框架。要使用 LlamaIndex，你需要确保它已安装在你的系统上。由于 LlamaIndex 的打包和命名空间最近发生了变化，最好查看官方文档以确保在本地环境中安装 LlamaIndex。

一旦启动并运行，并且 Ollama 正在运行带有 Llama3 模型，你可以将以下内容保存到文件中（改编自[这里](https://docs.llamaindex.ai/en/stable/getting_started/starter_example_local/)）：

```py
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.llms.ollama import Ollama

# My local documents
documents = SimpleDirectoryReader("data").load_data()

# Embeddings model
Settings.embed_model = HuggingFaceEmbedding(model_name="BAAI/bge-base-en-v1.5")

# Language model
Settings.llm = Ollama(model="llama3", request_timeout=360.0)

# Create index
index = VectorStoreIndex.from_documents(documents)

# Perform RAG query
query_engine = index.as_query_engine()
response = query_engine.query("What are the 5 stages of RAG?")
print(response)
```

这个脚本执行了以下操作：

+   文档存储在“data”文件夹中

+   用于创建 RAG 文档嵌入的模型是来自 Hugging Face 的 BGE 变体

+   语言模型是前面提到的 Llama 3，通过 Ollama 访问

+   我们的数据查询（“RAG 的 5 个阶段是什么？”）是合适的，因为我在数据文件夹中放入了许多与 RAG 相关的文档

以及我们查询的输出：

```py
The five key stages within RAG are: Loading, Indexing, Storing, Querying, and Evaluation.
```

请注意，我们可能需要以多种方式优化脚本，以促进更快的搜索并保持某些状态（例如嵌入），但我将留给感兴趣的读者去探索。

## 最终思考

好吧，我们做到了。我们成功地在本地使用 Ollama 在 3 个相当简单的步骤中搭建了一个基于 LlamaIndex 的 RAG 应用程序。你可以做很多其他事情，包括优化、扩展、添加 UI 等，但简单的事实是，我们能够在几个代码行和一组最小的支持应用程序和库中构建我们的基准模型。

希望你喜欢这个过程。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为[KDnuggets](https://www.kdnuggets.com/) & [Statology](https://www.statology.org/)的管理编辑，以及[Machine Learning Mastery](https://machinelearningmastery.com/)的贡献编辑，Matthew 的目标是使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法，以及探索新兴的 AI。他致力于在数据科学社区中普及知识。Matthew 从 6 岁起就开始编程。

### 更多关于这个话题

+   [LangChain + Streamlit + Llama: 将对话式 AI 带到你的…](https://www.kdnuggets.com/2023/08/langchain-streamlit-llama-bringing-conversational-ai-local-machine.html)

+   [7 Steps to Running a Small Language Model on a Local CPU](https://www.kdnuggets.com/7-steps-to-running-a-small-language-model-on-a-local-cpu)

+   [GPT4All 是你的文档的本地 ChatGPT，并且它是免费的！](https://www.kdnuggets.com/2023/06/gpt4all-local-chatgpt-documents-free.html)

+   [RAG 与微调：哪个是提升你的 LLM 应用的最佳工具？](https://www.kdnuggets.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application)

+   [本地运行 LlaMA 2 的简单指南](https://www.kdnuggets.com/a-simple-guide-to-running-llama-2-locally)

+   [Octoparse 8.5：增强本地抓取及更多功能](https://www.kdnuggets.com/2022/02/octoparse-85-empowering-local-scraping.html)
