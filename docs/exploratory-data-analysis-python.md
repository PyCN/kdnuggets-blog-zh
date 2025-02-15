# Python 中的探索性数据分析

> 原文：[`www.kdnuggets.com/2017/07/exploratory-data-analysis-python.html`](https://www.kdnuggets.com/2017/07/exploratory-data-analysis-python.html)

**作者：Chloe Mawer 和 Jonathan Whitmore，[硅谷数据科学](https://svds.com/)。**

今年早些时候，我们写了关于探索性数据分析的价值的文章以及[你为何应关心](https://svds.com/value-exploratory-data-analysis/)。在那篇文章中，我们从很高的层面介绍了探索性数据分析（EDA）是什么，以及数据科学家和业务利益相关者为何应该将其视为其分析项目成功的关键。然而，那篇文章可能让你想知道：我该如何自己进行 EDA？

上个月，我的同事高级数据科学家 Jonathan Whitmore 和我在[PyCon](https://us.pycon.org/2017/)上教授了一门名为[Python 中的探索性数据分析](https://us.pycon.org/2017/schedule/presentation/170/)的教程——你可以[在这里](https://www.youtube.com/watch?v=W5WE9Db2RLU)观看。在这篇文章中，我们将总结教程的目标和内容，然后提供跟随的指示，以便你开始发展自己的 EDA 技能。

### 教程目标

本教程的目标是帮助参与者：

1.  通过在各种探索阶段提出问题并指出需要注意的事项，培养 EDA 的思维方式

1.  学习如何有效调用一些基本的 EDA 方法，以便理解数据集并为更高级的分析做好准备。这些基本方法包括：

    +   切片和切分

    +   计算汇总统计数据

    +   数值和分类数据的基本绘图

    +   基于地图的地理空间数据基本可视化

    +   使用 Jupyter Notebook 小部件进行交互式探索

我们将 EDA 视为一棵树：每次进行 EDA 时，你都会执行一系列基本步骤（树的主干），但在每个步骤中，观察会引导你探索其他方向（树枝），提出你想解答的问题或需要检验的假设。

你追寻的树枝将取决于你觉得有趣或相关的内容。因此，你在跟随本教程进行实际探索时将是你自己的。我们没有答案或结论，认为你应该对数据集得出什么结论。我们的目标仅仅是帮助你使探索尽可能有效，并让你享受选择跟随哪些分支的乐趣。

![SVDS EDA](https://github.com/cmawer/pycon-2017-eda-tutorial)

### 教程大纲

演讲包括以下内容：

1.  探索性数据分析简介：我总结了 EDA 的动机以及我们在教程中深入探讨的一般策略。

1.  Jupyter Notebooks 入门：我们的教程涉及通过一系列 Jupyter Notebooks 进行操作，因此 Jonathan 对那些从未见过它们的人做了一个快速介绍。我们甚至从一位参与者那里学到了一项新技巧！

1.  [Redcard](https://osf.io/47tnc/) 数据集的探索性分析：Jonathan 进行了一项关于数据集的探索性分析，该数据集来源于一篇[有趣的论文](https://osf.io/gvm2z/)，该论文在[《自然》](http://www.nature.com/news/crowdsourced-research-many-hands-make-tight-work-1.18508)上发布，并附有评论。论文的核心问题在标题中有所反映：“许多分析师，一个数据集：如何透明地展示分析选择的变化如何影响结果”。作者招募了约 30 个分析团队，每个团队都被赋予了相同的研究问题：“足球裁判是否更可能对深色肤色的球员出示红牌，而不是浅色肤色的球员？”并提供了相同的数据。数据集来自 2012-13 年欧洲足球（足球）职业联赛中的球员。包括球员的年龄、身高、体重、位置、肤色评级等数据。然后比较各团队的结果，查看不同的数据查看方式如何得出不同的统计结论。丰富的数据集提供了充足的机会来进行探索性数据分析。从决定层级领域位置，到身高或体重的分位数。我们展示了几种有用的库，包括标准库如 pandas，以及较少见的库如`missingno`、`pandas-profiling`和`pivottablejs`。

1.  AQUASTAT 数据集的探索性分析：在这一部分，我将对[粮农组织](http://www.fao.org/)（FAO）的[ AQUASTAT](http://www.fao.org/nr/water/aquastat/data/query/index.html) 数据集进行探索。这些数据集提供了关于水资源可用性和使用情况的指标，以及自 1952 年起每五年报告的各国其他人口统计数据。该数据集通常被称为*面板数据*或*纵向数据*，因为它是针对相同主题（在此案例中为国家）重复收集的数据。我们讨论了将其作为面板数据进行探索的方法，以及仅关注数据横截面（在各国收集的单一时间段的数据）的方法。这些数据也是地理空间的，因为每个观测值都对应一个地理定位区域。我们展示了如何在 Python 中在地图上查看非常基本的数据，但地理空间分析是一个深奥的领域，我们在查看这个数据集时只是触及了它的表面。我们推荐[PySAL 教程](http://darribas.org/gds_scipy16/)作为 Python 中地理空间分析的入门介绍。

![SVDS EDA](https://github.com/cmawer/pycon-2017-eda-tutorial)

### 在家跟随学习

为了充分利用本教程，我们建议你实际操作我们开发的 Jupyter notebooks。你可以通过以下两种方式之一来完成：

1.  在云端通过 [Microsoft Azure notebooks](https://notebooks.azure.com/)：设置一个账户，然后克隆 [这个库](https://notebooks.azure.com/chloe/libraries/pycon-2017-eda-tutorial)。克隆此库将允许你在线打开、编辑和运行每一个 Jupyter notebook，无需担心设置 Jupyter notebooks 和 Python 环境。此服务是免费的，你的 notebooks 可以保存以备将来使用，没有任何限制。需要了解的唯一事项是，虽然 notebooks 会被无限期保存，但在工作会话结束后，数据或其他非 notebook 文件不会被保存。数据可以导入并分析，但任何 notebook 外的结果必须在离开之前下载。

1.  在本地计算机上：克隆 [github 仓库](https://github.com/cmawer/pycon-2017-eda-tutorial) 并根据 [README](https://github.com/cmawer/pycon-2017-eda-tutorial/blob/master/README.md) 中的说明设置你的 Python 环境。

在家跟随教程，你可以随时暂停。我们在三小时的教程中讲解了大量材料（并且需要应对一些技术问题，这些问题在 65+人使用不同操作系统和各种公司防火墙的实践教程中不可避免！）。为了充分利用内容，我们建议你在教程中暂停，当有建议尝试某些分析时，亲自进行尝试。

EDA 的可能性是无穷的，即使对于单个数据集。你可能希望以不同的方式查看数据，我们欢迎你通过 github 仓库中的拉取请求提交你自己对一个或两个数据集的 EDA notebooks。我们将提供反馈并批准 PR，使你的方法可以与其他开发 EDA 技能的人分享。

**[Chloe Mawer](https://www.linkedin.com/in/chloemawer/)** 背景为地球物理学和水文学，擅长利用数据进行预测并提供有价值的见解。她在学术研究和工程方面的经验使她能够解决新问题并创造实用有效的解决方案。

**[Jonathan Whitmore](https://www.linkedin.com/in/jonathanbwhitmore/)** 是 SVDS 的数据科学家。在完成天体物理学的博士后职位后，Jonathan 成为计算和天文学方面的受欢迎演讲者。他对将机器学习和统计技术应用于行业问题感到兴奋，并开发了新颖的数据分析技术。

[原文](https://svds.com/exploratory-data-analysis-python/)。经许可转载。

**相关：**

+   探索性数据分析的价值

+   Jupyter 生态系统导航简要指南

+   数据科学的 Jupyter Notebook 最佳实践

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [掌握 SQL、Python、数据清理、数据处理与探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [用于非结构化数据的探索性数据分析技术](https://www.kdnuggets.com/2023/05/exploratory-data-analysis-techniques-unstructured-data.html)

+   [数据科学家探索性数据分析的必备指南](https://www.kdnuggets.com/2023/06/data-scientist-essential-guide-exploratory-data-analysis.html)

+   [掌握探索性数据分析的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-exploratory-data-analysis)

+   [5 个用于地理空间数据分析的 Python 包](https://www.kdnuggets.com/2023/08/5-python-packages-geospatial-data-analysis.html)

+   [最佳 Python 课程：分析总结](https://www.kdnuggets.com/2022/01/best-python-courses-analysis-summary.html)
