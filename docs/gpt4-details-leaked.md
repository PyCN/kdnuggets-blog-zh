# GPT-4 细节已泄露！

> 原文：[`www.kdnuggets.com/2023/07/gpt4-details-leaked.html`](https://www.kdnuggets.com/2023/07/gpt4-details-leaked.html)

![GPT-4 细节已泄露！](img/1067a83c47c992ecaed5b9b38e3cedd7.png)

编辑提供的图片

很多人一直在想是什么让 GPT-4 比 GPT-3 更优秀。它在全球范围内引起了轰动。它是目前最受期待的 AI 模型，人们想了解更多。OpenAI 并没有发布任何关于 GPT-4 的信息，例如其规模、数据、内部结构，或者他们如何训练和构建它。我们都在想他们为什么要隐瞒这些信息。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

好吧，你马上就会知道，因为关于 GPT-4 的细节已被[泄露](https://archive.is/2RQ8X#selection-833.1-873.202)！

那么，我们发现了关于 GPT-4 的哪些细节呢？让我们深入了解一下……

# 模型规模

大型语言模型（LLM）多年来不断增长，模型规模也反映了这一点。在 2022 年，GPT-3 的模型规模为 1 万亿，这在过去 5 年中增长了[15,000 倍](https://d1.awsstatic.com/events/Summits/reinvent2022/AIM405_Train-and-deploy-large-language-models-on-Amazon-SageMaker.pdf)。据说 GPT-4 的规模是其前身 GPT-3 的 10 倍。它大约有 1.8 万亿个参数，跨越 120 层。GPT-4 的 120 层深度结构使其能够处理各种复杂任务——使其成为最先进的模型之一！

# 专家混合

OpenAI 使用的是 MOE - 专家混合。与 GPT-3 这个静态模型不同，GPT 是由 8 个 2200 亿参数的模型组成的专家混合。这 8 个 2200 亿参数的模型是在不同的数据和任务分布上训练的，利用了模型中的 16 个专家。每个模型大约有 1110 亿个参数用于多层感知器，每个专家有特定的角色，例如编码或格式化。

专家混合并不是一个新事物，它已经存在了一段时间。例如，谷歌使用了带有专家选择路由的专家混合，这意味着根据你提出的问题类型，它会将你路由到不同的专家，以回答你的问题。

GPT-4 大约使用了 55 亿个参数仅用于“注意力”机制，例如引导模型保持在当前主题上。

# 推理

推理是关于大语言模型如何进行预测的。与其他模型相比，GPT-4 的表现相当不错。据说每次前向传递推理生成 1 个令牌只使用了大约 2800 亿个参数和大约 560 万亿次浮点操作（衡量 GPU 性能的速率）。

# 数据集

你可以根据 GPT-4 的性能和作为最先进模型的表现来想象它使用了多少数据集。据称，GPT-4 的训练大约使用了 13 万亿个令牌，约合 10 万亿个词。它对基于文本的数据使用了 2 个周期，对基于代码的数据使用了 4 个周期。

数据集的实际大小未知，因为这些令牌中有一些是重复使用的，因此我们可以粗略估计数据集包含了数万亿个令牌。在内部，还有数百万行的指令，这些指令来自 ScaleAI 的数据微调。

# 上下文长度

在 GPT-4 的预训练阶段，它使用了 8000 个令牌的上下文长度。预训练后，序列长度是基于 8000 个令牌的微调。

# 批量大小

批量大小是指在模型更新之前处理的样本数量。批量大小在不断增加，OpenAI 使用了 6000 万的批量大小，相当于每个专家大约 750 万个令牌。为了确定实际的批量大小，你需要将这个数字除以序列长度。

# 训练成本

这是你们很多人会感兴趣的领域——训练成本。你可以想象 GPT-4 的构建和训练是多么昂贵。

OpenAI 大约花费了 2.1e25 次浮点操作（每秒浮点操作次数）来训练，使用了大约 25 个 A100 处理器，历时 3 个月。据说 GPT-4 的计算开销大约是 GPT-3.5 的 3 倍。还有人说 GPT-4 在提示方面的成本是 GPT-3 的 3 倍。

例如，如果 OpenAI 在云端训练的费用约为每小时 $1 的 A100 处理器，那么仅这一小时的训练成本将高达 6300 万美元。

# 推测解码

还有人说 OpenAI 可能使用了推测解码。关键词是‘可能’。这意味着他们使用了较小且更快速的模型来帮助解码令牌，然后将其作为一个批量输入到大型模型中。

这意味着如果较小模型做出的预测是正确的，那么较大模型将会同意这些预测。然而，如果较大模型拒绝了较小模型的预测，那么批量中的其余部分也会被丢弃。

# 总结一下

这一泄漏更多地反映了高层架构的泄漏，而不是模型的泄漏——这也是许多人所期待的。尽管不同，这种信息仍然很有用，因为我们继续看到大语言模型的增长，以及创建像 GPT-4 这样的 AI 模型需要多大的成本。

**[尼莎·阿雅](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一位数据科学家、自由技术写作人以及 KDnuggets 的社区经理。她特别关注提供数据科学职业建议或教程和理论知识。她还希望探索人工智能如何/可以促进人类寿命的不同方式。作为一名热心的学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [如果你没有合适的学位，如何进入数据分析领域](https://www.kdnuggets.com/2021/12/how-to-get-into-data-analytics.html)

+   [非结构化数据：2022 年分析的必备条件](https://www.kdnuggets.com/2022/01/unstructured-data-analytics-2022.html)

+   [每个数据科学家都应具备的 13 项技能](https://www.kdnuggets.com/2022/03/top-13-skills-every-data-scientist.html)

+   [21 份数据科学面试必备备忘单：解锁…](https://www.kdnuggets.com/2022/06/21-cheat-sheets-data-science-interviews.html)

+   [ETL 与机器学习有什么关系？](https://www.kdnuggets.com/2022/08/etl-machine-learning.html)

+   [8 篇创新的 BERT 知识蒸馏论文，它们改变了…](https://www.kdnuggets.com/2022/09/eight-innovative-bert-knowledge-distillation-papers-changed-nlp-landscape.html)
