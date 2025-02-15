# AgentGPT：浏览器中的自主 AI 代理

> 原文：[`www.kdnuggets.com/2023/06/agentgpt-autonomous-ai-agents-browser.html`](https://www.kdnuggets.com/2023/06/agentgpt-autonomous-ai-agents-browser.html)

![AgentGPT：浏览器中的自主 AI 代理](img/78114a222c3f48689145faa460ff3b1f.png)

作者提供的 Gif | AgentGPT

[AgentGPT web](https://agentgpt.reworkd.ai/) 是一个自主 AI 平台，使用户能够直接在浏览器中轻松构建和部署可定制的自主 AI 代理。你只需为你的 AI 代理提供一个名称和目标，然后观看它开始实现你分配的目标。该代理将自主获取知识、采取行动、进行沟通并适应以完成其指定目标。

# AgentGPT 如何工作？

AgentGPT 通过语言模型链（称为代理）来实现特定目标。这个过程涉及一个代理考虑实现给定目标的最有效任务，执行这些任务，评估其表现，并不断生成额外任务。

> **注意：** AgentGPT web 仅提供 2 次免费运行。你可以订阅 Pro 版本以获得 GPT-4、每天 30 个代理的访问权限，以及最新插件的使用权限。

AgentGPT 的远见者坚信使 AI 潜力民主化，使其对所有人都可及，并促进一个以社区驱动的协作方法。这正是他们为成为一个开源平台感到无比自豪的原因。

> **注意：** 你也可以使用 Docker 本地运行它，或者通过按照 GitHub 仓库中的指南进行部署：[reworkd/AgentGPT](https://github.com/reworkd/AgentGPT)。

# ChatGPT、AgentGPT 和 AutoGPT 之间的区别

ChatGPT 是一个极其有用的工具，旨在提供准确、具体的回答，并促进深入的对话。它不仅仅回答问题，还帮助维持关于复杂话题的有意义讨论。

另一方面，AgentGPT 作为一个完整的平台来处理自主 AI 代理。你可以给代理设定目标，它将独立思考、学习并采取行动来实现该目标。

AgentGPT 和 AutoGPT 都是以自主 AI 代理为中心的令人印象深刻的项目。然而，它们有一些关键差异。AgentGPT 是一个基于 Web 的平台，允许直接在浏览器中创建和部署 AI 代理。相比之下，AutoGPT 是一个本地运行的工具，能够在计算机上开发 AI 代理以执行任务。

# 使用 AgentGPT 构建鸟类分类器

只需在 [reworkd.ai](https://agentgpt.reworkd.ai/) 上创建一个账户，并通过提供你的姓名和目标来部署你的代理。

在我们的案例中，我们要求 AgentGPT 开发一个鸟类图像分类 web 应用程序。

![AgentGPT：浏览器中的自主 AI 代理](img/bd0a72796160f4419aa190cc286bd61e.png)

作者提供的图像 | AgentGPT

**在前两次运行中，它的表现：**

+   初始数据集研究与选择

+   使用 TensorFlow 训练深度学习模型

+   使用合适的框架构建 Web 应用程序并部署训练好的模型

+   测试与优化

+   用户界面增强与功能添加

![AgentGPT: Autonomous AI Agents in your Browser](img/dcf542a556dfe8f27ba31b1fa9bf7a47.png)

作者提供的图像 | AgentGPT

初步结果可能不符合预期；然而，通过进一步的迭代，有可能得到改进。可能在大约 5 次运行后，应用程序中的编码问题将得到解决。

# 如何改善结果？

提示在动态调整语言模型的行为以符合我们代理的当前目标和任务中扮演着关键角色。目前，AgetGPT 的免费版本使用 gpt-3.5-turbo，显示出即使是提示中的最小细节也会显著影响生成的结果。

提高结果的措施：

1.  **通过示例增强模型准确性：** 为进一步提高模型的准确性，您可以在提示中提供 1、2 或多个示例。

1.  **计划与解决（PS）：** 一种建立在思维链提示上的技术。通过请求模型逐步指令，它实现了更准确的推理和问题解决能力，从而改善结果。通过查看示例了解更多信息：[AGI-Edgerunners/Plan-and-Solve-Prompting](https://github.com/AGI-Edgerunners/Plan-and-Solve-Prompting)。

1.  **ReAct：** 是“推理加行动”的缩写。ReAct 是一种强大的提示技术，它将推理和行动生成结合在一个输出中。这种方法允许模型有效地将思想与行动同步，从而产生更连贯和实用的响应。

1.  **升级到专业版或本地部署：** 对于高级功能，您可以选择升级到专业版，以访问 GPT-4。或者，您可以在本地运行应用程序，并加入 GPT-4 API 密钥，以利用 GPT-4 模型的增强功能和性能。

# 入门

![AgentGPT: Autonomous AI Agents in your Browser](img/4ef84ef18b31cc4629ef2d2e46a083fd.png)

来自[reworkd/AgentGPT](https://github.com/reworkd/AgentGPT)的图像

在本节中，我们将学习如何在本地设置和运行 AgentGPT。要开始，请按照以下命令操作。

```py
git clone https://github.com/reworkd/AgentGPT.git && cd AgentGPT
./setup.sh
```

在深入之前，必须确保您的环境已正确配置。为此，请按照以下步骤操作：

+   将[.env.example](https://github.com/reworkd/AgentGPT/blob/main/.env.example)文件复制到./next/目录中。

+   将复制的文件重命名为.env。

+   请根据您的要求花时间更新.env 文件中的值。

> **注意：** 您还可以修改[数据库](https://github.com/reworkd/AgentGPT/tree/main/db)（Mysql）、[后端](https://github.com/reworkd/AgentGPT/tree/main/platform)（FastAPI）和[前端](https://github.com/reworkd/AgentGPT/tree/main/next)（Nextjs）设置。

构建 Docker 镜像是一个无缝的过程，应能顺利进行。在继续之前，确保你的系统上已安装 Docker。

```py
docker-compose up --build
```

通过运行此命令，你将启动前端、后端和数据库的容器创建，建立一个全面的应用环境。

> **注意：** 你也可以在没有 Docker 的情况下开发和运行 AgentGPT，详情请阅读 [AgentGPT 文档。](https://docs.reworkd.ai/development/setup)

# 路线图

AgentGPT 目前处于测试阶段，开发者们正积极开发许多令人兴奋的功能。以下是即将推出的一些预览，以便让你了解未来的发展！

## 当前功能

1.  **用户管理与认证：** 高效管理系统中的用户及其认证。

1.  **代理运行保存与分享：** 无缝保存和分享代理运行，以确保协作与知识共享。

1.  **多语言动态翻译：** 启用多样语言的动态翻译，促进跨语言沟通。

1.  **AI 模型定制：** 根据你的具体需求定制 AI 模型，使其能够满足你的独特要求。

## 正在开发的功能

1.  高级网页浏览功能

1.  后端迁移到 Python

1.  带有向量数据库的长期记忆

1.  代理可控性

1.  文档大修

# 结论

我坚信，在先进的大型语言模型时代之后，我们将见证自主 AI 代理的出现。这一变革性的发展将彻底改变我们处理工作和任务的方式。

随着自主 AI 代理的出现，我们将不再需要详细列出实现目标的步骤。相反，只需定义目标并提供一个示例，这些代理将自主进行研究、实验和执行，以显著准确地达到预期结果。

如果你有兴趣了解更多，可以尝试阅读：

+   Baby AGI: 完全自主 AI 的诞生

+   AutoGPT: 你需要知道的一切

+   Mojo Lang: 新编程语言

+   LangChain 101: 构建你自己的 GPT 驱动应用

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络开发一款帮助精神疾病学生的 AI 产品。

### 更多相关话题

+   [为什么你需要了解自主 AI 代理](https://www.kdnuggets.com/2023/06/need-know-autonomous-ai-agents.html)

+   [基于 LLM 的自主代理背后的发展](https://www.kdnuggets.com/the-growth-behind-llmbased-autonomous-agents)

+   [Web LLM：将 LLM 聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [Baby AGI：完全自主 AI 的诞生](https://www.kdnuggets.com/2023/04/baby-agi-birth-fully-autonomous-ai.html)

+   [将人类与 AI 代理结合以提升客户体验](https://www.kdnuggets.com/2024/06/softweb/bringing-human-and-ai-agents-together-for-enhanced-customer-experience)

+   [文本分类任务的最佳架构：基准测试…](https://www.kdnuggets.com/2023/04/best-architecture-text-classification-task-benchmarking-options.html)
