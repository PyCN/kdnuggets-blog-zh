# 2020 年 10 个惊人的机器学习项目

> 原文：[`www.kdnuggets.com/2021/03/10-amazing-machine-learning-projects-2020.html`](https://www.kdnuggets.com/2021/03/10-amazing-machine-learning-projects-2020.html)

评论

**由 [Anupam Chugh](https://www.linkedin.com/in/anupamchugh/?originalSubdomain=in)，iOS 开发者 | Medium 撰稿人 | BITS Pilani**。

![](img/3f52c957b761c877d775d26e30aff7ad.png)

*照片由 [Paul Hanaoka](https://unsplash.com/@plhnk?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。*

在过去的一年里，机器学习社区发生了很多事情。这里是最受欢迎和最具趋势的开源研究项目、演示和原型的巡礼。它涵盖了从照片编辑到自然语言处理到使用“无代码”训练模型的各个方面，希望它们能激发你在今年构建令人惊叹的人工智能产品。你还可以在 [这里找到更多机器学习项目](https://www.kdnuggets.com/2020/03/20-machine-learning-datasets-project-ideas.html)。

### 1\. 背景抠像 v2

[背景抠像 v2](https://github.com/PeterL1n/BackgroundMattingV2) 受到受欢迎的 [The World is Your Green Screen](https://github.com/senguptaumd/Background-Matting) 开源项目的启发，展示了如何实时去除或更改背景。它提供了更好的性能（4K 下 30fps 和 FHD 下 60fps），并且可以与流行的视频会议应用 Zoom 一起使用。

该技术使用额外捕获的背景帧，并将其用于恢复 alpha 遮罩和前景层。为了实时处理高分辨率图像，使用了两个神经网络。

如果你想在保留背景的同时从视频中去除一个人，这个项目肯定会很有帮助。

![](img/1435d1a137c845cb8869e71ab8c659e4.png)

*[演示](https://github.com/PeterL1n/BackgroundMattingV2)*

### 2\. SkyAR

这里还有另一个 [惊人项目](https://github.com/jiupinjia/SkyAR)，它进行视频天空替换和协调，能够自动生成现实且戏剧性的天空背景，并具有可控的风格。

基于 Pytorch，该项目部分采纳了 [pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) 项目的代码，利用天空抠像、通过光流进行运动估计和图像混合，在实时视频中提供艺术背景。

上述开源项目在电影和视频游戏中具有令人难以置信的潜力，例如添加假雨/晴天等。

![](img/3bc9c3a96cc41e80586e3d9beb560565.png)

*[源代码](https://github.com/jiupinjia/SkyAR)*

### 3\. AnimeGAN v2

将照片卡通化总是一个有趣的机器学习项目。不是吗？

[这个项目，AnimeGANv2](https://github.com/TachibanaYoshino/AnimeGANv2)，是 AnimeGAN 的改进版。它结合了神经风格迁移和生成对抗网络（GAN），以完成任务，同时确保防止高频伪影的生成。

![](img/5a5ba524f25ffc40b2aa668f4c28ac20.png)

*[来源](https://github.com/TachibanaYoshino/AnimeGANv2)*

### 4\. txtai

AI 优化的搜索引擎和 QA 聊天机器人始终是当下的需求。这正是这个[项目](https://github.com/neuml/txtai)所做的。

通过使用[sentence-transformers](https://github.com/UKPLab/sentence-transformers)、[transformers](https://github.com/huggingface/transformers)和[faiss](https://github.com/facebookresearch/faiss)，*txtai*构建了一个用于上下文搜索和抽取式问答的 AI 驱动引擎。

实质上，*txtai*支持构建文本索引以执行相似性搜索，并创建基于抽取式问答的系统。

![](img/a16e86b8346cb4fa5783f15b0dcd66d9.png)

*[来源](https://github.com/neuml/txtai)*

### 5\. 让旧照片重获新生

接下来，我们有[微软最新的照片修复项目](https://github.com/microsoft/Bringing-Old-Photos-Back-to-Life)，该项目能自动修复损坏的照片。

具体而言，它通过利用划痕检测、面部增强和其他技术，通过 PyTorch 中的深度学习实现，恢复受复杂退化影响的旧照片。

根据他们的[研究论文](https://arxiv.org/abs/2004.09484)：“我们训练了两个变分自编码器（VAEs），分别将旧照片和干净照片转换到两个潜在空间。这两个潜在空间之间的转换通过合成配对数据进行学习。这种转换对真实照片的泛化效果很好，因为在紧凑的潜在空间中缩小了领域间隙。此外，为了解决混合在一张旧照片中的多种退化情况，我们设计了一个全球分支，带有一个部分非局部块，针对结构化缺陷，如划痕和灰尘斑点，以及一个局部分支，针对非结构化缺陷，如噪声和模糊。”

该模型显然超越了传统的最先进方法，下面的演示就是明证：

![](img/7502992766a95abae6bf0fb4ab995793.png)

*[来源](https://github.com/microsoft/Bringing-Old-Photos-Back-to-Life)*

### 6\. Avatarify

Deepfake 项目在机器学习和 AI 社区引起了轰动。[这个项目](https://github.com/alievk/avatarify)展示了一个经典的例子，它让你在实时视频会议应用中创建逼真的头像。

基本上，它使用了[First Order Model](https://github.com/AliaksandrSiarohin/first-order-model)来从视频中提取动作，并通过使用光流将其应用到目标头像图像中。这样，你可以在虚拟相机上生成头像，甚至可以给经典画作动画。从埃隆·马斯克到蒙娜丽莎，你可以尽情模仿任何人以获取乐趣！

![](img/412d871fc0044077dd343e16aa7b8818.png)

*[来源](https://github.com/alievk/avatarify)*

### 7\. Pulse

这是一个展示如何从低分辨率图像生成真实面部图像的 AI 模型。

[PULSE](https://github.com/adamian98/pulse)，即自监督照片超分辨率通过生成模型的潜在空间探索，提供了基于创建真实 SR 图像的超分辨率问题的替代公式，该公式还可以正确地缩小回去。

![](img/7b36a047fadef9dbce49c48ac10b9d79.png)

*[来源](https://www.reddit.com/r/MachineLearning/comments/hciw10/r_wolfenstein_and_doom_guy_upscaled_into/)*

### 8\. pixel2style2pixel

基于[研究论文](https://arxiv.org/abs/2008.00951)《Encoding in Style: a StyleGAN Encoder for Image-to-Image Translation》，这个[项目](https://github.com/eladrich/pixel2style2pixel)使用了 Pixel2Pixel 框架，旨在通过使用相同的架构来解决各种图像到图像的任务，以避免任何可能的局部偏差。

基于一种新颖的编码器网络，该网络可以训练以将面部图像对齐到正面姿势、条件图像合成，并创建超分辨率图像。

从将几乎真实的人物从卡通图片生成到将素描或面部分割转换为照片级真实图像，你可以用这个做很多事情。

![](img/834aff9037b95b8487794ad63162005d.png)

*[来源](https://www.reddit.com/r/MachineLearning/comments/jcuch4/p_creating_real_versions_of_pixar_characters/)*

### 9\. igel

可能由于预算问题或缺乏明确的愿景，但找到具有相关机器学习专长的人始终是初创公司的挑战。尤其是因为这个领域总是不断进步的。

因此，最近出现了大量无代码机器学习平台，例如 Google 和 Apple 也发布了自己的工具集，以便快速训练模型。

这个令人愉快的开源机器学习项目正是通过允许你训练/拟合、测试和使用模型而无需编写代码来实现这一点的。虽然 GUI 拖放版本仍在开发中，但你可以通过该项目的命令行工具实现很多功能：

```py
//train or fit a model
igel fit -dp 'path_to_your_csv_dataset.csv' -yml 'path_to_your_yaml_file.yaml'

//evaluate
igel evaluate -dp 'path_to_your_evaluation_dataset.csv'

//predict
igel predict -dp 'path_to_your_test_dataset.csv'

```

还有一个单一命令*igel experiment*来结合所有阶段：训练、评估和预测。有关更多详细信息，请参阅[文档](https://github.com/nidhaloff/igel)。

![](img/9e342cb444bf3fe98419e7df56f89344.png)

*[来源](https://github.com/nidhaloff/igel)*

### 10\. Pose Animator

最后但同样重要的是，我们有一个网页动画工具。基本上，[这个项目](https://github.com/yemount/pose-animator/)使用 PoseNet 和 FaceMesh 地标结果，通过利用一些 TensorFlow.js 模型，将 SVG 矢量图像赋予生命。

你可以通过以下方式为自己的设计或骨架图像添加动画：

![](img/3a60e2cfad90130d1aad605af380ecd9.png)

*[来源](https://github.com/yemount/pose-animator/)*

[原始文章](https://medium.com/better-programming/the-top-10-trending-machine-learning-projects-of-2020-d923bf31abb7)。经许可转载。

**相关：**

+   [2020 年：充满惊人 AI 论文的一年 — 评述](https://www.kdnuggets.com/2020/12/2020-amazing-ai-papers.html)

+   [20+机器学习数据集和项目想法](https://www.kdnuggets.com/2020/03/20-machine-learning-datasets-project-ideas.html)

+   [2021 年你应该知道的所有机器学习算法](https://www.kdnuggets.com/2021/01/machine-learning-algorithms-2021.html)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败，进行审查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
