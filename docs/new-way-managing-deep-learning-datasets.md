# 管理深度学习数据集的新方式

> 原文：[`www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html`](https://www.kdnuggets.com/2022/03/new-way-managing-deep-learning-datasets.html)

![管理深度学习数据集的新方式](img/ffddfac73ba473fa6897fecbe7927bae.png)

作者提供的图像

# 什么是 Hub？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

[Hub](https://docs.activeloop.ai/) 是 Activeloop 提供的一个开源 Python 包，它将数据组织成类似 Numpy 的数组。它与 Tensorflow 和 PyTorch 等深度学习框架无缝集成，支持更快的 GPU 处理和训练。我们可以使用 Hub API 更新数据、可视化数据，并创建机器学习管道。

Hub 允许我们以闪电般的速度访问存储的图像、音频、视频和时间序列数据。数据可以存储在 GCS/S3 桶、本地存储或 Activeloop 云中。数据可以直接用于训练 Pytorch 模型，无需设置数据管道。Hub 还配备了数据版本控制、数据集搜索查询和分布式工作负载。

我与 Hub 的体验非常棒，因为我能够在几分钟内创建和上传数据到云端。在这篇博客中，我们将探讨如何使用 Hub 创建和管理数据集。

+   在 Activeloop 云上初始化数据集

+   图像处理

+   将数据推送到云端

+   数据版本控制

+   数据可视化

# Activeloop 存储

Activeloop 为开源数据集和私人数据集提供免费存储。通过推荐他人，你还可以获得最多 200 GB 的免费存储空间。Activeloop 的 Hub 与 AI 数据库接口，使我们能够可视化带有标签的数据集，并通过复杂的搜索查询有效分析数据。该平台还包含超过 100 个关于图像分割、分类和目标检测的数据集。

![管理深度学习数据集的新方式](img/e06eed015af0c30f966346024ac05645.png)

Activeloop 的 [AI 数据库](https://app.activeloop.ai/)

要创建账户，你可以通过 [Activeloop 网站](https://app.activeloop.ai/register) 注册，或输入 `!activeloop register`。该命令会要求你添加用户名、密码和电子邮件。成功创建账户后，我们将使用 `!activeloop login` 登录。现在，我们可以直接从本地机器创建和管理云数据集。

*如果你使用的是 Jupyter Notebook，请使用* ***“！”*** *，否则直接在 CLI 中添加命令，不需要使用它。*

```py
!activeloop register
!activeloop login -u  -p <password></password>
```

# 初始化 Hub 数据集

在本教程中，我们将使用 Kaggle 数据集 [Multi-class Weather](https://www.kaggle.com/pratik2901/multiclass-weather-dataset)，该数据集遵循 [(CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)。该数据集包含四个基于天气分类的文件夹：Sunrise、Sunshine、Rain 和 Cloudy。

首先，我们需要安装 hub 和 kaggle 包。kaggle 包将允许我们直接下载数据集并解压。

```py
!pip install hub kaggle
!kaggle datasets download -d pratik2901/multiclass-weather-dataset
!unzip multiclass-weather-dataset

```

在下一步中，我们将在 Activeloop 云上创建一个 hub 数据集。数据集函数也可以创建一个新的数据集或访问旧的数据集。你也可以提供一个 AWS 存储桶地址，在 Amazon 服务器上创建数据集。要在 Activeloop 上创建数据集，我们需要传递一个包含用户名和数据集名称的 URL。

**“hub://<username>/<datasetname>”**

```py
import hub
ds = hub.dataset('hub://kingabzpro/muticlass-weather-dataset')
```

# 数据预处理

在将数据处理成 hub 格式之前，我们需要准备数据。下面的代码将提取文件夹名称并将其存储在 `class_names` 变量中。在第二部分，我们将创建一个包含数据集文件夹中可用文件的列表。

```py
from PIL import Image
import numpy as np
import os

dataset_folder = '/work/multiclass-weather-dataset/Multi-class Weather Dataset'

class_names = os.listdir(dataset_folder)

files_list = []
for dirpath, dirnames, filenames in os.walk(dataset_folder):
    for filename in filenames:
        files_list.append(os.path.join(dirpath, filename))
```

**file_to_hub**函数接受三个参数：文件名、数据集和类别名称。它从每张图片中提取标签并将其转换为整数。同时，它也将图像文件转换为类似 Numpy 的数组，并将其附加到张量中。对于这个项目，我们只需要两个张量，一个用于标签，一个用于图像数据。

```py
@hub.compute
def file_to_hub(file_name, sample_out, class_names):
    ## First two arguments are always default arguments containing:
    #     1st argument is an element of the input iterable (list, dataset, array,...)
    #     2nd argument is a dataset sample
    # Other arguments are optional

    # Find the label number corresponding to the file
    label_text = os.path.basename(os.path.dirname(file_name))
    label_num = class_names.index(label_text)

    # Append the label and image to the output sample
    sample_out.labels.append(np.uint32(label_num))
    sample_out.images.append(hub.read(file_name))

    return sample_out
```

让我们创建一个使用‘png’压缩的图像张量和一个简单的标签张量。确保张量的名称应与我们在**file_to_hub**函数中提到的类似。要了解更多关于张量的知识，请参见 [API Summary - Hub 2.0](https://docs.activeloop.ai/api-basics#creating-tensors-and-adding-data)。

最后，我们将通过提供 files_lists、hub 数据集实例“ds”以及 class_names 来运行**file_to_hub**函数。这将需要几分钟，因为数据将被转换并推送到云端。

```py
with ds:
    ds.create_tensor('images', htype = 'image', sample_compression = 'png')
    ds.create_tensor('labels', htype = 'class_label', class_names = class_names)

    file_to_hub(class_names=class_names).eval(files_list, ds, num_workers = 2)
```

# 数据可视化

数据集现在可以在 [multiclass-weather-dataset](https://app.activeloop.ai/kingabzpro/muticlass-weather-dataset) 上公开访问。我们可以查看数据集的标签或添加描述，以便其他人可以了解更多关于许可证信息和数据分布的内容。Activeloop 正在不断添加新功能，以提升查看体验。

![一种管理深度学习数据集的新方法](img/1af4d1019bc8334a3537a9af9e9474ac.png)

作者提供的图像 | [multiclass-weather-dataset](https://app.activeloop.ai/kingabzpro/muticlass-weather-dataset)

我们还可以使用 Python API 访问我们的数据集。我们将使用 PIL 的 Image 函数将数组转换为图像，并在 Jupyter notebook 中显示它。

```py
Image.fromarray(ds["images"][0].numpy())
```

![一种管理深度学习数据集的新方法](img/7ba21582d694074a86e85b4647dfe649.png)

要访问标签，我们将使用包含分类信息的 `class_names`，并使用“labels”张量来显示标签。

```py
class_names = ds["labels"].info.class_names
class_names[ds["labels"][0].numpy()[0]]
>>> 'Cloudy'
```

# 提交

我们还可以创建不同的分支并管理不同的版本，类似于 Git 和 DVC。在本节中，我们将更新 `class_names` 信息并创建一个包含消息的提交。

```py
ds.labels.info.update(class_names = class_names)

ds.commit("Class names added")
>>> '455ec7d2b49a36c14f3d80d0879369c4d0a70143'
```

正如我们所见，我们的日志显示我们已成功提交了对主分支的更改。要了解更多关于版本控制的信息，请查看[数据集版本控制 - Hub 2.0](https://docs.activeloop.ai/getting-started/step-8-dataset-version-control)。

```py
log = ds.log()
---------------
Hub Version Log
---------------

Current Branch: main

Commit : 455ec7d2b49a36c14f3d80d0879369c4d0a70143 (main) 
Author : kingabzpro
Time   : 2022-01-31 08:32:08
Message: Class names added
```

你还可以通过 Hub UI 查看所有的分支和提交。

![管理深度学习数据集的新方法](img/842d04faa1f9fe781b6f2fc826d26e67.png)

作者提供的 Gif

# 结论

Hub 2.0 提供了新的数据管理工具，使机器学习工程师的工作变得轻松。Hub 可以与 AWS/GCP 存储集成，为深度学习框架如 PyTorch 提供直接的数据流。它还通过 Activeloop 云提供互动可视化和用于跟踪机器学习实验的版本控制。我认为 Hub 将成为未来数据管理的 MLOps 解决方案，因为它将解决数据科学家和工程师日常面临的许多核心问题。

在这篇博客中，我们了解了 Hub 及如何创建和推送数据到 Activeloop 云。下一步自然就是使用相同的数据集来训练模型并部署到生产环境中。因此，如果你有兴趣了解更多并想训练一个图像分类模型，可以查看[在 Pytorch 中训练图像分类模型](https://docs.activeloop.ai/hub-tutorials/training-an-image-classification-model-in-pytorch)。

# 使用 Hub 进行深度学习项目

+   [创建对象检测数据集](https://docs.activeloop.ai/hub-tutorials/creating-object-detection-datasets)

+   [创建复杂的数据集](https://docs.activeloop.ai/hub-tutorials/creating-complex-datasets)

+   [创建视频数据集](https://docs.activeloop.ai/hub-tutorials/creating-video-datasets)

+   [创建时间序列数据集](https://docs.activeloop.ai/hub-tutorials/creating-time-series-datasets)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan))是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作和撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个 AI 产品，帮助那些与心理疾病斗争的学生。

### 更多相关主题

+   [一本将彻底改变你组织数据处理方式的新书…](https://www.kdnuggets.com/2022/02/manning-new-book-revolutionize-way-organization-approaches-data.html)

+   [忘掉 ChatGPT，这款新的 AI 助手远超前者，将…](https://www.kdnuggets.com/2023/08/forget-chatgpt-new-ai-assistant-leagues-ahead-change-way-work-forever.html)

+   [作为数据科学家管理可重用的 Python 代码](https://www.kdnuggets.com/2021/06/managing-reusable-python-code-data-scientist.html)

+   [通过 MLOps 管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [使用 Poetry 与 Conda & Pip 管理 Python 依赖](https://www.kdnuggets.com/managing-python-dependencies-with-poetry-vs-conda-pip)

+   [管理数据科学项目的 4 个步骤](https://www.kdnuggets.com/2022/05/4-steps-managing-data-science-project.html)
