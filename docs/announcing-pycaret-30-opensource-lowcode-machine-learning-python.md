# 宣布 PyCaret 3.0：开源、低代码的 Python 机器学习

> 原文：[`www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html`](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/57070b418de77d6a0c875dcd4dc8cfd0.png)

由[Moez Ali](https://medium.com/@moez-62905)使用 Midjourney 生成

# 在这篇文章中：

1.  介绍

1.  稳定的时间序列预测模块

1.  新的面向对象 API

1.  更多实验日志记录选项

1.  重构的预处理模块

1.  与最新 sklearn 版本的兼容性

1.  分布式并行模型训练

1.  加速 CPU 上的模型训练

1.  RIP：NLP 和 Arules 模块

1.  更多信息

1.  贡献者

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

# 介绍

PyCaret 是一个开源的、低代码的 Python 机器学习库，自动化机器学习工作流。它是一个端到端的机器学习和模型管理工具，极大地加快了实验周期，提高了生产力。

与其他开源机器学习库相比，PyCaret 是一个替代的低代码库，可以用几行代码替代数百行代码。这使得实验变得极其快速和高效。PyCaret 本质上是 Python 中几个机器学习库和框架的包装器。

PyCaret 的设计和简洁性灵感来源于公民数据科学家的新兴角色，这是 Gartner 首次使用的术语。公民数据科学家是可以执行简单和中等复杂分析任务的高级用户，这些任务以前需要更多的技术专长。

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/e1572d430322c40c2797a97d8afb92f7.png)

想了解更多关于 PyCaret 的信息，请查看我们的[GitHub](https://www.github.com/pycaret/pycaret)或[官方文档](http://pycaret.gitbook.io/)。

查看我们的完整[发布说明](https://github.com/pycaret/pycaret/releases/tag/3.0.0)以了解 PyCaret 3.0

# 稳定的时间序列预测模块

PyCaret 的时间序列模块现在在 3.0 版本下稳定可用。目前，它支持预测任务，但计划在未来提供时间序列异常检测和聚类算法。

```py
# load dataset
from pycaret.datasets import get_data
data = get_data('airline')

# init setup
from pycaret.time_series import *
s = setup(data, fh = 12, session_id = 123)

# compare models
best = compare_models()
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/e581e621d533a54952a565eff5302902.png)

```py
# forecast plot

plot_model(best, plot = 'forecast')
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/36b865ba95f4e975dd7fb4874d56389f.png)

```py
# forecast plot 36 days out in future

plot_model(best, plot = 'forecast', data_kwargs = {'fh' : 36})
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/59ecbb3b0f364c077d87580088f813b6.png)

# 面向对象的 API

尽管 PyCaret 是一个很棒的工具，但它并不遵循 Python 开发者通常使用的面向对象编程实践。为了解决这个问题，我们不得不重新思考我们为 1.0 版本做出的一些初始设计决策。需要注意的是，这是一个重要的变化，将需要相当大的努力来实施。现在，让我们深入探讨这将如何影响你。

```py
# Functional API (Existing)

# load dataset
from pycaret.datasets import get_data
data = get_data('juice')

# init setup
from pycaret.classification import *
s = setup(data, target = 'Purchase', session_id = 123)

# compare models
best = compare_models()
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/6f6d7be08eb8d8866f6ab8f23252696d.png)

在同一个笔记本中进行实验很棒，但如果你想用不同的设置函数参数运行不同的实验，这可能会成为问题。尽管可以做到，但之前实验的设置将会被替换。

然而，通过我们的新面向对象 API，你可以轻松地在同一个笔记本中进行多个实验并进行比较。这是因为参数与一个对象相关联，并且可以与各种建模和预处理选项相关联。

```py
# load dataset
from pycaret.datasets import get_data
data = get_data('juice')

# init setup 1
from pycaret.classification import ClassificationExperiment

exp1 = ClassificationExperiment()
exp1.setup(data, target = 'Purchase', session_id = 123)

# compare models init 1
best = exp1.compare_models()

# init setup 2
exp2 = ClassificationExperiment()
exp2.setup(data, target = 'Purchase', normalize = True, session_id = 123)

# compare models init 2
best2 = exp2.compare_models()
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/027d8ac5bb25f6bd433eed577b60e1aa.png)

exp1.compare_models

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/064f4860e5275d94a02ffdff0c513f71.png)

exp2.compare_models

在进行实验后，你可以利用`get_leaderboard`函数为每个实验创建排行榜，使其更容易进行比较。

```py
import pandas as pd

# generate leaderboard
leaderboard_exp1 = exp1.get_leaderboard()
leaderboard_exp2 = exp2.get_leaderboard()
lb = pd.concat([leaderboard_exp1, leaderboard_exp2])
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/754973a783c843c8872492dd2e5cc021.png)

输出被截断

```py
# print pipeline steps
print(exp1.pipeline.steps)
print(exp2.pipeline.steps)
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/853f3da03cb2411005e7ce6cd2b9637e.png)

# 实验记录的更多选项

PyCaret 2 可以自动使用`MLflow`记录实验。虽然它仍然是默认选项，但在 PyCaret 3 中有更多实验记录选项。最新版本中新添加的选项有`[wandb](https://wandb.ai/)`、`[cometml](https://www.comet.com/site/)`和`[dagshub](https://www.dagshub.com/)`。

要将日志记录器从默认的 MLflow 更改为其他可用选项，只需在`log_experiment`参数中传递以下之一：‘mlflow’，‘wandb’，‘cometml’，‘dagshub’。

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/cfceee83801507e40dd29b7d9d5eea20.png)

# 重构的预处理模块

预处理模块进行了全面的重设计，以提高其效率和性能，并确保与最新版本的 Scikit-Learn 兼容。

PyCaret 3 包含了几种新的预处理功能，如创新的分类编码技术、在机器学习建模中对文本特征的支持、新颖的离群值检测方法以及先进的特征选择技术。

一些新功能包括：

+   新的分类编码方法

+   处理机器学习建模中的文本特征

+   检测离群值的新方法

+   特征选择的新方法

+   保证避免目标泄露，因为整个管道现在是在折叠级别上拟合的。

# 与最新 sklearn 版本的兼容性

PyCaret 2 强烈依赖 scikit-learn 0.23.2，这使得在同一环境中无法同时使用最新的 scikit-learn 版本（1.X）和 PyCaret。

PyCaret 现在与最新版本的 scikit-learn 兼容，我们希望保持这种状态。

# 分布式并行模型训练

要在大型数据集上进行扩展，你可以在分布式模式下在集群上运行 `compare_models` 函数。为此，你可以在 `compare_models` 函数中使用 `parallel` 参数。

这得益于 [Fugue](https://github.com/fugue-project/fugue/)，这是一个开源的统一接口，用于分布式计算，让用户可以在 Spark、Dask 和 Ray 上执行 Python、Pandas 和 SQL 代码，几乎无需重写

```py
# load dataset
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# init setup
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable', n_jobs = 1)

# create pyspark session
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

# import parallel back-end
from pycaret.parallel import FugueBackend

# compare models
best = compare_models(parallel = FugueBackend(spark))
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/b023b3e0b8e88c1b9c14cacdf8601e01.png)

# 在 CPU 上加速模型训练

你可以应用 [Intel 优化](https://github.com/intel/scikit-learn-intelex) 来加速机器学习算法并提高工作流程效率。要使用 Intel 优化训练模型，需要安装 Intel sklearnex 库：

```py
# install sklearnex
pip install scikit-learn-intelex
```

要使用 Intel 优化，只需在 `create_model` 函数中传递 `engine = 'sklearnex'`。

```py
# Functional API (Existing)

# load dataset
from pycaret.datasets import get_data
data = get_data('bank')

# init setup
from pycaret.classification import *
s = setup(data, target = 'deposit', session_id = 123)
```

无智能加速的模型训练：

```py
%%time
lr = create_model('lr')
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/7f6add516ec9cc8c3a21544fd0826826.png)

使用智能加速的模型训练：

```py
%%time
lr2 = create_model('lr', engine = 'sklearnex')
```

![宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](img/593f2d29ec74c2893ba77270d255adbb.png)

模型性能存在一些差异（在大多数情况下不重要），但在 30K 行数据集上的时间改进约为 ~60%。处理更大数据集时，这一好处更为显著。

# RIP：NLP 和 Arules 模块

NLP 变化迅速，许多专门的库和公司致力于解决端到端的 NLP 任务。由于资源不足、团队现有专长以及新贡献者愿意维护和支持 NLP 和 Arules，我们决定从 PyCaret 中移除它们。PyCaret 3.0 不再包含 `nlp` 和 `arules` 模块。这些内容也已从文档中删除。你仍然可以使用旧版本的 PyCaret。

# 更多信息

[文档](https://pycaret.gitbook.io/) 入门指南

[API 参考](https://pycaret.readthedocs.io/en/latest/index.html) 详细的 API 文档

[教程](https://pycaret.gitbook.io/docs/get-started/tutorials) 刚接触 PyCaret？查看我们的官方笔记本

[社区创建并维护的笔记本](https://www.github.com/pycaret/examples)

[博客](https://pycaret.gitbook.io/docs/learn-pycaret/official-blog) 贡献者的教程和文章

[视频](https://pycaret.gitbook.io/docs/learn-pycaret/videos) 视频教程和活动

[YouTube](https://www.youtube.com/@pycaret7041) 订阅我们的 YouTube 频道

[Slack](https://join.slack.com/t/pycaret/shared_invite/zt-row9phbm-BoJdEVPYnGf7_NxNBP307w) 加入我们的 Slack 社区

[LinkedIn](https://www.linkedin.com/company/pycaret/) 关注我们的 LinkedIn 页面

[讨论](https://github.com/pycaret/pycaret/discussions) 与社区和贡献者互动

[发布说明](https://github.com/pycaret/pycaret/releases)

# 贡献者

感谢所有参与 PyCaret 3 的贡献者。

@ngupta23

@Yard1

@tvdboom

@jinensetpal

@goodwanghan

@Alexsandruss

@daikikatsuragawa

@caron14

@sherpan

@haizadtarik

@ethanglaser

@kumar21120

@satya-pattnaik

@ltsaprounis

@sayantan1410

@AJarman

@drmario-gh

@NeptuneN

@Abonia1

@LucasSerra

@desaizeeshan22

@rhoboro

@jonasvdd

@PivovarA

@ykskks

@chrimaho

@AnthonyA1223

@ArtificialZeng

@cspartalis

@vladocodes

@huangzhhui

@keisuke-umezawa

@ryankarlos

@celestinoxp

@qubiit

@beckernick

@napetrov

@erwanlc

@Danpilz

@ryanxjhan

@wkuopt

@TremaMiguel

@IncubatorShokuhou

@moezali1

**[Moez Ali](https://www.linkedin.com/in/profile-moez/)** 讨论 PyCaret 及其在实际中的使用案例。如果你想自动接收通知，可以关注 Moez 在 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1)上的更新。

[原文](https://moez-62905.medium.com/announcing-pycaret-3-0-an-open-source-low-code-machine-learning-library-in-python-7479e9190fa1) 已获得许可重新发布。

### 相关阅读

+   [宣布博客写作比赛，获胜者将获得一台 NVIDIA GPU！](https://www.kdnuggets.com/2022/11/blog-writing-contest-nvidia-gpu.html)

+   [使用 PyCaret 进行 Python 聚类介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [使用 PyCaret 进行二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [开始使用 PyCaret](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)

+   [每个机器学习工程师应掌握的 5 项机器学习技能……](https://www.kdnuggets.com/2023/03/5-machine-learning-skills-every-machine-learning-engineer-know-2023.html)

+   [KDnuggets 新闻，12 月 14 日：3 门免费的机器学习课程……](https://www.kdnuggets.com/2022/n48.html)
