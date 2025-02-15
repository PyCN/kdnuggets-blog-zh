# Python 中的基本统计学：概率

> 原文：[`www.kdnuggets.com/2018/08/basic-statistics-python-probability.html`](https://www.kdnuggets.com/2018/08/basic-statistics-python-probability.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Christian Pascual，[Dataquest](https://www.dataquest.io/blog/)**

学习统计学时，你不可避免地需要了解概率论。虽然容易陷入概率的公式和理论中，但它在工作和日常生活中都有重要的应用。我们已经[讨论过](https://www.dataquest.io/blog/basic-statistics-in-python-probability/www.dataquest.io/blog/basic-statistics-with-python/)一些描述性统计的基本概念；现在我们将探讨统计学如何与概率论相关联。

### 前提条件：

类似于之前的帖子，本文假设没有统计学的先验知识，但需要至少具备基本的 Python 知识。如果你对[`for` 循环](https://www.python-tutorial.net/python-loops/)和[列表](https://www.python-tutorial.net/python-lists/)感到不熟悉，我建议你在继续之前简单学习一下它们。

### 什么是概率？

在最基本的层面上，概率试图回答这个问题：“事件发生的机会有多大？”一个**事件**是指感兴趣的某个结果。为了计算事件发生的机会，我们还需要考虑所有可能发生的其他事件。

概率的典型表示就是简单的掷硬币。在掷硬币中，唯一可能发生的事件是：

1.  翻到正面

1.  翻到反面

这两个事件形成了**样本空间**，即所有可能发生的事件的集合。要计算事件发生的概率，我们需要计算感兴趣的事件（比如掷出正面）的发生次数，并将其除以样本空间。因此，概率告诉我们理想的硬币正面或反面的机会是 1/2。通过观察可能发生的事件，概率为我们提供了一个框架，用于做出*预测*，即事件发生的频率。

然而，即使看起来很明显，如果我们真的尝试掷一些硬币，我们可能会偶尔得到异常高或低的正面次数。如果我们不想假设硬币是公平的，我们该怎么办？我们可以收集数据！我们可以使用统计学根据现实世界的观察来计算概率，并检查它与理想情况的比较。

### 从统计学到概率论

我们的数据将通过掷硬币 10 次并计算获得多少次正面来生成。我们将一组 10 次掷硬币称为*试验*。我们的数据点将是我们观察到的正面次数。我们可能得不到“理想的”5 次正面，但我们不会太担心，因为一次试验只是一个数据点。

如果我们进行很多次试验，我们期望所有试验中的*平均*正面朝上的次数接近 50%。下面的代码模拟了 10 次、100 次、1000 次和 1000000 次试验，并计算了观察到的正面比例的平均值。我们的过程也在下面的图像中总结了。

![硬币示例](img/4752c81faebbc9d98ddf1feb5b0b6b37.png)

```py
import random
def coin_trial():
    heads = 0
    for i in range(100):
        if random.random() <= 0.5:
            heads +=1
    return heads

def simulate(n):
   trials = []
   for i in range(n):
       trials.append(coin_trial())
   return(sum(trials)/n)

simulate(10)
>> 5.4

simulate(100)
>>> 4.83

simulate(1000)
>>> 5.055

simulate(1000000)
>>> 4.999781

```

`coin_trial`函数表示对 10 次抛硬币的模拟。它使用`random()`函数生成 0 到 1 之间的浮点数，如果该数在这个范围的一半内，则增加`heads`计数。然后，`simulate`根据你希望的次数重复这些试验，返回所有试验中的正面平均次数。

硬币抛掷模拟给我们一些有趣的结果。首先，数据证实我们的*平均*正面次数确实接近概率所预测的值。此外，这个平均值在更多试验中*改善*。在 10 次试验中，存在一些轻微的误差，但这种误差在 1,000,000 次试验中几乎完全消失。随着试验次数的增加，*偏差*从平均值中减少。听起来熟悉吗？

当然，我们可以自己抛硬币，但 Python 通过允许我们在代码中*建模*这一过程节省了很多时间。随着数据的增加，现实世界开始接近理想情况。因此，给定足够的数据，统计学使我们能够通过现实世界的观察来计算概率。概率提供理论，而统计学提供了使用数据测试该理论的工具。描述性统计，特别是均值和标准差，成为理论的代理。

你可能会问，“如果我可以直接计算理论概率，那我为什么需要一个代理？”硬币抛掷是一个简单的玩具示例，但更有趣的概率并不是那么容易计算的。有人在一段时间内发展某种疾病的机会是多少？在驾驶时，关键汽车部件失败的概率是多少？

计算概率没有简单的方法，因此我们必须依靠数据和统计来进行计算。随着数据量的增加，我们可以对计算结果代表这些重要事件发生的*真实*概率更加自信。

也就是说，请记住从[我们之前的统计帖子](https://www.dataquest.io/blog/basic-statistics-in-python-probability/www.dataquest.io/blog/basic-statistics-with-python/)中你是一个正在接受培训的侍酒师。在你开始购买葡萄酒之前，你需要弄清楚哪些葡萄酒比其他葡萄酒更好。你手头有很多数据，所以我们将利用统计来指导我们的决策。

### 数据和分布

在我们处理“哪种酒比平均水平更好”的问题之前，我们需要考虑数据的性质。直观上，我们希望使用酒的评分来比较不同组别，但这里会有一个问题：评分通常在一个范围内。我们如何在不同类型的酒之间比较评分组，并且以某种程度的确定性知道哪一种更好呢？

进入**正态分布**。正态分布指的是概率与统计领域中一个特别重要的现象。正态分布的形状如下：

![Normal Distribution Look](img/d8182c43bc0024c6efcc599f63c91885.png)

关于正态分布，最重要的特点是它的**对称性**和**形状**。我们称之为分布，但到底是什么被分布？这取决于背景。

在概率中，正态分布是一种特别的概率分布。x 轴表示我们想知道概率的事件值。y 轴是与每个事件相关的概率，从 0 到 1。虽然我们还没有深入讨论概率分布，但要知道正态分布是特别重要的一种概率分布。

在统计学中，我们的数据值是被分布的。这里，x 轴是我们的数据值，y 轴是这些值的计数。以下是正态分布的相同图示，但根据概率和统计背景进行了标注：

![Axes Comparison](img/86ff98b7768be0d7721b4558d710513c.png)

在概率的背景下，正态分布中的高点代表发生概率最高的事件。当你从这个事件的两侧远离时，概率迅速下降，形成熟悉的钟形曲线。在统计背景下，高点实际上代表的是均值。与概率一样，当你远离均值时，频率也迅速下降。也就是说，极高或极低的偏差存在，但极为罕见。

如果你怀疑正态分布在概率和统计之间存在其他关系，那么你的猜测是正确的！我们将在文章后面深入探讨这一重要关系，请稍等。

由于我们将使用评分分布来比较不同的酒，我们需要进行一些设置以捕获我们感兴趣的酒。我们将引入酒的数据，然后分开一些我们感兴趣的酒的评分。

要恢复数据，我们需要以下代码：

```py
import csv
with open("wine-data.csv", "r", encoding="latin-1") as f:
    wines = list(csv.reader(f))

```

数据如下表格所示。我们需要`points`列，因此我们将其提取到自己的列表中。我们听说过一位酒类专家说匈牙利 Tokaji 酒非常优秀，而一位朋友建议我们从意大利 Lambrusco 开始。我们有数据来比较这些酒！

如果你不记得数据的样子，这里有一个快速的表格供参考和重新熟悉。

| N | 国家 | 描述 | 评级 | 分数 | 价格 | 省份 | 区域 1 | 区域 2 | 品种 | 酒庄 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 美国 | "这瓶酒的绝对 100%..." | 玛莎葡萄园 | 96 | 235 | 加利福尼亚州 | 纳帕谷 | 纳帕 | 赤霞珠 | 海茨 |
| 1 | 西班牙 | "成熟的无花果香... | Carodorum Selecci Especial Reserva | 96 | 110 | 西北西班牙 | 托罗 |  | 托罗红葡萄 | 卡门·罗德里格斯酒庄 |
| 2 | 美国 | "Mac Watson honors... | 特别挑选晚收 | 96 | 90 | 加利福尼亚州 | 骑士谷 | 索诺玛 | 长相思 | 澳门利 |
| 3 | 美国 | "这瓶酒经过了 20 个月... | 储备酒 | 96 | 65 | 俄勒冈州 | 威拉米特谷 | 威拉米特谷 | 黑皮诺 | 潘齐 |
| 4 | 法国 | "这是顶级葡萄酒... | La Brelade | 95 | 66 | 普罗旺斯 | 邦多尔 |  | 普罗旺斯红葡萄酒混合 | 德拉贝古酒庄 |

```py
# Extract the Tokaji scores
tokaji = []
non_tokaji = []
for wine in wines:
    if points != '':
        points = wine[4]
    if wine[9] == "Tokaji":
        tokaji.append(float(points))
    else:
        non_tokaji.append(points)

# Extract the Lambrusco scores 
lambrusco = []
non_lambrusco = []
for wine in wines:
    if points != '':
        points = wine[4]
    if wine[9] == "Lambrusco":
        lambrusco.append(float(points))
    else:
        non_lambrusco.append(float(points))

```

如果我们将每组评分可视化为正态分布，我们可以立即判断两个分布是否不同，基于它们的位置。但这种方法很快会遇到问题，如下所示。我们假设评分会呈正态分布，因为我们有*a ton*的数据。虽然这个假设在这里是可以接受的，但稍后我们将讨论在什么情况下这样做可能会有危险。

![可视化的正常对](img/21a9ae6333dc1f3155937d46850291db.png)

当两个评分分布重叠过多时，最好假设它们实际上来自同一分布，而不是不同的分布。另一方面，如果完全没有重叠，则可以安全地假设这些分布不是相同的。我们的难点在于一些重叠的情况。鉴于一个分布的极高值可能与另一个分布的极低值相交，我们如何判断这些组是否不同？

在这里，我们必须再次利用正态分布来为我们提供答案，并搭建统计学与概率论之间的桥梁。

### 再次审视正态分布

正态分布对概率和统计学的重要性在于两个因素：**中心极限定理**和**三西格玛法则**。

### 中心极限定理

在上一节中，我们演示了如果我们多次重复 10 次掷硬币的试验，那么所有这些试验的*平均*正面出现次数将接近我们期望的理想硬币的 50%。随着试验次数的增加，这些试验的平均值将更接近*真实*概率，即使个别试验本身是不完美的。这一思想是中心极限定理的一个关键原则。

在我们的抛硬币示例中，一次 10 次抛掷的试验产生了一个*估计值*，即概率所建议的*应该*发生的情况（5 个正面）。我们称之为估计值，因为我们知道它不会是完美的（即我们不会每次都得到 5 个正面）。如果我们进行许多估计，中心极限定理规定这些**估计值**的**分布**将呈现正态分布。这种分布的峰值将与估计值应当取得的真实值对齐。在统计学中，正态分布的峰值与均值对齐，这正是我们观察到的。因此，给定多个“试验”作为我们的数据，中心极限定理建议，即使我们不知道真实概率，我们也可以接近概率所给出的理论理想。

中心极限定理让我们知道，许多试验的平均值将接近真实均值，三西格玛规则将告诉我们数据在这个均值周围的分布情况。

### 三西格玛规则

三西格玛规则，也称为经验规则或 68-95-99.7 规则，是关于我们观察值落在均值的某个距离内的表达。请记住，标准差（也称为“σ”）是数据集中一个观察值距离均值的平均距离。

三西格玛规则规定**在正态分布下**，68%的观察值将落在均值一个标准差范围内。95%将落在两个标准差范围内，99.7%将落在三个标准差范围内。推导这些值需要很多复杂的数学运算，因此超出了本文的范围。关键是要知道，三西格玛规则使我们能够了解在正态分布的不同区间内包含的数据量。下图是三西格玛规则的一个很好的总结。

![三西格玛](img/bfee5311aad5c7cafac4d38bbabdcfd9.png)

我们将这些概念应用到我们的葡萄酒数据中。作为一名侍酒师，我们希望能够高度自信地知道查尔多内和黑比诺比普通葡萄酒更受欢迎。我们有成千上万条葡萄酒评论，因此根据中心极限定理，这些评论的平均分数应该与所谓的葡萄酒质量的“真实”表现（由评论者判断）一致。

尽管三西格玛规则是关于数据中有多少数据落在已知值范围内的陈述，但它也表明了**极端值的稀有性**。任何与均值相差超过三倍标准差的值都应当谨慎对待。通过利用三西格玛规则和**Z 分数**，我们将能够最终为查尔多内和黑比诺与普通葡萄酒的差异的可能性指定一个值。

### Z 分数

Z 分数是一个简单的计算，回答了“给定一个数据点，它离均值有多少个标准差？”这个问题。下面的方程式是 Z 分数方程式。

![Z-score](img/d3411b8a18db4d186a4fcb9f86d3c58d.png)

Z-score 本身并没有提供太多信息。它的最大价值在于与**Z 表**进行比较，Z 表列出了标准正态分布在给定 Z-score 下的**累积概率**。标准正态分布是均值为 0，标准差为 1 的正态分布。Z-score 使我们能够参考 Z 表，即使我们的正态分布不是标准的。

累积概率是所有值发生的概率之和，直到某一点为止。一个简单的例子是均值本身。均值是正态分布的正中间，因此我们知道，从左侧到均值的所有概率之和是 50%。如果你尝试计算标准差之间的累积概率，实际上会得到“三个西格玛规则”的值。下面的图片提供了累积概率的可视化。

![Cumulative Probability](img/6063c525771f870d9fdda7d0e0ed4def.png)

我们知道*所有*概率的总和必须等于 100%，因此我们可以使用 Z 表来计算正态分布下 Z-score 两侧的概率。

![Cumulative Probability 2](img/a248fa7dd06f6d403fb701ce784a9c15.png)

计算超过某个 Z-score 的概率对我们来说非常有用。它让我们从“一个值离均值有多远”转到“一个值离均值这么远在同一组观察中有多可能？”因此，从 Z-score 和 Z 表得出的概率将回答我们关于葡萄酒的问题。

```py
import numpy as np
tokaji_avg = np.average(tokaji)
lambrusco_avg = np.average(lambrusco)

tokaji_std = np.std(tokaji)
lambrusco = np.std(lambrusco)

# Let's see what the results are
print("Tokaji: ", tokaji_avg, tokaji_std)
print("Lambrusco: ", lambrusco_avg, lambrusco_std)
>>> Tokaji:  90.9 2.65015722804
>>> Lambrusco:  84.4047619048 1.61922267961

```

这对我们朋友的推荐看起来不太好！为了本文的目的，我们将 Tokaji 和 Lambrusco 的评分视为正态分布。因此，每种葡萄酒的平均评分将代表它们在质量上的“真实”评分。我们将计算 Z-score，看看 Tokaji 的平均值离 Lambrusco 有多远。

```py
z = (tokaji_avg - lambrusco_avg) / lambrusco_std
>>> 4.0113309781438229

# We'll bring in scipy to do the calculation of probability from the Z-table
import scipy.stats as st
st.norm.cdf(z)
>>> 0.99996981130231266

# We need the probability from the right side, so we'll flip it!
1 - st.norm.cdf(z)
>>> 3.0188697687338895e-05

```

答案相当小，但这究竟意味着什么？这种概率的极小需要一些细致的解释。

假设我们相信**朋友的 Lambrusco 和葡萄酒专家的 Tokaji 没有区别**。也就是说，我们认为 Lambrusco 和 Tokaji 的质量大致相同。同样，由于不同葡萄酒之间的个体差异，这些葡萄酒的评分会有一些分散。如果我们绘制 Tokaji 和 Lambrusco 葡萄酒的直方图，由于中心极限定理，这将产生**正态分布的评分**。

现在，我们有一些数据可以计算这两种葡萄酒的均值和标准差。这些值使我们能够实际检验我们对 Lambrusco 和 Tokaji 质量相似的信念。我们使用 Lambrusco 的评分作为基础，并比较 Tokaji 的平均值，但我们也可以反过来做，唯一的区别只是 Z-score 会是负值。

Z 分数为 4.01！请记住，三西格玛规则告诉我们，**99.7%**的数据应落在**3 个标准差**之内，假设托卡伊和兰布鲁斯科是相似的。在一个假设托卡伊和兰布鲁斯科酒相同的世界里，像托卡伊那样极端的评分平均值的概率非常非常小。如此之小，以至于我们不得不考虑相反的情况：托卡伊酒与兰布鲁斯科酒不同，将产生不同的评分分布。

我们在这里仔细选择了措辞：我特意避免说“托卡伊酒比兰布鲁斯科酒更好”。它们很可能是这样。这是因为我们计算了一个概率，虽然微观上很小，但不为零。为了准确起见，我们可以说兰布鲁斯科酒和托卡伊酒确实不是来自同一评分分布，但我们不能说其中一个比另一个更好或更差。

这种推理属于**推断统计学**的范畴，本文仅旨在简要介绍其背后的理由。我们在本文中涵盖了许多概念，因此如果你发现自己迷失了方向，请返回并慢慢阅读。拥有这种思维框架是极其强大的，但容易被误用和误解。

### 结论

我们从描述性统计学开始，然后将其与概率联系起来。从概率出发，我们开发了一种量化方法来显示两个组是否来自同一分布。在这种情况下，我们比较了两种酒的推荐，并发现它们很可能不来自同一评分分布。换句话说，一种酒很可能比另一种更好。

统计学不必仅限于统计学家这个领域。作为数据科学家，对常见统计指标的直观理解将为你提供制定自己理论的优势，并随后测试这些理论的能力。我们在这里只是略微触及了推断统计学的表面，但这些基本思想将有助于指导你在统计学之旅中的直觉。我们的文章讨论了正态分布的优势，但统计学家们也开发了调整非正态分布的技术。

### 进一步阅读

本文围绕正态分布及其与统计学和概率的关系展开。如果你有兴趣阅读其他相关分布或学习更多关于推断统计学的内容，请参考下面的资源。

+   [学生 T 分布：当我们只有少量数据点时](https://en.wikipedia.org/wiki/Student%27s_t-distribution)

+   [深入探讨假设检验和推断统计学](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing)

+   [使用 Python 深入统计学](https://www.youtube.com/watch?v=TMmSESkhRtI)

[原文](https://www.dataquest.io/blog/basic-statistics-in-python-probability/?utm_source=kdnuggets&utm_medium=crosspost&utm_content=basic%20statistics%20probability)。已获授权转载。

**相关：**

+   描述性统计关键术语解释

+   解释正态分布中的 68-95-99.7 规则

+   为什么数据科学家喜欢高斯分布

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

### 更多相关话题

+   [数据科学中的统计与概率](https://www.kdnuggets.com/2022/06/statistics-probability-data-science.html)

+   [KDnuggets 新闻，7 月 6 日：12 个必备数据科学 VSCode 插件…](https://www.kdnuggets.com/2022/n27.html)

+   [数据科学中的 8 个基本统计概念](https://www.kdnuggets.com/2020/06/8-basic-statistics-concepts.html)

+   [分析未来成功的概率与智能…](https://www.kdnuggets.com/2022/02/analyzing-probability-future-success-intelligence-node-attributes-evolution-model.html)

+   [数据科学中概率的重要性](https://www.kdnuggets.com/2023/02/importance-probability-data-science.html)

+   [在斯坦福大学免费学习计算机科学中的概率](https://www.kdnuggets.com/learn-probability-in-computer-science-with-stanford-university-for-free)
