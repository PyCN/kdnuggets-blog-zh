# 2023 年你应该了解的 10 个令人惊叹的机器学习可视化

> 原文：[`www.kdnuggets.com/2022/11/10-amazing-machine-learning-visualizations-know-2023.html`](https://www.kdnuggets.com/2022/11/10-amazing-machine-learning-visualizations-know-2023.html)

![2023 年你应该了解的 10 个令人惊叹的机器学习可视化](img/94707c4df6887d0528c7e4b23ee48ee8.png)

图片由编辑提供

数据可视化在机器学习中发挥着重要作用。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

数据可视化在机器学习中的使用场景包括：

+   超参数调优

+   模型性能评估

+   验证模型假设

+   查找异常值

+   选择最重要的特征

+   识别特征之间的模式和关联

直接与上述机器学习关键点相关的可视化称为 ***机器学习可视化***。

创建机器学习可视化有时是一个复杂的过程，因为即使在 Python 中也需要编写大量代码。但幸好，有了 Python 的开源 **Yellowbrick** 库，即使是复杂的机器学习可视化也可以用更少的代码创建。该库扩展了 Scikit-learn API，并提供了 Scikit-learn 未提供的高级可视化诊断功能。

今天，我将详细讨论以下几种机器学习可视化类型、它们的使用场景以及 Yellowbrick 的实现。

# Yellowbrick — 快速入门

## 安装

可以通过运行以下命令之一来安装 Yellowbrick。

+   **pip** 包安装器：

```py
pip install yellowbrick
```

+   **conda** 包安装器：

```py
conda install -c districtdatalabs yellowbrick
```

## 使用 Yellowbrick

Yellowbrick 可视化工具具有类似 Scikit-learn 的语法。可视化工具是一个从数据中学习以生成可视化的对象。它通常与 Scikit-learn 估计器一起使用。要训练可视化工具，我们调用它的 fit() 方法。

## 保存图形

要保存使用 Yellowbrick 可视化工具创建的图形，我们调用 show() 方法如下。这将把图形保存为磁盘上的 PNG 文件。

```py
visualizer.show(outpath="name_of_the_plot.png")
```

# 1\. 主成分图

## 使用

主成分图通过 2D 或 3D 散点图可视化高维数据。因此，这种图对于识别高维数据中的重要模式极为有用。

## Yellowbrick 实现

使用传统方法创建此图复杂且耗时。我们需要先对数据集应用 PCA，然后使用 matplotlib 库创建散点图。

相反，我们可以使用 Yellowbrick 的 PCA 可视化器类来实现相同的功能。它利用主成分分析方法，减少数据集的维度，并用 2 或 3 行代码创建散点图！我们只需要在 PCA() 类中指定一些关键字参数即可。

让我们通过一个例子进一步理解这一点。在这里，我们使用*breast_cancer*数据集（请参见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#6fde)，该数据集具有 30 个特征和 569 个样本，分为两个类别（*Malignant*和*Benign*）。由于数据的高维度（30 个特征），除非对数据集应用 PCA，否则无法在二维或三维散点图中绘制原始数据。

以下代码解释了我们如何利用 Yellowbrick 的 PCA 可视化器创建一个 30 维数据集的二维散点图。

作者提供的代码

![2023 年你应该知道的 10 个惊人的机器学习可视化](img/045f880cb835bfa38de67690ac085953.png)

主成分图 — 2D|作者提供的图像

我们还可以通过在 PCA() 类中设置`projection=3`来创建三维散点图。

作者提供的代码

![2023 年你应该知道的 10 个惊人的机器学习可视化](img/f7228111c3dc766f74a9566110390476.png)

主成分图 — 3D|作者提供的图像

PCA 可视化器的最重要参数包括：

+   **scale:** bool，默认为`True`。这表示数据是否应该进行缩放。我们应该在运行 PCA 之前对数据进行缩放。了解更多信息[这里](https://rukshanpramoditha.medium.com/principal-component-analysis-18-questions-answered-4abd72041ccd#f853)。

+   **projection:** int，默认为 2。当`projection=2`时，创建一个二维散点图。当`projection=3`时，创建一个三维散点图。

+   **classes:** list，默认为`None`。这表示 y 中每个类别的类标签。类别名称将作为图例的标签。

# 2\. 验证曲线

## 使用方法

验证曲线绘制了*单个*超参数对训练集和验证集的影响。通过查看曲线，我们可以确定模型在指定超参数值下的过拟合、欠拟合和适中情况。当需要同时调整多个超参数时，不能使用验证曲线。此时，可以使用网格搜索或随机搜索。

## Yellowbrick 实现

使用传统方法创建验证曲线复杂且耗时。相反，我们可以使用 Yellowbrick 的 ValidationCurve 可视化器。

在 Yellowbrick 中绘制验证曲线时，我们将使用相同的*breast_cancer*数据集（请参见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#6fde)）。我们将绘制**max_depth**超参数在随机森林模型中的影响。

以下代码解释了如何利用 Yellowbrick 的 ValidationCurve 可视化工具使用*breast_cancer*数据集创建验证曲线。

代码由作者提供

![2023 年你应该知道的 10 个惊人的机器学习可视化](img/150429e016c8884c12190c126762d4e9.png)

验证曲线|图片由作者提供

模型在**max_depth**值为 6 之后开始过拟合。当`max_depth=6`时，模型很好地拟合了训练数据，并且在新的未见数据上也具有良好的泛化能力。

ValidationCurve 可视化工具的最重要参数包括：

+   **estimator:** 这可以是任何 Scikit-learn 机器学习模型，如决策树、随机森林、支持向量机等。

+   **param_name:** 这是我们希望监控的超参数的名称。

+   **param_range:** 这包括*param_name*的可能值。

+   **cv:** int，定义交叉验证的折数。

+   **scoring:** 字符串，包含模型的评分方法。对于分类，*accuracy* 是首选。

# 3\. 学习曲线

## 用法

学习曲线绘制了训练和验证错误或准确率与迭代次数或训练实例数的关系。你可能认为学习曲线和验证曲线看起来一样，但学习曲线的 x 轴绘制了迭代次数，而验证曲线的 x 轴绘制了超参数的值。

学习曲线的用途包括：

+   学习曲线用于检测模型的*欠拟合*、*过拟合*和*正好合适*条件。

+   学习曲线用于识别在寻找神经网络或机器学习模型的最佳学习率时的*慢收敛*、*振荡*、*振荡且发散*以及*适当收敛*场景。

+   学习曲线用于查看我们的模型从增加更多训练数据中获益多少。以这种方式使用时，x 轴显示训练实例的数量。

## Yellowbrick 实现

使用传统方法创建学习曲线复杂且耗时。相反，我们可以使用 Yellowbrick 的 LearningCurve 可视化工具。

为了在 Yellowbrick 中绘制学习曲线，我们将使用相同的*breast_cancer*数据集构建一个支持向量分类器（见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#6fde)）。

以下代码解释了如何利用 Yellowbrick 的 LearningCurve 可视化工具使用*breast_cancer*数据集创建验证曲线。

代码由作者提供

![2023 年你应该知道的 10 个惊人的机器学习可视化](img/3d6f70a3ea545013b87705ed288eedf1.png)

学习曲线|图片由作者提供

模型从增加更多训练实例中不会获得收益。模型已经用 569 个训练实例进行了训练。175 个训练实例之后，验证准确率没有改善。

LearningCurve 可视化工具的最重要参数包括：

+   **estimator:** 这可以是任何 Scikit-learn 机器学习模型，例如决策树、随机森林、支持向量机等。

+   **cv:** int，定义交叉验证的折数。

+   **scoring:** 字符串，包含模型的评分方法。对于分类任务，*准确率* 是首选。

# 4\. 肘部图

## 使用方法

肘部图用于选择 K-Means 聚类中的最佳簇数。模型在肘部图中肘部出现的位置拟合最佳。肘部是图表上的拐点。

## Yellowbrick 实现

使用传统方法创建肘部图复杂且耗时。相反，我们可以使用 Yellowbrick 的 KElbowVisualizer。

要在 Yellowbrick 中绘制学习曲线，我们将使用 *iris* 数据集构建一个 K-Means 聚类模型（见文末的 [Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#2bc7)）。

以下代码解释了我们如何利用 Yellowbrick 的 KElbowVisualizer 使用 *iris* 数据集创建肘部图。

作者提供的代码

![2023 年你应该知道的 10 个令人惊叹的机器学习可视化](img/3cde2829e689c0b5be926da3a47f32b6.png)

肘部图|作者提供的图片

*肘部* 发生在 k=4（用虚线标注）。图表显示模型的最佳簇数是 4。换句话说，模型在 4 个簇时拟合得很好。

KElbowVisualizer 的最重要参数包括：

+   **estimator:** K-Means 模型实例

+   **k:** int 或元组。如果是整数，它将计算范围为 (2, k) 内的簇的分数。如果是元组，它将计算给定范围内的簇的分数，例如 (3, 11)。

# 5\. 轮廓图

## 使用方法

轮廓图用于选择 K-Means 聚类中的最佳簇数，同时也可以检测簇的不平衡。与肘部图相比，这种图提供了更准确的结果。

## Yellowbrick 实现

使用传统方法创建肘部图复杂且耗时。相反，我们可以使用 Yellowbrick 的 SilhouetteVisualizer。

要在 Yellowbrick 中创建轮廓图，我们将使用 *iris* 数据集构建一个 K-Means 聚类模型（见文末的 [Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#2bc7)）。

以下代码块解释了我们如何利用 Yellowbrick 的 SilhouetteVisualizer 使用 *iris* 数据集和不同的 k（簇数）值创建轮廓图。

**k=2**

作者提供的代码

![2023 年你应该知道的 10 个令人惊叹的机器学习可视化](img/f703ccc9d7dcd76127416d55263c8354.png)

具有 2 个簇的轮廓图（k=2）|作者提供的图片

通过改变 KMeans() 类中的簇数，我们可以在不同的时间执行上述代码，以创建当 k=3、k=4 和 k=5 时的轮廓图。

**k=3**

![2023 年你应该知道的 10 种惊人的机器学习可视化](img/8903414955555e93c6cee0a303ae8a93.png)

|具有 3 个聚类（k=3）的轮廓图|作者提供的图像

**k=4**

![2023 年你应该知道的 10 种惊人的机器学习可视化](img/dda9337e16726fee4d16d7da155015ce.png)

具有 4 个聚类（k=4）的轮廓图|作者提供的图像

**k=5**

![2023 年你应该知道的 10 种惊人的机器学习可视化](img/31a7712a95ab447641da039dfce5e61d.png)

具有 4 个聚类（k=5）的轮廓图|作者提供的图像

轮廓图包含每个聚类一个刀形。每个刀形由表示聚类中所有数据点的条形创建。因此，刀形的宽度表示聚类中所有实例的数量。条形的长度表示每个实例的轮廓系数。虚线表示轮廓评分 — 来源：[*实践 K-Means 聚类*](https://medium.com/mlearning-ai/k-means-clustering-with-scikit-learn-e2af706450e4)（由我撰写）。

刀形宽度大致相等的图告诉我们聚类是平衡的，并且每个聚类中的实例数量大致相同 — 这是 K-Means 聚类中最重要的假设之一。

当刀形图中的条形延伸到虚线时，聚类被良好分隔 — 这是 K-Means 聚类中的另一个重要假设。

当 k=3 时，聚类平衡且良好分隔。因此，在我们的示例中，最佳的聚类数量是 3。

SilhouetteVisualizer 的最重要参数包括：

+   **估计器：**K-Means 模型实例

+   **颜色：** 字符串，用于每个刀形的颜色集合。‘yellowbrick’或 Matplotlib 的颜色映射字符串之一，如‘Accent’，‘Set1’，等。

# 6\. 类别不平衡图

## 用法

类别不平衡图检测分类数据集中目标列中的类别不平衡。

类别不平衡发生在一个类别的实例数量显著多于另一个类别时。例如，涉及垃圾邮件检测的数据集中，“非垃圾邮件”类别有 9900 个实例，而“垃圾邮件”类别只有 100 个实例。模型将无法捕捉到少数类别（*垃圾邮件*类别）。因此，当发生类别不平衡时，模型在预测少数类别时将不准确 — 来源：[*揭示机器学习和深度学习中的 20 个常见错误*](https://rukshanpramoditha.medium.com/top-20-machine-learning-and-deep-learning-mistakes-that-secretly-happen-behind-the-scenes-e211e056c867)（由我撰写）。

## Yellowbrick 实现

使用传统方法创建类别不平衡图复杂且耗时。相反，我们可以使用 Yellowbrick 的 ClassBalance 可视化工具。

要在 Yellowbrick 中绘制类别不平衡图，我们将使用*breast_cancer*数据集（分类数据集，详见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#6fde)）。

以下代码解释了我们如何利用 Yellowbrick 的 ClassBalance 可视化工具使用*breast_cancer*数据集创建类别不平衡图。

代码由作者提供

![2023 年你应该了解的 10 个惊人机器学习可视化](img/bef8df00aabfd074c350c6f16747c0a6.png)

类别不平衡图|作者提供的图像

在*恶性*类别中有超过 200 个实例，在*良性*类别中有超过 350 个实例。因此，尽管这些实例在两个类别之间分布不均，但我们在这里看不到明显的类别不平衡。

ClassBalance 可视化工具的最重要参数包括：

+   **labels:** 列表，目标列中唯一类别的名称。

# 7\. 残差图

## 用法

线性回归中的残差图用于通过分析回归模型中误差的方差来确定残差（观察值-预测值）是否不相关（独立）。

残差图是通过绘制残差与预测值的关系来创建的。如果预测值与残差之间存在任何模式，表明拟合的回归模型不完美。如果点在 x 轴周围随机分布，说明回归模型与数据拟合良好。

## Yellowbrick 实现

使用传统方法创建残差图复杂且耗时。相反，我们可以使用 Yellowbrick 的 ResidualsPlot 可视化工具。

要在 Yellowbrick 中绘制残差图，我们将使用*Advertising*([Advertising.csv](https://drive.google.com/file/d/1-1MgAOHbTI5DreeXObN6KLcSka6LS9G-/view?usp=share_link)，详见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#8bd8)）数据集。

以下代码解释了我们如何利用 Yellowbrick 的 ResidualsPlot 可视化工具使用*Advertising*数据集创建残差图。

代码由作者提供

![2023 年你应该了解的 10 个惊人机器学习可视化](img/43a4c0ed19f3dfa9bb5268674da76344.png)

残差图|作者提供的图像

我们可以清楚地看到残差图中预测值与残差之间存在某种非线性模式。拟合的回归模型不完美，但足够好。

ResidualsPlot 可视化工具的最重要参数包括：

+   **estimator:** 这可以是任何 Scikit-learn 回归器。

+   **hist:** 布尔值，默认为`True`。是否绘制残差的直方图，用于检查另一个假设——残差大致呈正态分布，均值为 0，标准差固定。

# 8\. 预测误差图

## 用法

预测误差图在回归分析中是一种图形方法，用于评估回归模型。

预测误差图是通过将预测值与实际目标值进行比较来创建的。

如果模型的预测非常准确，点应该落在 45 度线上的。如果不准确，点则会分散在这条线周围。

## Yellowbrick 实现

使用传统方法创建预测误差图既复杂又耗时。相反，我们可以使用 Yellowbrick 的 PredictionError 可视化工具。

要在 Yellowbrick 中绘制预测误差图，我们将使用*Advertising*（[Advertising.csv](https://drive.google.com/file/d/1-1MgAOHbTI5DreeXObN6KLcSka6LS9G-/view?usp=share_link)，详见末尾的[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#8bd8)）数据集。

以下代码解释了如何利用 Yellowbrick 的 PredictionError 可视化工具来创建一个使用*Advertising*数据集的残差图。

作者代码

![2023 年你应该知道的 10 个令人惊叹的机器学习可视化](img/975ad64ede24ec65172ffdc5e307d90c.png)

预测误差图|作者图像

点并没有完全落在 45 度线上的，但模型足够好。

PredictionError 可视化工具的最重要参数包括：

+   **estimator: ** 这可以是任何 Scikit-learn 回归器。

+   **identity: ** 布尔值，默认`True`。是否绘制 45 度线。

# 9\. 库克距离图

## 使用方法

库克距离衡量了实例对线性回归的影响。具有大影响的实例被视为异常值。具有大量异常值的数据集在没有预处理的情况下不适合进行线性回归。简而言之，库克距离图用于检测数据集中的异常值。

## Yellowbrick 实现

使用传统方法创建库克距离图既复杂又耗时。相反，我们可以使用 Yellowbrick 的 CooksDistance 可视化工具。

要在 Yellowbrick 中绘制库克距离图，我们将使用*Advertising*（[Advertising.csv](https://drive.google.com/file/d/1-1MgAOHbTI5DreeXObN6KLcSka6LS9G-/view?usp=share_link)，详见末尾的[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#8bd8)）数据集。

以下代码解释了如何利用 Yellowbrick 的 CooksDistance 可视化工具来创建一个使用*Advertising*数据集的库克距离图。

作者代码

![2023 年你应该知道的 10 个令人惊叹的机器学习可视化](img/61959f47735e7883841325fbb5347350.png)

库克距离图|作者图像

有一些观察点延伸出了阈值（水平红色）线。这些是异常值。因此，我们应该在构建回归模型之前对数据进行准备。

CooksDistance 可视化工具的最重要参数包括：

+   **draw_threshold: ** 布尔值，默认`True`。是否绘制阈值线。

# 10\. 特征重要性图

## 使用方法

特征重要性图用于选择产生 ML 模型所需的最小重要特征。由于并非所有特征对模型的贡献相同，我们可以从模型中删除不重要的特征。这将减少模型的复杂性。简单的模型容易训练和解释。

特征重要性图可视化每个特征的相对重要性。

## Yellowbrick 实现

使用传统方法创建特征重要性图复杂且耗时。相反，我们可以使用 Yellowbrick 的 FeatureImportances 可视化工具。

要在 Yellowbrick 中绘制特征重要性图，我们将使用*breast_cancer*数据集（见[Citation](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582#6fde)），该数据集包含 30 个特征。

以下代码解释了如何利用 Yellowbrick 的 FeatureImportances 可视化工具使用 *breast_cancer* 数据集创建特征重要性图。

作者提供的代码

![2023 年你应该知道的 10 种惊人的机器学习可视化](img/c9298cfb81fbeec3ff251f21569db3b8.png)

特征重要性图 | 作者提供的图片

数据集中的所有 30 个特征对模型的贡献并不相同。我们可以从数据集中删除条形较小的特征，并使用选定的特征重新拟合模型。

特征重要性可视化工具的最重要参数包括：

+   **estimator：** 任何支持 `feature_importances_` 属性或 `coef_` 属性的 Scikit-learn 估算器。

+   **relative：** bool，默认为 `True`。是否将相对重要性绘制为百分比。如果为 `False`，则显示特征重要性的原始数值分数。

+   **absolute：** bool，默认为 `False`。是否仅考虑系数的大小，忽略负号。

# ML 可视化工具的用途总结

1.  **主成分图：** *PCA()*，用法 — 将高维数据可视化为 2D 或 3D 散点图，可用于识别高维数据中的重要模式。

1.  **验证曲线：** *ValidationCurve()*，用法 — 绘制 *单一* 超参数对训练集和验证集的影响。

1.  **学习曲线：** *LearningCurve()*，用法 — 检测模型的 *欠拟合*、*过拟合* 和 *适中* 条件，识别 *收敛缓慢*、*震荡*、*震荡并发散* 和 *适当收敛* 情景，显示我们的模型从增加更多训练数据中获益多少。

1.  **肘部图：** *KElbowVisualizer()*，用法 — 选择 K-Means 聚类中的最佳簇数。

1.  **轮廓图：** *SilhouetteVisualizer()*，用法 — 选择 K-Means 聚类中的最佳簇数，检测 K-Means 聚类中的簇不平衡。

1.  **类别不平衡图：** *ClassBalance()*，用法 — 检测分类数据集中目标列的类别不平衡。

1.  **Residuals Plot:** *ResidualsPlot()*, 用法 — 通过分析回归模型中误差的方差，确定残差（观察值-预测值）是否不相关（独立）。

1.  **Prediction Error Plot:** *PredictionError()*, 用法 — 用于评估回归模型的图形方法。

1.  **Cook's Distance Plot:** *CooksDistance()*, 用法 — 基于实例的 Cook 距离检测数据集中的异常值。

1.  **Feature Importances Plot:** *FeatureImportances()*, 用法 — 根据每个特征的相对重要性选择所需的最少重要特征，以生成一个机器学习模型。

这就是今天帖子的结束。

> 如果你有任何问题或反馈，请告诉我。

## 乳腺癌数据集信息

+   **Citation: **Dua, D. 和 Graff, C.（2019）。UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]。加州欧文：加州大学信息与计算机科学学院。

+   **Source: **[`archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic)`](https://archive.ics.uci.edu/ml/datasets/breast+cancer+wisconsin+(diagnostic))

+   **License: ***威廉·H·沃尔伯格博士*（普通外科系）。

    威斯康星大学的*W. Nick Street*（计算机科学系）。

    威斯康星大学的*Olvi L. Mangasarian*（计算机科学系，威斯康星大学）拥有此数据集的版权。Nick Street 在*Creative Commons Attribution 4.0 International License*（[**CC BY 4.0**](https://creativecommons.org/licenses/by/4.0/)）下将此数据集捐赠给公众。你可以在[这里](https://rukshanpramoditha.medium.com/dataset-and-software-license-types-you-need-to-consider-d20965ca43dc#6ade)了解更多关于不同数据集许可证类型的信息。

## 鸢尾花数据集信息

+   **Citation: **Dua, D. 和 Graff, C.（2019）。UCI Machine Learning Repository [http://archive.ics.uci.edu/ml]。加州欧文：加州大学信息与计算机科学学院。

+   **Source: **[`archive.ics.uci.edu/ml/datasets/iris`](https://archive.ics.uci.edu/ml/datasets/iris)

+   **License: ***R.A.费舍尔*拥有此数据集的版权。Michael Marshall 在*Creative Commons Public Domain Dedication License*（[**CC0**](https://creativecommons.org/share-your-work/public-domain/cc0)）下将此数据集捐赠给公众。你可以在[这里](https://rukshanpramoditha.medium.com/dataset-and-software-license-types-you-need-to-consider-d20965ca43dc#6ade)了解更多关于不同数据集许可证类型的信息。

## 广告数据集信息

+   **Source:** [`www.kaggle.com/datasets/sazid28/advertising.csv`](https://www.kaggle.com/datasets/sazid28/advertising.csv)

+   **License: **此数据集在*Creative Commons Public Domain Dedication License*（[**CC0**](https://creativecommons.org/share-your-work/public-domain/cc0)）下公开提供。你可以在[这里](https://rukshanpramoditha.medium.com/dataset-and-software-license-types-you-need-to-consider-d20965ca43dc#6ade)了解更多关于不同数据集许可证类型的信息。

## 参考资料

+   [`www.scikit-yb.org/en/latest/`](https://www.scikit-yb.org/en/latest/)

+   [`www.scikit-yb.org/en/latest/quickstart.html`](https://www.scikit-yb.org/en/latest/quickstart.html)

+   [`www.scikit-yb.org/en/latest/api/index.html`](https://www.scikit-yb.org/en/latest/api/index.html)

**[Rukshan Pramoditha](https://www.linkedin.com/in/rukshan-manorathna-700a3916b/)** ([@rukshanpramoditha](https://rukshanpramoditha.medium.com/)) 拥有工业统计学的学士学位。自 2020 年以来支持数据科学教育。Medium 上排名前 50 的数据科学/人工智能/机器学习作家。他撰写过关于数据科学、机器学习、深度学习、神经网络、Python 和数据分析的文章。他有将复杂话题转化为有价值且易于理解内容的卓越记录。

[原文](https://towardsdatascience.com/10-amazing-machine-learning-visualizations-you-should-know-in-2023-528282940582)。经许可转载。

### 更多相关主题

+   [使用 Python 图形库制作惊人的可视化](https://www.kdnuggets.com/2022/12/make-amazing-visualizations-python-graph-gallery.html)

+   [2023 年你需要尝试的 5 个惊人且免费的 LLMs 游乐场](https://www.kdnuggets.com/5-amazing-free-llms-playgrounds-you-need-to-try-in-2023)

+   [每个机器学习工程师应掌握的 5 项机器学习技能…](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [KDnuggets 新闻，4 月 13 日：数据科学家应了解的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [2023 年你应考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [2023 年成为数据科学家所需掌握的 19 项技能](https://www.kdnuggets.com/2023/04/top-19-skills-need-know-2023-data-scientist.html)
