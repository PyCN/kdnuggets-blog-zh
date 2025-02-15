# 使用 Scikit-Learn 的主成分分析（PCA）

> 原文：[`www.kdnuggets.com/2023/05/principal-component-analysis-pca-scikitlearn.html`](https://www.kdnuggets.com/2023/05/principal-component-analysis-pca-scikitlearn.html)

![主成分分析（PCA）与 Scikit-Learn](img/6ada0542366997aba9e0f4fd17cb4e29.png)

图片作者

如果您对无监督学习范式比较熟悉，您可能会接触到降维及用于降维的算法，例如**主成分分析**（PCA）。机器学习的数据集通常包含大量特征，但如此高维的特征空间并不总是有用的。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

一般来说，并非所有特征都是*同等重要*的，某些特征对数据集的方差占比很大。降维算法的目标是将特征空间的维度减少到原始维度的一个小部分。在这样做的过程中，具有高方差的特征仍然被保留——但位于转换后的特征空间中。而主成分分析（PCA）是最受欢迎的降维算法之一。

在本教程中，我们将学习主成分分析（PCA）的工作原理，并了解如何使用 scikit-learn 库实现它。

# 主成分分析（PCA）是如何工作的？

在我们继续使用 scikit-learn 实现主成分分析（PCA）之前，了解 PCA 的工作原理是有帮助的。

如前所述，主成分分析是一种降维算法。这意味着它减少了特征空间的维度。但它是如何实现这种降维的呢？

该算法背后的动机是存在某些特征能够捕捉到原始数据集中很大一部分的方差。因此，找到数据集中**最大方差的方向**是很重要的。这些方向被称为**主成分**。而 PCA 实质上是将数据集投影到主成分上。

那么我们如何找到主成分呢？

假设数据矩阵 X 的维度为**num_observations x num_features**，我们对 X 的[协方差矩阵](https://en.wikipedia.org/wiki/Covariance_matrix)执行[eigenvalue decomposition](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)。

如果特征均值为零，则协方差矩阵为 X.T X。这里，X.T 是矩阵 X 的转置。如果特征最初不都是零均值，我们可以从每一列的每个条目中减去该列的均值，然后计算协方差矩阵。可以简单地看出，协方差矩阵是一个**num_features**阶的方阵。

![使用 Scikit-Learn 的主成分分析 (PCA)](img/b0e66c9e00d376e00349327e28a9a6da.png)

作者图片

前`k`个主成分是对应于*k 个最大特征值*的*特征向量*。

因此，PCA 的步骤可以总结如下：

![使用 Scikit-Learn 的主成分分析 (PCA)](img/f35220082c4d4abe69a4f216416d5acf.png)

作者图片

由于协方差矩阵是对称的且半正定的，特征分解采用如下形式：

X.T X = D Λ D.T

其中，D 是特征向量矩阵，Λ 是特征值的对角矩阵。

# 使用 SVD 的主成分

另一种可以用来计算主成分的矩阵分解技术是奇异值分解（SVD）。

奇异值分解（SVD）定义适用于所有矩阵。给定一个矩阵 X，X 的 SVD 为：X = U Σ V.T。这里，U、Σ 和 V 分别是左奇异向量、奇异值和右奇异向量的矩阵。V.T. 是 V 的转置。

因此，X 的协方差矩阵的 SVD 表示为：

![使用 Scikit-Learn 的主成分分析 (PCA)](img/7dc90d17b68d09a19669fea36b87f4b8.png)

比较这两种矩阵分解的等效性：

![使用 Scikit-Learn 的主成分分析 (PCA)](img/51544d8bf32a2031dd8b91a03f1625b2.png)

我们有以下内容：

![使用 Scikit-Learn 的主成分分析 (PCA)](img/85586a8362860c0358432f10e3217dba.png)

计算矩阵的 SVD 有计算效率高的算法。scikit-learn 实现的 PCA 在内部也使用 SVD 来计算主成分。

# 使用 Scikit-Learn 执行主成分分析 (PCA)

现在我们已经了解了主成分分析的基本概念，让我们继续进行 Scikit-Learn 的实现。

## 步骤 1 – 加载数据集

为了理解如何实现主成分分析，我们使用一个简单的数据集。在本教程中，我们将使用作为 scikit-learn 的**datasets**模块一部分的 wine 数据集。

我们从加载和预处理数据集开始：

```py
from sklearn import datasets
wine_data = datasets.load_wine(as_frame=True)
df = wine_data.data
```

它有 13 个特征和总共 178 条记录。

```py
print(df.shape)
Output >> (178, 13)
```

```py
print(df.info())
Output >>
 <class>RangeIndex: 178 entries, 0 to 177
Data columns (total 13 columns):
 #   Column                        Non-Null Count  Dtype  
---  ------                        --------------  -----  
 0   alcohol                       178 non-null    float64
 1   malic_acid                    178 non-null    float64
 2   ash                           178 non-null    float64
 3   alcalinity_of_ash             178 non-null    float64
 4   magnesium                     178 non-null    float64
 5   total_phenols                 178 non-null    float64
 6   flavanoids                    178 non-null    float64
 7   nonflavanoid_phenols          178 non-null    float64
 8   proanthocyanins               178 non-null    float64
 9   color_intensity               178 non-null    float64
 10  hue                           178 non-null    float64
 11  od280/od315_of_diluted_wines  178 non-null    float64
 12  proline                       178 non-null    float64
dtypes: float64(13)
memory usage: 18.2 KB
None</class>
```

## 步骤 2 – 预处理数据集

下一步，让我们对数据集进行预处理。特征都在不同的尺度上。为了将它们都转换到一个共同的尺度，我们将使用`StandardScaler`，它将特征转换为均值为零、方差为一的形式：

```py
from sklearn.preprocessing import StandardScaler
std_scaler = StandardScaler()
scaled_df = std_scaler.fit_transform(df)
```

## 步骤 3 – 对预处理的数据集进行 PCA

要找到主成分，我们可以使用来自 scikit-learn 的**decomposition**模块中的 PCA 类。

让我们通过将主成分数量 `n_components` 传递给构造函数来实例化一个 PCA 对象。

主成分的数量是你希望将特征空间减少到的维度数。在这里，我们将主成分数量设置为 3。

```py
from sklearn.decomposition import PCA
pca = PCA(n_components=3)
pca.fit_transform(scaled_df)
```

除了调用 `fit_transform()` 方法，你还可以先调用 `fit()`，然后再调用 `transform()` 方法。

请注意，当我们使用 scikit-learn 实现的 PCA 时，诸如计算协方差矩阵、对协方差矩阵进行特征分解或奇异值分解以获得主成分等步骤都被抽象化了。

## 第 4 步 – 检查 PCA 对象的一些有用属性

我们创建的 PCA 实例 `pca` 具有多个有用的属性，有助于我们理解其背后的情况。

`components_` 属性存储了最大方差的方向（主成分）。

```py
print(pca.components_)
```

```py
Output >>
[[ 0.1443294  -0.24518758 -0.00205106 -0.23932041  0.14199204  0.39466085
   0.4229343  -0.2985331   0.31342949 -0.0886167   0.29671456  0.37616741
   0.28675223]
 [-0.48365155 -0.22493093 -0.31606881  0.0105905  -0.299634   -0.06503951
   0.00335981 -0.02877949 -0.03930172 -0.52999567  0.27923515  0.16449619
  -0.36490283]
 [-0.20738262  0.08901289  0.6262239   0.61208035  0.13075693  0.14617896
   0.1506819   0.17036816  0.14945431 -0.13730621  0.08522192  0.16600459
  -0.12674592]]
```

我们提到主成分是数据集中最大方差的方向。但我们如何衡量在我们选择的主成分数量中*捕获了总方差的多少*呢？

`explained_variance_ratio_` 属性捕获了每个主成分所捕获的总方差比。因此，我们可以将这些比率相加以获得所选数量主成分的总方差。

```py
print(sum(pca.explained_variance_ratio_))
```

```py
Output >> 0.6652996889318527
```

在这里，我们看到三个主成分捕获了数据集总方差的超过 66.5%。

## 第 5 步 – 分析解释方差比的变化

我们可以尝试通过改变主成分数量 `n_components` 来运行主成分分析。

```py
import numpy as np
nums = np.arange(14)
```

```py
var_ratio = []
for num in nums:
  pca = PCA(n_components=num)
  pca.fit(scaled_df)
  var_ratio.append(np.sum(pca.explained_variance_ratio_))
```

为了可视化主成分数量的 `explained_variance_ratio_`，我们可以绘制如下两个量：

```py
import matplotlib.pyplot as plt

plt.figure(figsize=(4,2),dpi=150)
plt.grid()
plt.plot(nums,var_ratio,marker='o')
plt.xlabel('n_components')
plt.ylabel('Explained variance ratio')
plt.title('n_components vs. Explained Variance Ratio')
```

当我们使用所有 13 个主成分时，`explained_variance_ratio_` 为 1.0，表示我们已经捕获了数据集中的 100% 方差。

在这个例子中，我们看到使用 6 个主成分可以捕获输入数据集中超过 80% 的方差。

![使用 Scikit-Learn 进行主成分分析 (PCA)](img/42f9e084065da5c276ff87857d172689.png)

# 结论

希望你已经学会了如何使用 scikit-learn 库中的内置功能执行主成分分析。接下来，你可以尝试在你选择的数据集上实现 PCA。如果你在寻找好的数据集进行实验，可以查看这个数据科学项目数据集网站列表。

# 进一步阅读

[1] [计算线性代数](https://github.com/fastai/numerical-linear-algebra/blob/master/README.md)，fast.ai

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专业领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正致力于通过撰写教程、操作指南、评论文章等，学习并与开发者社区分享她的知识。

### 更多相关主题

+   [数据科学的基础数学：特征值及其在 PCA 中的应用](https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html)

+   [市场数据和新闻：时间序列分析](https://www.kdnuggets.com/2022/06/market-data-news-time-series-analysis.html)

+   [最佳 Python 课程：分析总结](https://www.kdnuggets.com/2022/01/best-python-courses-analysis-summary.html)

+   [机器学习的最佳领域：NLP 和文档分析中的纯方法](https://www.kdnuggets.com/2022/05/machine-learning-sweet-spot-pure-approaches-nlp-document-analysis.html)

+   [无代码时间序列分析使用 KNIME](https://www.kdnuggets.com/2022/10/packt-codeless-time-series-analysis-knime.html)

+   [如何收集客户情感分析的数据](https://www.kdnuggets.com/2022/12/collect-data-customer-sentiment-analysis.html)
