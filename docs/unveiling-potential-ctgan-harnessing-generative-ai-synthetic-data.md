# 揭示 CTGAN 的潜力：利用生成式 AI 生成合成数据

> 原文：[`www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html`](https://www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html)

我们都知道，GANs（生成对抗网络）在生成非结构化合成数据方面正在获得关注，例如图像和文本。然而，使用 GANs 生成合成表格数据的工作非常少。合成数据具有许多好处，包括在机器学习应用、数据隐私、数据分析和数据增强中的应用。生成合成表格数据的模型很少，而 CTGAN（条件表格生成对抗网络）就是其中之一。像其他 GANs 一样，它使用生成器和判别器神经网络创建具有类似统计特性的合成数据。CTGAN 可以保留真实数据的基础结构，包括列之间的相关性。CTGAN 的额外好处包括通过模式特定的归一化、少量架构更改以及通过使用条件生成器和训练采样来解决数据不平衡，从而增强训练过程。

在这篇博客文章中，我使用了 CTGAN 生成基于 Kaggle 收集的*信用分析*数据集的合成数据。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业领域。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

## CTGAN 的优点

+   生成具有类似统计特性的合成表格数据，包括不同列之间的相关性。

+   保留真实数据的基础结构。

+   CTGAN 生成的合成数据可以用于各种应用，例如数据增强、数据隐私和数据分析。

+   可以处理连续、离散和分类数据。

## CTGAN 的缺点

+   CTGAN 需要大量真实表格数据来训练模型，并生成具有类似统计特性的合成数据。

+   CTGAN 计算资源消耗大，可能需要大量计算资源。

+   CTGAN 生成的合成数据的质量可能会根据用于训练模型的真实数据的质量而有所不同。

## 调整 CTGAN

像所有其他机器学习模型一样，CTGAN 在调优时表现更好。在调优 CTGAN 时需要考虑多个参数。然而，为了此次演示，我使用了‘ctgan library’中所有默认参数：

+   训练轮次：生成器和鉴别器网络在数据集上训练的次数。

+   学习率：模型在训练过程中调整权重的速度。

+   批量大小：每次训练迭代中使用的样本数量。

+   生成器和鉴别器网络的规模。

+   优化算法的选择。

CTGAN 还考虑了超参数，如潜在空间的维度、生成器和鉴别器网络中的层数，以及每层使用的激活函数。参数和超参数的选择会影响生成的合成数据的性能和质量。

## CTGAN 的验证

CTGAN 的验证较为棘手，因为它有诸如生成的合成数据质量评估的困难等局限性，特别是在处理表格数据时。尽管可以使用一些指标来评估真实数据与合成数据之间的相似性，但仍然难以确定合成数据是否准确地表示了真实数据中的潜在模式和关系。此外，CTGAN 容易过拟合，可能生成与训练数据过于相似的合成数据，这可能会限制其对新数据的泛化能力。

一些常见的验证技术包括：

+   统计测试：比较生成数据和真实数据的统计属性。例如，使用相关性分析、Kolmogorov-Smirnov 检验、Anderson-Darling 检验和卡方检验等测试，比较生成数据和真实数据的分布。

+   可视化：通过绘制直方图、散点图或热图来可视化相似性和差异。

+   应用测试：通过在实际应用中使用合成数据，查看其是否与真实数据表现相似。

# 案例研究

## 关于信用分析数据

信用分析数据包含连续格式和离散/类别格式的客户数据。为了演示目的，我已经通过删除包含空值的行和一些不需要的列来预处理数据。由于计算资源的限制，处理所有数据和所有列需要大量的计算能力，这超出了我的能力范围。以下是连续变量和类别变量的列列表（如“儿童数量 (CNT_CHINDREN)”等离散值被视为类别变量）：

类别变量：

```py
TARGET
NAME_CONTRACT_TYPE
CODE_GENDER
FLAG_OWN_CAR
FLAG_OWN_REALTY
CNT_CHILDREN
```

连续变量：

```py
AMT_INCOME_TOTAL
AMT_CREDIT
AMT_ANNUITY
AMT_GOODS_PRICE
```

生成模型需要大量的清洁数据来进行训练以获得更好的结果。然而，由于计算能力的限制，我从超过 30 万行的真实数据中选择了仅 10,000 行（准确来说是 9,993）进行演示。虽然这个数量可能被认为相对较少，但对于本次演示来说应该足够。

**真实数据的位置：**

[`www.kaggle.com/datasets/kapoorshivam/credit-analysis`](https://www.kaggle.com/datasets/kapoorshivam/credit-analysis)

**生成的合成数据的位置：**

+   [由 CTGAN 生成的合成信用分析数据（Kaggle）](https://www.kaggle.com/datasets/drrayislam/synthetic-credit-analysis-data-by-ctgan)

+   [由 CTGAN 生成的合成表格数据集（Research Gate）](https://www.researchgate.net/publication/369826197_Synthetic_Tabular_Data_Set_Generated_by_CTGAN)

+   DOI: [10.13140/RG.2.2.23275.82728](https://dx.doi.org/10.13140/RG.2.2.23275.82728)

![XXXXX](img/fdf11e6f132e3463fb533bc183f261da.png)

信用分析数据 | 图片来源：作者

# 结果

我生成了 10k（准确来说是 9997）个合成数据点，并将其与真实数据进行了比较。结果看起来不错，尽管仍有改进的空间。在我的分析中，我使用了默认参数，以'relu'作为激活函数，并训练了 3000 轮。增加训练轮数应能生成更像真实的合成数据。生成器和判别器的损失也很好，较低的损失表明合成数据与真实数据的相似度较高：

![XXXXX](img/30e9337394c50196dca7aa21e051513f.png)

生成器和判别器损失 | 图片来源：作者

在绝对对数均值和标准差图中的对角线上的点表示生成的数据质量良好。

![XXXXX](img/d78ae917b78e7848fb70785bd3e2aa52.png)

数值数据的绝对对数均值和标准差 | 图片来源：作者

以下图中的连续列的累积和虽然没有完全重叠，但接近，这表明合成数据生成良好且没有过拟合。分类/离散数据的重叠表明生成的合成数据接近真实数据。进一步的统计分析见以下图示：

![XXXXX](img/190d689ae7e86a3e1a682c07013671d4.png)

每个特征的累积和 | 图片来源：作者 ![XXXXX](img/777416ef5219c3b162900c0c5e9861b2.png)

特征分布 | 图片来源：作者

![XXXXX](img/947bd52a82a71333174503e8bc5edf2e.png)

特征分布 | 图片来源：作者 ![XXXXX](img/0a3117c69dbc7114cb6ef198c4ae97a5.png)

主成分分析 | 图片来源：作者

以下相关图显示了变量之间明显的相关性。值得注意的是，即使经过彻底的微调，真实数据和合成数据之间可能仍会存在属性差异。这些差异实际上可能是有益的，因为它们可能揭示数据集中隐藏的属性，这些属性可以用于创建新颖的解决方案。观察发现，增加训练轮数会提高合成数据的质量。

![XXXXX](img/757eb3ac9a934085d45b8900328c92c0.png)

变量之间的相关性（真实数据）| 图片来源于作者

![XXXXX](img/f0eec5a1fb2c4b88d74823fa59ef96ae.png)

变量之间的相关性（合成数据）| 图片来源于作者

样本数据和真实数据的摘要统计结果也表现出令人满意的效果。

![XXXXX](img/aa2df72c0607132355d2d0199d17d4e0.png)

真实数据与合成数据的摘要统计 | 图片来源于作者

# Python 代码

```py
# Install CTGAN
!pip install ctgan

# Install table evaluator to analyze generated synthetic data
!pip install table_evaluator 
```

```py
# Import libraries
import torch
import pandas as pd
import seaborn as sns
import torch.nn as nn

from ctgan import CTGAN
from ctgan.synthesizers.ctgan import Generator

# Import training Data
data = pd.read_csv("./application_data_edited_2.csv")

# Declare Categorical Columns
categorical_features = [
    "TARGET",
    "NAME_CONTRACT_TYPE",
    "CODE_GENDER",
    "FLAG_OWN_CAR",
    "FLAG_OWN_REALTY",
    "CNT_CHILDREN",
]

# Declare Continuous Columns
continuous_cols = ["AMT_INCOME_TOTAL", "AMT_CREDIT", "AMT_ANNUITY", "AMT_GOODS_PRICE"]

# Train Model
from ctgan import CTGAN

ctgan = CTGAN(verbose=True)
ctgan.fit(data, categorical_features, epochs=100000)

# Generate synthetic_data
synthetic_data = ctgan.sample(10000)

# Analyze Synthetic Data
from table_evaluator import TableEvaluator

print(data.shape, synthetic_data.shape)
table_evaluator = TableEvaluator(data, synthetic_data, cat_cols=categorical_features)
table_evaluator.visual_evaluation()
# compute the correlation matrix
corr = synthetic_data.corr()

# plot the heatmap
sns.heatmap(corr, annot=True, cmap="coolwarm")

# show summary statistics SYNTHETIC DATA
summary = synthetic_data.describe()
print(summary) 
```

# 结论

CTGAN 的训练过程预计会收敛到一个点，使得生成的合成数据与真实数据不可区分。然而，实际上，收敛不能得到保证。CTGAN 的收敛可能受到多种因素的影响，包括超参数的选择、数据的复杂性以及模型的架构。此外，训练过程的不稳定性可能导致模式崩溃，即生成器只产生有限的相似样本，而不是探索数据分布的全部多样性。

**[Ray Islam](https://blog.umd.edu/rayislam/)** 是一名数据科学家（AI 和 ML）以及美国 Deloitte 的顾问专家领导。他拥有来自美国马里兰大学帕克分校的工程学博士学位，并曾与洛克希德·马丁和雷声等主要公司合作，服务于 NASA 和美国空军等客户。Ray 还拥有来自加拿大的工程硕士学位、国际市场营销硕士学位以及来自英国的 MBA 学位。他还是即将出版的同行评审《国际 AI 伦理研究期刊》（INTJEAI）的主编，他的研究兴趣包括生成式 AI、增强现实、XAI 和 AI 伦理。

### 更多相关内容

+   [利用数据的力量的 3 个步骤](https://www.kdnuggets.com/2022/05/3-steps-harnessing-power-data.html)

+   [利用 ChatGPT 实现自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [合成数据平台：释放生成式 AI 的力量…](https://www.kdnuggets.com/2023/07/synthetic-data-platforms-unlocking-power-generative-ai-structured-data.html)

+   [揭示 Meta 的 Llama 2 的力量：生成式 AI 的进步？](https://www.kdnuggets.com/2023/07/unveiling-power-metas-llama-2-leap-forward-generative-ai.html)

+   [高保真合成数据，适用于数据工程师和数据科学家](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [如何使用合成数据克服机器学习模型训练中的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)
