# 数据科学被高估了，这里是原因

> 原文：[`www.kdnuggets.com/2022/06/data-science-overrated.html`](https://www.kdnuggets.com/2022/06/data-science-overrated.html)

![数据科学被高估了，这里是原因](img/a4bd05a80db5b0d5276d34630b7462f7.png)

图片来自 [freepik](https://www.freepik.com/free-photo/skilled-satisfied-freelance-student-watches-streaming-business-news-sits-good-mood-coworking-space-with-opened-laptop-computer-thinks-ideas-new-science-project_12930957.htm#query=thinking%20science&position=11&from_view=search)

人们已经为数据科学大肆宣扬了大约 10 年，自从《哈佛商业评论》称其为“21 世纪最性感的职业”以来。我自己也被这种炒作所吸引，这也是我 4 年前攻读该学科学位的原因。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

随着时间的推移，我意识到数据科学被过度炒作了。这一领域被过度美化，以至于那些真正不了解其内容的人也在谈论这个词汇。

你一定听过“*85%的生产化机器学习模型将失败*”这一说法。这是研究公司 [Gartner](https://www.iiot-world.com/industrial-iot/connected-industry/why-85-of-machine-learning-projects-fail/) 的预测，老实说，如果这个数字更高我也不会感到惊讶。就我个人而言，我过去曾参与过多个失败的数据科学项目。

失败的数据科学投资可能会给组织带来巨大的费用，特别是如果基于预测算法的结果做出错误的业务决策。

在这篇文章中，我将突显出围绕数据科学的炒作所引发的一些主要问题以及如何克服这些问题。

# 人们往往将机器学习奉为神明

机器学习并不是解决所有问题的万能方案，这一点对利益相关者和非技术管理者至关重要。并非所有问题都可以用机器学习解决，**也并非所有问题都应该用机器学习解决**。

我曾与一位高级教授（他并不专攻数据科学）合作完成我的顶点项目。我们的学院与一家希望实施网络安全解决方案以过滤恶意流量的公司合作。

数据以日志文件的形式提供，而他们没有给我们足够的数据。

我们清理了数据并尝试构建一个仪表盘来分析数据点。我们的教授坚持让我们使用机器学习模型。他说这样会给客户留下深刻印象。

我们说这行不通。数据不够。

他没有听。

我们要求更多的数据。

他说这不可能，我们必须使用现有的资源。

我们构建了模型并进行了展示，他对演示过程中看到的高准确率很满意。数据集不平衡，所以我们的模型只预测了一个类别。他不知道这一点。我们尝试解释，但似乎对他没有影响。

任何在数据行业工作超过一个月的人可能都经历过类似的情况，尽管可能没有那么极端。

高层管理、利益相关者和业务团队通常来自非技术背景。听到“*数据科学*”和“*机器学习*”等术语时，他们的期望往往会升高。数据科学家需要让他们回到现实，清楚地解释什么是可能的，什么是不可能的。

# 在不需要时使用机器学习

我明白了。即使在真正不需要的情况下，建议构建机器学习模型也是很诱人的。我也会这样做。许多人，尤其是非技术人员，常常将数据科学等同于机器学习。

然而，作为数据科学家，直到你确定机器学习建模是最佳选项之前，不要开始构建预测算法。

构建机器学习模型，特别是在大量数据上，是计算密集型的，可能会给组织带来不必要的开支。

如果问题可以通过硬编码逻辑或在电子表格中进行简单计算来解决，为什么要浪费时间构建机器学习模型呢？

这是一个 [例子](https://sloanreview.mit.edu/article/why-so-many-data-science-projects-fail-to-deliver/) ，展示了数据科学家用机器学习解决非机器学习问题的情况：

Hiren 最近被聘为印度顶级银行之一的数据科学家。他构建了一个 KNN 算法，以识别银行应该关注的最盈利的行业细分。他强调，他们应该关注他们投资组合中的 33 个行业中的 2 个。

结果让业务团队成员失望，因为他们已经知道了这些。他们能够通过简单的草率计算得出相同的结论。

对他来说，花时间构建一个提供完全相同结果的机器学习模型是不必要的。

Hiren 随后决定深入挖掘并了解更多业务需求，以便他可以更好地利用自己的数据科学专业知识为团队做出贡献。在掌握了一些领域知识后，他意识到公司将从客户级别的推荐中受益，而不是行业级别。

他不仅仅告诉公司应该针对哪些行业，还能够识别这些行业中最有利可图的客户。这些是业务团队尚未掌握的洞察，因为从大量客户数据集中提取意义对他们来说要复杂得多。

上述故事在尝试构建数据科学解决方案时教会了我们两个关键的教训：

+   如果有更简单的解决方案，请不要使用机器学习。

+   只有在理解业务需求后才开始构建预测模型。否则，你将花费时间在一个没有人使用的华丽算法上。

# 招聘没有数据的 数据科学家

当数据科学在 2012 年急剧流行时，公司开始大规模招聘数据科学家。他们中的大多数人没有建立数据管道，但期望数据科学家能够进来并开始创造价值。

不出所料，这并没有发生。

大多数数据科学家[不知道如何构建数据管道](https://towardsdatascience.com/data-science-without-any-data-6c1ae9509d92)。他们使用的是可以从现有预处理数据库中轻松提取的数据。

这导致了双方的许多沮丧。

数据科学家准备好开始构建机器学习模型，但无法做到这一点。公司在数据科学团队上投入了大量资金，却没有从中获得任何业务收益。

仅仅因为炒作而决定进入数据科学领域，这些组织浪费了大量的时间和金钱。

# 那么……数据科学的职业仍然值得追求吗？

以上问题都源于对数据科学的过度炒作。学生往往过于急于进入这个领域，因为他们想学习一个高度需求的技能。雇主开始大规模招聘数据科学家，但没有完全理解这个角色。

这些不匹配的期望导致双方都感到非常沮丧。

数据科学家确实倾向于过分强调预测建模，而在大多数情况下，简单的数据分析就足够了。

当机器学习算法未能达到雇主的期望时，这会导致非常不愉快的工作情况。

然而，如果各方的期望达成一致，**追求数据科学职业仍然可以是极其值得和令人满足的**。如果你拥有数据科学技能，以下是一些找到合适公司的建议：

+   确保公司有一个数据团队。这意味着组织的数据素养很高，你被要求达成奇怪期望的机会更小。

+   采访你的雇主。询问你的招聘经理关于这个角色的真正职责；公司拥有多少数据，目前数据是如何存储的，以及你需要带来什么价值。

> **"**请记住，上述建议主要适用于初级数据科学家。高级数据科学家、顾问或经理可能会被聘请来帮助实施公司的数据科学流程，即使在没有结构化数据管道的阶段也是如此。**"**

一旦你在一家你感到舒适的公司找到工作，务必时刻质疑机器学习是否真的是正确的方法。在提出解决方案之前，尽量全面理解业务问题，否则解决方案可能与利益相关者无关。

**[Natassha Selvaraj](https://www.natasshaselvaraj.com/)** 是一位自学成才的数据科学家，对写作充满热情。你可以在[LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/)上与她联系。

### 更多相关内容

+   [找不到数据科学工作的原因？这里是为什么](https://www.kdnuggets.com/2022/01/unable-land-data-science-job.html)

+   [KDnuggets™ 新闻 22:n05, 2 月 2 日: 掌握机器学习的 7 个步骤…](https://www.kdnuggets.com/2022/n05.html)

+   [一个针对合成数据的社区来了，这就是我们需要它的原因](https://www.kdnuggets.com/2022/04/community-synthetic-data-need.html)

+   [Burtch Works 2023 年数据科学与 AI 专业人士薪资报告…](https://www.kdnuggets.com/2023/08/burtch-works-2023-data-science-ai-professionals-salary-report.html)

+   [想用你的数据技能解决全球问题？这里有一些建议…](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [这里是我用来赚取$10,000 的 AI 工具及我的技能…](https://www.kdnuggets.com/2023/07/ai-tools-along-skills-make-10000-monthly-bs.html)
