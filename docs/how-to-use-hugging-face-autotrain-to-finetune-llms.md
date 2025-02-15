# 如何使用 Hugging Face AutoTrain 对 LLM 进行微调

> 原文：[`www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms`](https://www.kdnuggets.com/how-to-use-hugging-face-autotrain-to-finetune-llms)

![如何使用 Hugging Face AutoTrain 对 LLM 进行微调](img/9535954ddf526bd0261f1d923485c8ba.png)

编辑器提供的图片

# 介绍

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

近年来，大型语言模型（LLM）改变了人们的工作方式，并已在教育、营销、研究等多个领域得到应用。鉴于其潜力，LLM 可以被增强以更好地解决我们的业务问题。这就是我们可以进行 LLM 微调的原因。

我们希望对 LLM 进行微调的原因包括采用特定领域的用例、提高准确性、数据隐私和安全、控制模型偏见等。鉴于所有这些好处，学习如何对 LLM 进行微调是非常重要的，以便将其投入生产。

自动进行 LLM 微调的一种方法是使用[Hugging Face 的 AutoTrain](https://huggingface.co/docs/autotrain/v0.6.10/index)。HF AutoTrain 是一个无代码平台，提供 Python API，用于训练各种任务（如计算机视觉、表格数据和自然语言处理任务）的先进模型。即使我们对 LLM 微调过程了解不多，我们仍然可以利用 AutoTrain 的能力。

那么，它是如何工作的呢？让我们进一步探索。

# 开始使用 AutoTrain

即使 HF AutoTrain 是一个无代码解决方案，我们仍然可以在 AutoTrain 的基础上使用 Python API 进行开发。我们将探索代码路径，因为无代码平台在训练时并不稳定。然而，如果你想使用无代码平台，我们可以通过以下[页面](https://huggingface.co/new-space?template=autotrain-projects/autotrain-advanced)创建 AutoTrain 空间。整体平台将在下图中显示。

![如何使用 Hugging Face AutoTrain 对 LLM 进行微调](img/9a19c37473b3ada3812e03be6c43a855.png)

作者提供的图片

要使用 Python API 对 LLM 进行微调，我们需要安装 Python 包，可以使用以下代码运行。

```py
pip install -U autotrain-advanced
```

我们还会使用来自[HuggingFace](https://huggingface.co/datasets/tatsu-lab/alpaca)的 Alpaca 样本数据集，该数据集需要通过数据集包进行获取。

```py
pip install datasets
```

然后，使用以下代码获取我们需要的数据。

```py
from datasets import load_dataset 

# Load the dataset
dataset = load_dataset("tatsu-lab/alpaca") 
train = dataset['train']
```

此外，我们会将数据保存为 CSV 格式，因为我们在微调过程中需要它们。

```py
train.to_csv('train.csv', index = False)
```

在环境和数据集准备好之后，让我们尝试使用 HuggingFace AutoTrain 来微调我们的 LLM。

# 微调过程和评估

我将从 AutoTrain 示例中调整微调过程，我们可以在 [这里](https://colab.research.google.com/github/huggingface/autotrain-advanced/blob/main/colabs/AutoTrain_LLM.ipynb) 找到。为了开始这个过程，我们将要微调的数据放入名为 data 的文件夹中。

![如何使用 Hugging Face AutoTrain 微调 LLM](img/d43661e233bf6f5c8ac7bc62b0def2fc.png)

作者提供的图片

在本教程中，我尝试仅采样 100 行数据，以便我们的训练过程可以更快。数据准备好后，我们可以使用 Jupyter Notebook 来微调模型。确保数据中包含‘text’列，因为 AutoTrain 只会从该列读取。

首先，让我们使用以下命令运行 AutoTrain 设置。

```py
!autotrain setup
```

接下来，我们将提供运行 AutoTrain 所需的信息。下面的信息是关于项目名称和你想要的预训练模型。你只能选择 HuggingFace 中可用的模型。

```py
project_name = 'my_autotrain_llm'
model_name = 'tiiuae/falcon-7b'
```

然后我们将添加 HF 信息，如果你想将模型推送到仓库或使用私有模型。

```py
push_to_hub = False
hf_token = "YOUR HF TOKEN"
repo_id = "username/repo_name"
```

最后，我们将在下面的变量中初始化模型参数信息。你可以根据需要修改这些变量，以查看结果是否良好。

```py
learning_rate = 2e-4
num_epochs = 4
batch_size = 1
block_size = 1024
trainer = "sft"
warmup_ratio = 0.1
weight_decay = 0.01
gradient_accumulation = 4
use_fp16 = True
use_peft = True
use_int4 = True
lora_r = 16
lora_alpha = 32
lora_dropout = 0.045
```

在所有信息准备好之后，我们将设置环境以接受我们之前设置的所有信息。

```py
import os
os.environ["PROJECT_NAME"] = project_name
os.environ["MODEL_NAME"] = model_name
os.environ["PUSH_TO_HUB"] = str(push_to_hub)
os.environ["HF_TOKEN"] = hf_token
os.environ["REPO_ID"] = repo_id
os.environ["LEARNING_RATE"] = str(learning_rate)
os.environ["NUM_EPOCHS"] = str(num_epochs)
os.environ["BATCH_SIZE"] = str(batch_size)
os.environ["BLOCK_SIZE"] = str(block_size)
os.environ["WARMUP_RATIO"] = str(warmup_ratio)
os.environ["WEIGHT_DECAY"] = str(weight_decay)
os.environ["GRADIENT_ACCUMULATION"] = str(gradient_accumulation)
os.environ["USE_FP16"] = str(use_fp16)
os.environ["USE_PEFT"] = str(use_peft)
os.environ["USE_INT4"] = str(use_int4)
os.environ["LORA_R"] = str(lora_r)
os.environ["LORA_ALPHA"] = str(lora_alpha)
os.environ["LORA_DROPOUT"] = str(lora_dropout)
```

在我们的 notebook 中运行 AutoTrain，我们将使用以下命令。

```py
!autotrain llm \
--train \
--model ${MODEL_NAME} \
--project-name ${PROJECT_NAME} \
--data-path data/ \
--text-column text \
--lr ${LEARNING_RATE} \
--batch-size ${BATCH_SIZE} \
--epochs ${NUM_EPOCHS} \
--block-size ${BLOCK_SIZE} \
--warmup-ratio ${WARMUP_RATIO} \
--lora-r ${LORA_R} \
--lora-alpha ${LORA_ALPHA} \
--lora-dropout ${LORA_DROPOUT} \
--weight-decay ${WEIGHT_DECAY} \
--gradient-accumulation ${GRADIENT_ACCUMULATION} \
$( [[ "$USE_FP16" == "True" ]] && echo "--fp16" ) \
$( [[ "$USE_PEFT" == "True" ]] && echo "--use-peft" ) \
$( [[ "$USE_INT4" == "True" ]] && echo "--use-int4" ) \
$( [[ "$PUSH_TO_HUB" == "True" ]] && echo "--push-to-hub --token ${HF_TOKEN} --repo-id ${REPO_ID}" )
```

如果你成功运行 AutoTrain，你应该在目录中找到以下文件夹，其中包含 AutoTrain 生成的所有模型和标记器。

![如何使用 Hugging Face AutoTrain 微调 LLM](img/48f5dfbec3ac40fe83b88dfa81ee3637.png)

作者提供的图片

为了测试模型，我们将使用 HuggingFace transformers 包，代码如下。

```py
from transformers import AutoModelForCausalLM, AutoTokenizer

model_path = "my_autotrain_llm"
tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(model_path)
```

然后，我们可以根据提供的训练输入来评估模型。例如，我们使用“规律锻炼的健康益处”作为输入。

```py
input_text = "Health benefits of regular exercise"
input_ids = tokenizer.encode(input_text, return_tensors="pt")
output = model.generate(input_ids)
predicted_text = tokenizer.decode(output[0], skip_special_tokens=False)
print(predicted_text)
```

![如何使用 Hugging Face AutoTrain 微调 LLM](img/4ba169bff91d6f375bd16de96a9d151f.png)

结果肯定还有提升空间，但至少比我们提供的样本数据更接近。我们可以尝试调整预训练模型和参数来改善微调效果。

# 成功微调的技巧

有一些最佳实践你可能想要了解，以改善微调过程，包括：

1.  准备我们的数据集，确保质量符合代表性任务，

1.  研究我们使用的预训练模型，

1.  使用适当的正则化技术以避免过拟合，

1.  尝试从较小的学习率开始，逐渐增大，

1.  使用更少的 epoch 进行训练，因为 LLM 通常对新数据的学习速度较快，

1.  不要忽视计算成本，因为随着数据、参数和模型的增大，计算成本会变得更高，

1.  确保你遵循有关数据使用的伦理考虑。

# 结论

微调我们的大型语言模型对我们的业务流程有益，特别是当有特定要求时。通过 HuggingFace AutoTrain，我们可以加快训练过程，并轻松使用现有的预训练模型来进行微调。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和撰写媒体分享 Python 和数据技巧。Cornellius 涉及多种 AI 和机器学习主题的撰写。

### 更多相关主题

+   [如何使用 Hugging Face AutoTrain 微调 Mistral AI 7B LLM](https://www.kdnuggets.com/how-to-finetune-mistral-ai-7b-llm-with-hugging-face-autotrain)

+   [如何使用 GPT 生成创意内容与 Hugging Face…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [如何使用 Hugging Face Tokenizers 库来预处理文本数据](https://www.kdnuggets.com/how-to-use-the-hugging-face-tokenizers-library-to-preprocess-text-data)

+   [如何使用 Hugging Face 的 Datasets 库进行高效数据加载](https://www.kdnuggets.com/how-to-use-hugging-faces-datasets-library-for-efficient-data-loading)

+   [前 10 大机器学习演示：Hugging Face Spaces 版](https://www.kdnuggets.com/2022/05/top-10-machine-learning-demos-hugging-face-spaces-edition.html)

+   [一个社区正在为客户数据建模开发 Hugging Face](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)
