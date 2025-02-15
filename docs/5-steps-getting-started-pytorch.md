# 使用 PyTorch 的 5 步入门指南

> 原文：[`www.kdnuggets.com/5-steps-getting-started-pytorch`](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

![使用 PyTorch 的 5 步入门指南](img/85ff1f259e44794911a7f7d810b43ab6.png)

# PyTorch 和 PyTorch Lightning 简介

[PyTorch](https://pytorch.org/) 是一个流行的基于 Python 的开源机器学习框架，并优化了 GPU 加速计算。最初由 Meta AI 在 2016 年开发，现在是 Linux 基金会的一部分，PyTorch 很快成为深度学习研究和应用中使用最广泛的框架之一。

与一些其他框架（如 TensorFlow）不同，PyTorch 使用动态计算图，这允许更大的灵活性和调试能力。PyTorch 的关键好处包括：

+   简单直观的 Python API，用于构建神经网络

+   广泛支持 GPU/TPU 加速

+   内置自动微分支持

+   分布式训练能力

+   与其他 Python 库（如 NumPy）的互操作性

[PyTorch Lightning](https://lightning.ai/docs/pytorch/stable/) 是一个建立在 PyTorch 之上的轻量级封装，进一步简化了研究人员的工作流程和模型开发过程。使用 Lightning，数据科学家可以更多地专注于设计模型，而不是重复的代码。Lightning 的主要优点包括：

+   提供组织 PyTorch 代码的结构

+   处理训练循环的样板代码

+   通过超参数调优加速研究实验

+   简化模型的扩展和部署

通过将 PyTorch 的强大功能和灵活性与 Lightning 的高级 API 相结合，开发人员可以快速构建可扩展的深度学习系统，并加快迭代速度。

# 第 1 步：安装和设置

要开始使用 PyTorch 和 Lightning，您首先需要安装一些先决条件：

+   Python 3.6 或更高版本

+   Pip 包管理器

+   推荐使用 NVidia GPU 进行加速操作（仅 CPU 设置可能会更慢）

## 安装 Python 和 PyTorch

建议使用 Anaconda 设置用于数据科学和深度学习工作的 Python 环境。请按照以下步骤操作：

+   从 [这里](https://www.anaconda.com/products/distribution) 下载并安装适用于您操作系统的 Anaconda

+   创建一个 Conda 环境（或使用其他 Python 环境管理器）：`conda create -n pytorch python=3.8`

+   激活环境：`conda activate pytorch`

+   安装 PyTorch：`conda install pytorch torchvision torchaudio -c pytorch`

通过在 Python 中运行一个快速测试来验证 PyTorch 是否正确安装：

```py
import torch
x = torch.rand(3, 3)
print(x)
```

这将打印出一个随机的 3x3 张量，确认 PyTorch 正常工作。

## 安装 PyTorch Lightning

安装了 PyTorch 后，我们现在可以使用 pip 安装 Lightning：

`pip install lightning`

让我们确认 Lightning 是否正确设置：

```py
import lightning
print(lightning.__version__)
```

这应该会打印出版本号，如 `0.6.0`。

现在我们准备开始构建深度学习模型了。

# 第 2 步：使用 PyTorch 构建模型

PyTorch 使用张量，类似于 NumPy 数组，作为其核心数据结构。张量可以由 GPU 操作，并支持自动微分以构建神经网络。

让我们定义一个用于图像分类的简单神经网络：

```py
import torch
import torch.nn as nn
import torch.nn.functional as F

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = torch.flatten(x, 1)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

net = Net()
```

这定义了一个具有两个卷积层和三个全连接层的卷积神经网络，用于分类 10 个类别。`forward()` 方法定义了数据如何通过网络传递。

我们现在可以使用 Lightning 在样本数据上训练这个模型。

# 第 3 步：使用 Lightning 训练模型

Lightning 提供了一个 `LightningModule` 类来封装 PyTorch 模型代码和训练循环样板代码。让我们转换我们的模型：

```py
import lightning as pl

class LitModel(pl.LightningModule):
    def __init__(self):
        super().__init__()
        self.model = Net()

    def forward(self, x):
        return self.model(x)

    def training_step(self, batch, batch_idx):
        x, y = batch
        y_hat = self.forward(x)
        loss = F.cross_entropy(y_hat, y)
        return loss

    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters(), lr=0.02)

model = LitModel()
```

`training_step()` 定义了前向传递和损失计算。我们配置了一个学习率为 0.02 的 Adam 优化器。

现在我们可以轻松地训练这个模型：

```py
trainer = pl.Trainer()
trainer.fit(model, train_dataloader, val_dataloader)
```

Trainer 处理时期循环、验证、日志记录自动化。我们可以在测试数据上评估模型（更多关于[数据模块的信息](https://lightning.ai/docs/pytorch/stable/data/datamodule.html)）：

```py
result = trainer.test(model, test_dataloader)
print(result)
```

作为对比，这里是纯 PyTorch 中的网络和训练循环代码：

```py
import torch
import torch.nn.functional as F
from torch.utils.data import DataLoader

# Assume Net class and train_dataloader, val_dataloader, test_dataloader are defined

class Net(torch.nn.Module):
    # Define your network architecture here
    pass

# Initialize model and optimizer
model = Net()
optimizer = torch.optim.Adam(model.parameters(), lr=0.02)

# Training Loop
for epoch in range(10):  # Number of epochs
    for batch_idx, (x, y) in enumerate(train_dataloader):
        optimizer.zero_grad()
        y_hat = model(x)
        loss = F.cross_entropy(y_hat, y)
        loss.backward()
        optimizer.step()

# Validation Loop
model.eval()
with torch.no_grad():
    for x, y in val_dataloader:
        y_hat = model(x)

# Testing Loop and Evaluate
model.eval()
test_loss = 0
with torch.no_grad():
    for x, y in test_dataloader:
        y_hat = model(x)
        test_loss += F.cross_entropy(y_hat, y, reduction='sum').item()
test_loss /= len(test_dataloader.dataset)
print(f"Test loss: {test_loss}") 
```

Lightning 使得 PyTorch 模型开发变得非常快速和直观。

# 第 4 步：高级主题

Lightning 提供了许多内置功能，用于超参数调优、防止过拟合和模型管理。

## 超参数调优

我们可以使用 Lightning 的` tuner`模块来优化超参数，例如学习率：

```py
tuner = pl.Tuner(trainer)
tuner.fit(model, train_dataloader)
print(tuner.results)
```

这在超参数空间上执行贝叶斯搜索。

## 处理过拟合

像 dropout 层和早停等策略可以减少过拟合：

```py
model = LitModel()
model.add_module('dropout', nn.Dropout(0.2)) # Regularization

trainer = pl.Trainer(early_stop_callback=True) # Early stopping
```

## 模型保存和加载

Lightning 使得保存和重新加载模型变得简单：

```py
# Save
trainer.save_checkpoint("model.ckpt") 

# Load
model = LitModel.load_from_checkpoint(checkpoint_path="model.ckpt")
```

这保留了完整的模型状态和超参数。

# 第 5 步：比较 PyTorch 和 PyTorch Lightning

PyTorch 和 PyTorch Lightning 都是强大的深度学习库，但它们服务于不同的目的并提供独特的功能。虽然 PyTorch 提供了设计和实现深度学习模型的基础构件，但 PyTorch Lightning 旨在简化模型训练的重复部分，从而加速开发过程。

## 关键区别

这是 PyTorch 和 PyTorch Lightning 之间关键差异的总结：

| 特征 | PyTorch | PyTorch Lightning |
| --- | --- | --- |
| 训练循环 | 手动编写 | 自动化 |
| 样板代码 | 需要 | 最小 |
| 超参数调优 | 手动设置 | 内置支持 |
| 分布式训练 | 可用但需要手动设置 | 自动化 |
| 代码组织 | 无特定结构 | 鼓励模块化设计 |
| 模型保存和加载 | 需要自定义实现 | 使用检查点简化 |
| 调试 | 高级但手动 | 使用内置日志更容易 |
| GPU/TPU 支持 | 可用 | 更容易设置 |

## 灵活性与便利性

PyTorch 以其灵活性而闻名，特别是动态计算图，这对于研究和实验非常出色。然而，这种灵活性通常需要编写更多的模板代码，特别是对于训练循环、分布式训练和超参数调优。另一方面，PyTorch Lightning 抽象化了这些模板代码，同时在需要时仍然允许完全自定义和访问低级 PyTorch API。

## 开发速度

如果你从零开始一个项目或进行复杂的实验，PyTorch Lightning 可以节省你大量的时间。LightningModule 类简化了训练过程，自动化日志记录，甚至简化了分布式训练。这使你可以更多地专注于模型架构，而不是模型训练和验证中的重复工作。

## 判决结果

总结来说，PyTorch 提供了更细粒度的控制，适合需要这种详细级别的研究人员。然而，PyTorch Lightning 的设计旨在使研究到生产的周期更顺畅、更快速，同时不剥夺 PyTorch 提供的力量和灵活性。你选择 PyTorch 还是 PyTorch Lightning 将取决于你的具体需求，但好消息是你可以轻松地在两者之间切换，甚至在项目的不同部分同时使用它们。

# 展望未来

在本文中，我们涵盖了使用 PyTorch 和 PyTorch Lightning 进行深度学习的基础知识：

+   PyTorch 提供了一个强大而灵活的框架，用于构建神经网络

+   PyTorch Lightning 简化了训练和模型开发工作流程

+   关键功能如超参数优化和模型管理加速了深度学习研究

有了这些基础，你可以开始构建和训练高级模型，如 CNN、RNN、GAN 等。活跃的开源社区还提供了 Lightning 支持和 Bolt 等组件及优化库。

祝深度学习愉快！

[**马修·梅奥**](https://www.linkedin.com/in/mattmayo13/) ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 KDnuggets 的主编，马修的目标是让复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、机器学习算法以及探索新兴的人工智能。他致力于在数据科学社区中普及知识。马修从 6 岁起就开始编程。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 相关话题

+   [入门 PyTorch Lightning](https://www.kdnuggets.com/2022/12/getting-started-pytorch-lightning.html)

+   [5 步骤入门 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [5 步骤入门 SQL](https://www.kdnuggets.com/5-steps-getting-started-with-sql)

+   [5 步骤入门 Scikit-learn](https://www.kdnuggets.com/5-steps-getting-started-scikit-learn)

+   [5 步骤入门 Google Cloud Platform](https://www.kdnuggets.com/5-steps-google-cloud-platform)

+   [5 个简单步骤系列：掌握 Python、SQL、Scikit-learn、PyTorch 等](https://www.kdnuggets.com/5-simple-steps-series-master-python-sql-scikit-learn-pytorch-google-cloud)
