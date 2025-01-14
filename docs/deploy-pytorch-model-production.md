# 将你的 PyTorch 模型部署到生产环境

> 原文：[`www.kdnuggets.com/2019/03/deploy-pytorch-model-production.html`](https://www.kdnuggets.com/2019/03/deploy-pytorch-model-production.html)

评论

**由 [Nicolás Metallo](https://www.linkedin.com/in/nicolas-metallo/?originalSubdomain=uk)，Audatex**

在上一篇关于[训练 Choripan 分类器](https://medium.com/@nicolas.metallo/train-a-choripan-classifier-with-fast-ai-v1-in-google-colab-6e438817656a)的文章中，我们讨论了如何使用 PyTorch 和 Google Colab 进行训练。接下来，我们将探讨如果你想将最近训练的模型作为 API 部署，可以采取哪些步骤。关于如何使用 Fast.ai 进行此操作的讨论[正在进行中](https://forums.fast.ai/t/using-a-fast-ai-model-in-production/12033)（[更多](https://forums.fast.ai/t/productionizing-models-thread/28353/54)），可能会持续到 PyTorch 发布其官方 1.0 版本。你可以在 Fast.ai 论坛、PyTorch 文档/论坛及其各自的 GitHub 仓库中找到更多信息。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 保存和加载模型

推荐你查看[PyTorch 文档](https://pytorch.org/tutorials/beginner/saving_loading_models.html)，它是一个很好的起点，简而言之，有两种序列化和恢复模型的方法。一种是仅加载权重，另一种是加载整个模型（包括权重）。你需要首先创建一个模型来定义其架构，否则你将得到一个只包含权重值的 `OrderedDict`。这两种选项都适用于推断和/或从以前的检查点恢复模型训练。

### 1\. 使用 `torch.save()` 和 `torch.load()`

这种保存/加载过程使用了最直观的语法，涉及的代码最少。以这种方式保存模型会使用 Python 的 [pickle](https://docs.python.org/3/library/pickle.html) 模块保存整个模块。这种方法的缺点是序列化数据绑定到模型保存时使用的特定类和确切目录结构。这是因为 pickle 不保存模型类本身，而是保存指向包含类的文件的路径，该路径在加载时使用。因此，当在其他项目或重构后使用时，代码可能会出现各种问题。

**保存模型**

```py
torch.save(learner.model, PATH)
```

有时 `pickle` 无法序列化一些模型创建函数（例如在旧版本的 Fastai 中发现的 `resnext_50_32x4d`），因此你需要使用 `dill`。这是解决方法。

```py
import dill as dill
torch.save(learner.model, PATH, pickle_module=dill)
```

你可以在这篇 文章 中阅读有关 `pickle` 限制的更多信息。一个常见的 PyTorch 约定是使用 `.pt` 或 `.pth` 文件扩展名保存模型。

**加载模型**

```py

# Model class must be defined somewhere
model = torch.load(PATH)
model.eval()

```

**2\. 使用 `state_dict`**

在 PyTorch 中，`torch.nn.Module` 模型的可学习参数（例如权重和偏差）包含在模型的 *parameters* 中（通过 `model.parameters()` 访问）。*state_dict* 只是一个 Python 字典对象，将每一层映射到其参数张量。注意，只有具有可学习参数的层（卷积层、线性层等）才会在模型的 *state_dict* 中有条目。

我们需要以与最初定义和创建模型时相同的方式重新初始化模型，并确保创建模型所需的变量、类、函数可用，无论是通过模块导入还是直接在同一个脚本/文件中。使用这种方法的一个潜在优势是，如果参数相同，你可以使用更新的脚本加载旧模型，它也是官方文档推荐的 [方法](https://pytorch.org/docs/master/notes/serialization.html)。另一个需要记住的事情是 `state_dict` 接受的是字典对象，而不是保存对象的路径，因此你不能使用 `model.load_state_dict(PATH)` 来加载。

**保存模型**

```py
torch.save(model.state_dict(), PATH)
```

**加载模型**

```py
model = TheModelClass(*args, **kwargs) # Model class must be defined somewhere
model.load_state_dict(torch.load(PATH))
model.eval() # run if you only want to use it for inference
```

加载后运行 `model.eval()`，因为你通常会有 `BatchNorm` 和 `Dropout` 层，它们在构建时默认是训练模式。如果你想恢复模型训练，则不需要调用 `model.eval()`。

由于我们已经使用 Fastai 进行了训练，我们可以调用 `[Learner.save](https://docs.fast.ai/basic_train.html#Learner.save)` 和 `[Learner.load](https://docs.fast.ai/basic_train.html#Learner.load)` 来保存和加载模型（更多信息请参见 [文档](https://docs.fast.ai/basic_train.html#Saving-and-loading-models)）。这会在后台运行 `state_dict()`，因此只会保存模型参数，而不是模型结构。这意味着你需要运行 `create_cnn` 方法来从给定的结构中获取一个预训练模型（与之前用于训练模型的结构相同，例如 *models.resnet34*），并且要为你的数据设置一个合适的自定义头。模型会保存在 `path`/`model_dir` 目录中，并且 `.pth` 扩展名会在这两个操作中自动添加。

### 使用 Flask 进行简单部署

在我们使用 Google Colab 提供的免费 GPU 训练完分类器后，我们准备在本地进行推理。我们可以在本地或云端进行推理，并且有许多不同的选项（AWS、Paperspace、Google Cloud 等）可供选择。由于我还有一些免费的 Amazon AWS 额度，我将使用一个预装了多个 ML 库的 Amazon AMI，并在 t2.medium 实例上托管。以下是一些在你的端运行 Docker 镜像的简单说明（在不进行 GPU 训练时应该差不多）。

**即开即用的 Docker 镜像**

Jupyter Docker Stacks 是一种快速启动 notebook 并使用最新库的绝佳方式。这些是即开即用的 Docker 镜像，包含 Jupyter 应用程序和交互式计算工具。在 [官方文档](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook) 中了解更多关于它们的信息。我们将使用来自 [此仓库](https://github.com/jupyter/docker-stacks) 的 **Jupyter Notebook 数据科学栈**。

一旦你 [安装](https://docs.docker.com/install/) 了 Docker，打开终端，`cd` 进入你的工作目录，然后运行

```py

$ docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jovyan/work jupyter/datascience-notebook:e5c5a7d3e52d

```

这将创建一个你可以登录的服务器，你可以在那里连接到 Jupyter notebook。你可以直接从那里运行一些命令，也可以通过获取 `container id`、在终端中输入 `docker ps`，然后运行 bash 或任何命令在 Docker 容器中执行。

```py

$ docker exec -it {container-id} /bin/bash

```

**安装 PyTorch 和 [Fastai](https://github.com/fastai/fastai)**

根据你的机器配置，你可能希望在 GPU 或 CPU 上运行推理。在我们的例子中，我们将所有操作都在 CPU 上进行，因此你需要运行以下命令来安装最新的 [PyTorch](https://pytorch.org/)。

```py

pip install torch_nightly -f https://download.pytorch.org/whl/nightly/cpu/torch_nightly.html

```

现在你可以通过 `pip install fastai` 安装 Fastai。

**创建 Flask 应用程序**

通过运行安装 Flask 库

```py

pip install -U flask

```

我们将创建一个名为 `flask_app` 的文件夹和两个新的 Python 文件 `server.py`（包含加载模型权重和运行推理服务器的代码）以及 `settings.py`（设置一些基本参数，为未来提供更多灵活性）。以下是 `flask_app/settings.py` 可能的示例。然后我们将使用 `from settings import *` 导入到 `server.py` 中。

```py
# add your custom labels
labels = ['Not Choripan', 'Choripan']
​
# set your data directory
data_dir = 'data'
​
# set the URL where you can download your model weights
MODEL_URL = 'https://s3.amazonaws.com/nicolas-dataset/stage1.pth' # example weights
​
# set some deployment settings
PORT = 8080
```

现在我们可以查看 `flask_app/server.py`。这第一部分将导入库和设置。

```py
# flask_app/server.py
​
# import libraries
print('importing libraries...')
from flask import Flask, request, jsonify
import logging
import random
import time
​
from PIL import Image
import requests, os
from io import BytesIO
​
# import fastai stuff
from fastai import *
from fastai.vision import *
import fastai
​
# import settings
from settings import * # import
​
print('done!\nsetting up the directories and the model structure...')
```

为了运行我们的单图像推理预测，我们首先需要创建一个新的模型，遵循我们训练时使用的相同文件夹结构。这就是为什么我们将基于之前在 `settings.py` 中设置的标签创建一个新的空目录。

```py

# set dir structure
def make_dirs(labels, data_dir):
    root_dir = os.getcwd()
    make_dirs = ['train', 'valid', 'test']
    for n in make_dirs:
        name = os.path.join(root_dir, data_dir, n)
        for each in labels:
            os.makedirs(os.path.join(name, each), exist_ok=True)
make_dirs(labels=labels, data_dir=data_dir) # comes from settings.py
path = Path(data_dir)

```

一旦 `path` 被定义，我们将创建一个新的 `learn` 模型并下载 [Choripan 分类器](https://medium.com/@nicolas.metallo/train-a-choripan-classifier-with-fast-ai-v1-in-google-colab-6e438817656a) 的预训练权重。

```py
# download model weights if not already saved
path_to_model = os.path.join(data_dir, 'models', 'model.pth')
if not os.path.exists(path_to_model):
    print('done!\nmodel weights were not found, downloading them...')
    os.makedirs(os.path.join(data_dir, 'models'), exist_ok=True)
    filename = Path(path_to_model)
    r = requests.get(MODEL_URL)
    filename.write_bytes(r.content)
print('done!\nloading up the saved model weights...')
fastai.defaults.device = torch.device('cpu') # run inference on cpu
empty_data = ImageDataBunch.single_from_classes(
    path, labels, tfms=get_transforms(), size=224).normalize(imagenet_stats)
learn = create_cnn(empty_data, models.resnet34)
learn = learn.load('model')

```

网上已经有很多很棒的 [教程](http://flask.pocoo.org/) 详细介绍了 Flask 的使用，因此我不会多讲 Flask 是如何工作的。我创建了一个 `predict` 函数，该函数接受作为输入的 URL，通过 `learn.predict(img)` 获得预测类别，然后返回一个 `json`。

```py

print('done!\nlaunching the server...')
# set flask params
app = Flask(__name__)
@app.route("/")
def hello():
    return "Image classification example\n"
@app.route('/predict', methods=['GET'])
def predict():
    url = request.args['url']
    app.logger.info("Classifying image %s" % (url),)

    response = requests.get(url)
    img = open_image(BytesIO(response.content))
    t = time.time() # get execution time
    pred_class, pred_idx, outputs = learn.predict(img)
    dt = time.time() - t
    app.logger.info("Execution time: %0.02f seconds" % (dt))
    app.logger.info("Image %s classified as %s" % (url, pred_class))
    return jsonify(pred_class)
if __name__ == '__main__':
    app.run(host="0.0.0.0", debug=True, port=PORT)
​
```

一旦完成这些，我们可以进入终端，切换到 `flask_app` 目录，并运行 `python server.py`。我们应该会看到类似的内容。

```py

* Serving Flask app "server" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:8080/ (Press CTRL+C to quit)
 * Restarting with stat
importing libraries...
done!
setting up the directories and the model structure...
done!
loading up the saved model weights...
done!
launching the server...
 * Debugger is active!
 * Debugger PIN: 261-786-850
```

**就这些！** 现在我们可以从终端运行类似这样的命令（我正在运行一个 AWS 实例）。以这张图片为例。

![](img/1bc93c133e36d2aa8f5cfd14033110ae.png)

```py

$ curl http://ec2-100-24-34-242.compute-1.amazonaws.com:8080/predict?url=https://media.minutouno.com/adjuntos/150/imagenes/028/853/0028853430.jpg
"Choripan"

```

从服务器端来看，它的样子是这样的

```py

[2018-11-13 16:49:32,245] INFO in server: Classifying image https://media.minutouno.com/adjuntos/150/imagenes/028/853/0028853430.jpg
[2018-11-13 16:49:33,836] INFO in server: Execution time: 1.35 seconds
[2018-11-13 16:49:33,858] INFO in server: Image https://media.minutouno.com/adjuntos/150/imagenes/028/853/0028853430.jpg classified as Choripan

```

**不错！** 你现在拥有了自己的“Choripan/Not Choripan” API。如果你想提升到下一个层次，请查看 [Flask 文档中的这个教程](http://flask.pocoo.org/docs/1.0/tutorial/deploy/) 以部署到生产环境和/或 [另一个教程](http://containertutorials.com/docker-compose/flask-simple-app.html)，如果你想将 Flask 应用程序 Docker 化（你也可以使用 [docker-compose](http://containertutorials.com/docker-compose/flask-compose.html)）。

### 部署到生产环境的其他方法

### 1\. 使用 [Clipper](http://clipper.ai) 的图像分类示例

在 ClipperTutorials GitHub 上有一个很棒的 `ipynb` 文件，你可以跟随它了解一切的基本工作原理。他们提供了一个 Docker 镜像，或者你可以直接运行他们的 Amazon AMI。遗憾的是，这仅适用于 PyTorch 0.4.0，这使得将其转换为最新预览版本的 PyTorch 和 Fastai 训练的模型变得非常麻烦。不过，它与示例预训练模型配合得很好。

**创建 ClipperConnection**

要启动 Clipper，你必须首先创建一个 `[ClipperConnection](http://docs.clipper.ai/en/develop/#clipper-connection)` 对象，并指定你想要使用的 `ContainerManager` 类型。在这种情况下，你将使用 `DockerContainerManager`。

```py

from clipper_admin import ClipperConnection, DockerContainerManager
clipper_conn = ClipperConnection(DockerContainerManager())

```

**启动 Clipper**

现在你已经拥有了一个 `ClipperConnection` 对象，你可以启动一个 Clipper 集群。

以下命令将启动 3 个 Docker 容器：

1.  查询前端：查询前端容器监听传入的预测请求，并将其调度和路由到已部署的模型。

1.  管理前端：管理前端容器管理和更新集群的内部配置状态，例如跟踪已部署的模型和已注册的应用程序端点。

1.  一个 Redis 实例：Redis 用于持久存储 Clipper 的内部配置状态。默认情况下，Redis 在端口 6380 上启动，而不是标准 Redis 默认端口 6379，以避免与已经运行的 Redis 实例发生冲突。

```py

clipper_conn.start_clipper()
clipper_addr = clipper_conn.get_query_addr()

```

查看 Clipper 启动的容器。

```py

!docker ps --filter label=ai.clipper.container.label

```

**创建应用程序**

```py

app_name = "squeezenet-classsifier"
default_output = "default"
clipper_conn.register_application(
    name=app_name,
    input_type="bytes",
    default_output=default_output,
    slo_micros=10000000)

```

当你列出已注册的应用程序时，你应该会看到新注册的 `squeezenet-classifier` 应用程序。

```py

clipper_conn.get_all_apps()

```

**加载一个示例预训练的 PyTorch 模型**

```py

from torchvision import models, transforms
model = models.squeezenet1_1(pretrained=True)

```

PyTorch 模型不能仅通过 pickle 序列化并加载。相反，它们必须使用 PyTorch 的原生序列化 API 保存。因此，你不能使用通用的 Python 模型部署工具来将模型部署到 Clipper。相反，你将使用 Clipper 的 PyTorch 部署工具进行部署。Docker 容器在启动时将从序列化的模型检查点中加载并重建模型。

**预处理**

```py

# First we define the preproccessing on the images:
normalize = transforms.Normalize(
   mean=[0.485, 0.456, 0.406],
   std=[0.229, 0.224, 0.225]
)
preprocess = transforms.Compose([
   transforms.Scale(256),
   transforms.CenterCrop(224),
   transforms.ToTensor(),
   normalize
])
# Then we download the labels:
labels = {int(key):value for (key, value)
          in requests.get('https://s3.amazonaws.com/outcome-blog/imagenet/labels.json').json().items()}

```

**定义预测函数并添加指标**

```py

import clipper_admin.metrics as metrics
def predict_torch_model(model, imgs):
    import io
    import PIL.Image
    import torch
    import clipper_admin.metrics as metrics

    metrics.add_metric("batch_size", 'Gauge', 'Batch size passed to PyTorch predict function.')
    metrics.report_metric('batch_size', len(imgs)) # TODO: Fill in the batch size

    # We first prepare a batch from `imgs`
    img_tensors = []
    for img in imgs:
        img_tensor = preprocess(PIL.Image.open(io.BytesIO(img)))
        img_tensor.unsqueeze_(0)
        img_tensors.append(img_tensor)
    img_batch = torch.cat(img_tensors)

    # We perform a forward pass
    with torch.no_grad():
        model_output = model(img_batch)

    # Parse Result
    img_labels = [labels[out.data.numpy().argmax()] for out in model_output]

    return img_labels

```

Clipper 必须从互联网下载这个 Docker 镜像，所以可能需要一点时间。

```py

from clipper_admin.deployers import pytorch as pytorch_deployer
pytorch_deployer.deploy_pytorch_model(
    clipper_conn,
    name="pytorch-model",
    version=1,
    input_type="bytes",
    func=predict_torch_model, # predict function wrapper
    pytorch_model=model, # pass model to function
    )

```

现在将生成的 `pytorch-model` 链接到之前创建的应用 `squeezenet-classsifier`。

```py

clipper_conn.link_model_to_app(app_name="squeezenet-classsifier", model_name="pytorch-model")

```

**就这样！**

**如何使用 Requests 查询 API**

```py

import requests
import json
import base64

clipper_addr = 'localhost:1337'

for img in ['img1.jpg', 'img2.jpg', 'img3.jpg']: # example with local images
  req_json = json.dumps({
          "input":
          base64.b64encode(open(img, "rb").read()).decode() # bytes to unicode
      })

  response = requests.post(
           "http://%s/%s/predict" % (clipper_addr, 'squeezenet-classsifier'),
           headers={"Content-type": "application/json"},
           data=req_jsn)

  print(response.json())

```

**停止 Clipper**

如果遇到问题并希望完全停止 Clipper，你可以通过调用 `[ClipperConnection.stop_all()](http://docs.clipper.ai/en/latest/#clipper_admin.ClipperConnection.stop_all)` 来实现。

```py

clipper_conn.stop_all()

```

当你最后列出所有 Docker 容器时，你应该会看到所有 Clipper 容器都已停止。

```py

!docker ps --filter label=ai.clipper.container.label

```

### 2\. 使用来自 Zeit 的 Now

论坛讨论中的另一个选项是使用 [Now](https://zeit.co/now) 服务，来自 [Zeit](https://zeit.co/)。你可以参考 Fast.ai 文档中的 [这个指南](https://course-v3.fast.ai/deployment_zeit.html)。我尝试过这个方法，但没有得到准确的结果（可能是由于归一化问题）。看起来很有前景。

你只需运行这些命令一次。第一次安装 Now 的 CLI（命令行界面）。

```py

sudo apt install npm # if not already installed
sudo npm install -g now

```

下一步下载基于 Fast.ai 课程 2 的模型部署 **入门包**。

```py

wget https://github.com/fastai/course-v3/raw/master/docs/production/zeit.tgz
tar xf zeit.tgz
cd zeit

```

**上传你的训练模型文件**

将你的训练模型文件（例如 `stage-2.pth`）上传到 Google Drive 或 Dropbox 等云服务。复制该文件的下载链接。**注意：**下载链接是直接启动文件下载的链接，通常与提供下载视图的分享链接不同（如有需要，使用 [`rawdownload.now.sh/`](https://rawdownload.now.sh/)）。

**根据你的模型自定义应用**

1.  打开 `app` 目录中的 `server.py` 文件，并用上面复制的 URL 更新 `model_file_url` 变量。

1.  在同一文件中，用你期望的模型类别更新 `classes = ['black', 'grizzly', 'teddys']` 行。

**部署**

在终端中，确保你在 `zeit` 目录下，然后输入：

```py

now

```

第一次运行时，它会提示输入你的电子邮件地址，并为你创建一个 Now 帐户。账户创建后，再次运行以部署你的项目。

每次使用 `now` 部署时，它都会为应用创建一个唯一的 **部署 URL**。其格式为 `xxx.now.sh`，在部署应用时会显示。

### 3\. 使用 `Torch Script` 和 `PyTorch C++ API`

这些是 PyTorch 官方 1.0 版本即将推出的一些最新变化。你可以按照[这篇](https://pytorch.org/tutorials/advanced/cpp_export.html)文档中的说明操作，或查看 Udacity 的 [Intro to Deep Learning with PyTorch](https://classroom.udacity.com/courses/ud188) 课程的最后一章，其中详细讲解了这些步骤。这只是主要步骤的概述。

```py

import torch
import torchvision
​
# An instance of your model.
model = torchvision.models.resnet18()
​
# An example input you would normally provide to your model's forward() method.
example = torch.rand(1, 3, 224, 224)
​
# Use torch.jit.trace to generate a torch.jit.ScriptModule via tracing.
traced_script_module = torch.jit.trace(model, example)
​
# Save the model
traced_script_module.save("model-resnet18-jit.pt")
```

**构建一个最小化的 C++ 应用程序**

+   按照 [这些步骤](https://pytorch.org/tutorials/advanced/cpp_export.html) 操作，构建 `example-app.cpp` 和 `CMakeLists.txt`。

+   安装 [Anaconda](https://www.anaconda.com/download/) 并在你的机器上运行 CMAKE。你可以通过他们的 [binaries](https://cmake.org/download/) 安装它，或者如果你使用的是 MacOS，输入 `brew install cmake`（通过 [这些](https://brew.sh/) 指令安装 `homebrew`）。如果你遇到 CMAKE 问题，记得直接从 [这里](https://developer.apple.com/download/more/) 下载 X-Code 命令行工具的 `.dmg`（在我的情况下是 MacOS 10.14）。

+   从 [这里](https://caffe2.ai/docs/getting-started.html?platform=mac) 安装 Caffe2 并运行 `conda install pytorch-nightly-cpu -c pytorch`

+   [构建应用程序](https://pytorch.org/tutorials/advanced/cpp_export.html)

**PyTorch, Libtorch, C++ 和 NodeJS**

查看 [`blog.christianperone.com/2018/10/pytorch-1-0-tracing-jit-and-libtorch-c-api-to-integrate-pytorch-into-nodejs/`](http://blog.christianperone.com/2018/10/pytorch-1-0-tracing-jit-and-libtorch-c-api-to-integrate-pytorch-into-nodejs/)

### 结论

我尽力总结了一些部署你最近训练的 PyTorch 模型的选项。希望这对你有帮助，期待阅读你的评论。

**简介： [Nicolás Metallo](https://www.linkedin.com/in/nicolas-metallo/?originalSubdomain=uk)** 是一位获奖企业家，拥有近 10 年的专业经验。他毕业于纽约大学，获得技术管理与创新硕士学位，并担任管理顾问和自由深度学习工程师。Nicolas 还是 INVIP Labs Inc. 的共同创始人，这是一个通过计算机视觉帮助盲人和低视力者更好地理解环境的社会企业。他对数据科学的特别兴趣在于为城市提供数据，使其更加互联、高效、韧性强、充满活力和繁荣。

[原文](https://medium.com/datadriveninvestor/deploy-your-pytorch-model-to-production-f69460192217)。已获得许可转载。

**相关：**

+   [如何在 5 分钟内使用 Flask 为机器学习模型构建 API](https://www.kdnuggets.com/2019/01/build-api-machine-learning-model-using-flask.html)

+   [PyTorch 深度学习简介](https://www.kdnuggets.com/2018/11/introduction-pytorch-deep-learning.html)

+   [深度学习备忘单](https://www.kdnuggets.com/2018/11/deep-learning-cheat-sheets.html)

### 更多相关内容

+   [将你的机器学习模型部署到云端生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [使用 MLOps 管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [从零到英雄：使用 PyTorch 创建你的第一个 ML 模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)

+   [如何成功部署数据科学项目](https://www.kdnuggets.com/2022/01/successfully-deploy-data-science-projects.html)

+   [学习如何设计和部署负责任的人工智能系统](https://www.kdnuggets.com/2023/10/teradata-design-deploy-responsible-ai-systems-whitepaper)
