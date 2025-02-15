# 10 个统计学问题助你在数据科学面试中脱颖而出

> 原文：[`www.kdnuggets.com/10-statistics-questions-to-ace-your-data-science-interview`](https://www.kdnuggets.com/10-statistics-questions-to-ace-your-data-science-interview)

![数据科学统计学面试问题](img/545e41c9df4e9562c1714364ab85cc47.png)

图片来源于作者

我是一名拥有计算机科学背景的数据科学家。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

我熟悉数据结构、面向对象编程和数据库管理，因为在大学里我学了这些概念三年。

然而，当进入数据科学领域时，我注意到一个显著的技能差距。

我没有在几乎所有数据科学职位中所需的数学或统计学背景。

我参加了一些统计学的在线课程，但似乎没有真正掌握。

大多数程序要么非常基础并且针对高级管理人员，要么详细且建立在我没有的前置知识上。

我花时间在互联网上搜索资源，以更好地理解诸如假设检验和置信区间等概念。

在面试多个数据科学职位后，我发现大多数统计学面试问题遵循类似的模式。

在本文中，我将列出我在数据科学面试中遇到的 10 个最流行的统计学问题，并附上这些问题的样本答案。

## 问题 1：什么是 p 值？

答案：在零假设为真的情况下，p 值是你观察到的结果至少极端的结果的概率。

p 值通常用于确定统计测试的结果是否显著。简单来说，p 值告诉我们是否有足够的证据来拒绝零假设。

## 问题 2：解释统计功效的概念

答案：如果你进行统计测试以检测是否存在效果，统计功效是测试准确检测该效果的概率。

这里是一个简单的例子来解释这个问题：

假设我们对 100 人的测试组进行广告投放，并获得了 80 次转化。

零假设是广告对转化次数没有影响。然而，实际上广告对销售量有显著影响。

统计功效是你准确拒绝零假设并实际检测到效应的概率。较高的统计功效表示测试能更好地检测到效应（如果存在的话）。

## 问题 3：你如何向非技术性利益相关者描述置信区间？

我们使用之前相同的示例，其中对 100 人的样本进行广告测试并获得了 80 次转换。

我们提供一个范围而不是说转换率是 80%，因为我们不知道真实的总体行为如何。换句话说，如果我们取无限多个样本，我们会看到多少次转换？

这里是一个仅根据我们从样本中获得的数据可能会说的示例：

“如果我们对一个更大的人群进行这个广告测试，我们有 95%的信心转换率会在 75%到 88%之间。”

我们使用这个范围，因为我们不知道总人口的反应，只能根据我们的测试组生成一个估计，而这只是一个样本。

## 问题 4：参数检验和非参数检验有什么区别？

参数检验假设数据集遵循某种潜在分布。进行参数检验时最常见的假设是数据呈正态分布。

参数检验的示例包括 ANOVA、T 检验、F 检验和卡方检验。

然而，非参数检验不会对数据集的分布做任何假设。如果你的数据集不是正态分布的，或者包含等级或异常值，选择非参数检验是明智的。

## 问题 5：协方差和相关性有什么区别？

协方差测量变量之间线性关系的方向。相关性测量这种关系的强度和方向。

尽管相关性和协方差都能提供特征关系的类似信息，但它们之间的主要区别在于尺度。

相关性范围在-1 和+1 之间。它是标准化的，容易让你理解特征之间是否存在正相关或负相关关系以及这种效应的强度。另一方面，协方差以与因变量和自变量相同的单位显示，这可能使其解释起来稍微困难一些。

## 问题 6：你如何分析和处理数据集中的异常值？

有几种方法可以检测数据集中的异常值。

+   视觉方法：可以通过箱型图和散点图等图表直观地识别异常值。箱型图中位于“胡须”之外的点通常是异常值。使用散点图时，异常值可以被检测为在可视化中远离其他数据点的点。

+   非视觉方法：一种非视觉技术用于检测异常值是 Z 分数。Z 分数通过从均值中减去一个值并除以标准差来计算。这告诉我们一个值距离均值有多少标准差。超过或低于均值 3 个标准差的值被认为是异常值。

## 问题 7：区分单尾检验和双尾检验。

单尾检验检查一个方向上是否存在关系或效果。例如，广告投放后，你可以使用单尾检验来检查是否有积极的影响，即销售的增加。这是一个右尾检验。

双尾检验检查两种方向上的关系可能性。例如，如果所有公立学校实施了一种新的教学风格，双尾检验将评估是否存在分数的显著增加或减少。

## 问题 8：在以下场景中，你会选择实施哪个统计检验？

一家在线零售商希望评估新广告活动的效果。他们收集了广告发布前后 30 天的每日销售数据。公司希望确定广告是否对每日销售产生了显著影响。

选项：

A) 卡方检验

B) 配对 t 检验

C) 单因素方差分析

d) 独立样本 t 检验

**答案**：要评估新广告活动的效果，我们应该使用配对 t 检验。

配对 t 检验用于比较两个样本的均值，并检查差异是否具有统计学意义。

在这种情况下，我们比较的是广告投放前后的销售数据，比较的是同一组数据的变化，因此我们使用配对 t 检验而不是独立样本 t 检验。

## 问题 9：什么是卡方独立性检验？

卡方独立性检验用于检查观察结果和预期结果之间的关系。该检验的零假设（H0）是任何观察到的特征差异纯粹是由于偶然。

简而言之，这种检验可以帮助我们确定两个分类变量之间的关系是否由于偶然，或者是否存在统计学上的显著关联。

例如，如果你想测试性别（男性与女性）和冰淇淋口味偏好（香草与巧克力）之间是否存在关系，可以使用卡方独立性检验。

## 问题 10：解释回归模型中的正则化概念。

正则化是一种通过添加额外信息来减少过拟合的技术，使模型能够更好地适应和概括尚未训练的数据集。

在回归分析中，有两种常用的正则化技术：岭回归和套索回归。

这些模型通过向回归模型添加惩罚项来稍微改变回归模型的误差方程。

对于岭回归而言，惩罚项与系数的平方和相乘。这意味着系数较大的模型受到的惩罚更重。而在套索回归中，惩罚项与系数的绝对值和相乘。

尽管这两种方法的主要目标都是在最小化模型误差的同时缩小系数的大小，但岭回归对大系数的惩罚更重。

另一方面，套索回归对每个系数施加了一个常量惩罚，这意味着在某些情况下，系数可能会收缩到零。

## 10 道统计学问题，助你通过数据科学面试——接下来的步骤

如果你已经跟进到现在，恭喜你！

你现在对数据科学面试中出现的统计问题有了很好的掌握。

作为下一步，我建议你参加一个在线课程来复习这些概念并将其付诸实践。

这里是一些我发现有用的统计学习资源：

+   [StatQuest](https://www.youtube.com/@statquest/videos)

+   [Krish Naik 的 YouTube 频道](https://www.youtube.com/@krishnaik06)

+   [edX 上的统计学习](https://www.edx.org/learn/python/stanford-university-statistical-learning-with-python)

最终课程可以在 edX 上免费审计，而前两个资源是广泛涵盖统计学和机器学习的 YouTube 频道。

&nbsp

&nbsp

[](https://linktr.ee/natasshaselvaraj)**[Natassha Selvaraj](https://linktr.ee/natasshaselvaraj)** 是一位自学成才的数据科学家，对写作充满热情。Natassha 关注所有与数据科学相关的内容，是所有数据主题的真正大师。你可以通过 [LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/) 与她联系，或查看她的 [YouTube 频道](https://www.youtube.com/@natassha_ds)。

### 更多相关话题

+   [通过数据科学面试的 7 张必备备忘单](https://www.kdnuggets.com/top-7-essential-cheat-sheets-to-ace-your-data-science-interview)

+   [你需要的 10 张备忘单来通过数据科学面试](https://www.kdnuggets.com/2022/10/10-cheat-sheets-need-ace-data-science-interview.html)

+   [你需要的 7 张超级备忘单来通过机器学习面试](https://www.kdnuggets.com/2022/12/7-super-cheat-sheets-need-ace-machine-learning-interview.html)

+   [24 道你可能会在下一次面试中遇到的 SQL 问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [KDnuggets 新闻，5 月 4 日：9 门免费的哈佛课程学习数据…](https://www.kdnuggets.com/2022/n18.html)

+   [如何回答数据科学编码面试问题](https://www.kdnuggets.com/2022/01/answer-data-science-coding-interview-questions.html)
