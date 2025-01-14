# 使用 Python 进行数据缩放

> 原文：[`www.kdnuggets.com/2023/07/data-scaling-python.html`](https://www.kdnuggets.com/2023/07/data-scaling-python.html)

![数据缩放与 Python](img/f0302aa41f65da53da03fad65b3b1405.png)

图像由 [Unsplash](https://unsplash.com/photos/98MbUldcDJY) 提供

在机器学习过程中，数据缩放属于数据预处理或特征工程。在用于模型构建之前缩放数据可以实现以下目的：

+   缩放确保特征值在相同范围内

+   缩放确保用于模型构建的特征是无量纲的

+   缩放可用于检测离群值

* * *

## 我们的三大推荐课程

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

数据缩放有多种方法。其中两个最重要的缩放技术是归一化和标准化。

# 使用归一化的数据缩放

当数据使用归一化进行缩放时，转换后的数据可以使用此方程计算

![方程](img/a228570bc55074300b222ac2581b3858.png)

其中 ![方程](img/460e7ddbd8d06b9714acf7c971df15cb.png) 和 ![方程](img/204fd8187a69f9198d100ec96dc558b7.png) 是数据的最大值和最小值。得到的缩放数据范围在 [0, 1] 之间。

## Python 实现归一化

使用归一化进行缩放可以使用下面的代码在 Python 中实现：

```py
from sklearn.preprocessing import Normalizer
norm = Normalizer()
X_norm = norm.fit_transform(data)
```

设 X 为给定数据，具有 ![方程](img/d9f5876351204741a5e6c55c08894aed.png) 和 ![方程](img/271b256b2dc053cd669828ac5d3d92d4.png)。数据 X 如下图所示：

![数据缩放与 Python](img/4085e2934676bfc1f46ae2e6f0e50874.png)

图 1\. 数据 X 的箱线图，值在 17.7 和 71.4 之间。图像由作者提供。

归一化后的 X 如下图所示：

![数据缩放与 Python](img/f6449899c171b4fa2b3a96c9bf95cc9a.png)

图 2\. 归一化后的 X 值在 0 和 1 之间。图像由作者提供。

# 使用标准化的数据缩放

理想情况下，当数据呈正态分布或高斯分布时，应使用标准化。标准化的数据可以按如下方式计算：

![方程](img/e8b2d0e2bee7ffc0938640b3208a676d.png)

在这里，![公式](img/628f5cb6469c6a962d97fb0855c36436.png) 是数据的均值，而 ![公式](img/62bc082726295453c2636d93de924b80.png) 是标准差。标准化值通常应在[-2, 2]范围内，这代表 95%的置信区间。标准化值小于-2 或大于 2 的情况可以被视为异常值。因此，标准化可以用于异常值检测。

## 标准化的 Python 实现

使用标准化进行缩放可以通过以下代码在 Python 中实现：

```py
from sklearn.preprocessing import StandardScaler
stdsc = StandardScaler()
X_std = stdsc.fit_transform(data)
```

使用上述描述的数据，标准化数据如下所示：

![使用 Python 进行数据缩放](img/5a47e485f9dd55d58cac14b95f922ae7.png)

图 3. 标准化 X。作者提供的图片。

标准化均值为零。从上面的图中我们可以观察到，除了少数几个异常值，大多数标准化数据都在[-2, 2]范围内。

# 结论

总结一下，我们讨论了两种最流行的特征缩放方法，即标准化和归一化。归一化数据的范围在[0, 1]，而标准化数据通常在[-2, 2]范围内。标准化的优点是它可以用于异常值检测。

**[本杰明·O·塔约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一名物理学家、数据科学教育者和作家，同时也是 DataScienceHub 的所有者。此前，本杰明曾在中欧大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关主题

+   [通过 Apache Gobblin 扩展数据管理](https://www.kdnuggets.com/2023/01/scaling-data-management-apache-gobblin.html)

+   [你应该知道的扩展 Web 数据驱动产品的事项](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)

+   [通过《数据科学中的快速 Python》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化 Python 代码性能：深入分析 Python 性能分析工具](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python Enum：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [关于掌握 SQL、Python、数据清洗、数据处理等的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)
