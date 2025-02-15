# 随机森林算法是否需要归一化？

> 原文：[`www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html`](https://www.kdnuggets.com/2022/07/random-forest-algorithm-need-normalization.html)

![随机森林算法是否需要归一化？](img/01e6640db11dfd6c875a881e214a27bf.png)

[Irina Iriser](https://unsplash.com/@iriser) 通过 Unsplash

我要从一些定义开始，以更好地理解这篇博客。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

随机森林是一个基于树的算法，由多个决策树组成，以改善决策。它被称为随机森林，因为它是一个使用袋装和特征随机性的树的森林。这些多棵树结合在一起进行预测。

归一化是一种在机器学习的数据准备阶段进行的技术。它是通过创建新值并展示数据，使其具有一般分布的过程。这是一种缩放技术，其中值被重新缩放到 0 和 1 之间——这也被称为 Min-Max 缩放。这是为了减少或完全去除冗余数据和数据错误。

# 我如何归一化我的数据？

你可以使用 sklearn 库通过导入 MinMaxScaler 来归一化你的数据

```py
# data normalization with sklearn

from sklearn.preprocessing import MinMaxScaler

# fit scaler on your training data

norm = MinMaxScaler().fit(X_train)

# transform your training data

X_train_norm = norm.transform(X_train)

# transform testing database

X_test_norm = norm.transform(X_test)

```

如果你在谷歌上搜索标题中的问题，大多数人会说‘不需要’。原因如下...

# 为什么随机森林算法不需要归一化数据？

通过归一化来缩放数据的过程是为了确保某个特征不会优先于另一个特征。这种技术在基于距离的算法中尤其重要，例如 K 最近邻和 K-means，因为它们需要欧几里得距离。

然而，随机森林算法不是基于距离的模型——它是基于树的模型。随机森林中的每个节点不是比较特征值，而是简单地拆分一个需要绝对值的排序列表以进行分支。该算法基于数据分区来进行预测，因此不需要归一化。

例如，决策树在特征上拆分节点，而该特征不会被另一个特征影响，也不会影响另一个特征。这意味着所有剩余特征对拆分没有影响——所以我们可以说基于树的算法对特征的缩放不敏感。

# 基尼指数

随机森林算法通过基尼指数进一步理解特征的重要性，而不是缩放特征。基尼不纯度也被称为基尼 impurity，决定了决策树、随机森林或其他基于树的模型的分裂效果。

**公式：**

![基尼指数](img/3bf349e90272695b3ad6a6e91126cee8.png)

它计算了一个特定特征在随机选择时被错误分类的概率。基尼 impurity 的范围在 0 到 0.5 之间，其中最小值 0 是最佳值（分类是纯的），0.5 是我们能得到的最差值。

# 熵

熵是数据点中不纯或随机性的度量。在使用机器学习算法时，你的主要目标应是减少不确定性和随机性。

![熵](img/48512193a267aadafb0cd2ac52a32033.png)

熵的范围从 0 到 1，其中最小值 0 是最佳值（纯度），1 是最差值（高水平的不纯）。

# 结论

当你的数据需要被缩放，且你选择的机器学习算法无法对数据的分布做出假设时，归一化是一个很好的技术。

基于树的模型并不依赖于特征间的距离关系。基尼指数和熵都用于计算信息增益，因此不需要归一化。

**[Nisha Arya](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由职业技术作家。她特别关注提供数据科学职业建议或教程及理论知识，同时希望探索人工智能如何有利于人类寿命的延续。作为一个热衷学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关话题

+   [随机森林与决策树：关键区别](https://www.kdnuggets.com/2022/02/random-forest-decision-tree-key-differences.html)

+   [调整随机森林超参数](https://www.kdnuggets.com/2022/08/tuning-random-forest-hyperparameters.html)

+   [K-Means 聚类是什么，它的算法如何工作？](https://www.kdnuggets.com/2023/05/kmeans-clustering-algorithm-work.html)

+   [数据转换：标准化与归一化](https://www.kdnuggets.com/2020/04/data-transformation-standardization-normalization.html)

+   [优化数据存储：SQL 中的数据类型和归一化探索](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [朴素贝叶斯算法：你需要知道的一切](https://www.kdnuggets.com/2020/06/naive-bayes-algorithm-everything.html)
