# Andrew Ng 的《机器学习渴望》中的 7 个有用建议

> 原文：[`www.kdnuggets.com/2018/05/7-useful-suggestions-machine-learning-yearning.html`](https://www.kdnuggets.com/2018/05/7-useful-suggestions-machine-learning-yearning.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) comments

![Andrew Ng 机器学习渴望](img/87499a00403f44f5dfa88b36c2de320e.png)

AI、机器学习和深度学习正在迅速发展并改变许多行业。[Andrew Y. Ng](http://www.andrewng.org/) 是这一领域的领先人物之一——他是 Coursera 的共同创始人、前百度 AI 部门负责人以及前 Google Brain 负责人。他正在撰写一本书，[《机器学习渴望》](http://www.mlyearning.org/)（你可以获取免费草稿），教你如何构建机器学习项目。

Andrew 写道

> 本书的重点不是教你机器学习算法，而是如何让机器学习算法发挥作用。一些技术 AI 课程会给你一个锤子；本书则教你如何使用这个锤子。如果你渴望成为 AI 领域的技术领导者，并想学习如何为团队设定方向，这本书将对你有所帮助。

我们已经阅读了草稿，并从书中挑选了 7 个最有趣且实用的建议：

1.  **优化和满足指标**

与其使用单一公式或指标来评估算法，你应该考虑使用多个评估指标。可以通过设置“优化”指标和“满足”指标来实现这一点。

![结合多种评估指标](img/9eed2d9c8143749038f0af01d6f402e5.png)

以上述示例为例，我们可以首先定义一个可接受的运行时间——例如少于 100ms——这可以作为我们的“满足”指标。你的分类器只需满足这一要求，即运行时间低于此值即可，无需更多。准确性则是“优化”指标。这可以是评估算法的一种极其高效且简单的方法。

1.  **快速选择开发/测试集——如有需要，不要害怕更改它们**

在开始新项目时，Ng 解释说，他会尽快选择开发/测试集，从而给团队设定一个明确的目标。设定一个初步的一周目标；不完美的初步结果也比过度考虑这一阶段要好，并且能够迅速启动。

话虽如此，如果你突然意识到最初的开发/测试集不合适，不要害怕进行更改。选择不正确的开发/测试集可能有三种原因：

+   你需要在实际分布上表现良好，而与开发/测试集不同

+   你已经过度拟合了开发/测试集

+   该指标测量的是项目需要优化的内容以外的东西

记住，改变这些并不是大事。只需进行更改，并让你的团队知道你正在前进的新方向。

1.  **机器学习是一个迭代过程：不要指望它第一次就能成功**

Ng 说明了他构建机器学习软件的方法有三方面：

+   从一个想法开始

+   将想法实现为代码

+   进行实验以得出这个想法的效果如何

![机器学习迭代过程](img/1b9a0259f4872fe7372a1f1a21140265.png)

你绕过这个循环的速度越快，进展就会越快。这也是为什么提前选择测试/开发集很重要，因为它可以节省在迭代过程中宝贵的时间。将性能与这个集进行比较可以让你迅速看到是否在朝着正确的方向前进。

1.  **快速构建你的第一个系统，然后进行迭代**

如第 3 点所述，构建机器学习算法是一个迭代过程。Ng 专门 dedicates 一个章节来解释快速构建第一个系统并从中发展起来的好处：“不要一开始就试图设计和构建完美的系统。相反，快速构建和训练一个基础系统——也许只需几天。即使基础系统远非你能构建的‘最佳’系统，也很有价值去检查基础系统的功能：你将迅速找到表明你最有前景的方向的线索，从而投入你的时间。”

1.  **并行评估多个想法**

当你的团队有很多关于如何改进算法的想法时，你可以高效地并行评估这些想法。Ng 以创建一个可以检测猫图片的算法为例，解释了他如何创建一个电子表格并在查看约 100 个错误分类的开发/测试集图像时填充它。

![评估多个想法并行](img/59879418b7320ff6a5e4fc095c294fdb.png)

包括对每张图像的分析，为什么失败以及任何可能有助于未来反思的附加评论。完成后，你可以看到哪个想法能够消除更多的错误，因此应该优先考虑哪个想法。

1.  **考虑是否清理错误标签的开发/测试集是否值得**

在错误分析过程中，你可能会注意到开发/测试集中一些示例被错误标记了。即，图像已经被人为地错误标记。如果你怀疑部分错误是由于此原因导致的，可以在你的电子表格中添加一个额外的类别：

![评估错误标签图像](img/c0d5d99a888bf9ad1a7d0845f234f67e.png)

完成后，你可以考虑是否值得花时间修复这些错误。Ng 给出了两个可能的场景来判断是否值得修复这些错误：

示例 1：

开发集的总体准确率……………….90%（总体错误率 10%）

由于标签错误导致的错误……0.6%（开发集错误的 6%）

由于其他原因导致的错误…………………9.4%（开发集错误的 94%）

“在这里，由于标签错误导致的 0.6%的不准确率可能相对于你可以改进的 9.4%的错误来说并不显著。手动修正开发集中的标签错误没有害处，但并不是关键：可能不需要知道你的系统的总体错误率是 10%还是 9.4%。”

示例 2：

开发集的总体准确率……………….98.0%（总体错误率 2.0%）

错误由于标记错误的示例……. 0.6%。 （开发集错误的 30%）

其他原因导致的错误………………… 1.4%（开发集错误的 70%）

“30% 的错误是由于开发集中标记错误的图像，给你的准确性估计带来了显著的误差。现在提高开发集中标签的质量是值得的。处理这些标记错误的示例将帮助你确定分类器的错误更接近 1.4% 还是 2%——这是一个显著的相对差异。”

1.  **考虑将开发集分成独立的子集**

Ng 解释说，如果你有一个大的开发集，其中 20% 的数据存在错误率，那么将这些数据分成两个独立的子集可能是值得的：

“以算法错误分类 5,000 个开发集示例中的 1,000 个为例。假设我们想手动检查大约 100 个错误以进行错误分析（10% 的错误）。你应该随机选择 10% 的开发集，并将其放入我们称之为 Eyeball 开发集 的集合中，以提醒自己我们是用眼睛查看它的。（对于语音识别项目，可能会称这个集合为 Ear 开发集）。因此，Eyeball 开发集有 500 个示例，我们期望算法错误分类大约 100 个。

开发集的第二个子集，称为 Blackbox 开发集，将包含剩余的 4500 个示例。你可以使用 Blackbox 开发集通过测量其错误率来自动评估分类器。你还可以用它来选择算法或调整超参数。然而，你应该避免用眼睛查看它。我们使用“Blackbox”这个术语，因为我们只会使用这个子集的数据来获得对分类器的“Blackbox”评估。”

**相关：**

+   [不要在 24 小时内学习机器学习](https://www.kdnuggets.com/2018/04/dont-learn-machine-learning-24-hours.html)

+   [机器学习的基本配方](https://www.kdnuggets.com/2018/02/basic-recipe-machine-learning.html)

+   [10 本必读的机器学习和数据科学免费书籍](https://www.kdnuggets.com/2017/04/10-free-must-read-books-machine-learning-data-science.html)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [4 个对数据科学有用的中级 SQL 查询](https://www.kdnuggets.com/2022/12/4-useful-intermediate-sql-queries-data-science.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [KDnuggets 新闻，12 月 7 日：十大数据科学误区破解 • 4…](https://www.kdnuggets.com/2022/n47.html)

+   [5 个真正有用的 Bash 脚本用于数据科学](https://www.kdnuggets.com/2023/02/bash-scripts-data-science.html)

+   [Kaggle 竞赛对现实世界问题有帮助吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)
