# 使用 PyTorch 的迁移学习实用指南

> 原文：[`www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html`](https://www.kdnuggets.com/2023/06/practical-guide-transfer-learning-pytorch.html)

与[Naresh](https://medium.com/u/1e659a80cffd)和[Gaurav](http://www.gaurav.ai/)联合撰写。

本文将涵盖迁移学习的“什么”、“为什么”和“如何”。

+   **什么**是迁移学习

+   **为什么**应该使用迁移学习

+   **如何**在实际分类任务中使用迁移学习

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 部门

* * *

具体来说，我们将涵盖迁移学习的以下方面。

+   迁移学习理念的动机及其好处。

+   培养对基础模型选择的直觉。 ([notebook](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/torchvision-model-exploration.ipynb))

+   讨论不同的选择以及在过程中做出的权衡。

+   使用 PyTorch 实现图像分类任务。 ([notebook](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/flowers102-classification-using-pre-trained-models.ipynb))

+   各种基础模型的性能比较。

+   了解更多关于迁移学习及其当前前沿技术的资源

迁移学习是一个庞大且不断发展的领域，本文仅涵盖了其中的一些方面。然而，还有许多深度学习在线社区讨论迁移学习。例如，[这里有一篇不错的文章](https://www.learndatasci.com/tutorials/hands-on-transfer-learning-keras/)介绍了如何利用迁移学习来实现比从头训练模型更高的基准。

## 目标受众及先决条件

+   你已经熟悉基本的机器学习（ML）概念，例如定义和训练分类模型

+   你已经熟悉[PyTorch](https://pytorch.org/)和[torchvision](https://pytorch.org/vision/stable/index.html)

在下一节中，我们将正式介绍迁移学习并通过示例进行解释。

## 迁移学习是什么？

从[此页面](https://machinelearningmastery.com/transfer-learning-for-deep-learning/)获取，

> *“迁移学习是一种机器学习方法，其中为一个任务开发的模型被重新用作第二个任务模型的起点。”*

深度学习模型是一个权重网络，其权重值在训练过程中通过损失函数进行优化。网络的权重通常在训练过程开始前被随机初始化。在迁移学习中，我们使用一个[预训练模型](https://pytorch.org/vision/stable/models.html)，它已经在相关任务上进行了训练。这为我们提供了一组初始权重，这些权重很可能比随机初始化的权重表现更好。我们进一步优化这些预训练的权重以适应我们的特定任务。

Jeremy Howard（来自 fast.ai）[说](https://www.fast.ai/posts/2020-01-13-self_supervised.html)。

> *“在可能的情况下，你应该以一个预训练模型开始你的神经网络训练并进行微调。你真的不希望从随机权重开始，因为那意味着你从一个完全不知道如何做任何事情的模型开始！通过预训练，你可以使用比从头开始少 1000 倍的数据。”*

接下来，我们将看看如何将迁移学习的概念与人类学习联系起来进行思考。

## 迁移学习的人类类比

+   **模型训练**：在孩子出生后，他们需要一段时间来学习站立、平衡和行走。在这个过程中，他们经历了建立身体肌肉的阶段，他们的大脑也学会了理解和内化站立、平衡和行走的技能。他们经历了若干次尝试，有些成功，有些失败，最终达到能够稳定地站立、平衡和行走的阶段。这类似于训练深度学习模型的过程，模型需要经过大量的时间（训练周期）才能学习到一个通用任务（例如将图像分类为 1000 个 ImageNet 类别之一），而这在训练该任务时非常重要。

+   **迁移学习**：一个已经学会走路的孩子发现学习相关的高级技能，如跳跃和跑步，要容易得多。迁移学习类似于这种人类学习的方面，其中一个已经学会了通用技能的预训练模型被用来高效地训练其他相关任务。

现在我们已经对迁移学习有了直观的理解，并且与人类学习进行了类比，让我们看看为什么在机器学习模型中使用迁移学习是有意义的。

## 为什么我应该使用迁移学习？

许多视觉 AI 任务，如图像分类、图像分割、目标定位或检测，仅在它们分类、分割或检测的具体对象上有所不同。训练这些任务的模型已经学习了训练数据集中对象的特征。因此，它们可以很容易地适应相关任务。例如，一个训练用来识别图像中是否有汽车的模型可以被微调以识别猫或狗。

迁移学习的主要优势是能够使你在任务上获得更好的准确性。我们可以将其优势分解如下：

+   **训练效率**：当你开始使用一个已经学会数据一般特征的预训练模型时，你只需***微调***模型以适应你的具体任务，这可以更快地完成（即使用更少的训练周期）。

+   **模型准确性**：使用迁移学习可以比使用相同资源从头开始训练模型获得[显著的性能提升](https://www.learndatasci.com/tutorials/hands-on-transfer-learning-keras/)。然而，为你的具体任务选择合适的预训练模型很重要。

+   **训练数据量**：由于预训练模型已经学会识别与你的任务相关的许多特征，你可以用***较少的领域特定数据***来训练预训练模型。如果你没有足够多的标记数据用于你的具体任务，这一点非常有用。

那么，我们在实践中如何进行迁移学习呢？下一部分将在 PyTorch 中实现一个花卉分类任务的迁移学习。

# 使用 PyTorch 进行迁移学习

为了使用 PyTorch 进行迁移学习，我们首先需要选择一个数据集和一个用于图像分类的预训练视觉模型。本文集中在使用 torch-vision（一个与 PyTorch 一起使用的领域库）。让我们了解一下在哪里可以找到这些预训练模型和数据集。

## 哪里可以找到用于图像分类的预训练视觉模型？

有许多网站提供高质量的预训练图像分类模型。例如：

1.  [Torchvision](https://pytorch.org/vision/stable/models.html)

1.  [PyTorch 图像模型](https://github.com/huggingface/pytorch-image-models)

为了本文的目的，我们将使用[torchvision 的预训练模型](https://pytorch.org/vision/stable/models.html)。值得了解这些模型是如何训练的。接下来我们来探讨这个问题！

## torchvision 模型是预训练于哪些数据集上的？

对于涉及图像的视觉相关任务，torchvision 模型通常在[ImageNet 数据集](https://www.image-net.org/download.php)上进行预训练。研究人员和模型预训练中使用的最流行的 ImageNet 子集包含大约 120 万张图像，涵盖 1000 个类别。由于以下原因，ImageNet 分类被用作预训练任务：

1.  它对研究社区的**可用性**

1.  它所包含的**广度**和图像种类

1.  各种研究人员的使用 - 使得使用**共同的基准**进行结果比较变得有吸引力

你可以在这个[维基百科页面](https://en.wikipedia.org/wiki/ImageNet)上阅读有关 ImageNet 挑战的历史、背景信息以及完整数据集的信息。

**使用预训练模型时的法律考虑**

ImageNet 仅用于非商业研究目的 ([`image-net.org/download`](https://image-net.org/download))。因此，目前不清楚是否可以合法地将预训练于 ImageNet 的模型权重用于商业目的。如果你计划这样做，请寻求法律建议。

现在我们知道了可以找到用于迁移学习的预训练模型的位置，我们来看看可以在哪里获取用于自定义分类任务的数据集。

## 数据集：牛津花卉 102

我们将使用 Flowers 102 数据集来说明如何使用 PyTorch 进行迁移学习。我们将训练一个模型，将 [Flowers 102](https://www.robots.ox.ac.uk/~vgg/data/flowers/102/) 数据集中的图像分类为 102 个类别之一。这是一个多类（单标签）分类问题，其中预测的类别是互斥的。我们将利用 Torchvision 来完成这个任务，因为它已经 [为我们提供了这个数据集](https://pytorch.org/vision/stable/generated/torchvision.datasets.Flowers102.html)。

Flowers 102 数据集来自于 [牛津视觉几何组](https://www.robots.ox.ac.uk/~vgg/data/flowers/102/)。有关使用数据集的许可条款，请参见页面。

接下来，我们来看一下这个过程中的高级步骤。

## 迁移学习是如何工作的？

图像分类任务的迁移学习可以视为三个步骤的序列，如图 1 所示。这些步骤如下：

![使用 PyTorch 进行迁移学习的实用指南](img/ca3d49ee22f436287775d519acd189ad.png)

图 1：使用 PyTorch 进行迁移学习。来源：作者

1.  **替换分类器层**：在此阶段，我们识别并用我们自己的具有正确输出特征数量（在此示例中为 102）的“分类头”替换预训练模型的最后一个“[分类头](https://doc.arcgis.com/en/allsource/latest/analysis/geoprocessing-tools/geoai/how-text-classification-works.htm)”。

1.  **特征提取**：在此阶段，我们将冻结（使这些层不可训练）模型中除了新添加的分类层之外的所有层，只训练这一新添加的层。

1.  **微调**：在此阶段，我们会解冻模型中的一些子集（解冻一层意味着使其可训练）。在本文中，我们将解冻模型的所有层，并像训练任何机器学习（ML）PyTorch 模型一样训练它们。

这些阶段每一个都有许多额外的细节和微妙之处，我们需要了解和关注。我们将很快深入这些细节。目前，让我们深入探讨两个关键阶段，即特征提取和微调。

## 特征提取和微调

你可以在这里找到有关特征提取和微调的更多信息。

1.  [迁移学习中的特征提取和微调有什么区别？](https://ai.stackexchange.com/questions/28138/what-is-the-difference-between-feature-extraction-and-fine-tuning-in-transfer-le)

1.  [不遗忘学习](https://arxiv.org/pdf/1606.09282.pdf)

下面的图示直观地展示了特征提取和微调。

![使用 PyTorch 进行迁移学习的实用指南](img/6c3d03dfd4a4c1adcdd479febd32b4d5.png)

图 2：微调（b）和特征提取（c）的视觉解释。来源：[不遗忘学习](https://arxiv.org/pdf/1606.09282.pdf)

![使用 PyTorch 进行迁移学习的实用指南](img/d372d5f5eccded958b1771c00b2695ce.png)

图 3：图示显示了在特征提取和微调阶段哪些层是可训练的（未冻结）。来源：作者

现在我们已经对自定义分类任务、用于该任务的预训练模型以及迁移学习的工作原理有了很好的理解，让我们来看看一些执行迁移学习的具体代码。

# 给我看代码

在本节中，你将学习诸如探索性模型分析、初始模型选择、如何定义模型、实现迁移学习步骤（上述讨论）以及如何防止过拟合等概念。我们将讨论数据集的训练/验证/测试划分，并解读结果。

本实验的完整代码可以在[这里（使用预训练模型的 Flowers102 分类）](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/torchvision-model-exploration.ipynb)找到。关于探索性模型分析的部分在[一个单独的笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/torchvision-model-exploration.ipynb)中。

## 探索性模型分析

类似于数据科学中的探索性数据分析，迁移学习的第一步是探索性模型分析。在此步骤中，我们探索所有用于图像分类任务的预训练模型，并确定每个模型的结构。

通常情况下，很难知道哪个模型对我们的任务表现最佳，因此尝试几个看起来有前途或适用的模型并不罕见。在这个假设场景中，我们假设模型的大小并不重要（我们不打算将这些模型部署到移动设备或其他边缘设备上）。我们将首先查看 torchvision 中可用的预训练分类模型列表。

```py
classification_models = torchvision.models.list_models(module=torchvision.models)

print(len(classification_models), "classification models:", classification_models)
```

将打印

```py
80 classification models: ['alexnet', 'convnext_base', 'convnext_large', 'convnext_small', 'convnext_tiny', 'densenet121', 'densenet161', 'densenet169', 'densenet201', 'efficientnet_b0', 'efficientnet_b1', 'efficientnet_b2', 'efficientnet_b3', 'efficientnet_b4', 'efficientnet_b5', 'efficientnet_b6', 'efficientnet_b7', 'efficientnet_v2_l', 'efficientnet_v2_m', 'efficientnet_v2_s', 'googlenet', 'inception_v3', 'maxvit_t', 'mnasnet0_5', 'mnasnet0_75', 'mnasnet1_0', 'mnasnet1_3', 'mobilenet_v2', 'mobilenet_v3_large', 'mobilenet_v3_small', 'regnet_x_16gf', 'regnet_x_1_6gf', 'regnet_x_32gf', 'regnet_x_3_2gf', 'regnet_x_400mf', 'regnet_x_800mf', 'regnet_x_8gf', 'regnet_y_128gf', 'regnet_y_16gf', 'regnet_y_1_6gf', 'regnet_y_32gf', 'regnet_y_3_2gf', 'regnet_y_400mf', 'regnet_y_800mf', 'regnet_y_8gf', 'resnet101', 'resnet152', 'resnet18', 'resnet34', 'resnet50', 'resnext101_32x8d', 'resnext101_64x4d', 'resnext50_32x4d', 'shufflenet_v2_x0_5', 'shufflenet_v2_x1_0', 'shufflenet_v2_x1_5', 'shufflenet_v2_x2_0', 'squeezenet1_0', 'squeezenet1_1', 'swin_b', 'swin_s', 'swin_t', 'swin_v2_b', 'swin_v2_s', 'swin_v2_t', 'vgg11', 'vgg11_bn', 'vgg13', 'vgg13_bn', 'vgg16', 'vgg16_bn', 'vgg19', 'vgg19_bn', 'vit_b_16', 'vit_b_32', 'vit_h_14', 'vit_l_16', 'vit_l_32', 'wide_resnet101_2', 'wide_resnet50_2']
```

哇！这真是一个相当庞大的模型选择列表！如果你感到困惑，不要担心——在下一节中，我们将讨论选择初始模型集进行迁移学习时需要考虑的因素。

## 初始模型选择

现在我们有 80 个候选模型需要选择，我们需要将其缩小到几个可以进行实验的模型。预训练模型骨干网络的选择是一个超参数，我们可以（且应该）通过实验探索多个选项，以查看哪个效果最佳。运行实验成本高且耗时，而且不太可能尝试所有模型，这就是为什么我们首先尝试将列表缩小到 3-4 个模型的原因。

我们决定首先使用以下预训练模型骨干网络。

1.  Vgg16: 135M 参数

1.  ResNet50: 23M 参数

1.  ResNet152: 58M 参数

以下是我们选择这三种模型的原因。

1.  我们不受限于模型大小或推理延迟，因此我们不需要寻找超级高效的模型。如果你想要对各种移动设备视觉模型进行比较研究，请阅读题为“[移动设备上 AI 模型和框架的比较与基准测试](https://arxiv.org/pdf/2005.05085.pdf)”的论文。

1.  我们选择的模型在视觉机器学习社区中相当受欢迎，并且通常是分类任务的良好选择。你可以使用这些模型论文的引用计数作为模型有效性的一个较好指标。然而，请注意，像 AlexNet 这样的老旧模型的论文引用计数可能较多，但它们通常不是进行严肃分类任务的默认选择。

1.  即使在模型架构中，通常也有许多不同的变种或尺寸。例如，EfficientNet 有从 B0 到 B7 的不同版本。有关这些版本的具体细节，请参考相关模型的论文。

torchvision 中各种预训练分类模型的引用计数。

1.  Resnet: 165k

1.  AlexNet: 132k

1.  Vgg16: 102k

1.  MobileNet: 19k

1.  Vision Transformers: 16k

1.  EfficientNet: 12k

1.  ShuffleNet: 6k

如果你想了解更多可能影响你选择的预训练模型的因素，请阅读以下文章：

1.  [4 种适用于计算机视觉迁移学习的预训练 CNN 模型](https://towardsdatascience.com/4-pre-trained-cnn-models-to-use-for-computer-vision-with-transfer-learning-885cb1b2dfc)

1.  [如何选择适合你卷积神经网络的最佳预训练模型？](https://data-science-blog.com/blog/2022/04/11/how-to-choose-the-best-pre-trained-model-for-your-convolutional-neural-network/)

1.  [代表性深度神经网络架构的基准分析](https://arxiv.org/pdf/1810.00736.pdf)

让我们查看这些模型的分类头。

```py
vgg16 = torchvision.models.vgg16_bn(weights=None)
resnet50 = torchvision.models.resnet50(weights=None)
resnet152 = torchvision.models.resnet152(weights=None)

print("vgg16**\n**", vgg16.classifier)
print("resnet50**\n**", resnet50.fc)
print("resnet152**\n**", resnet152.fc)
```

```py
vgg16
 Sequential(
  (0): Linear(in_features=25088, out_features=4096, bias=True)
  (1): ReLU(inplace=True)
  (2): Dropout(p=0.5, inplace=False)
  (3): Linear(in_features=4096, out_features=4096, bias=True)
  (4): ReLU(inplace=True)
  (5): Dropout(p=0.5, inplace=False)
  (6): Linear(in_features=4096, out_features=1000, bias=True)
)
resnet50
 Linear(in_features=2048, out_features=1000, bias=True)
resnet152
 Linear(in_features=2048, out_features=1000, bias=True) 
```

你可以在[这里找到完整的探索性模型分析笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/torchvision-model-exploration.ipynb)。

由于我们将对 3 个预训练模型进行实验，并分别对每个模型执行转移学习，因此让我们定义一些抽象和类，以帮助我们运行和跟踪这些实验。

## 定义一个 PyTorch 模型来封装预训练模型

为了方便探索，我们将定义一个名为`Flowers102Classifier`的 PyTorch 模型，并在整个过程中使用它。我们将逐步为该类添加功能，直到实现最终目标。有关[Flowers 102 分类的转移学习的完整笔记本可以在这里找到](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/flowers102-classification-using-pre-trained-models.ipynb)。

下面的部分将深入探讨执行转移学习所需的每一个机械步骤。

## 替换旧的分类头为新的分类头

每个在 ImageNet 分类任务上进行预训练的模型的现有分类头有 1000 个输出特征。我们自定义的花卉分类任务有 102 个输出特征。因此，我们需要将最终的分类头（层）替换为一个具有 102 个输出特征的新分类头。

我们类的构造函数将包括加载来自`torchvision`的预训练模型的代码，并使用预训练权重，并将分类头替换为一个用于 102 类的自定义分类头。

```py
def __init__(self, backbone, load_pretrained):
    super().__init__()
    assert backbone in backbones
    self.backbone = backbone
    self.pretrained_model = None
    self.classifier_layers = []
    self.new_layers = []

    if backbone == "resnet50":
        if load_pretrained:
            self.pretrained_model = torchvision.models.resnet50(
                weights=torchvision.models.ResNet50_Weights.IMAGENET1K_V2
            )
        else:
            self.pretrained_model = torchvision.models.resnet50(weights=None)
        # end if

        self.classifier_layers = [self.pretrained_model.fc]
        # Replace the final layer with a classifier for 102 classes for the Flowers 102 dataset.
        self.pretrained_model.fc = nn.Linear(
            in_features=2048, out_features=102, bias=True
        )
        self.new_layers = [self.pretrained_model.fc]
    elif backbone == "resnet152":
        if load_pretrained:
            self.pretrained_model = torchvision.models.resnet152(
                weights=torchvision.models.ResNet152_Weights.IMAGENET1K_V2
            )
        else:
            self.pretrained_model = torchvision.models.resnet152(weights=None)
        # end if

        self.classifier_layers = [self.pretrained_model.fc]
        # Replace the final layer with a classifier for 102 classes for the Flowers 102 dataset.
        self.pretrained_model.fc = nn.Linear(
            in_features=2048, out_features=102, bias=True
        )
        self.new_layers = [self.pretrained_model.fc]
    elif backbone == "vgg16":
        if load_pretrained:
            self.pretrained_model = torchvision.models.vgg16_bn(
                weights=torchvision.models.VGG16_BN_Weights.IMAGENET1K_V1
            )
        else:
            self.pretrained_model = torchvision.models.vgg16_bn(weights=None)
        # end if

        self.classifier_layers = [self.pretrained_model.classifier]
        # Replace the final layer with a classifier for 102 classes for the Flowers 102 dataset.
        self.pretrained_model.classifier[6] = nn.Linear(
            in_features=4096, out_features=102, bias=True
        )
        self.new_layers = [self.pretrained_model.classifier[6]] 
```

由于我们将执行特征提取和微调，因此我们将把新添加的层保存到`self.new_layers`列表中。这将帮助我们根据操作来设置这些层的权重为可训练或不可训练。

现在我们已经将旧的分类头替换为具有随机初始化权重的新分类头，我们需要训练这些权重，以便模型能够进行准确的预测。这包括特征提取和微调，我们将接下来查看这一点。

## 转移学习（可训练参数和学习率）

转移学习包括按特定顺序运行特征提取和微调。让我们更详细地了解为什么需要按该顺序运行它们，以及如何处理各种转移学习阶段的可训练参数。

**特征提取**：我们将模型中所有层的权重的`requires_grad`设置为`False`，仅将新添加的层的`requires_grad`设置为`True`。

我们用**16 个 epochs**和学习率为 1e-3 训练新层。这确保了新层能够调整和适应其权重，以适应网络中特征提取器部分的权重。冻结网络中的其他层并仅训练新层是重要的，以免使网络忘记已经学到的内容。如果我们不冻结较早的层，它们将会在添加新分类头时被重新训练为随机初始化的垃圾权重。

**微调**：我们将所有层的权重的 requires_grad 设置为 True。我们训练整个网络**8 个周期**。然而，在这种情况下，我们采用了差异学习率策略。我们使学习率（LR）衰减，以便 LR 在向输入层（远离输出分类头）移动时减少。我们在向模型的初始层移动时衰减学习率，因为这些初始层已经学习了关于图像的基本特征，这对于大多数视觉 AI 任务都是共通的。因此，初始层用非常低的学习率进行训练，以避免干扰它们已学到的知识。当我们向分类头部的模型后层移动时，模型正在学习一些任务特定的内容，因此用更高的学习率训练这些后层是有意义的。这里可以采用不同的策略，在我们的案例中，我们使用了 2 种不同的策略来说明它们的有效性。

1.  **VGG16**：对于 vgg16 网络，我们将学习率**线性**地从 LR=1e-4 衰减到 LR=1e-7（比分类层的学习率低 1000 倍）。由于特征提取阶段有 44 层，每一层的学习率比前一层低（1e-7 - 1e-4）/44 = 2.3e-6。

1.  **ResNet**：对于 ResNet（50/152）网络，我们从 LR=1e-4 开始以指数方式衰减学习率。每向上一层，我们将学习率减少 3 倍。

![使用 PyTorch 进行迁移学习的实用指南](img/43ffa221f6003b98afc4a6c4a37b2961.png)

图 4：一个示例显示学习率（LR）以 10 的因子指数衰减，当我们向靠近网络输入的层移动时。来源：作者。

用于冻结特征提取和微调的层的代码显示在下面名为 fine_tune()的函数中。

```py
def fine_tune(self, what: FineTuneType):
    # The requires_grad parameter controls whether this parameter is
    # trainable during model training.
    m = self.pretrained_model
    for p in m.parameters():
        p.requires_grad = False
    if what is FineTuneType.NEW_LAYERS:
        for l in self.new_layers:
            for p in l.parameters():
                p.requires_grad = True
    elif what is FineTuneType.CLASSIFIER:
        for l in self.classifier_layers:
            for p in l.parameters():
                p.requires_grad = True
    else:
        for p in m.parameters():
            p.requires_grad = True 
```

代码片段：在特征提取（NEW_LAYERS）和微调（ALL）阶段使用 requires_grad 冻结和解冻参数。

在 PyTorch 中，为每一层设置差异学习率的方式是将需要该学习率的权重指定给在迁移学习期间使用的优化器。在我们的笔记本中，我们使用 Adam 优化器。下面的 get_optimizer_params()方法获取优化器参数，以传递给我们将使用的 Adam（或其他）优化器。

```py
def get_optimizer_params(self):
    """This method is used only during model fine-tuning when we need to
    set a linearly or exponentially decaying learning rate (LR) for the
    layers in the model. We exponentially decay the learning rate as we
    move away from the last output layer.
    """
    options = []
    if self.backbone == "vgg16":
        # For vgg16, we start with a learning rate of 1e-3 for the last layer, and
        # decay it to 1e-7 at the first conv layer. The intermediate rates are
        # decayed linearly.
        lr = 0.0001
        options.append(
            {
                "params": self.pretrained_model.classifier.parameters(),
                "lr": lr,
            }
        )
        final_lr = lr / 1000.0
        diff_lr = final_lr - lr
        lr_step = diff_lr / 44.0
        for i in range(43, -1, -1):
            options.append(
                {
                    "params": self.pretrained_model.features[i].parameters(),
                    "lr": lr + lr_step * (44 - i),
                }
            )
        # end for
    elif self.backbone in ["resnet50", "resnet152"]:
        # For the resnet class of models, we decay the LR exponentially and reduce
        # it to a third of the previous value at each step.
        layers = ["conv1", "bn1", "layer1", "layer2", "layer3", "layer4", "fc"]
        lr = 0.0001
        for layer_name in reversed(layers):
            options.append(
                {
                    "params": getattr(self.pretrained_model, layer_name).parameters(),
                    "lr": lr,
                }
            )
            lr = lr / 3.0
        # end for
    # end if
    return options

# end def 
```

代码片段：在微调模型时，每层的差异学习率。

一旦我们有了各自学习率的模型参数，就可以用一行代码将它们传递给优化器。对那些在 get_optimizer_params()返回的字典中未指定权重的参数，使用默认学习率 1e-8。

```py
optimizer = torch.optim.Adam(fc.get_optimizer_params(), lr=1e-8)
```

代码片段：将具有各自学习率的参数传递给 Adam 优化器。

现在我们知道了如何进行迁移学习，让我们来看看在微调模型之前还需要考虑哪些其他因素。这包括我们需要采取的步骤以防止过拟合，以及选择合适的训练/验证/测试集划分。

## 防止过拟合

在我们的[笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/flowers102-classification-using-pre-trained-models.ipynb)中，我们对训练数据应用了以下数据增强技术，以防止过拟合，并使模型能够学习特征，从而对未见过的数据进行预测。

1.  色彩抖动

1.  水平翻转

1.  旋转

1.  剪切

对验证拆分没有应用数据增强。

还应探讨[权重衰减](https://towardsdatascience.com/this-thing-called-weight-decay-a7cd4bcfccab)，这是一种正则化技术，通过减少模型的复杂性来防止过拟合。

## 训练/验证/测试拆分

Flowers 102 数据集的作者推荐的训练/验证/测试拆分是 1020/1020/6149。许多作者采取不同的方法。例如，

1.  在[ResNet strikes back](https://arxiv.org/pdf/2110.00476.pdf)论文中，作者使用了训练+验证（2040 张图像）拆分作为训练集，测试集作为测试集。是否存在验证拆分尚不明确。

1.  在[Flowers 102 分类](https://towardsdatascience.com/build-train-and-deploy-a-real-world-flower-classifier-of-102-flower-types-a90f66d2092a)这篇文章中，作者使用了 6149 的测试拆分作为训练拆分。

1.  在这个[笔记本](https://github.com/bduvenhage/pytorch_challenge/blob/master/Image_Classifier_Project_Colab.ipynb)中，作者使用了 6552、818 和 819 的训练/验证/测试拆分。

了解哪些作者在做什么的唯一方法是阅读论文或代码。

在我们的笔记本（在本文中），我们使用了 6149 的拆分作为训练拆分，2040 的拆分作为验证拆分。我们没有使用测试拆分，因为我们并不真正尝试竞争。

此时，你应该感到有信心访问[这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/flowers102-classification-using-pre-trained-models.ipynb)，它执行了上述所有步骤并展示了结果。请随意在 Kaggle 或 Google Colab 上克隆该笔记本，并在 GPU 上自行运行。如果你使用 Google Colab，你需要修正一些路径，这些路径涉及数据集和预训练模型的下载以及微调模型最佳权重的存储。

下面，我们将查看我们的迁移学习实验的结果！

## 结果

结果中有一些共同的主题，我们将在下面探讨。

1.  单独经过特征提取步骤后，几乎所有网络的准确率都在 91%到 94%之间。

1.  几乎所有的网络在微调步骤后都表现非常好，准确率达到 96%以上。这表明微调步骤在迁移学习过程中确实很有帮助。

我们网络中的参数数量差异显著，vgg16 为 135M 参数，ResNet50 为 23M 参数，ResNet152 为 58M 参数。这表明我们可能可以找到一个具有类似准确率和性能的较小网络。

![使用 PyTorch 进行转移学习的实用指南](img/f62f4d7b6be49cdb920d90146304b94e.png)

图 5：转移学习过程中的训练/验证损失和准确率。来源：作者。

垂直的红线表示我们从特征提取（16 个时期）切换到微调（8 个时期）的时间点。你可以看到，当我们切换到微调时，所有网络的准确率都有所提高。这表明在特征提取后进行微调是非常有效的。

![使用 PyTorch 进行转移学习的实用指南](img/b87247c2760e4623a9c283252b70b34c.png)

图 6：在花卉分类任务上进行转移学习后，所有 3 个预训练模型的验证准确率。图中显示了第 16 个时期特征提取后的验证准确率以及每个模型在微调阶段的最佳验证准确率。来源：作者。

# 文章回顾

1.  转移学习是一种节省成本且有效的训练网络的方法，通过从一个类似但不相关的任务上的预训练网络开始。

1.  Torchvision 为研究人员提供了许多在 ImageNet 上预训练的模型，以便在转移学习过程中使用。

1.  使用预训练模型时要小心，以确保你不违反模型预训练数据集的任何许可证或使用条款。

1.  转移学习包括特征提取和微调，这必须按照特定顺序进行。

# 想了解更多？

现在我们知道如何从在不同数据集上预训练的模型开始进行定制任务的转移学习，如果我们能避免使用单独的数据集进行预训练（预文本任务），而使用我们自己的数据集，这不是更好吗？事实证明，这已经变得可行！

最近，研究人员和从业者一直在使用 [自监督学习](https://www.fast.ai/posts/2020-01-13-self_supervised.html) 作为进行模型预训练（学习预文本任务）的方式，这种方法的好处是可以在具有与目标数据集相同分布的数据集上训练模型。如果你对自监督预训练和层次化预训练感兴趣，请参阅这篇 2021 年的论文 [自监督预训练改善自监督预训练](https://arxiv.org/pdf/2103.12718.pdf)。

如果你拥有特定任务的数据，你可以使用自监督学习来进行模型的预训练，而无需担心使用 ImageNet 数据集进行预训练，因此在使用 ImageNet 数据集方面不会有问题。

# 使用的术语词汇表

+   **分类头**：在 PyTorch 中，这是一层 [nn.Linear](https://pytorch.org/docs/stable/generated/torch.nn.Linear.html)，将多个输入特征映射到一组输出特征。

+   **冻结权重**：使权重不可训练。在 PyTorch 中，通过设置 requires_grad=False 来实现。

+   **解冻（或融化）权重**：使权重可训练。在 PyTorch 中，通过设置 requires_grad=True 来实现。

+   **自监督学习**：一种训练 ML 模型的方法，使其可以在没有任何人为生成标签的数据上进行训练。这些标签可能是自动生成或机器生成的。

# 参考文献及进一步阅读

+   [关于如何在 PyTorch 中微调预训练模型的想法](https://medium.com/udacity-pytorch-challengers/ideas-on-how-to-fine-tune-a-pre-trained-model-in-pytorch-184c47185a20)

+   [深入了解深度学习：微调](https://d2l.ai/chapter_computer-vision/fine-tuning.html)

+   [自监督学习与计算机视觉](https://www.fast.ai/posts/2020-01-13-self_supervised.html)

+   [深度学习迁移学习的温和介绍](https://machinelearningmastery.com/transfer-learning-for-deep-learning/)

+   [笔记本：探索性模型分析](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/torchvision-model-exploration.ipynb)

+   [笔记本：使用迁移学习进行花卉 102 分类](https://github.com/dhruvbird/ml-notebooks/blob/main/Flowers-102-transfer-learning/flowers102-classification-using-pre-trained-models.ipynb)

**[Dhruv Matani](https://www.linkedin.com/in/dhruvbird/)** 是一位专注于 PyTorch、CNNs、视觉、语音和文本 AI 的机器学习爱好者。他在设备端 AI、模型优化和量化、ML 和数据基础设施方面是专家。著作《Efficient Deep Learning Book》中关于 Efficient PyTorch 的章节可以在 https://efficientdlbook.com/ 上找到。他的观点仅代表个人，非任何雇主的观点；无论是过去、现在还是未来。

**[Naresh](https://www.linkedin.com/in/naresh-singh-15916b17/)** 深度关注神经网络的“学习”方面。他的工作集中于神经网络架构以及简单的拓扑变化如何增强其学习能力。在他的十年职业生涯中，他曾在 Microsoft、Amazon 和 Citrix 担任工程职位。在过去的 6-7 年里，他一直参与深度学习领域。你可以在 https://medium.com/u/1e659a80cffd 上找到他。

**[Gaurav](http://www.gaurav.ai/)** 是谷歌研究部门的高级软件工程师，他领导的研究项目旨在优化大型机器学习模型，以实现从微型控制器到基于 TPU 的服务器的高效训练和推理。他的工作对 YouTube、Cloud、Ads、Chrome 等超过 10 亿活跃用户产生了积极影响。他还将与 Manning 出版社合作出版一本关于高效机器学习的书。在谷歌之前，Gaurav 在 Facebook 工作了 4.5 年，并对 Facebook 的搜索系统和大规模分布式数据库做出了重要贡献。他拥有 Stony Brook 大学的计算机科学硕士学位。

### 更多相关主题

+   [每个数据科学家都应了解的工具：实用指南](https://www.kdnuggets.com/tools-every-data-scientist-should-know-a-practical-guide)

+   [每个 AI 工程师都应了解的工具：实用指南](https://www.kdnuggets.com/tools-every-ai-engineer-should-know-a-practical-guide)

+   [使用迁移学习提升模型性能](https://www.kdnuggets.com/using-transfer-learning-to-boost-model-performance)

+   [PyTorch 初学者指南](https://www.kdnuggets.com/a-beginners-guide-to-pytorch)

+   [如何开始使用 PyTorch 进行自然语言处理](https://www.kdnuggets.com/2022/04/start-natural-language-processing-pytorch.html)

+   [实用深度学习课程来自 fast.ai！](https://www.kdnuggets.com/2022/07/practical-deep-learning-fastai-2022.html)
