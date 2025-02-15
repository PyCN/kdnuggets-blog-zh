# 17 个必须知道的数据科学面试问题及答案

> 原文：[https://www.kdnuggets.com/2017/02/17-data-science-interview-questions-answers.html/2](https://www.kdnuggets.com/2017/02/17-data-science-interview-questions-answers.html/2)

### Q4\. 为什么包含较少的预测变量比包含许多预测变量更可取？

**[Anmol Rajpurohit](https://twitter.com/hey_anmol) 的回答：**

这里有几个理由说明为什么使用较少的预测变量可能比使用许多预测变量更好：

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT。

* * *

**冗余/不相关：**

如果你处理了许多预测变量，那么很可能其中一些变量之间存在隐藏的关系，导致冗余。除非在数据分析的早期阶段识别并处理这些冗余（通过仅选择非冗余的预测变量），否则它可能对后续步骤造成巨大阻碍。

也有可能并非所有预测变量对因变量都有显著影响。你应该确保你选择的预测变量集合中没有任何不相关的变量——即使你知道数据模型会通过赋予这些变量较低的重要性来处理它们。

*注意：冗余和不相关是两个不同的概念——一个相关的特征由于存在其他相关特征而可能变得冗余。*

**过拟合**：

即使你有大量的预测变量，并且它们之间没有任何关系，仍然建议使用较少的预测变量。具有大量预测变量的数据模型（也称为复杂模型）通常会出现过拟合的问题，这种情况下，数据模型在训练数据上表现很好，但在测试数据上表现不佳。

**生产力**：

假设你有一个项目，其中包含大量的预测变量，并且所有这些变量都是相关的（即对因变量有可测量的影响）。显然，你会希望使用所有这些变量，以便构建一个成功率非常高的数据模型。尽管这种方法听起来非常诱人，但实际考虑（如可用数据量、存储和计算资源、完成所需时间等）使得这种做法几乎不可能实现。

因此，即使你有大量相关的预测变量，使用较少的预测变量（通过特征选择筛选或通过特征提取开发）也是一个好主意。这本质上类似于帕累托原则，该原则指出，对于许多事件，约80%的效果来自20%的原因。

关注那20%最重要的预测变量，将对在合理时间内构建成功率较高的数据模型非常有帮助，而不需要大量数据或其他资源。

![](../Images/b30a107ef5009d61aa270f2047eb9950.png)

*训练误差与测试误差 vs 模型复杂性（来源：[Quora](https://www.quora.com/Why-might-it-be-preferable-to-include-fewer-predictors-over-many/answer/Sergül-Aydöre) 发布者：[Sergul Aydore](https://www.quora.com/profile/Sergül-Aydöre)）*

**可理解性**：

使用较少预测变量的模型更易于理解和解释。由于数据科学步骤将由人类执行，结果将由人类呈现（并且希望被使用），因此考虑到人脑的综合能力非常重要。这基本上是一个权衡——你放弃了一些潜在的好处，以提高数据模型的成功率，同时使数据模型更易于理解和优化。

如果在项目结束时你需要向他人展示你的结果，而这些人不仅对高成功率感兴趣，还对“幕后”发生的情况感兴趣，这一点特别重要。

* * *

### Q5. 你会使用什么误差指标来评估一个二分类器的好坏？如果类别不平衡呢？如果有多于2个组呢？

[**Prasad Pore**](/author/prasad-pore) **回答：**

二分类涉及将数据分类为两个组，例如，客户是否购买某一特定产品（是/否），基于性别、年龄、位置等独立变量。

由于目标变量不是连续的，二分类模型预测目标变量为“是/否”的概率。为了评估这样的模型，使用了一种称为混淆矩阵的指标，也叫分类矩阵或一致性矩阵。借助混淆矩阵，我们可以计算出重要的性能指标：

1.  真阳性率（TPR）或命中率或召回率或敏感性 = TP / (TP + FN)

1.  假阳性率（FPR）或假警报率 = 1 - 特异性 = 1 - (TN / (TN + FP))

1.  准确率 = (TP + TN) / (TP + TN + FP + FN)

1.  错误率 = 1 – 准确率 或 (FP + FN) / (TP + TN + FP + FN)

1.  精确度 = TP / (TP + FP)

1.  F-measure: 2 / ( (1 / 精确度) + (1 / 召回率) )

1.  ROC（接收操作特征曲线）= FPR vs TPR的图示

1.  AUC（曲线下面积）

1.  Kappa统计量

你可以在这里找到更多关于这些指标的详细信息：[测量分类模型准确性的最佳指标](/2016/12/best-metric-measure-accuracy-classification-models.html)。

所有这些措施都应结合领域技能并加以平衡，例如，如果你仅在预测无癌症患者时获得更高的TPR，那么在诊断癌症时将没有帮助。

在癌症诊断数据的相同示例中，如果只有2%或更少的患者有癌症，则这是一个类别不平衡的情况，因为癌症患者的比例相对于其他人群非常小。处理此问题的主要有2种方法：

1.  **使用成本函数**：在这种方法中，借助成本矩阵（类似于混淆矩阵，但更关注假阳性和假阴性）来评估与数据误分类相关的成本。主要目的是减少误分类的成本。假阴性的成本总是高于假阳性的成本。例如，错误地预测癌症患者为无癌症比错误地预测无癌症患者为癌症更危险。

总成本 = 假阴性的成本 * 假阴性的数量 + 假阳性的成本 * 假阳性的数量

1.  **使用不同的采样方法**：在这种方法中，你可以使用过采样、欠采样或混合采样。在过采样中，少数类观察值被复制以平衡数据。观察值的复制会导致过拟合，导致训练中准确度高但在未见数据中准确度低。在欠采样中，去除多数类观察值会导致信息丢失。这有助于减少处理时间和存储，但仅在你有大量数据集时才有用。

[在这里了解更多关于类别不平衡的信息](/2016/08/learning-from-imbalanced-classes.html)。

如果目标变量中有多个类别，则会形成一个与类别数量相等的混淆矩阵，并可以计算每个类别的所有性能指标。这称为多类混淆矩阵。例如，如果响应变量中有3个类别X、Y、Z，那么每个类别的召回率计算如下：

Recall_X = TP_X/(TP_X+FN_X)

Recall_Y = TP_Y/(TP_Y+FN_Y)

Recall_Z = TP_Z/(TP_Z+FN_Z)

* * *

### Q6. 我可以通过哪些方法使我的模型对异常值更具鲁棒性？

[**Thuy Pham**](/author/thuy-pham) **回答：**

有多种方法可以使模型对异常值更具鲁棒性，从不同的角度（数据准备或模型构建）。**异常值**在问答中被认为是人类知识中不需要的、意外的或必须是错误的值（例如，没有人200岁），而不是可能但稀有的事件。

异常值通常是相对于分布来定义的。因此，可以在预处理步骤（任何学习步骤之前）中通过使用标准差（对于正态分布）或四分位数范围（对于非正态/未知分布）作为阈值水平来移除异常值。

![](../Images/d5cb6f2fe1fb2779c49fcc7574c92c98.png)

**异常值。** [图片来源](https://www.neuraldesigner.com/blog/3_methods_to_deal_with_outliers)

此外，**数据转换**（例如对数转换）可能有助于当数据存在明显尾部时。当异常值与收集仪器的灵敏度有关，这些仪器可能无法准确记录小值时，**Winsorization**可能会有用。这种类型的转换（以Charles P. Winsor (1895–1951)的名字命名）具有与剪切信号相同的效果（即用不那么极端的值替换极端数据值）。 另一种减少异常值影响的选项是使用**平均绝对差**而不是均方误差。

对于模型构建，一些模型对异常值具有抗性（例如，[基于树的方法](https://www.quora.com/Why-are-tree-based-models-robust-to-outliers)）或非参数测试。类似于中位数效应，树模型在每次分裂时将每个节点分成两个。因此，在每次分裂时，所有数据点在一个桶中都可以被平等对待，无论它们可能有多极端。研究[Pham 2016]提出了一种检测模型，该模型结合了数据的四分位数信息来预测数据的异常值。

**参考文献：**

[Pham 2016] T. T. Pham, C. Thamrin, P. D. Robinson, 和 P. H. W. Leong. 在强迫振荡测量中的呼吸伪影去除：一种机器学习方法。IEEE生物医学工程学报，2016。

[这个Quora回答](https://www.quora.com/What-are-methods-to-make-a-predictive-model-more-robust-to-outliers)包含了更多信息。

* * *

这里是[第2部分](/2017/02/17-data-science-interview-questions-answers-part-2.html)和[第3部分](/2017/03/17-data-science-interview-questions-answers-part-3.html)的更多答案。

### 相关话题

+   [建立稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [7个数据分析面试问题与答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5个Python面试问题与答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [检测伪数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [检测伪数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)
��中客户行为随时间变化。例如，一家电话公司开发了一个预测客户流失的模型，或一家信用卡公司开发了一个预测交易欺诈的模型。训练数据是历史数据，而（新）测试数据是当前数据。

这样的模型需要定期重新训练，并且要确定何时可以比较旧数据（训练集）和新数据中的关键变量分布，如果有足够显著的差异，则需要重新训练模型。

有关更详细和技术性的讨论，请参见下面的参考文献。

**参考文献：**

[1] Marco Saerens, Patrice Latinne, Christine Decaestecker: 调整分类器输出以适应新的先验概率：一个简单的过程。Neural Computation 14(1): 21-41 (2002)

[2] 《非平稳环境中的机器学习：协变量转移适应导论》，Masashi Sugiyama, Motoaki Kawanabe，MIT出版社，2012，ISBN 0262017091，9780262017091

[3] Quora 对于 [测试数据分布与训练数据分布显著不同可能会有什么问题？](https://www.quora.com/What-could-be-some-issues-if-the-distribution-of-the-test-data-is-significantly-different-than-the-distribution-of-the-training-data)

[4] [分类中的数据集转移：方法与问题](http://iwann.ugr.es/2011/pdf/InvitedTalk-FHerrera-IWANN11.pdf)，Francisco Herrera 邀请讲座，2011。

[5] [当训练集和测试集不同：学习迁移的特征](http://homepages.inf.ed.ac.uk/amos/publications/Storkey2009TrainingTestDifferent.pdf)，Amos Storkey，2013。

* * *

### Q3\. 偏差和方差是什么，它们与建模数据的关系是什么？

**[Matthew Mayo](/author/matt-mayo) 回答：**

**偏差**是模型预测与正确性之间的偏离程度，而**方差**是这些预测在模型迭代间变化的程度。

![偏差与方差](../Images/b96b54746b97c6cc4d80a70ba9f99dab.png)

**偏差与方差**，[图像来源](http://scott.fortmann-roe.com/docs/BiasVariance.html)

[举个例子](/2016/08/bias-variance-tradeoff-overview.html)，通过一个简单的有缺陷的总统选举调查作为示例，通过偏差和方差的双重视角解释调查中的错误：从电话簿中选择调查参与者是一个偏差来源；小样本量是方差的来源。

最小化总模型误差依赖于偏差和方差误差的平衡。理想情况下，模型是**低方差的无偏数据**的集合。然而，不幸的是，模型越复杂，其倾向于减少偏差但增加方差；因此，最佳模型需要考虑这两种属性之间的平衡。

交叉验证的统计评估方法既有助于展示这种平衡的**重要性**，也有助于实际**寻找**这种平衡。使用的数据折数——即*k*-折交叉验证中的*k*值——是一个重要决策；值越低，误差估计的偏差越高，而方差则较少。

![偏差方差总误差](../Images/110b09d1df30869536aad9bf0fdda2a9.png)**偏差和方差对总误差的贡献**，[图像来源](http://scott.fortmann-roe.com/docs/BiasVariance.html) 相反，当*k*等于实例数量时，误差估计的偏差非常低，但方差可能很高。

最重要的要点是偏差和方差是构建模型时两个重要的权衡方面，即使是最常规的统计评估方法也直接依赖于这种权衡。

在下一页，我们回答

+   为什么选择较少的预测变量可能比选择更多的预测变量更优？

+   你会使用什么错误度量来评估一个二分类器的好坏？

+   有哪些方法可以使我的模型对异常值更加稳健？

### 更多相关话题

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [7 个数据分析面试问题及答案](https://www.kdnuggets.com/2022/09/7-data-analytics-interview-questions-answers.html)

+   [5 个 Python 面试问题及答案](https://www.kdnuggets.com/2022/09/5-python-interview-questions-answers.html)

+   [识别假数据科学家的 20 个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别假数据科学家的 20 个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)
