# 用 TensorFlow 和 Keras 构建和训练你的第一个神经网络

> 原文：[`www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html`](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

![用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](img/9cb7eead0e69b3990b8f32fcef9da07c.png)

图像来源 作者

人工智能现在已经发展到了很高的水平，各种先进的 AI 模型正在不断演变，这些模型用于聊天机器人、类人机器人、自动驾驶汽车等。它已成为增长最快的技术，目标检测和目标分类如今也非常流行。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在这篇博客文章中，我们将涵盖从零开始构建和训练一个图像分类模型的完整步骤，使用卷积神经网络。我们将使用公开的[Cifar-10](https://www.cs.toronto.edu/~kriz/cifar.html) 数据集来训练模型。这个数据集的独特之处在于它包含了日常见到的物体的图像，如汽车、飞机、狗、猫等。通过对这些物体进行训练，我们将开发智能系统来对现实世界中的这些事物进行分类。它包含超过 60000 张 32x32 大小的图像，涵盖 10 种不同类型的物体。在本教程结束时，你将拥有一个能够根据视觉特征判断物体的模型。

![用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](img/318b7682220fb53e7a4b4b88bc58d4ef.png)

图 1 数据集样本图像 | 图像来源：[datasets.activeloop](https://datasets.activeloop.ai/docs/ml/datasets/cifar-10-dataset/)

我们将从零开始覆盖所有内容，因此如果你尚未了解神经网络的实际应用，这完全没有问题。本教程唯一的前提是你的时间和基础的 Python 知识。在本教程结束时，我将分享包含完整代码的协作文件。让我们开始吧！

这是本教程的完整工作流程，

1.  导入必要的库

1.  数据加载

1.  数据预处理

1.  构建模型

1.  评估模型性能

![用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](img/3504fcd934dd9aaa393cfa1477512c69.png)

图 2 完整模型流程 | 图像来源 作者

# 导入必要的库

你需要安装一些模块来开始项目。我将使用 Google Colab，因为它提供免费的 GPU 训练，最后我会提供一个包含完整代码的协作文件。

这是安装所需库的命令：

```py
$ pip install tensorflow, numpy, keras, sklearn, matplotlib
```

将库导入 Python 文件中。

```py
from numpy import *
from pandas import *
import matplotlib.pyplot as plotter

# Split the data into training and testing sets.
from sklearn.model_selection import train_test_split

# Libraries used to evaluate our trained model.
from sklearn.metrics import classification_report, confusion_matrix
import keras

# Loading our dataset.
from keras.datasets import cifar10

# Used for data augmentation.
from keras.preprocessing.image import ImageDataGenerator

# Below are some layers used to train convolutional nueral network.
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation
from keras.layers import Conv2D, MaxPooling2D, GlobalMaxPooling2D, Flatten
```

1.  **Numpy：** 它用于高效地计算包含图像的大型数据集。

1.  **Tensorflow：** 这是一个由 Google 开发的开源机器学习库。它提供了许多构建大型和可扩展模型的函数。

1.  **Keras：** 另一个高级神经网络 API，运行在 TensorFlow 之上。

1.  **Matplotlib：** 这个 Python 库创建图表和图形，提供更好的数据可视化。

1.  **Sklearn：** 它提供了进行数据预处理和特征提取的函数。它包含内置函数来查找模型的评估指标，如准确率、精确度、假阳性、假阴性等。

现在，让我们进入数据加载的步骤。

# 加载数据

本节将加载我们的数据集并执行训练-测试拆分。

**数据加载与拆分：**

```py
# number of classes
nc = 10

(training_data, training_label), (testing_data, testing_label) = cifar10.load_data()
(
    (training_data),
    (validation_data),
    (training_label),
    (validation_label),
) = train_test_split(training_data, training_label, test_size=0.2, random_state=42)
training_data = training_data.astype("float32")
testing_data = testing_data.astype("float32")
validation_data = validation_data.astype("float32")
```

cifar10 数据集直接从 Keras 数据集库加载。这些数据也被拆分为训练数据和测试数据。训练数据用于训练模型，以便它能够识别其中的模式。测试数据对模型保持未知，用于检查其性能，即模型正确预测的数据点数与总数据点数的比率。

`training_label` 包含与 `training_data` 中图像对应的标签。

然后，训练数据再次通过内置的 sklearn `train_test_split`函数拆分为验证数据。验证数据用于选择和调整最终模型。最后，所有的训练、测试和验证数据都被转换为 32 位浮点数。

现在，我们的数据集加载完成。在下一节中，我们将对数据进行一些预处理步骤。

# 数据预处理

数据预处理是开发机器学习模型时的第一步，也是最重要的一步。让我们看看如何做到这一点。

```py
# Normalization
training_data /= 255
testing_data /= 255
validation_data /= 255

# One Hot Encoding
training_label = keras.utils.to_categorical(training_label, nc)
testing_label = keras.utils.to_categorical(testing_label, nc)
validation_label = keras.utils.to_categorical(validation_label, nc)

# Printing the dataset
print("Training: ", training_data.shape, len(training_label))
print("Validation: ", validation_data.shape, len(validation_label))
print("Testing: ", testing_data.shape, len(testing_label))
```

**输出：**

```py
Training:  (40000, 32, 32, 3) 40000
Validation:  (10000, 32, 32, 3) 10000
Testing:  (10000, 32, 32, 3) 10000
```

数据集包含 10 类图像，每张图像的大小为 32x32 像素。每个像素的值范围从 0 到 255，我们需要将其标准化到 0 到 1 之间，以便于计算。之后，我们将类别标签转换为独热编码标签。这是为了将类别数据转换为数值数据，以便我们可以顺利应用机器学习算法。

现在，进入 CNN 模型的构建阶段。

# 构建 CNN 模型

CNN 模型分为 3 个阶段。第一阶段由卷积层组成，用于从图像中提取相关特征。第二阶段由池化层组成，用于减少图像的维度，同时有助于降低模型的过拟合。第三阶段由全连接层组成，将二维图像转换为一维数组。最终，这个数组被输入到全连接层中，执行最终的预测。

这里是代码。

```py
model = Sequential()

model.add(
    Conv2D(32, (3, 3), padding="same", activation="relu", input_shape=(32, 32, 3))
)
model.add(Conv2D(32, (3, 3), padding="same", activation="relu"))
model.add(MaxPooling2D((2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding="same", activation="relu"))
model.add(Conv2D(64, (3, 3), padding="same", activation="relu"))
model.add(MaxPooling2D((2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(96, (3, 3), padding="same", activation="relu"))
model.add(Conv2D(96, (3, 3), padding="same", activation="relu"))
model.add(MaxPooling2D((2, 2)))

model.add(Flatten())
model.add(Dropout(0.4))
model.add(Dense(256, activation="relu"))
model.add(Dropout(0.4))
model.add(Dense(128, activation="relu"))
model.add(Dropout(0.4))
model.add(Dense(nc, activation="softmax"))
```

我们应用了三组层，每组包含两个卷积层、一个最大池化层和一个 dropout 层。Conv2D 层接受 `input_shape` 为 (32, 32, 3)，这必须与图像的尺寸相同。

每个 Conv2D 层还采用激活函数，即 ‘relu’。激活函数用于增加系统中的非线性。简单来说，它决定了神经元是否需要根据某个阈值被激活。激活函数有很多类型，如 ‘ReLu’，‘Tanh’，‘Sigmoid’，‘Softmax’ 等，它们使用不同的算法来决定神经元的激发。

之后，添加了 Flattening 层和全连接层，中间插入了几个 Dropout 层。Dropout 层随机拒绝一些神经元对网层的贡献。其内部参数定义了拒绝的程度。主要用于避免过拟合。

以下是 CNN 模型架构的示例图像。

![用 TensorFlow 和 Keras 构建和训练您的第一个神经网络](img/2e7ac8569ec83f230107a500193881ad.png)

图 3 示例 CNN 架构 | 图片来源于 [researchgate](https://www.researchgate.net/figure/A-vanilla-Convolutional-Neural-Network-CNN-representation_fig2_339447623)

# 编译模型

现在，我们将编译并准备模型进行训练。

```py
# initiate Adam optimizer
opt = keras.optimizers.Adam(lr=0.0001)

model.compile(loss="categorical_crossentropy", optimizer=opt, metrics=["accuracy"])
# obtaining the summary of the model
model.summary()
```

**输出：**

![用 TensorFlow 和 Keras 构建和训练您的第一个神经网络](img/84fe154dca81ad2d192933c95c419c42.png)

图 4 模型总结 | 图片来源于作者

我们使用了学习率为 0.0001 的 Adam 优化器。优化器决定了模型如何根据损失函数的输出改变行为。学习率是训练过程中更新权重的步长，是一个可配置的超参数，不应过小或过大。

# 拟合模型

现在，我们将模型拟合到训练数据中，并开始训练过程。但在此之前，我们将使用图像增强技术来增加样本图像的数量。

在卷积神经网络中使用的图像增强将增加训练图像的数量，而无需新图像。它会通过产生一些变异来复制图像。这可以通过旋转图像、添加噪声、水平或垂直翻转等方式来完成。

```py
augmentor = ImageDataGenerator(
    width_shift_range=0.4,
    height_shift_range=0.4,
    horizontal_flip=False,
    vertical_flip=True,
)

# fitting in augmentor
augmentor.fit(training_data)

# obtaining the history
history = model.fit(
    augmentor.flow(training_data, training_label, batch_size=32),
    epochs=100,
    validation_data=(validation_data, validation_label),
)
```

**输出：**

![用 TensorFlow 和 Keras 构建和训练第一个神经网络](img/cfe89739896f1c3a0c892de6403495c3.png)

图 5 每个 Epoch 的准确率与损失率 | 图片来源：作者

`ImageDataGenerator()` 函数用于创建增强图像。`fit()` 用于拟合模型。它接受训练和验证数据、批次大小（Batch Size）和迭代次数（Epochs）作为输入。

批次大小（Batch Size）是指模型更新前处理的样本数量。一个关键的超参数，必须大于等于 1 并且小于等于样本数量。通常，32 或 64 被认为是最佳的批次大小。

迭代次数（Epochs）表示所有样本在网络的前向和反向传播中被处理的次数。100 次迭代意味着整个数据集通过模型 100 次，模型本身运行 100 次。

我们的模型已经训练完成，现在我们将评估它在测试集上的表现。

# 评估模型表现

在本节中，我们将检查模型在测试集上的准确率和损失率。同时，我们将绘制准确率对比 Epoch 和损失率对比 Epoch 的图表，用于训练和验证数据。

```py
model.evaluate(testing_data, testing_label)
```

**输出：**

```py
313/313 [==============================] - 2s 5ms/step - loss: 0.8554 - accuracy: 0.7545
[0.8554493188858032, 0.7545000195503235]
```

我们的模型达到了 75.34% 的准确率和 0.8554 的损失率。由于这不是最先进的模型，所以准确率可以提高。我使用这个模型来解释构建模型的过程和流程。CNN 模型的准确率取决于许多因素，如层的选择、超参数的选择、数据集的类型等。

现在我们将绘制曲线以检查模型的过拟合情况。

```py
def acc_loss_curves(result, epochs):
    acc = result.history["accuracy"]
    # obtaining loss and accuracy
    loss = result.history["loss"]
    # declaring values of loss and accuracy
    val_acc = result.history["val_accuracy"]
    val_loss = result.history["val_loss"]
    # plotting the figure
    plotter.figure(figsize=(15, 5))
    plotter.subplot(121)
    plotter.plot(range(1, epochs), acc[1:], label="Train_acc")
    plotter.plot(range(1, epochs), val_acc[1:], label="Val_acc")
    # giving title to plot
    plotter.title("Accuracy over " + str(epochs) + " Epochs", size=15)
    plotter.legend()
    plotter.grid(True)
    # passing value 122
    plotter.subplot(122)
    # using train loss
    plotter.plot(range(1, epochs), loss[1:], label="Train_loss")
    plotter.plot(range(1, epochs), val_loss[1:], label="Val_loss")
    # using ephocs
    plotter.title("Loss over " + str(epochs) + " Epochs", size=15)
    plotter.legend()
    # passing true values
    plotter.grid(True)
    # printing the graph
    plotter.show()

acc_loss_curves(history, 100)
```

输出：

![用 TensorFlow 和 Keras 构建和训练第一个神经网络](img/9c6f3061bc69ad9bb4da9b0043d8b60a.png)

图 6 准确率和损失率对比 Epoch | 图片来源：作者

在我们的模型中，我们可以看到模型过拟合了测试数据集。蓝色线表示训练准确率，橙色线表示验证准确率。训练准确率持续提升，但验证错误在 20 次迭代后变得更糟。

请查看本文中使用的 Google Colab 链接 - [链接](https://colab.research.google.com/drive/1DjicDz2iTqcs3LWiPLUcHGvH6wRNTyTm?usp=sharing)

# 结论

本文展示了从头开始构建和训练卷积神经网络的整个过程。我们得到了大约 75% 的准确率。你可以尝试调整超参数，并使用不同的卷积和池化层组合来提高准确率。你还可以尝试迁移学习，它利用像 ResNet 或 VGGNet 这样的预训练模型，并在某些情况下获得非常好的准确率。如果你想，我们可以在其他文章中详细讨论。

在那之前，继续阅读和学习。如有任何问题或建议，欢迎通过 [Linkedin](https://www.linkedin.com/in/aryan-garg-1bbb791a3/) 联系我。

**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)** 是一名 B.Tech. 电气工程学生，目前在本科最后一年。他对 Web 开发和机器学习领域感兴趣。他已经追求了这一兴趣，并渴望在这些方向上继续工作。

### 更多相关主题

+   [通过在 2022 年构建 15 个神经网络项目学习深度学习](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [逐步教程：构建你的第一个机器学习模型](https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model)

+   [使用 Bash 构建你的第一个 ETL 管道](https://www.kdnuggets.com/building-your-first-etl-pipeline-with-bash)

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [神经网络预测中置换的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)
