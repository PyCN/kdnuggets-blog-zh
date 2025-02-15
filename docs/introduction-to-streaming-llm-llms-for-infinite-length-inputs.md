# 介绍 Streaming-LLM：无限长度输入的 LLMs

> 原文：[`www.kdnuggets.com/introduction-to-streaming-llm-llms-for-infinite-length-inputs`](https://www.kdnuggets.com/introduction-to-streaming-llm-llms-for-infinite-length-inputs)

大型语言模型（LLM）改变了人们的工作方式。像 GPT 家族这样广泛使用的模型，每个人都习惯于这些模型。利用 LLM 的力量，我们可以快速解答问题，调试代码等。这使得模型在许多应用中非常有用。

LLM 的挑战之一是模型不适用于流应用，因为其无法处理超过预定义训练序列长度的长对话。此外，内存消耗也是一个问题。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 走上网络安全职业快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的 IT 组织

* * *

因此，以上问题促使研究来解决它们。这项研究是什么？让我们深入了解。

# StreamingLLM

StreamingLLM 是由 [Xiao 等人 (2023)](https://arxiv.org/pdf/2309.17453.pdf) 研究创建的框架，用于解决流应用问题。现有方法在预训练期间由于关注窗口的限制而受到挑战。

注意窗口技术可能是有效的，但在处理超过其缓存大小的文本时存在问题。这就是为什么研究人员尝试使用几个初始标记的键和值状态（关注沉降）与最近标记。StreamingLLM 与其他技术的比较可以在下图中看到。

![介绍 Streaming-LLM：无限长度输入的 LLMs](img/81538ada1aa8d1aa3f4a6c4af0152c6b.png)

StreamingLLM vs 现有方法（[Xiao 等人 (2023)](https://arxiv.org/pdf/2309.17453.pdf))

我们可以看到 StreamingLLM 如何使用关注沉降方法应对挑战。这种关注沉降（初始标记）用于稳定的关注计算，并将其与最近的标记结合起来，以提高效率，并在较长文本上保持稳定性能。

此外，现有方法存在内存优化问题。然而，LLM 通过在最近标记的键和值状态上保持固定大小窗口来避免这些问题。作者还提到了 StreamingLLM 作为滑动窗口重新计算基准，速度提升高达 22.2×。

从性能方面来看，StreamingLLM 提供了优于现有方法的准确度，如下表所示。

![引入 Streaming-LLM：适用于无限长度输入的 LLM](img/ce77ac4fa07427e249d2be0758ba66d5.png)

StreamingLLM 准确度 ([Xiao *et al*. (2023)](https://arxiv.org/pdf/2309.17453.pdf))

上表显示，StreamingLLM 的准确度可以超越基准数据集中的其他方法。这就是为什么 StreamingLLM 可能在许多流媒体应用中具有潜力。

要尝试 StreamingLLM，你可以访问他们的 [GitHub 页面](https://github.com/mit-han-lab/streaming-llm)。将代码库克隆到你想要的目录中，然后在 CLI 中使用以下代码来设置环境。

```py
conda create -yn streaming python=3.8
conda activate streaming

pip install torch torchvision torchaudio
pip install transformers==4.33.0 accelerate datasets evaluate wandb scikit-learn scipy sentencepiece

python setup.py develop
```

然后，你可以使用以下代码运行带有 LLMstreaming 的 Llama 聊天机器人。

```py
CUDA_VISIBLE_DEVICES=0 python examples/run_streaming_llama.py  --enable_streaming
```

总体的样本比较可以在下面的图片中看到。

![引入 Streaming-LLM：适用于无限长度输入的 LLM](img/76e7b64ada489af40bed5d29e4af83dc.png)

StreamingLLM 在更长对话中表现出色 ([Streaming-llm](https://github.com/mit-han-lab/streaming-llm))

这就是对 StreamingLLM 的介绍。总体来看，我相信 StreamingLLM 可以在流媒体应用中占有一席之地，并有助于改变未来应用的工作方式。

# 结论

在流媒体应用中使用 LLM 会从长远来看对业务有所帮助；然而，实施过程中存在挑战。大多数 LLM 无法超越预定义的训练序列长度，并且内存消耗较高。 [Xiao *et al*. (2023)](https://arxiv.org/pdf/2309.17453.pdf) 开发了一种新的框架，称为 StreamingLLM，以处理这些问题。通过使用 StreamingLLM，现在可以在流媒体应用中实现有效的 LLM。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉及各种 AI 和机器学习话题的写作。

### 更多相关话题

+   [机器学习不像你的大脑 第五部分：生物神经元……](https://www.kdnuggets.com/2022/07/machine-learning-like-brain-part-5-biological-neurons-cant-summation-inputs.html)

+   [RedPajama 项目：一个开源倡议，旨在民主化 LLM](https://www.kdnuggets.com/2023/06/redpajama-project-opensource-initiative-democratizing-llms.html)

+   [Falcon LLM：开源 LLM 的新王者](https://www.kdnuggets.com/2023/06/falcon-llm-new-king-llms.html)

+   [确保 LLM 的少量示例提示选择可靠](https://www.kdnuggets.com/2023/07/ensuring-reliable-fewshot-prompt-selection-llms.html)

+   [水印如何帮助缓解 LLM 的潜在风险？](https://www.kdnuggets.com/2023/03/watermarking-help-mitigate-potential-risks-llms.html)

+   [在您的笔记本上轻松探索 LLMs，使用 openplayground](https://www.kdnuggets.com/2023/04/explore-llms-easily-laptop-openplayground.html)
