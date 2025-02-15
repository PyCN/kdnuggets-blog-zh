# 哪些大数据、数据挖掘和数据科学工具常常一起使用？

> 原文：[`www.kdnuggets.com/2015/06/data-mining-data-science-tools-associations.html`](https://www.kdnuggets.com/2015/06/data-mining-data-science-tools-associations.html)

由 **[Gregory Piatetsky](https://www.kdnuggets.com/author/gregory-piatetsky "Posts by Gregory Piatetsky")** 撰写，发表于 2015 年 6 月 11 日，内容涉及 [Apache Spark](https://www.kdnuggets.com/tag/apache-spark)、[数据挖掘软件](https://www.kdnuggets.com/tag/data-mining-software)、[Excel](https://www.kdnuggets.com/tag/excel)、[Hadoop](https://www.kdnuggets.com/tag/hadoop)、[Knime](https://www.kdnuggets.com/tag/knime)、[调查](https://www.kdnuggets.com/tag/poll)、[Python](https://www.kdnuggets.com/tag/python)、[R](https://www.kdnuggets.com/tag/r)、[RapidMiner](https://www.kdnuggets.com/tag/rapidminer)、[SQL](https://www.kdnuggets.com/tag/sql)![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

*（与 Shashank Iyer 共同撰写。）* 我们使用了 2015 KDnuggets 数据挖掘软件调查 的匿名数据，并对前 20 个工具进行了关联分析。数据集包含 2759 票，每票为一个或多个工具。在这篇文章的底部有一个链接可以下载匿名数据集。

我们使用了一种 Apriori 算法的版本来分析结果。

测量两个名义或二元特征之间关联显著性的方法有很多，例如卡方检验或 T 检验，但我们使用了一种简单的度量，我们称之为“提升度”，其定义为

提升度（X & Y）= pct (X & Y) / ( pct (X) * pct (Y) )

其中 pct(X) 是选择 X 的用户百分比。

提升度（X&Y）> 1 表示 X&Y 一起出现的频率高于它们独立时的预期频率，

如果 X 和 Y 的出现频率与它们独立时的预期频率相同，则提升度 = 1，

如果 X 和 Y 一起出现的频率低于预期，则提升度（Lift）< 1（负相关）。

注意，这一度量是对称的：提升度（X & Y）= 提升度（Y & X）

图 1 显示了前 10 大数据挖掘工具的热图。提升度值显示在各自的矩阵位置中，颜色梯度表示关联程度，从高到低。

如果提升度 > 1.2，方块为红色；如果小于 0.8，方块为蓝色；否则为灰色。

Spark 和 Hadoop 的提升度最高，达到了 3.31，其次是 Spark 和 Python（提升度 = 2.05）。我们还注意到 Excel 和 SQL 之间，以及 Tableau 和 SQL 之间存在较强的关联。

最低的关联度出现在 SAS base 和 KNIME（0.51），SAS base 和 RapidMiner（0.52），以及 KNIME 和 RapidMiner（0.56）之间。

![前 10 大数据挖掘工具热图](img/0484f94fa4fb39691a033ad2d2cb32b4.png)

图 1：前 10 大数据挖掘工具的关联矩阵热图

计算了一个类似的热图（图 2），显示了开源工具和商业工具之间的各种关联。

![关联——开源与商业数据挖掘工具](img/1906236195dab0204f36f96f75f95347.png)

图 2: 开源工具与商业工具之间的混淆矩阵热图

为了可视化前 20 个最受欢迎工具之间的相关性，计算了图 3 中的网络图。

每个节点代表一个前 20 名工具，节点的颜色为红色：免费/开源工具，绿色：商业工具，紫红色：Hadoop/大数据工具。节点大小根据每个工具获得的投票百分比而变化。边缘大致分为两个部分——低关联到高关联，这在从浅粉色到深紫色的陡色渐变中显示。此分段还显示在每条边的权重中，较厚的边表示高关联，较细的边表示低关联。

![前 20 名数据挖掘工具关联网络](img/associations-top-20-data-mining-tools-network-large.jpg)

图 3: 最受欢迎的 20 个工具的网络图（点击图表以获取更高分辨率的图像）

以下是图 3 网络图中数据的顶级、高级和低级关联。

表 1.  前 20 个工具中 20 个最高关联（提升）

| 工具 X | 工具 Y | 提升 |
| --- | --- | --- |
| Alteryx | Tableau | 2.14 |
| Excel | Microsoft SQL Server | 2.15 |
| Excel | SQL | 1.93 |
| Hadoop | Spark | 3.31 |
| IBM SPSS Modeler | IBM SPSS Statistics | 6.18 |
| KNIME | Weka | 1.93 |
| MATLAB | Weka | 2.53 |
| Pig | Hadoop | 4.24 |
| Pig | Spark | 4.44 |
| Pig | scikit-learn | 2.25 |
| Pig | Unix shell/awk/gawk | 2.25 |
| Pig | Python | 2.09 |
| Python | scikit-learn | 3.12 |
| Python | Spark | 2.05 |
| Python | Unix shell/awk/gawk | 1.91 |
| SAS base | SAS Enterprise Miner | 6.39 |
| scikit-learn | Spark | 2.63 |
| scikit-learn | Unix shell/awk/gawk | 2.56 |
| SQL | Microsoft SQL Server | 2.38 |
| SQL | Unix shell/awk/gawk | 2.01 |

正如预期的那样，Excel 经常与 SQL 配对，Hadoop 与 Spark 配对，Pig 与 Hadoop 配对，IBM SPSS Modeler 与 IBM SPSS Statistics 配对。在不那么明显的关联中，我们看到 Pig 与 scikit-learn，Weka 与 KNIME 和 MATLAB。

表 2.  前 20 个工具中 20 个最低关联（提升）

| 工具 X | 工具 Y | 提升 |
| --- | --- | --- |
| Alteryx | IBM SPSS Modeler | 0.54 |
| Alteryx | IBM SPSS Statistics | 0.17 |
| Alteryx | KNIME | 0.26 |
| Alteryx | MATLAB | 0.37 |
| Alteryx | Python | 0.55 |
| Alteryx | RapidMiner | 0.23 |
| Alteryx | SAS base | 0.28 |
| Alteryx | SAS Enterprise Miner | 0.24 |
| Alteryx | scikit-learn | 0.08 |
| Alteryx | Unix shell/awk/gawk | 0.08 |
| Alteryx | Weka | 0.11 |
| IBM SPSS Modeler | Unix shell/awk/gawk | 0.51 |
| IBM SPSS Statistics | scikit-learn | 0.34 |
| IBM SPSS Statistics | Spark | 0.54 |
| IBM SPSS Statistics | Unix shell/awk/gawk | 0.53 |
| KNIME | SAS base | 0.51 |
| KNIME | SAS Enterprise Miner | 0.48 |
| RapidMiner | SAS base | 0.52 |
| RapidMiner | SAS Enterprise Miner | 0.54 |

这里我们看到，Alteryx 用户几乎不使用其他工具（除了 Tableau），IBM SPSS 用户不使用 Unix，KNIME 和 RapidMiner 用户不使用 SAS。

最后，我们来看一下与 R 配套使用的工具。由于几乎一半的投票者使用了 R，没有工具与 R 的关联度超过 2，但以下是与 R 的提升值最高的工具。

表格 3. 最常与 R 关联的工具/软件

| 工具 | 提升 (工具, R) |
| --- | --- |
| Hive | 1.54 |
| Tableau | 1.42 |
| Python | 1.41 |
| Spark | 1.39 |
| scikit-learn | 1.36 |
| Unix shell/awk/gawk | 1.32 |
| MATLAB | 1.32 |
| SQLang | 1.30 |
| Weka | 1.30 |
| Microsoft SQL Server | 1.29 |

这是匿名数据集的链接（CSV 格式），包含 3 列

+   ord: 投票顺序

+   ntools: 工具数量

+   votes: 工具名称，用";"分隔。注意：为了匹配模式，R 被编码为 Rlang，SQL 被编码为 SQLang。

让我们知道你的发现！

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关主题

+   [数据科学家和数据工程师如何协作？](https://www.kdnuggets.com/2022/08/data-scientists-data-engineers-work-together.html)

+   [将人类与 AI 代理结合以增强客户体验](https://www.kdnuggets.com/2024/06/softweb/bringing-human-and-ai-agents-together-for-enhanced-customer-experience)

+   [哪个更好：数据科学训练营 vs 学位 vs 在线课程](https://www.kdnuggets.com/2022/09/best-data-science-bootcamp-degree-online-course.html)

+   [ETL 与 ELT：哪个更适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [我应该使用哪个指标？准确率 vs. AUC](https://www.kdnuggets.com/2022/10/metric-accuracy-auc.html)

+   [RAG vs 微调：哪个是提升 LLM 应用的最佳工具？](https://www.kdnuggets.com/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application)
