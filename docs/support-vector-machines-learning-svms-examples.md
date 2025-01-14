# 支持向量机（SVM）教程：从示例中学习SVM

> 原文：[https://www.kdnuggets.com/2017/08/support-vector-machines-learning-svms-examples.html](https://www.kdnuggets.com/2017/08/support-vector-machines-learning-svms-examples.html)

**作者：阿比谢克·戈斯，24/7公司**

![SVM header](../Images/f320969486bc138a3dc06807f50d8acb.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT需求

* * *

*在*[*Statsbot*](http://statsbot.co/?utm_source=kdnuggets)*团队发布了关于*[*时间序列异常检测*](https://blog.statsbot.co/time-series-anomaly-detection-algorithms-1cef5519aef2)*的文章后，许多读者要求我们介绍支持向量机的方法。现在是时候跟上步伐，向你介绍SVM，无需复杂数学，并分享一些有用的库和资源，帮助你入门。*

如果你曾经使用机器学习进行分类，你可能听说过*支持向量机（SVM）*。它们在50多年前被引入，随着时间的推移不断发展，并且已被适应到各种其他问题，如*回归、异常检测*和*排序*。

SVM是许多机器学习从业者手中的常用工具。在[[24]7](https://www.247-inc.com/)公司，我们也使用它们来解决各种问题。

在这篇文章中，我们将尝试对支持向量机（SVM）的工作原理进行高层次的理解。我会侧重于培养直觉，而不是严格的数学推导。这意味着我们将尽量跳过数学部分，着重于对工作原理的直观理解。

### **分类问题**

假设你的大学开设了一门机器学习（ML）课程。课程讲师观察到，如果学生在数学或统计方面表现优秀，他们能够从课程中获得最多的收益。随着时间的推移，他们记录了所有注册学生在这些学科上的成绩。此外，他们还为每个学生标注了在ML课程中的表现：“好”或“差”。

现在他们想要确定数学和统计成绩与机器学习课程表现之间的关系。也许，根据他们的发现，他们想要为课程注册指定一个先决条件。

他们会如何进行呢？我们从表示他们拥有的数据开始。我们可以绘制一个二维图，其中一个坐标轴代表数学成绩，另一个坐标轴代表统计成绩。某个学生的成绩会在图上显示为一个点。

点的颜色——绿色或红色——表示他在机器学习课程中的表现：分别为“好”或“差”。

这样的图表可能会是什么样子的：

![](../Images/79c4b84c10d4cb0e6e52f98cd05e0d63.png)

当学生申请入学时，我们的教员会要求她提供她的数学和统计分数。基于他们已经拥有的数据，他们会对她在机器学习课程中的表现做出有根据的猜测。

我们本质上想要某种“算法”，你将“分数元组”输入其中，格式为*(math_score, stats_score)*。它告诉你学生在图中是红点还是绿点（红色/绿色也称为*类别*或*标签*）。当然，这个算法以某种方式体现了我们已经拥有的数据中的模式，也就是*训练数据*。

在这种情况下，找到一条通过红色和绿色簇之间的线，然后确定一个分数元组位于这条线的哪一侧，是一个好的算法。我们将一侧——绿色侧或红色侧——视为她在课程中表现最可能的良好指标。

![](../Images/3e9989de3e3e721fc6d5f35361f05f31.png)

这里的线是我们的*分隔边界*（因为它将标签分开）或*分类器*（我们用它来分类点）。图中显示了我们问题的两种可能分类器。

### **好分类器与差分类器**

这里有一个有趣的问题：上面的两条线都分开了红色和绿色的簇。是否有充分的理由选择其中一条而不是另一条？

记住，分类器的价值不在于它如何分隔训练数据。我们最终希望它对尚未见过的数据点（称为*测试数据*）进行分类。鉴于此，我们希望选择一条能够捕捉*一般模式*的线，以便它在测试数据上表现良好。

上面的第一条线看起来有些“偏斜”。在它的下半部分，它似乎离红色簇太近，而在上半部分，它似乎离绿色簇太近。确实，它能完美分隔训练数据，但如果它遇到一个离簇稍远的测试点，很可能会把标签搞错。

第二条线没有这个问题。例如，看看下面图中的测试点（用方形表示）以及分类器分配的标签。

![](../Images/767fdf264794af85373021d024b0b031.png)

第二条线尽可能远离两个簇，同时正确分隔训练数据。由于它正好位于两个簇之间，它的“风险”较小，可以给每个类别的数据分布留有一些余地，因此在测试数据上表现良好。

支持向量机（SVM）尝试找到第二种类型的线。我们通过视觉选择了更好的分类器，但为了在一般情况下应用它，我们需要更精确地定义其基本哲学。以下是SVM所做的简化版本：

1.  找出能够正确分类训练数据的线

1.  在所有这些直线中，选择与最近点距离最大的那一条。

确定这条直线的最近点称为*支持向量*。它们定义的围绕直线的区域称为*间隔*。

这是显示支持向量的第二条线：有黑边的点（有两个）和间隔（阴影区域）。

![](../Images/cc2ceb6b2e9585689425c23b690358ea.png)

支持向量机为你提供了一种选择众多可能分类器的方式，保证了更高的正确标记测试数据的机会。挺不错，对吧？

尽管上述图表显示的是二维中的一条直线和数据，但必须注意SVM可以在任何维度中工作；在这些维度中，它们找到的是二维直线的类比。

例如，在三维中，它们找到一个*平面*（我们将很快看到这个示例），在更高维度中，它们找到一个*超平面*——二维直线和三维平面的推广到任意维度。

可以通过直线（或一般来说，通过超平面）分隔的数据称为*线性可分*数据。超平面作为一个*线性分类器*。

### **允许错误**

在上一节中，我们讨论了完美线性可分数据的简单情况。然而，现实世界中的数据通常是混乱的。你几乎总会遇到一些线性分类器无法正确分类的实例。

这是一个这样的数据示例：

![](../Images/224a0ce98e517966c72d3e0c79292422.png)

显然，如果我们使用线性分类器，我们永远无法完美地分离标签。我们也不想完全放弃线性分类器，因为除了少数异常点，它确实适合这个问题。

SVM如何处理这些？它们允许你指定你愿意接受多少错误。

你可以向你的SVM提供一个名为“C”的参数；这使你能够决定以下之间的权衡：

1.  保持较大的间隔。

1.  正确分类**训练**数据。较高的C值意味着你希望训练数据上的错误更少。

值得重复强调的是，这是一种**权衡**。你在*牺牲*较大间隔的情况下，能获得更好的训练数据分类。

以下图表显示了当我们增加C值时，分类器和间隔的变化（未显示支持向量）：

![](../Images/2f66ef5206eb12837c8248f957c8518a.png)

注意，当我们增加C值时，直线如何“倾斜”。在高值时，它尝试适应图表右下角大多数红点的标签。这可能不是我们对测试数据的最终要求。C=0.01的第一个图似乎更好地捕捉了总体趋势，尽管它在训练数据上的准确度较低，相比于更高的C值。

> *既然这是一个权衡，请注意随着C值的增加，间隔的宽度是如何缩小的。*

在前面的例子中，边界是一个“无人区”区域。在这里，我们看到不再可能同时拥有*良好的分隔边界*和*无点的边界*。一些点逐渐进入了边界区域。

一个重要的实际问题是决定一个好的C值。由于现实世界的数据几乎从未被干净地分隔开，这个需求经常出现。我们通常使用像*交叉验证*这样的技术来选择一个好的C值。

### 更多相关主题

+   [支持向量机：直观的方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python 向量数据库和向量索引：LLM 应用的架构](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [选择实例以理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [集成学习与实例](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)
��络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

### 相关主题更多

+   [支持向量机：直观方法](https://www.kdnuggets.com/2022/08/support-vector-machines-intuitive-approach.html)

+   [支持向量机的温和介绍](https://www.kdnuggets.com/2023/07/gentle-introduction-support-vector-machines.html)

+   [语义向量搜索如何改变客户支持互动](https://www.kdnuggets.com/how-semantic-vector-search-transforms-customer-support-interactions)

+   [Python向量数据库和向量索引：LLM应用的架构](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [选择示例来理解机器学习模型](https://www.kdnuggets.com/2022/11/picking-examples-understand-machine-learning-model.html)

+   [带实例的集成学习](https://www.kdnuggets.com/2022/10/ensemble-learning-examples.html)
