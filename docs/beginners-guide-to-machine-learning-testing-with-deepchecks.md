# 机器学习测试初学者指南

> 原文：[`www.kdnuggets.com/beginners-guide-to-machine-learning-testing-with-deepchecks`](https://www.kdnuggets.com/beginners-guide-to-machine-learning-testing-with-deepchecks)

![机器学习测试初学者指南](img/4044f479fcdea590b166dc7a2be4d384.png)

作者提供的图像 | Canva

[DeepChecks](https://github.com/deepchecks/deepchecks)是一个 Python 包，提供各种内置检查，以测试模型性能、数据分布、数据完整性等问题。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 工作

* * *

在本教程中，我们将学习 DeepChecks，并使用它来验证数据集和测试训练过的机器学习模型，以生成全面的报告。我们还将学习如何在特定测试上测试模型，而不是生成完整的报告。

## 我们为什么需要机器学习测试？

机器学习测试对于确保 AI 模型的可靠性、公平性和安全性至关重要。它有助于验证模型性能、检测偏见、增强对抗攻击的安全性，尤其是在大型语言模型（LLMs）中，确保符合监管要求，并实现持续改进。像 Deepchecks 这样的工具提供了一个全面的测试解决方案，涵盖了从研究到生产的 AI 和 ML 验证的所有方面，使其在开发稳健、值得信赖的 AI 系统中变得不可或缺。

## 使用 DeepChecks 入门

在本入门指南中，我们将加载数据集并执行数据完整性测试。这个关键步骤确保我们的数据集是可靠和准确的，为成功的模型训练铺平道路。

1.  我们将通过使用`pip`命令安装 DeepChecks Python 包来开始。

```py
!pip install deepchecks --upgrade
```

1.  导入必要的 Python 包。

1.  使用 pandas 库加载数据集，该数据集包含 569 个样本和 30 个特征。[癌症分类](https://www.kaggle.com/datasets/sahilnbajaj/cancer-classification)数据集来源于乳腺肿块的细针抽取（FNA）数字化图像，每个特征代表图像中细胞核的一个特征。这些特征使我们能够预测癌症是良性还是恶性。

1.  使用目标列‘benign_0__mal_1’将数据集拆分为训练集和测试集。

```py
import pandas as pd
from sklearn.model_selection import train_test_split

# Load Data
cancer_data = pd.read_csv("/kaggle/input/cancer-classification/cancer_classification.csv")
label_col = 'benign_0__mal_1'
df_train, df_test = train_test_split(cancer_data, stratify=cancer_data[label_col], random_state=0)
```

1.  通过提供额外的元数据创建 DeepChecks 数据集。由于我们的数据集没有类别特征，因此我们将参数留空。

```py
from deepchecks.tabular import Dataset

ds_train = Dataset(df_train, label=label_col, cat_features=[])
ds_test =  Dataset(df_test,  label=label_col, cat_features=[])
```

1.  对训练数据集运行数据完整性测试。

```py
from deepchecks.tabular.suites import data_integrity

integ_suite = data_integrity()
integ_suite.run(ds_train)
```

生成报告将需要几秒钟。

数据完整性报告包含以下测试结果：

+   特征-特征相关性

+   特征-标签相关性

+   列中的单一值

+   特殊字符

+   混合空值

+   混合数据类型

+   字符串不匹配

+   数据重复

+   字符串长度超出范围

+   冲突标签

+   异常值样本检测

![数据验证报告](img/865a79fad12a02046b532cd134f5486a.png)

## 机器学习模型测试

让我们训练我们的模型，然后运行一个模型评估套件，以了解更多关于模型性能的信息。

1.  加载必要的 Python 包。

1.  构建三个机器学习模型（逻辑回归、随机森林分类器和高斯朴素贝叶斯）。

1.  使用投票分类器将它们集成起来。

1.  在训练数据集上拟合集成模型。

```py
from sklearn.linear_model import LogisticRegression
from sklearn.naive_bayes import GaussianNB
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import VotingClassifier

# Train Model
clf1 = LogisticRegression(random_state=1,max_iter=10000)
clf2 = RandomForestClassifier(n_estimators=50, random_state=1)
clf3 = GaussianNB()

V_clf = VotingClassifier(
    estimators=[('lr', clf1), ('rf', clf2), ('gnb', clf3)],
    voting='hard')

V_clf.fit(df_train.drop(label_col, axis=1), df_train[label_col]);
```

1.  训练阶段完成后，使用训练和测试数据集以及模型运行 DeepChecks 模型评估套件。

```py
from deepchecks.tabular.suites import model_evaluation

evaluation_suite = model_evaluation()
suite_result = evaluation_suite.run(ds_train, ds_test, V_clf)
suite_result.show()
```

模型评估报告包含以下测试结果：

+   未使用的特征 - 训练数据集

+   未使用的特征 - 测试数据集

+   训练测试性能

+   预测漂移

+   简单模型比较

+   模型推理时间 - 训练数据集

+   模型推理时间 - 测试数据集

+   混淆矩阵报告 - 训练数据集

+   混淆矩阵报告 - 测试数据集

套件中还有其他测试未运行，这是由于模型的集成类型。如果你运行了一个简单模型，如逻辑回归，你可能会得到完整的报告。

![模型评估报告 DeepChecks](img/5b6a1a9ca455e76fd75b13a22e6c1846.png)

1.  如果你想以结构化格式使用模型评估报告，你可以始终使用`.to_json()`函数将报告转换为 JSON 格式。

```py
suite_result.to_json()
```

![模型评估报告 JSON 输出](img/3d1b4ff48dbcd417baa9dfa18ad74e45.png)

1.  此外，你还可以使用`.save_as_html()`函数将此交互式报告保存为网页。

## 运行单项检查

如果你不想运行整个模型评估测试套件，你也可以在单项检查上测试你的模型。

例如，你可以通过提供训练和测试数据集来检查标签漂移。

```py
from deepchecks.tabular.checks import LabelDrift
check = LabelDrift()
result = check.run(ds_train, ds_test)
result
```

结果将得到一个分布图和漂移分数。

![运行单项检查：标签漂移](img/d5088bd7523c4e7de1f282d1ab4089ec.png)

你甚至可以提取漂移分数的值和方法论。

```py
result.value
```

```py
{'Drift score': 0.0, 'Method': "Cramer's V"}
```

## 结论

你学习旅程的下一步是自动化机器学习测试过程并跟踪性能。你可以通过遵循[Deepchecks In CI/CD](https://docs.deepchecks.com/stable/general/usage/ci_cd.html#using-deepchecks-ci-cd)指南来实现这一点。

在这篇适合初学者的文章中，我们学习了如何使用 DeepChecks 生成数据验证和机器学习评估报告。如果你在运行代码时遇到困难，我建议你查看一下[Kaggle Notebook 上的机器学习测试与 DeepChecks](https://www.kaggle.com/code/kingabzpro/machine-learning-testing-with-deepchecks?scriptVersionId=180206809)并自行运行。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个人工智能产品，帮助那些在心理健康方面挣扎的学生。

### 更多相关内容

+   [假设检验与 A/B 测试](https://www.kdnuggets.com/hypothesis-testing-and-ab-testing)

+   [A/B 测试：综合指南](https://www.kdnuggets.com/ab-testing-a-comprehensive-guide)

+   [像专家一样测试：Python 的 Mock 库逐步指南](https://www.kdnuggets.com/testing-like-a-pro-a-step-by-step-guide-to-pythons-mock-library)

+   [机器学习的有效测试](https://www.kdnuggets.com/2022/01/effective-testing-machine-learning.html)

+   [机器学习中训练数据与测试数据的区别](https://www.kdnuggets.com/2022/08/difference-training-testing-data-machine-learning.html)

+   [初学者的端到端机器学习指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)
