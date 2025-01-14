# 使用 R 进行深度学习

> 原文：[`www.kdnuggets.com/2023/05/deep-learning-r.html`](https://www.kdnuggets.com/2023/05/deep-learning-r.html)

![使用 R 进行深度学习](img/a12e3fec2c14e7244e1ca348fa621207.png)

图片来源于 Bing 图片创作者

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

谁没有被特别是 [人工智能](https://saturncloud.io/glossary/artificial-intelligence) 的技术进步所吸引，从 Alexa 到特斯拉的自动驾驶汽车以及无数其他创新？我每天都对这些进展感到惊叹，但更有趣的是，当你了解支撑这些创新的基础时。欢迎来到 [人工智能](https://saturncloud.io/glossary/artificial-intelligence) 和 [深度学习](https://saturncloud.io/glossary/deep-learning) 的无限可能。如果你一直在想这是什么，那么你来对地方了。

在本教程中，我将深入剖析术语，并带你了解如何在 R 中执行深度学习任务。需要注意的是，本文假设你对 [机器学习](https://saturncloud.io/glossary/machine-learning) 概念有一些基础了解，例如 [回归](https://saturncloud.io/glossary/regression)、分类和 [聚类](https://saturncloud.io/glossary/clustering/)。

让我们从一些围绕深度学习概念的术语定义开始：

[深度学习](https://saturncloud.io/glossary/deep-learning/) 是机器学习的一个分支，教会计算机模拟人脑的认知功能。这是通过使用人工神经网络来解开数据集中的复杂模式来实现的。通过深度学习，计算机可以对声音、图像甚至文本进行分类。

在深入了解深度学习的细节之前，了解机器学习和人工智能是什么以及这三者如何相互关联是很有帮助的。

[人工智能](https://saturncloud.io/glossary/artificial-intelligence/)：这是计算机科学的一个分支，关注开发模拟人脑功能的机器。

**机器学习**：这是人工智能的一个子集，使计算机能够从数据中学习。

根据以上定义，我们现在对深度学习如何与人工智能和机器学习相关有了一个概念。

下面的图示将帮助展示这些关系。

![R 中的深度学习](img/8a21ab290890dfba333613947867f24f.png)

关于深度学习，有两个关键点需要注意：

+   需要大量的数据

+   需要高性能的计算能力

# 神经网络

这些是深度学习模型的构建块。正如名称所示，神经一词来源于神经元，就像人脑中的神经元一样。实际上，深度神经网络的结构灵感来自于人脑的结构。

一个神经网络有一个输入层、一个隐藏层和一个输出层。这个网络被称为浅层神经网络。当我们有多个隐藏层时，它就成为了深层神经网络，其中层数可以多达 100 层。

下图展示了神经网络的样子。

![R 中的深度学习](img/fad2d6bb6d4d335f1f11aa3f711f9608.png)

这就引出了一个问题：如何在 R 中构建深度学习模型？答案是 kera！

[Keras](https://saturncloud.io/glossary/Keras)是一个开源深度学习库，使得在机器学习中使用神经网络变得简单。这个库是一个包装器，使用[TensorFlow](https://saturncloud.io/glossary/tensorflow/)作为后端引擎。然而，也有其他后端选项，如 Theano 或 CNTK。

现在让我们安装[TensorFlow](https://saturncloud.io/glossary/tensorflow)和 Keras。

从使用 reticulate 创建虚拟环境开始

```py
library(reticulate)
virtualenv_create("virtualenv", python = "/path/to/your/python3")

install.packages(“tensorflow”) #This is only done once!

library(tensorflow)

install_tensorflow(envname = "/path/to/your/virtualenv", version = "cpu")

install.packages(“keras”) #do this once!

library(keras)

install_keras(envname = "/path/to/your/virtualenv")

# confirm the installation was successful
tf$constant("Hello TensorFlow!")
```

现在我们的配置已经设置好，我们可以开始探讨如何使用深度学习解决分类问题。

# 数据简介

我将用于本教程的数据来自一个正在进行的薪资调查，该调查由[`www.askamanager.org`](https://www.askamanager.org/)进行。

表单中主要询问的是你赚多少钱，另外还有一些细节，比如行业、年龄、工作经验年数等。这些细节被收集到一个 Google 表单中，我从中获取了数据。

我们希望解决的问题是能够构建一个深度学习模型，该模型可以根据年龄、性别、工作经验年数和最高学历等信息预测某人的潜在收入。

加载我们需要的库。

```py
library(dplyr)
library(keras)
library(caTools)
```

导入数据

```py
url <- “https://raw.githubusercontent.com/oyogo/salary_dashboard/master/data/salary_data_cleaned.csv”

salary_data <- read.csv(url)
```

选择我们需要的列

```py
salary_data <- salary_data %>% select(age,professional_experience_years,gender,highest_edu_level,annual_salary) 
```

# 数据清理

还记得计算机科学中的 GIGO 概念吗？（垃圾进，垃圾出）。这个概念在这里同样适用，就像在其他领域一样。我们的训练结果在很大程度上依赖于我们使用的数据质量。这就是为什么数据清理和转换在任何[数据科学](https://saturncloud.io/glossary/data-science)项目中都至关重要。

数据清理旨在解决的一些关键问题包括；一致性、缺失值、拼写问题、异常值和数据类型。我不会详细讲解这些问题是如何处理的，这只是为了避免偏离本文的主题。因此，我将使用清理后的数据版本，但如果你有兴趣了解清理的处理方法，可以查看这篇文章。

# 数据转换

人工神经网络仅接受数值变量，而由于我们的一些变量是分类性质的，我们需要将这些变量编码为数字。这是 [数据预处理](https://saturncloud.io/glossary/data-preprocessing) 步骤的一部分，因为通常情况下，你不会得到已经准备好建模的数据。

```py
# create an encoder function
encode_ordinal <- function(x, order = unique(x)) {
  x <- as.numeric(factor(x, levels = order, exclude = NULL))
}

salary_data <- salary_data %>% mutate(
  highest_edu_level = encode_ordinal(highest_edu_level, order = c("High School","College degree","Master's degree","Professional degree (MD, JD, etc.)","PhD")),
  professional_experience_years = encode_ordinal(professional_experience_years, 
  order = c("1 year or less", "2 - 4 years","5-7 years",  "8 - 10 years", "11 - 20 years", "21 - 30 years", "31 - 40 years", "41 years or more")),
  age = encode_ordinal(age, order = c( "under 18", "18-24","25-34", "35-44", "45-54", "55-64","65 or over")),
  gender = case_when(gender== "Woman" ~ 0,
                 	gender == "Man" ~ 1))
```

看到我们想要解决一个分类问题，我们需要将年薪分成两类，以便将其作为响应变量使用。

```py
salary_data <- salary_data %>%
  mutate(categories = case_when(
	annual_salary <= 100000 ~ 0,
	annual_salary > 100000 ~ 1))

salary_data <- salary_data %>% select(-annual_salary)
```

# 切分数据

与基本的机器学习方法（回归、分类和聚类）一样，我们需要将数据分成训练集和测试集。我们使用 80-20 规则，即将 80% 的数据集用于训练，20% 用于测试。这并不是一成不变的，你可以根据需要选择任何拆分比例，但请记住，训练集应该占有足够的百分比。

```py
set.seed(123)

sample_split <- sample.split(Y = salary_data$categories, SplitRatio = 0.7)
train_set <- subset(x=salary_data, sample_split == TRUE)
test_set <- subset(x = salary_data, sample_split == FALSE)

y_train <- train_set$categories
y_test <- test_set$categories
x_train <- train_set %>% select(-categories)
x_test <- test_set %>% select(-categories)
```

Keras 接受矩阵或数组形式的输入。我们使用 as.matrix 函数进行转换。此外，我们需要缩放预测变量，然后将响应变量转换为分类数据类型。

```py
x <- as.matrix(apply(x_train, 2, function(x) (x-min(x))/(max(x) - min(x))))

y <- to_categorical(y_train, num_classes = 2)
```

## 实例化模型

创建一个顺序模型，然后我们将使用管道操作符添加层。

```py
model = keras_model_sequential()
```

## 配置层

**input_shape** 指定输入数据的形状。在我们的情况下，我们使用 **ncol** 函数获得了这一点。**activation**：在这里我们指定激活函数；这是一个将输出转换为所需非线性格式的数学函数，然后传递到下一层。

**units**：神经网络每层中的神经元数量。

```py
model %>%
  layer_dense(input_shape = ncol(x), units = 10, activation = "relu") %>%
  layer_dense(units = 10, activation = "relu") %>%
  layer_dense(units = 2, activation = "sigmoid")
```

# 配置模型学习过程

我们使用 compile 方法来完成这项工作。该函数接受三个参数；

**optimizer**：该对象指定训练过程。**loss**：这是优化过程中要最小化的函数。可选项有 mse（均方误差）、binary_crossentropy 和 categorical_crossentropy。

**metrics**：我们用来监控训练的指标。分类问题的准确率。

```py
model %>%
  compile(
	loss = "binary_crossentropy",
	optimizer = "adagrad",
	metrics = "accuracy"
)
```

# 模型拟合

我们现在可以使用 Keras 的 fit 方法来拟合模型。fit 接受的一些参数包括：

**epochs**：一个 epoch 是对训练数据集的迭代。

**batch_size**：模型将传递给它的矩阵/数组切成更小的批次，然后在训练期间进行迭代。

**validation_split**：Keras 需要切分一部分训练数据，以获取用于评估模型每个周期性能的验证集。

**shuffle**：在这里你可以指示是否希望在每个周期之前打乱训练数据。

```py
fit = model %>%
  fit(
	x = x,
	y = y,
	shuffle = T,
	validation_split = 0.2,
	epochs = 100,
	batch_size = 5
)
```

## 评估模型

要获取模型的准确度值，请使用以下的 evaluate 函数。

```py
y_test <- to_categorical(y_test, num_classes = 2)
model %>% evaluate(as.matrix(x_test),y_test)
```

## 预测

要对新数据进行预测，请使用 keras 库中的 predict_classes 函数，如下所示。

```py
model %>% predict(as.matrix(x_test))
```

# 结论

本文向你介绍了使用 Keras 在 R 中进行深度学习的基础知识。你可以深入学习以获得更好的理解，尝试调整参数，动手进行数据准备，或许还可以通过利用云计算的力量来扩展计算。

**[Clinton Oyogo](https://www.linkedin.com/in/oyogo-clinton-53359198/)** 是 [Saturn Cloud](https://saturncloud.io/) 的作者，他认为分析数据以获取可操作的见解是他日常工作中的关键部分。凭借在数据可视化、数据处理和机器学习方面的技能，他为自己作为数据科学家的工作感到自豪。

[原文](https://saturncloud.io/blog/deep-learning-with-r/)。经许可转载。

### 进一步阅读

+   [学习数据科学、机器学习和深度学习的可靠计划](https://www.kdnuggets.com/2023/01/mwiti-solid-plan-learning-data-science-machine-learning-deep-learning.html)

+   [人工智能、分析、机器学习、数据科学、深度学习…](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [15 本免费机器学习和深度学习书籍](https://www.kdnuggets.com/2022/10/15-free-machine-learning-deep-learning-books.html)

+   [KDnuggets 新闻，11 月 2 日：数据科学的现状…](https://www.kdnuggets.com/2022/n43.html)

+   [15 本更多免费机器学习和深度学习书籍](https://www.kdnuggets.com/2022/11/15-free-machine-learning-deep-learning-books.html)

+   [使用 Datawig，AWS 深度学习库进行缺失值填补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)
