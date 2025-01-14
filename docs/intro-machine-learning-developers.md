# 开发人员的机器学习简介

> 原文：[`www.kdnuggets.com/2016/11/intro-machine-learning-developers.html/2`](https://www.kdnuggets.com/2016/11/intro-machine-learning-developers.html/2)

朴素贝叶斯分类是一种基于以前标记的数据使用概率模型进行预测的算法。特征之间相互独立，即一个特征不会影响另一个特征的值，并且一组标签会提前考虑并分配。

在分类器中使用的一些标签示例包括情感分数（可以是字符串、整数或浮点数的缩放分数），或者在目标检测中，你可以使用诸如椅子、桌子或书桌这样的标签来描述图像中的物体。特征检测是在提前决定的，比如垃圾邮件检测中的关键词出现或邮件长度。

![朴素贝叶斯示例代码截图](http://blog.algorithmia.com/wp-content/uploads/2016/10/code_screenshot.png)

这个示例展示了从 NLTK 书籍中修改的代码，章节 [学习文本分类](http://www.nltk.org/book/ch06.html)，并展示了如何用名字的最后一个字母作为特征在已知数据上训练模型的步骤。

![传统验证方法机器学习](http://blog.algorithmia.com/wp-content/uploads/2016/10/conventional_validation.png)

使用具有大数据集的分类模型所需的基本步骤：

+   训练集：根据已知数据拟合模型

+   验证集：用于参数调整——选择模型复杂度

    +   超参数：可以通过设置不同的值并选择测试效果更好的值或通过统计方法来完成

        +   K-means 中的聚类数量：在我们的 K-means 示例中，我们使用了肘部法则。

        +   决策树中的叶子节点数量

+   测试集：在模型运行在训练集上后评估模型——运行混淆矩阵以查找错误并比较模型

![交叉验证机器学习方法](http://blog.algorithmia.com/wp-content/uploads/2016/10/cross_validation.png)

交叉验证方法有助于理解模型如何对未见数据进行泛化，且用于较小的数据集。例如，K 折交叉验证遵循以下步骤：

+   训练数据集被划分为数据子集——一个作为测试集，其余数据集用于训练。——因此你在用于训练数据的每个子集上使用相同的测试集。

+   计算每个测试/训练集的标准差。

+   平均误差率通过多个回合来估计模型性能。

![R 中的机器学习资源](http://blog.algorithmia.com/wp-content/uploads/2016/10/r_resources.png)

[R](https://www.r-project.org/) 非常适合统计/数据分析和机器学习，但由于性能和安全问题，不适合用于生产系统或实用功能。

回归诊断：异常值测试（p 值）、影响观察、评估非线性、相关性、描述性统计。

所有的统计内容：ANOVA、重采样技术、聚类、用于无监督机器学习的 PCA、决策树等。

![用于机器学习的 Pandas 资源](http://blog.algorithmia.com/wp-content/uploads/2016/10/pandas_resources.png)

[Pandas](http://pandas.pydata.org/) 是一个使用数据框的 Python 库，类似于 R。虽然在生产中使用时较慢（[Numpy](http://www.numpy.org/) 数组更快），但 Pandas 在 Python 环境中进行数据分析和机器学习时非常受欢迎。

使用 Pandas 的好处在于它可以将你的代码减少至少三分之二，并且你可以使用非常酷的 SQL 风格的功能，如连接、合并、透视和聚合函数。

还提供了许多 I/O 方法，使得数据的输入和导出变得简单，如：DataFrame.to_excel、.to_json、.to_csv 等。

![Scikit-learn 机器学习资源](http://blog.algorithmia.com/wp-content/uploads/2016/10/sklearn.png)

[Scikit-learn](http://scikit-learn.org/stable/) 是另一个受欢迎的 Python 库，是寻找机器学习模型的绝佳选择，提供了经过众多 Python 开发者验证的教程和文档。它涵盖了从图像分类算法到自然语言处理算法的各种内容。

![机器学习资源](http://blog.algorithmia.com/wp-content/uploads/2016/10/other-ml-resources.png)

以下是上面幻灯片中列出的工具、教程和视频的可点击链接：

+   [R Data Camp 教程](https://www.datacamp.com/community/tutorials/machine-learning-in-r#gs.JZgwzZg)

+   [R-bloggers 机器学习视频](https://www.r-bloggers.com/in-depth-introduction-to-machine-learning-in-15-hours-of-expert-videos/)

+   R KDnuggets 机器学习包

+   Python KDnuggets 机器学习教程

+   [Python 自然语言工具包在线书籍](http://www.nltk.org/book)

查看博客的其他部分，获取有关[natural language processing](http://blog.algorithmia.com/introduction-natural-language-processing-nlp/)和机器学习算法的更多资源，例如[LDA 文本分类](http://blog.algorithmia.com/lda-algorithm-classify-text-documents/)或提高[Nudity Detection](http://blog.algorithmia.com/improving-nudity-detection-nsfw-image-recognition/)算法准确度，以及使用[Scikit-learn 解决 FizzBuzz](http://blog.algorithmia.com/solve-fizzbuzz-machine-learning-scikit-learn/)的入门教程。

**简介：[Stephanie Kim](https://www.linkedin.com/in/stephanielkim)** 是 Algorithmia 的开发者推广专员。

[原始文章](http://blog.algorithmia.com/introduction-machine-learning-developers/)。经许可转载。

**相关：**

+   掌握 Python 机器学习的 7 个步骤

+   机器学习关键术语解释

+   理解深度学习的 7 个步骤

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

### 更多相关话题

+   [为什么越来越多的开发者使用 Python 来进行机器学习项目？](https://www.kdnuggets.com/2022/01/developers-python-machine-learning-projects.html)

+   [低代码：开发者仍然需要吗？](https://www.kdnuggets.com/2022/04/low-code-developers-still-needed.html)

+   [KDnuggets 新闻，4 月 27 日：简要介绍 Papers With Code；…](https://www.kdnuggets.com/2022/n17.html)

+   [基础回顾第 3 周：机器学习简介](https://www.kdnuggets.com/back-to-basics-week-3-introduction-to-machine-learning)

+   [统计学习简介，Python 版：免费书籍](https://www.kdnuggets.com/2023/07/introduction-statistical-learning-python-edition-free-book.html)

+   [深度学习库简介：PyTorch 和 Lightning AI](https://www.kdnuggets.com/introduction-to-deep-learning-libraries-pytorch-and-lightning-ai)
