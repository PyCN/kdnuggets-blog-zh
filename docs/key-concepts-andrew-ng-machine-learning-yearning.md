# Andrew Ng《机器学习的渴望》的 6 个关键概念

> 原文：[`www.kdnuggets.com/2019/08/key-concepts-andrew-ng-machine-learning-yearning.html`](https://www.kdnuggets.com/2019/08/key-concepts-andrew-ng-machine-learning-yearning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由 [Niklas Donges](https://machinelearning-blog.com/about/)，SAP。**

[*机器学习的渴望*](https://www.mlyearning.org/) 主要讲述了如何构建机器学习项目的开发。此书包含了难以在其他地方找到的实用见解，以易于与团队成员和合作伙伴共享的格式呈现。大多数技术 AI 课程会向你解释不同的 ML 算法在幕后如何工作，但这本书教你如何实际使用它们。如果你希望成为 AI 领域的技术领袖，这本书将助你一臂之力。历史上，学习如何对 AI 项目做出战略决策的唯一途径是参加研究生项目或在公司工作积累经验。*机器学习的渴望* 帮助你迅速获得这一技能，使你在构建复杂的 AI 系统方面变得更出色。

### 目录

+   关于作者

+   介绍

+   概念 1：反复迭代……

+   概念 2：使用单一评估指标

+   概念 3：错误分析至关重要

+   概念 4：定义一个最佳错误率

+   概念 5：解决人类能够做好的问题

+   概念 6：如何拆分你的数据集

+   摘要

![](img/fbaf83bb2139d386f11de96a20bd3971.png)Andrew NG: *由 NVIDIA 公司拍摄，采用“CC BY-NC-ND 2.0”许可证。未做任何修改。* ([来源](https://www.flickr.com/photos/nvidia/16841620756))

### **关于作者**

Andrew NG 是一位计算机科学家、执行官、投资者、企业家，以及人工智能领域的领先专家之一。他曾是百度的副总裁兼首席科学家，斯坦福大学的兼职教授，也是最受欢迎的机器学习在线课程的创建者、Coursera.com 的联合创始人，以及前谷歌大脑负责人。在百度，他显著参与了将 AI 团队扩展到数千人的工作。

### **介绍**

这本书开始于一个小故事。假设你想为公司建立一个领先的猫检测系统。你已经建立了一个原型，但不幸的是，系统的性能并不理想。你的团队提出了几种改进系统的想法，但你对应该走哪个方向感到困惑。你可以建立世界领先的猫检测平台，也可以浪费数月时间走错方向。

这本书旨在告诉你在这种情况下如何决策和优先排序。根据 Andrew NG 的说法，大多数机器学习问题会提供关于最有前景的下一步以及应该避免做什么的线索。他进一步解释了学习如何“读取”这些线索在我们的领域中是一项关键技能。

**简而言之，《机器学习渴望》旨在帮助你深入理解如何设定机器学习项目的技术方向。**

由于你的团队成员在你提出新想法时可能会持怀疑态度，他将章节写得非常简短（1-2 页），以便你的团队成员可以在几分钟内阅读并理解概念背后的思想。如果你对阅读这本书感兴趣，请注意，它不适合完全的初学者，因为它需要对监督学习和深度学习有基本的了解。

在这篇文章中，我将用自己的语言分享书中的六个概念。

### **概念 1：迭代、迭代，再迭代……**

NG 在整本书中强调，快速迭代是至关重要的，因为机器学习是一个迭代过程。与其考虑如何为你的问题构建完美的 ML 系统，你应该尽快构建一个简单的原型。如果你在该领域不是专家，尤其如此，因为很难准确预测最有前景的方向。

你应该在几天内构建第一个原型，然后会有线索浮现，显示出改进原型性能的最有前景方向。在下一个迭代中，你将基于其中一个线索改进系统，并构建下一个版本。你会一遍遍地这样做。

他继续解释说，你迭代的速度越快，你的进展就会越大。书中的其他概念也基于这一原则。注意，这本书是针对那些只想构建基于 AI 的应用程序而不是进行领域研究的人。

### **概念 2：使用单一评估指标**

![](img/1c3d66f6e1aaaf3e9e4369f28da2f25c.png)([图片来源](https://pixabay.com/de/antrieb-auto-verkehr-stra%C3%9Fe-44276/))

这个概念建立在前一个概念之上，关于为何应该选择单一的评估指标，其解释非常简单：它使你能够快速评估你的算法，从而能够更快地进行迭代。使用多个评估指标只会使得算法比较变得更加困难。

想象一下你有两个算法。第一个算法的精度为 94%，召回率为 89%。第二个算法的精度为 88%，召回率为 95%。

在这里，如果没有选择单一的评估指标，没有分类器明显优越，因此你可能需要花费一些时间来弄清楚。问题是，每次迭代时，你在此任务上会浪费大量时间，并且这些时间会随着时间的推移而增加。你将尝试许多关于架构、参数、特征等的想法。如果你使用单一数字评估指标（例如精度或 F1 分数），它使你能够根据性能对所有模型进行排序，并快速决定哪个效果最好。另一种改进评估过程的方法是将多个指标合并成一个，例如通过对多个错误指标取平均值。

然而，会有一些机器学习问题需要满足多个指标，例如考虑运行时间。NG 解释说，你应该定义一个“可接受”的运行时间，这样你可以迅速筛选出过慢的算法，并根据单一数字评估指标对令人满意的算法进行比较。

**简而言之，单一数字评估指标使你能够快速评估算法，从而加快迭代速度。**

### **概念 3：错误分析至关重要**

![](img/e5f584d8a83084b956dd5cc109a2e162.png)([图像来源](https://pixabay.com/de/fehler-mathematik-1966460/))

错误分析是查看算法输出不正确的示例的过程。例如，假设你的猫探测器把鸟误认为猫，而你已经有了几个解决该问题的想法。

通过适当的错误分析，你可以估计一个改进想法实际上会提高系统性能的程度，而无需花费数月时间实现该想法后发现它对系统不重要。这使你能够决定哪个想法最值得投入资源。如果你发现只有 9%的错误分类图像是鸟类，那么无论你如何提高算法在鸟类图像上的性能，都无济于事，因为它不会改善超过 9%的错误。

此外，它使你能够快速并行判断多个改进想法。你只需创建一个电子表格，并在检查例如 100 个错误分类的开发集图像时填写它。在电子表格中，你为每个错误分类的图像创建一行，为每个改进想法创建一列。然后，你逐一查看每个错误分类的图像，并标记图像将被正确分类的想法。

之后，你会准确知道，例如，使用想法-1 系统将 40%的错误分类图像正确分类，想法-2 是 12%，而想法-3 只有 9%。然后你会知道，处理想法-1 是团队应集中精力的最有希望的改进方向。

此外，一旦你开始查看这些示例，你可能会发现新的改进算法的想法。

### **概念 4：定义最佳错误率**

最佳错误率有助于指导你的下一步。在统计学中，它也常被称为贝叶斯误差率。

想象一下你正在构建一个语音转文本系统，并且你发现预计用户提交的 19% 的音频文件有如此强烈的背景噪音，以至于即使是人类也无法识别里面说了什么。如果是这种情况，你知道即使是最好的系统也可能有大约 19% 的错误率。相比之下，如果你处理的是一个几乎 0% 错误率的最佳问题，你可以期望你的系统也会表现得同样好。

这也帮助你检测你的算法是否存在高偏差或方差，从而帮助你定义下一步来改进你的算法。

但是我们如何知道最佳错误率是什么呢？对于人类擅长的任务，你可以将你的系统性能与人类的表现进行比较，这可以给你一个最佳错误率的估计。在其他情况下，往往很难定义最佳率，这也是为什么你应该处理人类能够做得好的问题，我们将在下一个概念中讨论。

### **概念 5：处理人类能够做得好的问题**

在整本书中，他多次解释了为什么建议处理人类能够很好地完成的机器学习问题。例如，语音识别、图像分类、目标检测等。这有几个原因。

首先，更容易获取或创建标记数据集，因为如果人们能够自行解决问题，他们可以为你的学习算法提供高准确率的标签。

其次，你可以使用人类的表现作为你希望算法达到的最佳错误率。NG 解释说，定义一个合理且可实现的最佳错误率有助于加快团队的进展。它也帮助你检测你的算法是否存在高偏差或方差。

第三，这使你可以基于人类的直觉进行错误分析。例如，如果你正在构建一个语音识别系统，而你的模型错误地分类了输入，你可以尝试理解人类会使用什么信息来获得正确的转录，并利用这些信息来相应地修改学习算法。尽管算法在越来越多的人类无法做到的任务上超越了人类，但你应尽量避免这些问题。

总结来说，你应该避免这些任务，因为它们使得获得数据标签变得更困难，你无法再依赖人类直觉，而且很难知道最佳错误率是多少。

### **概念 6：如何拆分数据集**

NG 还提出了一种拆分数据集的方法。他建议如下：

**训练集：** 使用它来训练你的算法，别无其他。

**开发集：**这个集合用于进行超参数调优、选择和创建适当的特征，以及进行错误分析。它基本上是用来做出关于你的算法的决策。

**测试集：**测试集用于评估你的系统性能，但不用于做决策。它只是用于评估，没有其他功能。

开发集和测试集使你的团队能够快速评估算法的表现。它们的目的是引导你做出对系统最重要的修改。

**他建议选择开发集和测试集，以便它们能反映出你希望在系统部署后未来表现良好的数据。** 如果你预计数据会与当前训练的数据不同，这一点尤其重要。例如，你在使用普通相机图像进行训练，但系统未来只会接收手机拍摄的照片，因为这是移动应用的一部分。如果你没有足够的手机照片来训练系统，这种情况可能会出现。因此，你应该选择那些能够真实反映你未来希望表现良好的测试集样本，而不是用于训练的数据。

**此外，你应该选择来自相同分布的开发集和测试集。** 否则，你的团队可能会构建一个在开发集上表现良好的系统，却发现它在测试数据上表现极差，而这些测试数据才是你最关心的。

### 总结

在这篇文章中，你了解了《*机器学习年鉴*》的 6 个概念。你现在知道为什么快速迭代很重要，为什么应该使用单一数字的评估指标，错误分析的内容以及它的重要性。同时，你了解了最佳错误率，为什么应该处理人类能够做得好的问题，以及如何拆分数据。此外，你还了解了应该选择反映未来希望表现良好的数据的开发集和测试集，以及这些集合应来自相同的分布。我希望这篇文章给你对书中的一些概念做了介绍，我可以肯定地说，它值得阅读。

**个人简介：[Niklas Donges](https://machinelearning-blog.com/about/)** 是一位机器学习和数据科学爱好者，在柏林 CODE 应用科学大学学习软件工程，深度关注机器学习，并兼职为 SAP 的机器学习基金会工作。

[原文](https://machinelearning-blog.com/2019/02/18/6-concepts-of-andrew-ngs-book-machine-learning-yearning/)。经许可转载。

**相关：**

+   [Andrew Ng《机器学习年鉴》的 7 个实用建议](https://www.kdnuggets.com/2018/05/7-useful-suggestions-machine-learning-yearning.html)

+   [机器学习基础配方](https://www.kdnuggets.com/2018/02/basic-recipe-machine-learning.html)

+   [伟大的数据科学家不仅是跳出框框思考，他们还重新定义了框框](https://www.kdnuggets.com/2018/03/great-data-scientists-think-outside-redefine-box.html)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并找到目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的 AI 失败，解析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学的统计学顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的五个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [为什么 Python 是初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
