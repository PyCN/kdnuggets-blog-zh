# 机器学习中的主要监督学习算法

> 原文：[`www.kdnuggets.com/2022/06/primary-supervised-learning-algorithms-used-machine-learning.html`](https://www.kdnuggets.com/2022/06/primary-supervised-learning-algorithms-used-machine-learning.html)

![机器学习中的主要监督学习算法](img/58ae394e18949785608d8320685fe8db.png)

# 什么是监督学习？

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

[监督学习](https://www.exxactcorp.com/blog/Deep-Learning/supervised-vs-unsupervised-machine-learning)是机器学习的一个子集，其中机器学习模型在已标记（输入）数据上进行训练。结果是，监督模型能够尽可能准确地预测进一步的结果（输出）。

监督学习背后的概念可以通过实际场景来解释，例如教师第一次辅导孩子学习新主题。

为了简化，假设教师想要教孩子成功识别猫和狗的图像。教师将通过不断展示猫或狗的图像，并告知孩子这些图像是狗还是猫，来开始辅导过程。

猫和狗的图像可以被视为已标记的数据，因为孩子/机器学习模型在训练过程中将被告知哪些数据属于哪个类别。

监督学习用于什么？监督学习可以用于回归和分类问题。**分类**模型允许算法确定给定数据属于哪个组。例子包括真假、狗/猫等。

虽然**回归**模型能够根据之前的数据预测数值，例如预测某员工的薪资或房地产的售价。

在本教程中，我们将列出一些最常见的监督学习算法，并提供这些算法的实际教程。

请注意，这些教程使用 Python 编写。所有使用的数据集都可以在 [Kaggle](https://www.kaggle.com/) 上找到，因此无需下载任何大型数据集。

# 线性回归

线性回归是一种监督学习算法，根据给定的输入值预测输出值。线性回归用于目标（输出）变量返回连续值的情况。

线性算法主要有两种类型，***简单***和***多重***线性回归。

简单线性回归仅使用一个独立（输入）变量。例如，根据一个孩子的身高预测其年龄。

另一方面，多重线性回归可以使用多个独立变量来预测最终结果。例如，根据房产的位置、大小、需求等预测房地产价格。

***以下是线性回归公式***

![机器学习中的主要监督学习算法](img/d3a79b226a6e048cd2774fe9e919f155.png)

在我们的 Python 示例中，我们将使用线性回归根据给定的 x 值预测 y 值。

我们给定的数据集将仅包含两列；x 和 y。注意，y 结果将返回连续值。

要查找使用的数据集，请查看[Kaggle 上的随机线性回归数据集](https://www.kaggle.com/datasets/andonians/random-linear-regression)。以下是该数据集的截图。

![机器学习中的主要监督学习算法](img/280d2410807a2293b81ba02ad2c3b310.png)

# Python 中的线性回归模型示例

## 1\. 导入必要的库

```py
import numpy as np 

    import pandas as pd

    import matplotlib.pyplot as plt

    import seaborn as sns

    from sklearn import linear_model

    from sklearn.model_selection import train_test_split

    import os
```

## 2\. 读取和采样我们的数据集

为了简化数据集，我们取了 50 行数据的样本，并将数据值四舍五入到 2 位有效数字。

注意，在完成此步骤之前，您应导入给定的数据集。

```py
df = pd.read_csv("../input/random-linear-regression/train.csv")

    df=df.sample(50)

    df=round(df,2)
```

## 3\. 过滤空值和无限值

如果我们的数据集中包含空值和无限值，可能会出现不必要的错误。因此，我们将使用 clean_dataset 函数清除数据集中这些值。

```py
def clean_dataset(df):

       assert isinstance(df, pd.DataFrame), "df needs to be a pd.DataFrame"

       df.dropna(inplace=True)

       indices_to_keep = ~df.isin([np.nan, np.inf, -np.inf]).any(1)

       return df[indices_to_keep].astype(np.float64)

    df=clean_dataset(df)
```

## 4\. 选择我们的依赖值和独立值

注意，我们将数据转换为 DataFrame 格式。[dataframe](https://www.geeksforgeeks.org/python-pandas-dataframe/) 数据类型是一个二维结构，将数据排列成行和列。

## 5\. 拆分数据集

我们将把数据集分为训练集和测试集。我们选择测试数据集的大小为总数据集的 20%。

注意，通过设置 random_state=1，每次模型运行时将发生相同的数据分割，从而得到完全相同的训练集和测试集。

这在你想进一步调整模型时可能会很有用。

```py
x_train,  x_test, y_train, y_test = train_test_split(X, Y, test_size=0.2, random_state=1)
```

## 6\. 建立线性回归模型

使用导入的线性回归模型，我们可以在模型中自由使用线性回归算法，而无需考虑获得的 x 和 y 训练变量。

```py
lm=linear_model.LinearRegression()

    lm.fit(x_train,y_train)
```

## 7\. 以散点图方式绘制我们的数据

```py
df.plot(kind="scatter", x="x", y="y")
```

## 8\. 绘制我们的线性回归线

```py
plt.plot(X,lm.predict(X), color="red")
```

![机器学习中的主要监督学习算法](img/44a460808ee0fdd55b638530812db36f.png)

蓝色点表示我们的数据点，而红色线是由模型绘制的最佳拟合线。线性模型算法将始终尝试绘制最佳拟合线，以尽可能准确地预测结果。

# 逻辑回归

类似于线性回归，逻辑回归用于根据定义的输入变量预测输出值，但这两种算法的主要区别在于逻辑回归算法的输出应为分类（离散）变量。

对于我们的 Python 示例，我们将使用逻辑回归将给定的花分类为两种不同的类别/物种。我们的数据集将包括不同花卉物种的多个特征。

我们模型的主要目的是将给定的花识别为**鸢尾花-石斛（Iris-setosa）**、**鸢尾花-变色（Iris-versicolor）**或**鸢尾花-维吉尼卡（Iris-virginica）**。

要查找使用的数据集，请查看 Kaggle 上的[鸢尾数据集？—？逻辑回归](https://www.kaggle.com/datasets/tanyaganesan/iris-dataset-logistic-regression)。下面是给定数据集的截图。

![机器学习中的主要监督学习算法](img/cc63d26f3c6b156d835fe84606b06bcf.png)

# 使用 Python 的逻辑回归模型示例

## 1\. 导入必要的库

```py
import numpy as np 

    import pandas as pd 

    from sklearn.model_selection import train_test_split

    import warnings

    warnings.filterwarnings('ignore')
```

## 2\. 导入数据集

```py
data = pd.read_csv('../input/iris-dataset-logistic-regression/iris.csv')
```

## 3\. 选择自变量和因变量

对于我们的自变量（x），我们将包括所有可用的列，除了类型列。至于我们的因变量（y），我们只会包括类型列。

```py
X = data[['x0','x1','x2','x3','x4']]

    y = data[['type']]
```

## 4\. 划分数据集

我们将把数据集分为两部分，80%用于训练数据集，20%用于测试数据集。

```py
X_train,X_test,y_train,y_test = train_test_split(X,y, test_size=0.2, random_state=1)
```

## 5\. 运行逻辑回归模型

我们将从 linear_model 库中导入整个逻辑回归算法。然后，我们可以将 X 和 y 的训练数据适配到逻辑模型中。

```py
from sklearn.linear_model import LogisticRegression

    model = LogisticRegression(random_state = 0)

    model.fit(X_train, y_train)
```

## 6\. 评估模型的性能

```py
print(lm.score(x_test, y_test))
```

返回的值为***0.9845128775509371***，这表明我们的模型表现优异。

请注意，随着测试得分的提高，模型的性能也会提高。

## 7\. 绘制图表

```py
import matplotlib.pyplot as plt

    %matplotlib inline

    plt.plot(range(len(X_test)), pred,'o',c='r')
```

**输出图表：**

![机器学习中的主要监督学习算法](img/6cfa9d706714e1571d57e5137efcef0e.png)

在逻辑图中，红色点表示给定的数据点。点被清晰地分为 3 个类别：维吉尼卡、变色和石斛花物种。

使用这种技术，逻辑回归模型可以根据花卉在图上的位置轻松地对花的类型进行分类。

### 支持向量机

支持向量机（SVM）算法是由弗拉基米尔· Vapnik 创造的另一种著名的监督机器学习模型，能够处理分类和回归问题。尽管如此，它更常用于分类问题而非回归问题。

SVM 算法能够将给定的数据点分成不同的组。这是通过让我们的算法绘制数据，然后画出最合适的线来将数据分成多个类别来完成的。

如下图所示，绘制的线将数据集完美地分成了两个不同的组，蓝色和绿色。

![机器学习中使用的主要监督学习算法](img/56b09dbf86a5394d6e3c16ae464a18d2.png)

SVM 模型可以根据绘图的维度绘制线条或超平面。线条只能处理二维数据集，即只有两列的数据集。

由于可能会使用多个特征来预测给定的数据集，因此需要更高的维度。当我们的数据集结果超过 2 维时，支持向量机模型会绘制出更合适的超平面。

在我们的支持向量机 Python 示例中，我们将对 3 种不同的花卉类型进行物种分类。我们的自变量将包括某种花卉的所有给定特征，而因变量将是该花卉所属的指定物种。

我们的花卉物种包括**Iris-setosa**、**Iris-versicolor**和**Iris-virginica**。

要查找使用的数据集，请查看[Kaggle 上的 Iris Flower Dataset](https://www.kaggle.com/datasets/arshid/iris-flower-dataset)。下面是给定数据集的截图。

![机器学习中使用的主要监督学习算法](img/14cd824baf785e990fae8fd11c793f63.png)

# 使用 Python 的支持向量机模型示例

## 1\. 导入必要的库

```py
import numpy as np

    import pandas as pd

    from sklearn.model_selection import train_test_split

    from sklearn.datasets import load_iris
```

## 2\. 阅读给定的数据集

注意，在进行此步骤之前，您应该导入给定的数据集。

```py
data = pd.read_csv(‘../input/iris-flower-dataset/IRIS.csv’)
```

## 3\. 将数据列拆分为因变量和自变量

我们将 X 值作为自变量，其中包含除物种列之外的所有列。

对于我们的因变量，y 变量将仅包含物种列，这是我们的模型应该预测的。

```py
X = data.drop(‘species’, axis=1)

    y = data[‘species’]
```

## 4\. 将数据集拆分为训练数据集和测试数据集

我们将数据集分为两部分，其中 80%的数据用于训练数据集，20%的数据用于测试数据集。

```py
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1)
```

## 5\. 导入 SVM 并运行模型

我们整体导入了支持向量机算法。然后我们使用从前面步骤中获得的 X 和 y 训练数据集运行了它。

```py
from sklearn.svm import SVC

    model = SVC( )

    model.fit(X_train, y_train)
```

## 6\. 测试我们模型的性能

```py
model.score(X_test, y_test)
```

为了评估我们模型的性能，我们将使用评分函数。在其中，我们将把在第四步中创建的 X 和 y 测试值传递给评分方法。

返回的值是***0.9666666666667***，这表明我们的模型表现良好。

注意，测试分数的增加也意味着模型性能的提升。

# 其他流行的监督机器学习算法

尽管线性、逻辑回归和支持向量机算法相当可靠，我们还会提到一些值得一提的监督机器学习算法。

## 1\. 决策树

![机器学习中使用的主要监督学习算法](img/3f76edeac1c2f31994a01fa2f72eb6ce.png)

[决策树算法](https://deepai.org/machine-learning-glossary-and-terms/decision-tree) 是一种监督机器学习模型，利用类似树的结构进行决策。决策树通常用于分类问题，在这种问题中，模型可以决定数据集中给定项所属的类别。

请注意，使用的树形结构是倒置树。

## 2\. 随机森林

![机器学习中使用的主要监督学习算法](img/4d2ed484eab865d3bc8fcfd3ba800d31.png)

被认为是一种更复杂的算法，随机森林算法通过构建大量决策树来实现其最终目标。

意味着同时构建多个决策树，每个树返回自己的结果，然后将结果结合以获得更好的结果。

对于分类问题，随机森林模型会生成多个决策树，并根据大多数树预测的分类组来分类给定对象。

这样，模型可以修正由单棵树造成的任何过拟合。虽然随机森林算法也可以用于回归，但可能会导致较差的结果。

## 3\. k-最近邻

![机器学习中使用的主要监督学习算法](img/3cfa3ab71b28c7219079e8fd385acaa7.png)

k-最近邻（KNN）算法是一种监督机器学习方法，将所有给定数据分组为不同的组。

这种分组是基于不同个体之间的共同特征。KNN 算法可以用于分类和回归问题。

KNN 问题的一个例子是将动物图像分类为不同的组。

# 结论

回顾我们在本文中学到的内容，我们首先定义了监督机器学习以及它可以解决的两种问题类型。

接下来我们解释了分类和回归问题，并给出了每种问题类型的输出数据类型的一些示例。

然后我们解释了什么是线性回归，它是如何工作的，并提供了一个用 Python 编写的具体例子，该例子试图根据独立的 X 变量预测 Y 值。

我们还解释了一个与线性回归模型类似的模型，即逻辑回归模型。我们说明了什么是逻辑回归，并给出了一个分类模型的例子，该模型将给定的图像分类为特定的花卉品种。

对于我们的最后一个算法，我们使用了支持向量机算法，我们也用它来预测三种不同花卉的花卉品种。

在我们文章的结尾，我们简要介绍了其他知名的监督机器学习算法，如决策树、随机森林和 K 最近邻算法。

无论你是出于学校、工作还是娱乐阅读这篇文章，我们认为从这些基本算法入手可能是开始你新的机器学习热情的好方法。

如果你感兴趣并希望了解更多关于机器学习的世界，我们建议你进一步了解这些算法的工作原理以及如何调整这些模型以进一步提升其性能。别忘了，还有许多其他监督算法等着你去学习。

**[Kevin Vu](https://www.kdnuggets.com/author/kevin-vu)** 负责管理 Exxact Corp 博客，并与许多才华横溢的作者合作，这些作者撰写有关深度学习的不同方面的文章。

[原文](https://www.exxactcorp.com/blog/Deep-Learning/primary-supervised-learning-algorithms-used-in-machine-learning)。经许可转载。

### 更多相关话题

+   [KDnuggets 新闻，6 月 22 日：主要监督学习算法…](https://www.kdnuggets.com/2022/n25.html)

+   [理解监督学习：理论与概述](https://www.kdnuggets.com/understanding-supervised-learning-theory-and-overview)

+   [动手实践监督学习：线性回归](https://www.kdnuggets.com/handson-with-supervised-learning-linear-regression)

+   [DINOv2：Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [KDnuggets 新闻，8 月 3 日：10 个最常用的 Tableau 函数 • 是…](https://www.kdnuggets.com/2022/n31.html)

+   [10 个最常用的 Tableau 函数](https://www.kdnuggets.com/2022/08/10-used-tableau-functions.html)
