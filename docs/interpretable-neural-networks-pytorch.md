# 使用 PyTorch 的可解释神经网络

> 原文：[`www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html`](https://www.kdnuggets.com/2022/01/interpretable-neural-networks-pytorch.html)

![可解释神经网络与 PyTorch](img/5ba5c52449e7cf37809764fe94987d2c.png)

图片由 [Jan Schulz # Webdesigner Stuttgart](https://unsplash.com/@wombat?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

有几种方法来评估机器学习模型，其中两种是 **准确性** 和 **可解释性**。一个具有高准确性的模型通常被称为 *优秀* 模型，它很好地学习了输入 *X* 和输出 *y* 之间的关系。

如果一个模型具有高可解释性或解释性，我们可以理解模型如何做出预测，以及如何通过改变输入特征来 **影响** 这种预测。虽然很难说当我们增加或减少输入的某个特征时深度神经网络的输出会如何变化，但对于线性模型来说非常简单：如果你增加特征值，输出会按该特征的系数增加。很简单。

现在，你可能经常听到这样的说法：

> “有可解释的模型，也有表现良好的模型。” — 不了解情况的人

然而，如果你读过我关于 [可解释增强型机器](https://github.com/interpretml/interpret)（EBM）的文章，那么你已经知道这并不准确。EBM 是一个在保持可解释性的同时具有出色性能的模型示例。

[**可解释增强型机器**](https://towardsdatascience.com/the-explainable-boosting-machine-f24152509ebb)

在我的旧文章中，我创建了以下图表，展示了我们如何将一些模型放置在 *可解释性-准确性空间* 中。

![可解释神经网络与 PyTorch](img/62ba27e46c8b8ea08845bc506f6bd858.png)

作者提供的图片。

特别是，我将深度神经网络（省略 *深度*）更多地放在 **非常准确但难以解释** 的区域。当然，你可以通过使用像 [shap](https://github.com/slundberg/shap) 或 [lime](https://github.com/marcotcr/lime) 这样的库在一定程度上缓解解释性问题，但这些方法也有其自身的假设和问题。因此，让我们在本文中采用另一条路径，创建一种设计上具有可解释性的神经网络架构。

> ***免责声明：*** *我即将呈现的架构只是我脑海中的构想。我不知道是否已有相关文献，至少我没有找到相关资料。但如果你知道任何做过类似工作的论文，请告知我！我会将其添加到参考文献中。*

## 可解释架构理念

*请注意，我期望你了解前馈神经网络的工作原理。我不会在这里进行详细介绍，因为已有许多很好的资源。*

考虑以下玩具神经网络，它具有三个输入节点 *x*₁、*x*₂、*x*₃，一个输出节点 *ŷ*，以及三个具有六个节点的隐藏层。我在这里省略了偏置项。

![可解释的神经网络与 PyTorch](img/73ca1116db1a3c9fe17827a150296371.png)

图片由作者提供。

这种架构在可解释性上的问题在于，由于全连接层，输入完全混合在一起。每个单独的输入节点影响所有隐藏层节点，这种影响随着我们深入网络而变得更加复杂。

## 源自树的灵感

对于基于树的模型来说，通常是相同的，因为如果我们不加限制，决策树可以使用每个特征来创建一个划分。例如，标准的梯度提升及其衍生版本如 [XGBoost](https://xgboost.readthedocs.io/en/stable/)、[LightGBM](https://lightgbm.readthedocs.io/en/latest/) 和 [CatBoost](https://catboost.ai/) 本身并不是很具备可解释性。

然而，通过使用**仅依赖于单一特征的决策树**，如 EBM 所做的那样，你可以使梯度提升模型具有可解释性（阅读我的文章了解更多！????）。

将树限制为这样的形式在许多情况下不会对性能产生太大影响，但使我们能够像这样可视化特征的影响：

![可解释的神经网络与 PyTorch](img/6010d7abeb3804d7d84a7e363a4dcfc6.png)

**interpretml** 的 show 函数输出。图片由作者提供。

只需看看图形顶部的蓝线部分。它显示了 feature_4 对某些回归问题输出的影响。在 *x* 轴上，你可以看到 feature_4 的范围。*y* 轴显示了 **分数**，即输出的变化值。下面的直方图显示了 feature_4 的分布。

从图形中，我们可以看到以下内容：

+   如果 feature_4 约为 0.62，则输出比 feature_4 为 0.6 或 0.65 时增加约 10。

+   如果 feature_4 大于 0.66，则对输出的影响为负。

+   改变 feature_4 在 0.4 到 0.56 范围内的值会显著改变输出。

模型的最终预测只是不同特征分数的总和。这种行为类似于 Shapley 值，但无需计算它们。很棒，对吧？现在，让我展示如何对神经网络进行相同的操作。

## 删除边缘

因此，如果问题在于神经网络的输入由于边缘过多而散布在隐藏层中，我们可以考虑删除一些边缘。特别是，我们需要删除那些允许一个特征的信息流向另一个特征的边缘。仅删除这些*溢出边缘*，上面的玩具神经网络变为：

![可解释的神经网络与 PyTorch](img/e9f1afaa6b7136c25c2db9c8e15edef7.png)

图片由作者提供。

我们为三个输入变量创建了三个单独的**块**，每个块都是一个具有单个部分输出的完全连接网络*ŷᵢ*。作为最后一步，这些*ŷᵢ*被加和，并且加上一个偏置（在图示中省略）以产生最终输出*ŷ*。

我们引入部分输出是为了能够创建与 EBM 允许的相同类型的图。上图中的一个块允许一个图：*xᵢ* 输入，ŷᵢ* 输出。稍后我们将看到如何做到这一点。

现在我们已经有了完整的架构！我认为理论上理解它是相当简单的，但让我们也来实现一下。这样，你会很高兴，因为你可以使用神经网络，而业务也会很高兴，因为神经网络是可解释的。

## 在 PyTorch 中的实现

我不期望你完全熟悉[PyTorch](https://pytorch.org/)，所以我会在过程中解释一些基础知识，这将帮助你理解我们自定义的实现。如果你了解 PyTorch 的基础知识，可以跳过**完全连接层**部分。如果你还没有安装 PyTorch，[选择你的版本](https://pytorch.org/)。

### 完全连接层

这些层在 PyTorch 中也被称为**线性**，在[Keras](https://keras.io/)中称为**密集**。它们将*n*个输入节点连接到*m*个输出节点，使用*nm*条带有乘法权重的边。这基本上是矩阵乘法加上一个偏置项，如以下两个代码片段所示：

```py
import torchtorch.manual_seed(0) **# keep things reproducible**x = torch.tensor([1., 2.]) **# create an input array**
linear_layer = torch.nn.Linear(2, 3) **# define a linear layer**
print(linear_layer(x)) **# putting the input array into the layer****# Output:
# tensor([ 0.7393, -1.0621,  0.0441], grad_fn=<AddBackward0>)**
```

这就是你如何创建完全连接层并将它们应用于 PyTorch 张量。你可以通过`linear_layer.weight`获取用于乘法的矩阵，通过`linear_layer.bias`获取偏置。然后你可以这样做

```py
print(linear_layer.weight @ x + linear_layer.bias) **# @ = matrix mult****# Output:
# tensor([ 0.7393, -1.0621,  0.0441], grad_fn=<AddBackward0>)**
```

很好，它是一样的！现在，PyTorch、Keras 等的一个好处是你可以将许多这样的层堆叠在一起以创建神经网络。在 PyTorch 中，你可以通过`torch.nn.Sequential`来实现这种堆叠。为了重建上面的密集网络，你可以做一个简单的

```py
model = torch.nn.Sequential(
    torch.nn.Linear(3, 6),
    torch.nn.ReLU(),
    torch.nn.Linear(6, 6),
    torch.nn.ReLU(),
    torch.nn.Linear(6, 6),
    torch.nn.ReLU(),
    torch.nn.Linear(6, 1),
)print(model(torch.randn(4, 3))) **# feed it 4 random 3-dim. vectors**
```

> ***注意：*** 我尚未向你展示如何训练这个网络，这只是架构的定义，包括参数的初始化。但你可以输入三维输入并接收一维输出。

既然我们想要创建自己的层，让我们先从简单的开始：重建 PyTorch 的`Linear`层。以下是如何做到这一点：

```py
import torch
import mathclass MyLinearLayer(torch.nn.Module):
    def __init__(self, in_features, out_features):
        super().__init__()
        self.in_features = in_features
        self.out_features = out_features

        **# multiplicative weights
        weights = torch.Tensor(out_features, in_features)
        self.weights = torch.nn.Parameter(weights)
        torch.nn.init.kaiming_uniform_(self.weights)** 

        # bias
        bias = torch.Tensor(out_features)
        self.bias = torch.nn.Parameter(bias)
        bound = 1 / math.sqrt(in_features)
        torch.nn.init.uniform_(self.bias, -bound, bound)   ** def forward(self, x):
        return x @ self.weights.t() + self.bias**
```

这段代码需要一些解释。在第一个**粗体**块中，我们通过

1.  创建一个 PyTorch 张量（包含所有零，但这并不重要）

1.  将其注册为可学习的参数到该层，这意味着梯度下降可以在训练过程中更新它，然后

1.  初始化参数。

神经网络的参数初始化是一个完整的话题，所以我们不会深入探讨。如果这让你感到困扰，你也可以通过使用标准正态分布`torch.randn(out_features, in_features)`来初始化，但这可能会导致训练变慢。不管怎样，我们对偏置做同样的处理。

然后，层需要知道它在`forward`方法中应该执行的数学操作。这只是线性操作，即矩阵乘法和偏置的加法。

好的，现在我们准备为我们的可解释神经网络实现这一层了！

### Block Linear Layers

现在我们设计一个`BlockLinear`层，我们将以以下方式使用它：首先，我们从*n*特征开始。`BlockLinear`层应该创建*n*个块，每个块由*h*个隐藏神经元组成。为了简化问题，*h*在每个块中是相同的，但你当然可以对其进行泛化。总的来说，第一个隐藏层将由*nh*个神经元组成，但只有*nh*条边连接到它们（而不是*n*²*h *用于完全连接的层*）。*要更好地理解，请再次查看上面的图片。在这里，*n* = 3，*h* = 2。

![使用 PyTorch 的可解释神经网络](img/850b88736969d2cad733483126d2b29b.png)

作者提供的图片。

然后——在使用一些非线性操作如 ReLU 之后——我们将在这层后面再加一个`BlockLinear`层，因为不同的块不应该再次合并。我们重复这个过程，直到最后使用`Linear`层将一切重新绑定起来。

### 实现 Block Linear Layer

让我们开始编写代码。它与我们定制的线性层非常相似，因此代码应该不会太令人畏惧。

```py
class BlockLinear(torch.nn.Module):
    def __init__(self, n_blocks, in_features, out_features):
        super().__init__()
        self.n_blocks = n_blocks
        self.in_features = in_features
        self.out_features = out_features
        self.block_weights = []
        self.block_biases = []        **for i in range(n_blocks):
            block_weight = torch.Tensor(out_features, in_features)
            block_weight = torch.nn.Parameter(block_weight)
            torch.nn.init.kaiming_uniform_(block_weight)
            self.register_parameter(
                f'block_weight_{i}',
                block_weight
            )
            self.block_weights.append(block_weight)** **block_bias = torch.Tensor(out_features)
            block_bias = torch.nn.Parameter(block_bias)
            bound = 1 / math.sqrt(in_features)
            torch.nn.init.uniform_(block_bias, -bound, bound)
            self.register_parameter(
                f'block_bias_{i}',
                block_bias
            )
            self.block_biases.append(block_bias)**    def forward(self, x):
        block_size = x.size(1) // self.n_blocks
        **x_blocks = torch.split(
            x,
            split_size_or_sections=block_size,
            dim=1
        )** **block_outputs = []
        for block_id in range(self.n_blocks):
            block_outputs.append(
                x_blocks[block_id] @ self.block_weights[block_id].t() + self.block_biases[block_id]
            )**        return torch.cat(block_outputs, dim=1)
```

我再次强调了几行。第一组粗体行类似于我们在自制线性层中看到的，只是重复了`n_blocks`次。这为每个块创建了一个独立的线性层。

在前向方法中，我们得到一个`x`作为单个张量，我们首先需要使用`torch.split`将其拆分成块。举例来说，块大小为 2 时，结果如下：[1, 2, 3, 4, 5, 6] -> [1, 2], [3, 4], [5, 6]。然后，我们对不同的块应用独立的线性变换，并使用`torch.cat`将结果粘合在一起。完成了！

### 训练可解释神经网络

现在，我们拥有了定义我们可解释神经网络所需的所有要素。我们只需要先创建一个数据集：

```py
X = torch.randn(1000, 3)
y = 3*X[:, 0] + 2*X[:, 1]**2 + X[:, 2]**3 + torch.randn(1000)
y = y.reshape(-1, 1)
```

我们可以看到这里处理的是一个包含一千个样本的三维数据集。真实关系是线性的，如果你对特征 1 进行平方，对特征 2 进行立方——这就是我们想要用模型恢复的！所以，让我们定义一个小模型，应该能够捕捉到这种关系。

```py
class Model(torch.nn.Module):
    def __init__(self):
        super().__init__()

        **self.features** = torch.nn.Sequential(
            BlockLinear(3, 1, 20),
            torch.nn.ReLU(),
            BlockLinear(3, 20, 20),
            torch.nn.ReLU(),
            BlockLinear(3, 20, 20),
            torch.nn.ReLU(),
            BlockLinear(3, 20, 1),
        )

        **self.lr** = torch.nn.Linear(3, 1)

    def forward(self, x):
        x_pre = self.features(x)
        return self.lr(x_pre)

model = Model()
```

我将模型分为两个步骤：

1.  使用`self.features`计算部分输出*ŷᵢ*，然后

1.  计算最终预测*ŷ*作为*ŷᵢ*的加权和，使用`self.lr`。

这使得提取特征解释变得更容易。在`self.features`的定义中，你可以看到我们创建了一个包含三个块的神经网络，因为数据集中有三个特征。对于每个块，我们创建了许多隐藏层，每个块有 20 个神经元。

现在，我们可以创建一个简单的训练循环：

```py
optimizer = torch.optim.Adam(model.parameters())
criterion = torch.nn.MSELoss()for i in range(2000):
    optimizer.zero_grad()
    y_pred = model(X)
    loss = criterion(y, y_pred)
    loss.backward()
    optimizer.step()
    if i % 100 == 0:
        print(loss)
```

基本上，我们选择 Adam 作为优化器，MSE 作为损失函数，然后进行标准梯度下降，即用`optimizer.zero_grad()`擦除旧梯度，计算预测值，计算损失，通过`loss.backward()`计算损失的梯度，并通过`optimizer.step()`更新模型参数。你可以看到训练损失随着时间的推移而下降。我们在这里不关注验证集或测试集。**训练** *r*² 在最后应该大于 0.95。

我们现在可以通过以下方式打印模型解释：

```py
import matplotlib.pyplot as pltx = torch.linspace(-5, 5, 100).reshape(-1, 1)
x = torch.hstack(3*[x])for i in range(3):
    plt.plot(
        x[:, 0].detach().numpy(),
        model.get_submodule('lr').weight[0][i].item() * model.get_submodule('features')(x)[:, i].detach().numpy())
    plt.title(f'Feature {i+1}')
    plt.show()
```

并获得

![可解释的神经网络与 PyTorch](img/21a485232a93af651ab9510e4408481f.png)

![可解释的神经网络与 PyTorch](img/7d728428c5301fd14a4a70d1ca9995b6.png)

![可解释的神经网络与 PyTorch](img/3109aaf24d8963643432d4720e885970.png)

图片由作者提供。

这看起来非常不错！模型发现特征 1 的影响是线性的，特征 2 的影响是二次的，特征 3 的影响是立方的。不仅如此，**模型能够向我们展示这一点**，这是整个构建过程的伟大之处！

> **你甚至可以丢掉网络，仅仅基于这些图表进行预测！**

举个例子，让我们估计网络对于*x* = (2, -2, 0)的输出。

+   *x*₁ = 2 根据第一幅图表转换为**+5**的预测结果。

+   *x*₂ = -2 根据第二幅图表转换为**+9**的预测结果。

+   *x*₃ = 0 根据第三幅图表转换为**+0**的预测结果。

+   从最后一层线性层仍然存在一个**偏差**，你可以通过`model.get_submodule('lr').bias`访问这个偏差，这也需要添加，但应该很小。

总的来说，你的预测结果应该在*ŷ* ***≈** 5 + 9 + 0 + bias **≈** 14，这相当准确。

你还可以看到你需要做什么才能最小化输出：为特征 1 选择小值，为特征 2 选择接近零的值，为特征 3 选择小值。这是你通常不能仅通过查看神经网络看到的，但通过得分函数，我们可以。这是可解释性的一个巨大好处。

请注意，上述学习到的得分函数只能对**实际有训练数据**的区域有信心。在我们的数据集中，我们实际上只观察到每个特征在 -3 到 3 之间的值。因此，我们可以看到，在边缘没有得到完美的*x*²和*x*³多项式。但我认为即便如此，图形的方向也大致正确，这还是很令人印象深刻的。为了充分欣赏这一点，可以将其与 EBM 的结果进行比较：

![可解释的神经网络与 PyTorch](img/6f653b465a72e7ced37b73d08bc971a9.png)

-   ![使用 PyTorch 的可解释神经网络](img/64cb4eb2acb266b653ac6cc856e9f59d.png)

-   作者提供的图片。

-   **这些曲线是块状的，外推只是两边的直线，这是基于树的方法的主要缺点之一。**

## -   结论

-   在这篇文章中，我们讨论了模型的可解释性，以及神经网络和梯度提升如何未能提供它。虽然 interpretml 包的作者创建了 EBM，一个可解释的梯度提升算法，但我向你介绍了一种创建可解释神经网络的方法。

-   我们已经在 PyTorch 中实现了它，虽然代码量有点多，但也没什么太疯狂的。至于 EBM，我们可以提取每个特征的学习得分函数，甚至可以用它们来进行预测。

-   **实际训练的模型甚至不再必要，这使得在低配置硬件上部署和使用成为可能。** 这是因为我们只需存储每个特征的一个查找表，这在内存上很轻量。使用每个查找表的网格大小为*g*会导致**只存储*O*(*n*_features * *g*)个元素**，而不是可能的数百万甚至数十亿个模型参数。进行预测也很便宜：只需从查找表中加一些数字。由于这有**仅为***O*(*n*_features)**的时间复杂度，查找和加法速度远快于网络的前向传播。

> -   ***再次声明：**** 我不确定这是否是一个新想法，但反正就是这样！如果你知道有解释相同想法的论文，请给我留言，我会引用它。*

-   我希望你今天学到了一些新的、有趣的、有用的东西。感谢阅读！

-   **作为最后一点，如果你**

1.  -   **想要支持我继续撰写更多关于机器学习的内容**

1.  -   **计划无论如何获取 Medium 订阅，**

-   **为什么不通过[这个链接](https://dr-robert-kuebler.medium.com/membership)来支持我呢**？这对我帮助很大！????

-   *为了透明起见，您的价格不会变化，但大约一半的订阅费用会直接到我这里。*

-   **非常感谢，如果你考虑支持我！**

> -   如果你有任何问题，可以在[LinkedIn](https://www.linkedin.com/in/dr-robert-k%C3%BCbler-983859150/)上联系我！

-   **[Dr. Robert Kübler](https://www.linkedin.com/in/robert-kuebler/)** 是 Publicis Media 的数据科学家和 Towards Data Science 的作者。

-   [原文](https://towardsdatascience.com/interpretable-neural-networks-with-pytorch-76f1c31260fe)。转载许可。

* * *

## -   我们的前三个课程推荐

-   ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

-   ![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关主题

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [在神经网络之前尝试的 10 个简单方法](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [深度神经网络不会引导我们走向 AGI](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [最先进的深度学习技术解释性预测和现况预测](https://www.kdnuggets.com/2021/12/sota-explainable-forecasting-and-nowcasting.html)

+   [使用卷积神经网络（CNNs）进行图像分类](https://www.kdnuggets.com/2022/05/image-classification-convolutional-neural-networks-cnns.html)

+   [关于可信赖的图神经网络的全面调查：隐私、鲁棒性、公平性和可解释性](https://www.kdnuggets.com/2022/05/comprehensive-survey-trustworthy-graph-neural-networks-privacy-robustness-fairness-explainability.html)
