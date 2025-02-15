# 数据科学的“什么、在哪里和如何”

> 原文：[`www.kdnuggets.com/2018/06/what-where-how-data-science.html`](https://www.kdnuggets.com/2018/06/what-where-how-data-science.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

数据科学是一个难以用单一完整定义来概括的术语，这使得其使用变得困难，尤其是当目标是正确使用它时。大多数文章和出版物随意使用这一术语，假设它是普遍理解的。然而，数据科学——其方法、目标和应用——随着时间和技术的发展而不断演变。25 年前的数据科学指的是收集和清理数据集，然后应用统计方法。在 2018 年，[数据科学已发展成为一个涵盖数据分析、预测分析、数据挖掘、商业智能、机器学习](https://365datascience.com/data-science-vs-ml-vs-data-analytics/)等更多领域的领域。

实际上，由于没有一个定义可以完全适用，因此由[从事数据科学的人来定义它。](https://365datascience.com/interview-philippe-van-impe/)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能。

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求。

* * *

![365ds 数据科学信息图](https://365datascience.com/wp-content/uploads/2018/05/365-Data-Science-Infographic.png)

认识到需要对数据科学进行明确的解释，[365 数据科学](https://365datascience.com)团队设计了[什么-哪里-谁信息图](https://365datascience.com/wp-content/uploads/2018/05/365-Data-Science-Infographic.png)。我们定义了数据科学的关键过程并传播该领域。以下是我们对数据科学的解读（点击信息图查看更大图像）。

当然，这看起来可能是大量的信息，但其实并不是如此。在这篇文章中，我们将拆解数据科学，并将其重新构建成一个连贯且易于管理的概念。请耐心等待！

数据科学，“在一分钟内解释”，如下所示。

你有数据。要利用这些数据来指导你的决策，它需要相关、组织良好，并且最好是数字化的。一旦数据连贯，你就可以开始分析数据，创建仪表板和报告，以更好地了解业务表现。然后，你会把目光投向未来，开始生成预测分析。通过预测分析，你评估潜在的未来场景，并以创新的方式预测消费者行为。

但让我们从头开始。

### 数据科学中的数据

在任何其他事情之前，总是要有数据。数据是数据科学的基础；它是所有分析的依据。在数据科学的背景下，[有两种类型的数据：传统数据和大数据。](https://365datascience.com/techniques-for-processing-traditional-and-big-data/)

传统数据是结构化存储在数据库中的数据，分析师可以从一台计算机上进行管理；它是表格格式的，包含数字或文本值。实际上，"传统"这个术语是为了清晰起见引入的。它有助于强调大数据与其他类型数据之间的区别。

另一方面，大数据则比传统数据“大”得多，而且不是简单的意义上的大。从种类（数字、文本，甚至图像、音频、移动数据等），到速度（实时检索和计算），到体积（以太字节、拍字节、艾字节计量），大数据通常分布在计算机网络上。

也就是说，让我们定义一下数据科学中的 What-Where-and-Who 的特点。

### 在数据科学中你对数据做什么？

#### **数据科学中的传统数据**

传统数据存储在关系数据库管理系统中。

![365ds 图 1 数据什么](img/1cdb2c89b92b3d24e323ef08bd012517.png)

也就是说，在准备处理之前，所有数据都需要经过预处理。这是一组必要的操作，将原始数据转换为更易于理解的格式，从而对进一步处理有用。常见的过程包括：

+   收集原始数据并存储在服务器上

这是未经处理的数据，科学家无法直接分析。这些数据可以来自调查，或通过更常见的自动数据收集方式，如网站上的 cookies。

+   给观察值打上类别标签

这包括按类别排列数据或将数据点标记为正确的数据类型。例如，数字型或分类型。

+   数据清洗 / 数据擦洗

处理不一致的数据，如拼写错误的类别和缺失值。

+   数据平衡

如果数据不平衡，使得类别包含的观察数不均等且不具代表性，则应用**数据平衡**方法，如为每个类别提取相等数量的观察，并准备好进行处理，可以解决这个问题。

+   数据洗牌

重新排列数据点以消除不必要的模式，并进一步提高预测性能。例如，当数据中的前 100 个观察值来自使用网站的前 100 个人时；数据并未随机化，因此会出现由于抽样而产生的模式。

#### 数据科学中的大数据

当涉及到[大数据和数据科学](https://365datascience.com/techniques-for-processing-traditional-and-big-data/)时，虽然与传统数据处理方法有一些重叠，但也存在许多差异。

首先，大数据存储在许多服务器上，其复杂程度要高得多。

![365ds Fig2 Big Data What](img/512051b5b2b6aec331708343e56fd6a9.png)

为了用大数据进行数据科学，预处理尤为关键，因为数据的复杂性大大增加。你会发现，从概念上讲，一些步骤与传统数据预处理类似，但这是处理*数据*时的固有特性。

+   收集数据

+   对数据进行分类标记

请记住，大数据极其多样，因此标签不是“数字”与“分类”之分，而是“文本”、“数字图像数据”、“数字视频数据”、“数字音频数据”等。

+   数据清洗

这里的方法也极其多样；例如，你可以验证数字图像观察是否准备好进行处理；或者数字视频，等等。

+   数据掩码

在大规模收集数据时，目的是确保数据中的任何机密信息保持私密，而不影响分析和洞察提取。该过程涉及用随机和虚假的数据掩盖原始数据，使科学家能够进行分析而不泄露私人细节。当然，科学家也可以对传统数据进行这种处理，有时也是这样做，但在大数据中，信息可能更为敏感，因此掩盖更为紧迫。

#### 数据来自哪里？

传统数据可能来自基本的客户记录或历史股票价格信息。

然而，大数据无处不在。越来越多的公司和行业在使用和生成大数据。例如，在线社区，如 Facebook、Google 和 LinkedIn；或金融交易数据。各种地理位置的温度测量网格也构成了大数据，以及来自工业设备传感器的机器数据。当然，还有可穿戴技术。

#### 谁处理数据？

处理原始数据和预处理、创建数据库并维护数据库的数据专家可能有不同的名称。尽管他们的职位名称相似，但他们所承担的角色存在明显差异。考虑以下几点。

**数据架构师**和**数据工程师**（以及大数据架构师和大数据工程师）在数据科学市场中至关重要。前者从零开始创建数据库；他们设计数据的检索、处理和使用方式。因此，数据工程师利用数据架构师的工作作为基础，并处理（预处理）可用的数据。他们确保数据的清洁和组织，使其准备好供分析师使用。

另一方面，数据库管理员是控制数据流入和流出数据库的人。当然，随着大数据的发展，这一过程几乎完全自动化，因此实际上不需要人工管理员。数据库管理员主要处理传统数据。

也就是说，一旦数据处理完成，数据库清洁和组织好，真正的数据科学才开始。

### 数据科学

还有两种看待数据的方式：一种是解释已经发生的行为，并为此收集了数据；另一种是利用已有数据预测尚未发生的未来行为。

![365ds 图 3 数据科学何时何因](img/ae7bc5096c7af52fd866d3f13438862b.png)

### 数据科学解释过去

#### 商业智能

在数据科学进入预测分析之前，它必须查看过去提供的行为模式，分析这些模式以提取洞察，并为预测提供路径。商业智能正是专注于此：提供数据驱动的答案，例如：*销售了多少单位？哪个地区销售最多？哪种类型的商品销售在哪里？电子邮件营销在上个季度的点击率和产生的收入如何？与去年同季度的表现相比如何？*

尽管商业智能在其标题中没有“数据科学”这个词，但它是数据科学的一部分，而且不是任何微不足道的部分。

#### 商业智能做什么？

当然，商业智能分析师可以应用数据科学来衡量业务表现。但是，为了使商业智能分析师实现这一目标，他们必须使用特定的数据处理技术。

所有数据科学的起点是数据。一旦相关数据掌握在商业智能分析师手中（月度收入、客户、销售量等），他们必须量化观察结果，计算关键绩效指标（KPI），并检查度量，以从数据中提取洞察。

#### 数据科学就是讲述一个故事

除了处理严格的数值信息外，数据科学，尤其是商业智能，还涉及可视化发现，并创建仅由最相关数字支持的易于理解的图像。毕竟，各级管理层应能够理解数据中的洞察，并以此指导决策。

![365ds 图 4 商业智能](img/209f512efae6381009add2b7f6c9727f.png)

商业智能分析师创建仪表板和报告，并配以图表、图解、地图和其他类似的可视化效果，以展示与当前业务目标相关的发现。

### 商业智能在哪里被使用？

#### 价格优化与数据科学

值得注意的是，分析师应用数据科学来指导如价格优化技术等事务。他们实时提取相关信息，与历史数据进行比较，并采取相应措施。以酒店管理行为为例：管理层在许多人想要访问酒店的时期提高房价，而在低需求时期则降低房价，以吸引游客。

#### 库存管理和数据科学

数据科学和商业智能对于处理过剩和短缺问题是不可或缺的。对过去销售交易的深入分析可以识别季节性模式和销售最高的时间段，从而实施有效的库存管理技术，以最低成本满足需求。

### 谁负责数据科学的 BI 分支？

BI 分析师主要集中于对过去历史数据的分析和报告。

BI 顾问通常只是‘外部 BI 分析师’。许多公司外包他们的数据科学部门，因为他们不需要或不想维持一个。BI 顾问如果被聘用，实际上就是 BI 分析师，不过，他们的工作更为多样化，因为他们会参与不同的项目。角色的动态特性为 BI 顾问提供了不同的视角，而 BI 分析师拥有高度专业化的知识（即深度），BI 顾问则贡献于数据科学的广度。

BI 开发人员是处理更高级编程工具（如 Python 和 SQL）以创建专门为公司设计的分析的人员。这是 BI 团队中第三频繁出现的职位。

### 那么，这就是数据科学的全部内容吗？

数据科学是一个模糊的术语，涵盖了从处理数据——传统数据或大数据——到解释模式和预测行为的所有内容。

数据科学可以通过传统方法如回归分析和聚类分析完成，也可以通过非传统的机器学习技术完成。

我们在[这篇文章的第二部分](https://www.kdnuggets.com/2018/06/data-science-predicting-future.html)讨论了数据科学预测方法。希望当你阅读完这部分内容后，数据科学的所有拼图都能拼凑完整！

**简历: [Iliya Valchanov](https://www.linkedin.com/in/iliya-valchanov-607293a6/)** 是 365 Data Science 的联合创始人。

本博客的一部分内容已发布在 [365 Data Science Blog](https://365datascience.com/defining-data-science/)。经授权转载。

**相关：**

+   [数据科学与机器学习的执行指南](https://www.kdnuggets.com/2018/05/executive-guide-data-science-machine-learning.html)

+   [数据科学家的命令行技巧](https://www.kdnuggets.com/2018/06/command-line-tricks-data-scientists.html)

+   [DIY 深度学习项目](https://www.kdnuggets.com/2018/06/diy-deep-learning-projects.html)

### 更多相关话题

+   [停止学习数据科学以寻找目标，然后寻找目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [每个数据科学家都应该了解的三大 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [一个 $9B 的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [是什么使 Python 成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
