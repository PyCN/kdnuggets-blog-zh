# 如何有效使用 Docker 标签管理镜像版本

> 原文：[`www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively`](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

![如何有效使用 Docker 标签管理镜像版本](img/dd653bcf22dab12e9752b41efb278659.png)

编辑器提供的图片 | Midjourney 和 Canva

了解如何使用 Docker 标签来管理不同版本的 Docker 镜像，确保一致和有序的开发工作流。本指南涵盖了标记、更新和维护 Docker 镜像的最佳实践。

## 前提条件

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

在你开始之前：

+   你应该在开发环境中安装 Docker。如果还没有，[获取 Docker](https://docs.docker.com/get-docker/)。

+   一个你想 Docker 化的示例应用。如果你愿意，可以使用 [GitHub 上的这个示例](https://github.com/balapriyac/data-science-tutorials/tree/main/docker/minimal-img-python-apps)。

## 标记 Docker 镜像

Docker 标签是一个指向存储库中特定镜像的标签。默认情况下，如果未指定标签，Docker 使用 `latest` 标签。但如果你正在开发应用并在版本间进行改进，你可能希望添加更明确的标签。这些标签对于区分不同版本或状态的镜像非常有用。

假设你有一个 Python 项目：一个用于库存管理的 Flask 应用，所有必需的文件都在项目目录中：

```py
project-dir/
├── app.py
├── Dockerfile
├── requirements.txt 
```

你可以在构建镜像时进行标签，如下所示：

```py
$ docker build -t image_name:tag_name 
```

现在让我们构建 `inventory-app` 镜像并给它打标签：

```py
$ docker build -t inventory-app:1.0.0 . 
```

在这里：

+   `inventory-app` 是存储库名称或镜像名称。

+   `1.0.0` 是此特定镜像构建的标签。

你可以运行 `docker images` 命令来查看带有指定标签的新构建镜像：

```py
$ docker images
REPOSITORY      TAG           IMAGE ID       CREATED        SIZE
inventory-app   1.0.0         32784c60a992   6 minutes ago   146MB 
```

你也可以给现有的镜像打标签，如下所示：

```py
$ docker tag inventory-app:1.0.0 inventory-app:latest
```

在这里，我们将现有的镜像 `inventory-app:1.0.0` 标记为 `inventory-app:latest`。你会看到我们有两个不同标签的 inventory-app 镜像，并且它们的镜像 ID 相同：

```py
$ docker images
REPOSITORY      TAG           IMAGE ID       CREATED        SIZE
inventory-app   1.0.0         32784c60a992   6 minutes ago   146MB
inventory-app   latest        32784c60a992   5 minutes ago   146MB
```

## 推送已标记的镜像到存储库

要共享你的 Docker 镜像，你可以将它们推送到 Docker 存储库（如 DockerHub）。你可以注册一个免费的 DockerHub 账户，登录并推送镜像。你应该首先登录 DockerHub：

```py
$ docker login 
```

系统会提示你输入用户名和密码。经过身份验证成功后，你可以使用 `docker push` 命令推送已标记的镜像。

确保你的仓库名称与你的 Docker Hub 用户名或组织匹配。如果你的 Docker Hub 用户名是 `user`，并且你想推送镜像的版本为 1.0.1，你可以将镜像标记为 `user/inventory-app:1.0.1`：

```py
$ docker tag user/inventory-app:1.0.1
$ docker push user/inventory-app:1.0.1 
```

当你需要使用特定版本的镜像时，可以使用标签来拉取它：

```py
$ docker pull user/inventory-app:1.0.1 
```

## Docker 镜像标记的最佳实践

这里是标记 Docker 镜像时的一些最佳实践：

+   **使用语义版本控制**：遵循如 `MAJOR.MINOR.PATCH`（1.0.0, 1.0.1）这样的版本控制方案。这有助于识别更改的重要性。

+   **避免在生产环境中使用 `latest`**：在生产部署中使用明确的版本标签。

+   **在 CI/CD 管道中自动化标记**：将 Docker 标记集成到你的 CI/CD 管道中，以确保一致和自动的版本控制。

+   **在标签中包含元数据**：如果有意义的话，可以在标签中添加构建号、提交哈希或日期。

遵循这些实践来使用 Docker 标签，你可以保持一组干净、组织良好且有版本控制的 Docker 镜像。

## 额外资源

这里有几个你会觉得有用的资源：

+   [Docker 镜像标签](https://docs.docker.com/reference/cli/docker/image/tag/)

+   [构建、标记并发布镜像](https://docs.docker.com/guides/docker-concepts/building-images/build-tag-and-publish-an-image/)

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术作者。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正在通过撰写教程、操作指南、观点文章等来学习和与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 主题扩展

+   [管理过多的 Python 版本？Pyenv 来救援](https://www.kdnuggets.com/too-many-python-versions-to-manage-pyenv-to-the-rescue)

+   [如何有效管理 Pandas 中的分类数据](https://www.kdnuggets.com/how-to-manage-categorical-data-effectively-with-pandas)

+   [如何有效使用 Pandas GroupBy](https://www.kdnuggets.com/2023/01/effectively-pandas-groupby.html)

+   [数据分析：分析数据的四种方法及其应用](https://www.kdnuggets.com/2023/04/data-analytics-four-approaches-analyzing-data-effectively.html)

+   [创建一个简单的 Docker 数据科学镜像](https://www.kdnuggets.com/2023/08/simple-docker-data-science-image.html)

+   [数据可视化：有效呈现复杂信息](https://www.kdnuggets.com/data-visualization-presenting-complex-information-effectively)
