# 使用 Seaborn 创建美丽的直方图

> 原文：[`www.kdnuggets.com/2023/01/creating-beautiful-histograms-seaborn.html`](https://www.kdnuggets.com/2023/01/creating-beautiful-histograms-seaborn.html)

![使用 Seaborn 创建美丽的直方图](img/cc413cca2ba9e0614555020335f52c68.png)

图片来源：[Pixabay](https://www.pexels.com/photo/laptop-technology-ipad-tablet-35550/)来自 Pexels

可视化是数据世界的重要部分，因为当信息以正确的方式呈现时，人们更容易理解。因此，任何数据人员都应该能够创建信息丰富且吸引人的可视化。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 需求

* * *

直方图是最常见且实用的图表之一。直方图是一个用条形表示数据频率的图表，这些数据被划分为特定的区间。它通常用于可视化数值数据，以理解数据分布并识别趋势。

如何创建一个美丽的直方图图表？让我们学习一下。

# 使用 Seaborn 进行直方图可视化

对于我们的数据集示例，我们将使用 Seaborn 包中的 MPG 开放数据。

```py
import seaborn as sns

mpg = sns.load_dataset('mpg')
mpg.head()
```

![使用 Seaborn 创建美丽的直方图](img/5e34da9d33dfc4547c21463dc8870b9c.png)

从我们的数据集示例中，我们可以快速使用 Seaborn 包开发一个简单的直方图图表。为此，我们需要使用 histplot 函数。

该函数默认以数值数据变量作为参数，输出是传递值的直方图。让我们尝试一下这个函数。

```py
# Create a histogram of the "mpg" variable
sns.histplot(data=mpg, x="mpg")
```

![使用 Seaborn 创建美丽的直方图](img/960308601a3d5e50d1ed2060f9239d8e.png)

从一行代码开始，我们得到了一个漂亮的直方图可视化。“mpg”变量的分布右偏，因为许多值集中在 15 到 25 之间。这是我们可以通过直方图获得的信息。

# 自定义直方图可视化

Seaborn 直方图的默认可视化效果很好，但我们可能希望更改直方图图表，使其更美观。

在这种情况下，可以使用 Seaborn 包的各种自定义选项。

## 基于分类列的多个直方图图示

有时，我们想要比较基于其他变量值的变量数值分布。为此，我们可以将要比较的变量名称传递给 hue 参数。

```py
sns.histplot(data=mpg, x="mpg", hue = 'origin')
```

![使用 Seaborn 创建美丽的直方图](img/d6f04bf8fd7d57ac0722b04f2d866606.png)

## 显示核密度估计 (KDE) 曲线

核密度估计（KDE）是一种使用密度函数估计数据概率的非参数方法。基本上，KDE 平滑直方图以显示分布。要显示 KDE 曲线，我们可以使用以下代码。

```py
sns.histplot(data=mpg, x="mpg",  kde=True)
```

![使用 Seaborn 创建美丽的直方图](img/c92d7954100dd6791e12c509ce27f18b.png)

## 更改箱的数量

直方图的绘制依赖于将变量值分箱的区间数。如果我们想要改变箱的数量，可以通过以下代码传递 `bins` 参数来实现。

```py
sns.histplot(data=mpg, x="mpg",  bins = 5)
```

![使用 Seaborn 创建美丽的直方图](img/87f3cebabae2d6d6e95e32f690fb1af1.png)

也可以通过 `binwidth` 参数根据宽度来改变箱的数量。

```py
sns.histplot(data=mpg, x="mpg",  binwidth=5)
```

![使用 Seaborn 创建美丽的直方图](img/24275d0baab5de60611f52e8da61aa93.png)

此外，还可以使用 `binrange` 参数限制最小和最大箱范围。

```py
sns.histplot(data=mpg, x="mpg",  binrange=(5, 30))
```

![使用 Seaborn 创建美丽的直方图](img/fdb13ae2a3eac955958cc896dc746400.png)

## 更改聚合统计量

默认情况下，Seaborn 假设直方图用于计算每个箱中的值。然而，我们可以改变聚合统计量。在 seaborn 中有几个选项可供选择，包括：

1.  **频率**

显示观察值的数量除以箱宽度。

```py
sns.histplot(data=mpg, x="mpg", stat = 'frequency')
```

![使用 Seaborn 创建美丽的直方图](img/4aec33e9e836dcd04151f135658d4ad8.png)

1.  **概率**

显示标准化值，使条形高度的总和为 1。

```py
sns.histplot(data=mpg, x="mpg", stat = 'probability')
```

![使用 Seaborn 创建美丽的直方图](img/d54bea9d1cc1a79951c6352619145be3.png)

1.  **密度**

显示标准化值，使直方图的总面积为 1。

```py
sns.histplot(data=mpg, x="mpg", stat = 'density')
```

![使用 Seaborn 创建美丽的直方图](img/8d27d1597b174d9648c6a4df3640521b.png)

## 调整直方图美学

可以改变直方图的颜色和透明度。对于单个直方图，我们可以将颜色字符串值传递给 `color` 参数，将透明度值传递给 `alpha` 参数。

```py
sns.histplot(data=mpg, x="mpg",  color = 'red', alpha = 0.5)
```

![使用 Seaborn 创建美丽的直方图](img/23b86572f3decedb4596385941309a75.png)

如果我们有多个直方图，我们可以通过更改 `palette` 参数来改变整体配色方案。要了解 `palette` 参数中可以使用的值，可以查阅 [文档](https://seaborn.pydata.org/tutorial/color_palettes.html)。

```py
sns.histplot(data=mpg, x="mpg", kde = True, palette = "Spectral", hue ='origin')
```

![使用 Seaborn 创建美丽的直方图](img/6adcd1406c469719a5799cfcdb910a3a.png)

# 结论

直方图是可视化数值变量并获取分布趋势信息的一种图表。当我们需要展示数据中发生的情况时，它是一种有用的可视化工具。使用 Seaborn Python 包，我们可以轻松创建美丽的直方图并根据需要调整它们。

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。

### 更多相关内容

+   [通过直方图探索数据分布](https://www.kdnuggets.com/2023/05/exploring-data-distributions-histograms.html)

+   [使用 Matplotlib 和 Seaborn 创建可视化](https://www.kdnuggets.com/creating-visuals-with-matplotlib-and-seaborn)

+   [用 Pandas 制作美丽的交互式可视化的最简单方法](https://www.kdnuggets.com/2021/12/easiest-way-make-beautiful-interactive-visualizations-pandas.html)

+   [逐步指南：使用 Python 和 Beautiful Soup 进行网页抓取](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [使用 Seaborn 进行 Python 数据可视化](https://www.kdnuggets.com/2022/04/data-visualization-python-seaborn.html)

+   [KDnuggets 新闻 22:n16, 4 月 20 日：学习的顶级 YouTube 频道…](https://www.kdnuggets.com/2022/n16.html)
