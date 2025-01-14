# 最佳稀疏数据机器学习模型

> 原文：[`www.kdnuggets.com/2023/04/best-machine-learning-model-sparse-data.html`](https://www.kdnuggets.com/2023/04/best-machine-learning-model-sparse-data.html)

![最佳稀疏数据机器学习模型](img/671203cd62e0f4332672e22ce8527e1a.png)

作者提供的图片

稀疏数据是指特征值为零的数据集。这会在不同领域造成问题，特别是在机器学习中。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

稀疏数据可能由于不适当的特征工程方法而发生。例如，使用一热编码创建大量虚拟变量。

稀疏性可以通过计算数据集中零值的比例与总元素数的比率来衡量。解决稀疏性问题将影响你的机器学习模型的准确性。

此外，我们应区分稀疏性和缺失数据。缺失数据仅意味着某些值不可用。在稀疏数据中，所有值都是存在的，但大多数值为零。

此外，稀疏性对机器学习带来了独特的挑战。具体来说，它会导致过拟合、丢失有用数据、内存问题和时间问题。

本文将探讨与稀疏数据相关的这些常见问题。然后，我们将介绍处理这些问题的技术。

最后，我们将应用不同的机器学习模型于稀疏数据，并解释为什么这些模型适用于稀疏数据。

在整个文章中，我将主要使用 scikit-learn 库，如果你希望修改代码和参数，我也会提供官方文档链接。

现在让我们开始讨论稀疏数据的常见问题。

# 稀疏数据的常见问题

稀疏数据可能对数据分析提出独特的挑战。我们已经提到了一些最常见的问题，包括过拟合、丢失有用数据、内存问题和时间问题。

现在，让我们详细查看每一项。

![最佳稀疏数据机器学习模型](img/f8b50e24377d552147b6d62f797db89e.png)

作者提供的图片

## 过拟合

过拟合发生在模型变得过于复杂，开始捕捉数据中的噪声而不是潜在模式时。

在稀疏数据中，可能有大量特征，但只有少数特征与分析实际相关。这会使得识别哪些特征重要、哪些不重要变得困难。

结果可能导致模型对数据中的噪声过拟合，并在新数据上表现不佳。

如果你对机器学习不熟悉或想了解更多，你可以参考[scikit-learn 关于过拟合的文档](https://scikit-learn.org/stable/auto_examples/model_selection/plot_underfitting_overfitting.html)。

## 丢失有用数据

稀疏数据的一个最大问题是它可能导致潜在有用信息的丢失。

当我们拥有非常有限的数据时，识别数据中的有意义模式或关系变得更加困难。这是因为任何数据集固有的噪声和随机性在数据稀疏时更容易掩盖重要特征。

此外，由于可用数据的数量有限，我们更有可能错过数据中一些真正有价值的模式或关系。特别是在由于采样不足而导致数据稀疏的情况下，这一点尤其真实。在这种情况下，我们甚至可能未意识到丢失了数据点，因此可能无法意识到我们正在丢失有价值的信息。

这就是为什么如果移除过多特征，或者数据压缩过度，可能会丢失重要信息，从而导致模型准确性降低。

## 内存问题

内存问题可能由于数据集的巨大规模而出现。稀疏数据通常导致许多特征，存储这些数据可能需要计算上的高开销。这可能限制一次能处理的数据量，或者需要大量计算资源。

[这里](https://scikit-learn.org/0.15/modules/scaling_strategies.html)你可以查看使用 scikit-learn 进行数据缩放的不同策略。

## 时间问题

时间问题也可能由于数据集的巨大规模而发生。稀疏数据可能需要更长的处理时间，特别是当处理大量特征时。这可能限制数据处理的速度，在时间敏感的应用中可能会有问题。

# 处理稀疏特征的方法有哪些？

![适用于稀疏数据的最佳机器学习模型](img/1943174e04fa31dcc50b815eb278f98a.png)

作者提供的图片

稀疏数据在数据分析中带来了挑战，因为其非零值出现频率较低。然而，有几种方法可以缓解这个问题。

一个常见的方法是移除导致数据集稀疏的特征。

另一个选项是使用主成分分析（PCA）来减少数据集的维度，同时保留重要信息。

特征哈希是一种可以采用的技术，它涉及将特征映射到固定长度的向量。

T-分布随机邻域嵌入（t-SNE）是另一种有用的方法，可以用来可视化高维数据集。

除了这些技术，选择一个适合处理稀疏数据的机器学习模型，如 SVM 或逻辑回归，也是至关重要的。

通过实施这些策略，可以有效解决数据分析中与稀疏数据相关的挑战。

现在我们先从减少稀疏数据的策略开始，然后我们将深入探讨模型。

## 移除它！

在处理稀疏数据时，一种方法是移除包含大多数零值的特征。这可以通过设置每个特征中非零值的百分比阈值来完成。任何低于该阈值的特征都可以从数据集中移除。

这种方法可以帮助减少数据集的维度，并提高某些机器学习算法的性能。

### 代码示例

在这个示例中，我们设置了数据集的维度以及稀疏度，稀疏度决定了数据集中有多少值为零。

然后，我们生成具有指定稀疏度的随机数据，以检查我们的方法是否有效。在此步骤中，我们计算稀疏度以供之后比较。

接下来，代码设置要移除的零的数量，并随机移除数据集中特定数量的零。然后，我们重新计算修改后的数据集的稀疏度，以检查我们的方法是否有效。

最后，我们重新计算稀疏度以查看变化。

这是代码。

```py
import numpy as np

# Set the dimensions of the dataset
num_rows = 1000
num_cols = 100

# Set the sparsity level of the dataset
sparsity = 0.9

# Generate random data with the specified sparsity level
data = np.random.random((num_rows, num_cols))
data[data < sparsity] = 0

# Calculate the sparsity of the dataset
num_zeros = (data == 0).sum()
total_elements = data.shape[0] * data.shape[1]
sparsity = num_zeros / total_elements

print(f"The sparsity of the dataset before removal {sparsity:.4f}")

# Set the number of zeros to remove
num_zeros_to_remove = 50000

# Remove a specific number of zeros randomly from the dataset
zero_indices = np.argwhere(data == 0)
zeros_to_remove = np.random.choice(
    zero_indices.shape[0], num_zeros_to_remove, replace=False
)
data[
    zero_indices[zeros_to_remove, 0], zero_indices[zeros_to_remove, 1]
] = np.nan

# Calculate the sparsity of the modified dataset

num_zeros = (data == 0).sum()
total_elements = data.shape[0] * data.shape[1]
sparsity = num_zeros / total_elements

print(
    "Sparsity after removing {} zeros:".format(num_zeros_to_remove), sparsity
) 
```

这是输出结果。

![最佳稀疏数据机器学习模型](img/1f7e7f923e68f352886c4fdd2a123ecf.png)

## PCA

PCA 是一种流行的降维技术。它识别数据的主成分，这些主成分是数据变化最大的方向。

然后可以使用这些主成分在较低维度的空间中表示数据。

在稀疏数据的背景下，PCA 可以用来识别数据中包含最多变异的最重要特征。

通过仅选择这些特征，我们可以减少数据集的维度，同时保留大部分重要信息。

你可以使用 scikit-learn 库来实现 PCA，我们将在接下来的代码示例中这样做。[这里](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)是官方文档，如果你想了解更多信息。

### 代码示例

要对稀疏数据应用 PCA，我们可以使用 Python 中的 scikit-learn 库。

该库提供了一个 PCA 类，我们可以使用它来拟合 PCA 模型并将数据转换为较低维度的空间。

在接下来的代码的第一部分，我们创建一个数据集，如同在前一部分中一样，具有给定的维度和稀疏度。

在第二部分，我们将应用 PCA 将数据集的维度减少到 10。之后，我们将重新计算稀疏度。

这是代码。

```py
import numpy as np

# Set the dimensions of the dataset
num_rows = 1000
num_cols = 100

# Set the sparsity level of the dataset
sparsity = 0.9

# Generate random data with the specified sparsity level
data = np.random.random((num_rows, num_cols))
data[data < sparsity] = 0

# Calculate the sparsity of the dataset
num_zeros = (data == 0).sum()
total_elements = data.shape[0] * data.shape[1]
sparsity = num_zeros / total_elements

print(f"The sparsity of the dataset before removal {sparsity:.4f}")

# Apply PCA to the dataset
pca = PCA(n_components=10)
data_pca = pca.fit_transform(data)
# Calculate the sparsity of the reduced dataset
num_zeros = (data_pca == 0).sum()
total_elements = data_pca.shape[0] * data_pca.shape[1]
sparsity = num_zeros / total_elements

print(f"Sparsity after PCA: {sparsity:.4f}") 
```

这是输出结果。

![最佳稀疏数据机器学习模型](img/c722563f5d7f093ec75871b17dfc8296.png)

## 特征哈希

处理稀疏数据的另一种方法叫做特征哈希。这种方法通过哈希函数将每个特征转换为固定长度的值数组。

哈希函数将每个输入特征映射到固定长度数组中的一组索引。如果多个输入特征映射到相同的索引，则将这些值相加。特征哈希对于存储大型特征字典可能不可行的大型数据集非常有用。

我们将在下一部分一起讨论这个内容，但如果你想深入了解，[这里](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.FeatureHasher.html)可以查看 scikit-learn 库中特征哈希器的官方文档。

### 代码示例

在这里，我们再次使用相同的方法来创建数据集。

然后我们使用 scikit-learn 的 FeatureHasher 类对数据集应用特征哈希。

我们使用**n_features**参数指定输出特征的数量，并使用**input_type**参数指定输入类型为字典。

然后我们使用 FeatureHasher 对象的 transform 方法将输入数据转换为哈希数组。

最后，我们计算结果数据集的稀疏度。

这是代码。

```py
import numpy as np

# Set the dimensions of the dataset
num_rows = 1000
num_cols = 100

# Set the sparsity level of the dataset
sparsity = 0.9

# Generate random data with the specified sparsity level
data = np.random.random((num_rows, num_cols))
data[data < sparsity] = 0

# Calculate the sparsity of the dataset
num_zeros = (data == 0).sum()
total_elements = data.shape[0] * data.shape[1]
sparsity = num_zeros / total_elements

print(f"The sparsity of the dataset before removal {sparsity:.4f}")

# Apply feature hashing to the dataset
hasher = FeatureHasher(n_features=10, input_type="dict")
data_dict = [
    dict(("feature" + str(i), val) for i, val in enumerate(row))
    for row in data
]
data_hashed = hasher.transform(data_dict).toarray()

# Calculate the sparsity of the reduced dataset
num_zeros = (data_hashed == 0).sum()
total_elements = data_hashed.shape[0] * data_hashed.shape[1]
sparsity = num_zeros / total_elements

print(f"Sparsity after feature hashing: {sparsity:.4f}") 
```

这是输出结果。

![最佳稀疏数据机器学习模型](img/9415c1a75c52d781b730b48385a8e84b.png)

## t-SNE 嵌入

t-SNE（t-分布随机邻域嵌入）是一种非线性降维技术，用于可视化高维数据。它在减少数据的维度的同时保持其全局结构，已成为机器学习中可视化和聚类高维数据的热门工具。

t-SNE 特别适用于处理稀疏数据，因为它可以有效地降低数据的维度，同时保持其结构。t-SNE 算法通过计算高维和低维空间中数据点之间的成对距离来工作。然后，它最小化这些距离在高维和低维空间中的差异。

为了在稀疏数据上使用 t-SNE，数据必须首先转换为密集矩阵。这可以通过各种技术完成，如 PCA 或特征哈希。数据转换完成后，t-SNE 可以用来获得数据的低维嵌入。

此外，如果你对 t-SNE 感兴趣，[这里](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html)是 scikit-learn 的官方文档，可以查看更多信息。

### 代码示例

以下代码首先设置数据集的维度和稀疏度级别，生成具有指定稀疏度级别的随机数据，并计算应用 t-SNE 之前数据集的稀疏度，正如我们在前面的示例中所做的那样。

它然后对具有 3 个组件的数据集应用 t-SNE，并计算 t-SNE 嵌入的稀疏度。最后，它打印出 t-SNE 嵌入的稀疏度。

这是代码。

```py
import numpy as np

# Set the dimensions of the dataset
num_rows = 1000
num_cols = 100

# Set the sparsity level of the dataset
sparsity = 0.9

# Generate random data with the specified sparsity level
data = np.random.random((num_rows, num_cols))
data[data < sparsity] = 0

# Calculate the sparsity of the dataset
num_zeros = (data == 0).sum()
total_elements = data.shape[0] * data.shape[1]
sparsity = num_zeros / total_elements

print(f"The sparsity of the dataset before removal {sparsity:.4f}")

# Apply t-SNE to the dataset
tsne = TSNE(n_components=3)
data_tsne = tsne.fit_transform(data)

# Calculate the sparsity of the t-SNE embedding
num_zeros = (data_tsne == 0).sum()
total_elements = data_tsne.shape[0] * data_tsne.shape[1]
sparsity = num_zeros / total_elements

print(f"Sparsity after t-SNE: {sparsity:.4f}") 
```

这是输出结果。

![最佳稀疏数据机器学习模型](img/5f5d3328773ed82df3f8582ace6c31da.png)

# 稀疏数据的最佳机器学习模型

既然我们已经解决了处理稀疏数据的挑战，我们可以探索专门为稀疏数据设计的机器学习模型。

这些模型可以处理稀疏数据的独特特征，例如大量特征中有许多零和有限的信息，这使得传统模型很难实现准确预测。

通过使用专门为稀疏数据设计的模型，我们可以确保预测结果更加精准可靠。

现在我们来谈谈适合稀疏数据的模型。

## SVC（支持向量分类器）

SVC（支持向量分类器）与线性核可以很好地处理稀疏数据，因为它使用训练点的子集，即支持向量，进行预测。这意味着它可以高效地处理高维稀疏数据。

你也可以使用支持向量进行回归。

如果你想了解更多关于支持向量算法的内容，包括分类和回归，我在 [这里解释了支持向量机](https://www.stratascratch.com/blog/machine-learning-algorithms-explained-support-vector-machine/?utm_source=blog&utm_medium=click&utm_campaign=kdn+sparse+data)。

## 逻辑回归

这也可以很好地处理稀疏数据，因为逻辑回归使用正则化项来控制模型复杂性，这有助于防止在稀疏数据集上的过拟合。

如果你想了解更多关于逻辑回归以及其他分类算法的信息，请查看 [机器学习算法概述：分类](https://www.stratascratch.com/blog/overview-of-machine-learning-algorithms-classification/?utm_source=blog&utm_medium=click&utm_campaign=kdn+sparse+data)。

## KNeighboursClassifier

该算法可以很好地处理稀疏数据，因为它计算数据点之间的距离，并且能够处理高维数据。

你可以在 [这里的机器学习算法](https://www.stratascratch.com/blog/machine-learning-algorithms-you-should-know-for-data-science/?utm_source=blog&utm_medium=click&utm_campaign=kdn+sparse+data) 查看 KNN 和其他你需要了解的数据科学算法。

## MLPClassifier

MLPClassifier 在输入数据标准化时可以很好地处理稀疏数据，因为它使用梯度下降进行优化。

[这里](https://www.stratascratch.com/blog/here-is-how-chatgpt-will-help-you-be-a-better-data-scientist/?utm_source=blog&utm_medium=click&utm_campaign=kdn+sparse+data)你可以看到 MLP 分类器的实现，以及其他算法，借助 ChatGPT 的帮助。

## 决策树分类器

当特征数量较少时，它可以很好地处理稀疏数据。如果你对决策树不了解，我在 [这里解释了决策树和随机森林](http://stratascratch.com/blog/decision-tree-and-random-forest-algorithm-explained/?utm_source=blog&utm_medium=click&utm_campaign=kdn+sparse+data)，这是我们分析稀疏数据模型的最终模型。

## RandomForestClassifier

RandomForestClassifier 在特征较少时可以很好地处理稀疏数据。

![处理稀疏数据的最佳机器学习模型](img/5b2bb7e52165ba96f0ff593375d81136.png)

图片由作者提供

现在，我将展示这些模型在生成的数据上的表现。但是，我会添加另一个算法，以查看这些算法是否会超越这个通常不适合稀疏数据的算法。

### 代码示例

在本节中，我们将测试多个机器学习模型在稀疏数据集上的表现，这是一种包含大量空值或零值的数据集。

我们将计算数据集的稀疏性，并使用 F1 分数评估模型。

然后，我们将创建一个包含每个模型 F1 分数的数据框，以比较它们的表现。此外，我们将过滤掉在评估过程中可能出现的任何警告。

```py
import numpy as np
from scipy.sparse import random
import numpy as np
from scipy.sparse import random
from sklearn.model_selection import train_test_split
from sklearn.metrics import f1_score
from sklearn.svm import SVC
from sklearn.linear_model import LogisticRegression, Lasso
from sklearn.cluster import KMeans
from sklearn.neighbors import KNeighborsClassifier
from sklearn.neural_network import MLPClassifier
from sklearn.datasets import make_classification
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.exceptions import ConvergenceWarning
import warnings

# Generate a sparse dataset
X = random(1000, 20, density=0.1, format="csr", random_state=42)
y = np.random.randint(2, size=1000)

# Calculate the sparsity of the dataset
sparsity = 1.0 - X.nnz / float(X.shape[0] * X.shape[1])
print("Sparsity:", sparsity)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Train and evaluate multiple classifiers
classifiers = [
    SVC(kernel="linear"),
    LogisticRegression(),
    KMeans(
        n_clusters=2,
        init="k-means++",
        max_iter=100,
        random_state=42,
        algorithm="full",
    ),
    KNeighborsClassifier(n_neighbors=5),
    MLPClassifier(
        hidden_layer_sizes=(100, 50),
        max_iter=1000,
        alpha=0.01,
        solver="sgd",
        verbose=0,
        random_state=21,
        tol=0.000000001,
    ),
    DecisionTreeClassifier(),
    RandomForestClassifier(),
]

# Create an empty DataFrame with column names
df = pd.DataFrame(columns=["Classifier", "F1 Score"])

# Filter out the specific warning
warnings.filterwarnings(
    "ignore", category=ConvergenceWarning
)  # Filter warning that mlp classifier will possibly print out.

for clf in classifiers:
    clf.fit(X_train, y_train)
    y_pred = clf.predict(X_test)
    f1 = f1_score(y_test, y_pred)
    df = pd.concat(
        [
            df,
            pd.DataFrame(
                {"Classifier": [type(clf).__name__], "F1 Score": [f1]}
            ),
        ],
        ignore_index=True,
    )
df = df.sort_values(by="F1 Score", ascending=True)
df 
```

这里是输出结果。

![处理稀疏数据的最佳机器学习模型](img/e3a1ac872b9e24830ac4b1b0935ba9cb.png)

到目前为止，你可能会发现一个不适合稀疏数据的算法。没错，答案是 KMeans。但为什么呢？

KMeans 通常不适合稀疏数据，因为它基于距离度量，这在高维稀疏数据中可能会出现问题。

也有一些算法我们甚至不能尝试。例如，如果你试图将 GaussianNB 分类器包含在这个列表中，你会遇到错误。它提示 GaussianNB 分类器期望的是密集数据而不是稀疏数据。这是因为 GaussianNB 分类器假设输入数据遵循高斯分布，因此不适合稀疏数据。

# 结论

总结来说，处理稀疏数据可能会面临各种问题，比如过拟合、丢失有效数据、内存和时间问题。

然而，有几种方法可以处理稀疏特征，包括移除特征、使用 PCA 和特征哈希。

此外，某些机器学习模型如 SVM、Logistic Regression、Lasso、Decision Tree、Random Forest、MLP 和 k-最近邻适合处理稀疏数据。

这些模型已被设计为高效处理高维和稀疏数据，使它们成为处理稀疏数据问题的最佳选择。使用这些方法和模型可以提高模型的准确性，节省时间和资源。

**[Nate Rosidi](https://www.stratascratch.com)** 是一位数据科学家和产品战略专家。他还担任分析学兼职教授，并且是 [StrataScratch](https://www.stratascratch.com/) 的创始人，该平台帮助数据科学家准备面试，提供来自顶级公司的真实面试问题。可以通过 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 与他联系。

### 了解更多

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [Python 中的稀疏矩阵表示](https://www.kdnuggets.com/2020/05/sparse-matrix-representation-python.html)

+   [广义可扩展的最优稀疏决策树（GOSDT）](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [Segment Anything 模型：图像分割的基础模型](https://www.kdnuggets.com/2023/07/segment-anything-model-foundation-model-image-segmentation.html)

+   [使用 OpenAI GPT 模型的最佳实践](https://www.kdnuggets.com/2023/08/best-practices-openai-gpt-model.html)

+   [如何利用合成数据克服机器学习模型训练中的数据短缺问题](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)
