# 在 Python 中执行 T 检验

> 原文：[`www.kdnuggets.com/2023/01/performing-ttest-python.html`](https://www.kdnuggets.com/2023/01/performing-ttest-python.html)

![在 Python 中执行 T 检验](img/3ca1e7b8c32364c694374acd4135372a.png)

图片由编辑提供

## 主要要点

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

+   t 检验是一种统计检验方法，用于确定两个独立样本的均值是否存在显著差异。

+   我们通过使用鸢尾花数据集和 Python 的 Scipy 库来说明如何应用 t 检验。

t 检验是一种统计检验方法，用于确定两个独立样本的均值是否存在显著差异。在本教程中，我们演示了 t 检验的最基本版本，我们假设两个样本具有相等的方差。其他高级版本的 t 检验包括 Welch 的 t 检验，它是 t 检验的一种改编，更适用于两个样本方差不等且可能样本量不等的情况。

# t 检验的基本公式

t 统计量或 t 值的计算方法如下：

![在 Python 中执行 T 检验](img/53a3b06be1f03347b4d6b878ce8b63a2.png)

其中 ![公式](img/9251a7b9652929122a830d1907a8035c.png) 是样本 1 的均值，![公式](img/54cd35dc543894b32b5df03500b8fd4d.png) 是样本 2 的均值，![公式](img/fc383e1e73062945c8a8cedd1bfa8e44.png) 是样本 1 的方差，![公式](img/68dad44d35c824dbe0e908f31827721c.png) 是样本 2 的方差，![公式](img/4dc03704aac32bccf9cceffd86a41714.png) 是样本 1 的样本量，和 ![公式](img/19b4eaa769cb990a0a403cb856277b46.png) 是样本 2 的样本量。

# 使用鸢尾花数据集的示例

为了说明 t 检验的使用，我们将使用鸢尾花数据集展示一个简单的例子。假设我们观察到两个独立的样本，例如花萼长度，我们需要考虑这两个样本是否来自同一总体（例如，相同的花卉物种或两个具有相似花萼特征的物种）或两个不同的总体。

t 检验量化了两个样本算术均值之间的差异。p 值量化了在假设原假设（即样本来自均值相同的总体）为真的情况下获得观察结果的概率。p 值大于选择的阈值（例如 5% 或 0.05）表示我们的观察结果不太可能偶然发生。因此，我们接受均值相等的原假设。如果 p 值小于我们的阈值，则我们有证据反对均值相等的原假设。

## T 检验输入

执行 t 检验所需的输入或参数包括：

+   包含样本 1 和样本 2 数据的两个数组 *a* 和 *b*

## T 检验输出

t 检验返回如下结果：

+   计算得到的 t 统计量

+   p 值

# 使用等样本大小的 Python 实现

导入必要的库

```py
import numpy as np
from scipy import stats 
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
```

加载鸢尾花数据集

```py
from sklearn import datasets
iris = datasets.load_iris()
sep_length = iris.data[:,0]
a_1, a_2 = train_test_split(sep_length, test_size=0.4, random_state=0)
b_1, b_2 = train_test_split(sep_length, test_size=0.4, random_state=1)
```

计算样本均值和样本方差

```py
mu1 = np.mean(a_1)

mu2 = np.mean(b_1)

np.std(a_1)

np.std(b_1) 
```

实现 t 检验

```py
stats.ttest_ind(a_1, b_1, equal_var = False)
```

**输出**

```py
Ttest_indResult(statistic=0.830066093774641, pvalue=0.4076270841218671)
```

```py
stats.ttest_ind(b_1, a_1, equal_var=False)
```

**输出**

```py
Ttest_indResult(statistic=-0.830066093774641, pvalue=0.4076270841218671)
```

```py
stats.ttest_ind(a_1, b_1, equal_var=True)
```

**输出**

```py
Ttest_indResult(statistic=0.830066093774641, pvalue=0.4076132965045395)
```

## 观察结果

我们观察到使用“true”或“false”作为“equal-var”参数并不会显著改变 t 检验结果。我们还观察到交换样本数组 a_1 和 b_1 的顺序会得到负的 t 检验值，但不会改变 t 检验值的大小，正如预期的那样。由于计算得到的 p 值远大于阈值 0.05，我们可以拒绝均值相等的原假设。这表明样本 1 和样本 2 的萼片长度来自相同的总体数据。

# 使用不等样本大小的 Python 实现

```py
a_1, a_2 = train_test_split(sep_length, test_size=0.4, random_state=0)
b_1, b_2 = train_test_split(sep_length, test_size=0.5, random_state=1)
```

计算样本均值和样本方差

```py
mu1 = np.mean(a_1)

mu2 = np.mean(b_1)

np.std(a_1)

np.std(b_1) 
```

实现 t 检验

```py
stats.ttest_ind(a_1, b_1, equal_var = False)
```

**输出**

```py
stats.ttest_ind(a_1, b_1, equal_var = False)
```

## 观察结果

我们观察到使用不等大小的样本不会显著改变 t 统计量和 p 值。

![在 Python 中执行 T 检验](img/d2312557b695d3ecd1b4b5a99cbbf967.png)

总结一下，我们展示了如何使用 Python 的 scipy 库实现一个简单的 t 检验。

**[本杰明·O·塔约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，同时也是 DataScienceHub 的所有者。此前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 相关主题

+   [如何通过使用自动 EDA 工具掌握数据科学评估测试](https://www.kdnuggets.com/2022/04/ace-data-science-assessment-test-automatic-eda-tools.html)

+   [超越准确性：使用 NLP 测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [通过《Fast Python for Data Science》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化 Python 代码性能：深入探讨 Python 分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python 枚举：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [通过 Python 和 Scikit-learn 简化决策树解释](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)
