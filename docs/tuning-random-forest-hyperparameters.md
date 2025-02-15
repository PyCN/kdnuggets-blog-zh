# 调整随机森林超参数

> 原文：[`www.kdnuggets.com/2022/08/tuning-random-forest-hyperparameters.html`](https://www.kdnuggets.com/2022/08/tuning-random-forest-hyperparameters.html)

![调整随机森林超参数](img/3414b807dafcc166035b47ce74dfcfd6.png)

丛林矢量由 [freepik](https://www.freepik.com/vectors/jungle) 创建

如果你还不知道，让我们快速了解一下随机森林。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

随机森林是一种用于分类、回归和其他任务的集成学习方法，其中包含多个决策树。

**集成学习**最简单的解释就是将多个分类器堆叠在一起以提高性能。**决策树**是一种非参数监督学习方法，其最终目标是通过学习基于数据特征推断和创建的规则来建立一个预测目标变量值的模型。

```py
sklearn.ensemble.RandomForestClassifier
```

**随机森林**由许多决策树组成。树木的多样性构建了森林，我想这就是它被称为随机森林的原因。

Bagging 是创建随机森林“森林”的方法。其目的是减少过拟合训练数据的模型的复杂性。Boosting 是 Bagging 的对立面，旨在增加高偏差模型的复杂性，解决欠拟合问题。

随机森林的结果基于由决策树生成的预测，这些预测通过取各种决策树输出的平均值或均值来完成。如果树木数量增加，结果的精度就会提高——因此准确性更高，过拟合得到减少。

# 超参数调整的重要性

超参数调整对算法至关重要。它提高了机器学习模型的整体性能，并且在学习过程之前设定，并发生在模型之外。如果不进行超参数调整，模型将产生错误和不准确的结果，因为损失函数没有被最小化。

超参数调整是关于找到一组最佳的超参数值，这些值可以最大化模型的性能，最小化损失，并产生更好的输出。

# 随机森林的超参数

下面是最重要参数的列表，以及如何提高预测能力和简化模型训练阶段的更详细部分。

**max_depth:** 树的最大深度 - 意味着根节点与叶节点之间的最长路径。

**min_sample_split:** 分裂内部节点所需的最小样本数，其中默认值 = 2

**max_leaf_nodes:** 这是决策树可以拥有的最大叶节点数。

**min_samples_leaf:** 这是在叶节点所需的最小样本数，其中默认值 = 1

**n_estimators:** 这是森林中的树木数量。

**max_sample:** 这决定了分配给任何单棵树的原始数据集的比例。

**max_features:** 这是在寻找最佳分裂时要考虑的特征数量。

**bootstrap:** 如果设置为 False，则使用整个数据集来构建每棵树，但默认值为 True。

**criterion:** 用于衡量分裂质量的函数

## 超参数调整以提高预测能力

**n_estimators:** ***int，默认值=100***

这是森林中的树木数量。如前所述，随着树木数量的增加，结果的精度会提高，从而提高准确性，并减少过拟合。然而，这会使模型变得更慢，因此选择一个处理器可以处理的 n_estimator 值可以使模型更加稳定，并表现良好。

**max_features**{“sqrt”, “log2”, None}, int 或 float，默认值=”sqrt”}

随机森林模型在单个树中只能有最大特征数。许多人会认为如果增加 max_features，会提高模型的整体性能。然而，这自然会减少单个树的多样性，同时增加模型产生输出的时间。因此，找到一个最佳的 max_features 对于模型的性能至关重要。

**min_samples_leaf:** ***int 或 float，默认值=1***

这是在叶节点所需的最小样本数。叶节点是决策树的末端节点，较小的 min_sample_leaf 值会使模型更容易检测噪声。再次强调，超参数调整是关于寻找最佳值的，因此建议尝试不同的叶节点大小。

## 超参数调整以改善模型训练阶段

**random_state:** ***int，RandomState 实例或 None，默认值=None***

这个参数控制了构建树时样本的自助抽样随机性以及在每个节点寻找最佳分裂时考虑的特征采样。

**n_jobs:** ***int，默认值=None***

这个参数指的是并行运行的作业数量，它基本上告诉引擎可以使用多少个处理器。-1 表示没有限制，1 表示只能使用 1 个处理器。

# 结论

本文为您详细介绍了什么是随机森林、超参数调整的重要性、最重要的参数以及如何提高预测能力和模型训练阶段。

如果你想了解更多关于这些参数的信息，请点击这个[链接](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)**是一位数据科学家和自由职业技术作家。她特别关注提供数据科学职业建议或教程，并且围绕数据科学提供理论知识。她还希望探索人工智能在促进人类寿命方面的不同方式。作为一个热衷学习者，她致力于拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [随机森林与决策树：关键区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [随机森林算法是否需要归一化？](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

+   [使用网格搜索和随机搜索进行超参数调整（Python 版）](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [调整 XGBoost 超参数](https://www.kdnuggets.com/2022/08/tuning-xgboost-hyperparameters.html)

+   [神经网络中的超参数调整](https://www.kdnuggets.com/tuning-hyperparameters-in-neural-networks)

+   [决策树与随机森林的比较说明](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)
