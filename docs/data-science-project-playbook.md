# 数据科学项目手册

> 原文：[`www.kdnuggets.com/2017/03/data-science-project-playbook.html`](https://www.kdnuggets.com/2017/03/data-science-project-playbook.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**作者：Matthew Coffman, High Alpha。**

> 本文是对"[塑造产品战略的四大关键思想](https://medium.com/high-alpha/four-big-ideas-shaping-product-strategy-today-8517d5d9b91b#.64qsy6v6q)"的跟进。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

![](img/80f090fb152ca67f710027970206a5c8.png)

Slack 数据工程部负责人**Josh Wills**对数据科学持务实的观点。

鉴于我们多么频繁地听到或讨论机器学习和人工智能作为初创公司区分自身的方式，我一直在努力找出可以让初创公司识别和创建有价值的数据科学项目的初步步骤。我最近参加了[MLconf 2016](http://mlconf.com/mlconf-2016-sf/)，这是一个汇集了学术界、产品领导者和实践数据科学家的活动。

我发现这次经历既令人鼓舞又让人谦逊，看到了更大和更成熟的公司如何应对这些挑战。讲座内容涵盖了各种方案、反思和建议。由于我更偏向工程思维，有些内容超出了我的理解。不过，总体而言，我从讲者和其他与会者那里学到了很多。我带着一些关于我们作为初创产品领导者如何最佳应对数据科学挑战的思考离开了。我想尝试将这些想法组织成一本其他人可以用来入门的手册。

### **第一步：了解数据科学领域**

当然，数据科学/机器学习/人工智能已经成为一个独立的行业，并取得了关键的规模。各种平台、工具和算法的供应商不乏其人，可以应对几乎任何应用。另一方面，找到有能力处理你挑战的专家则是另一回事。大公司之间的竞争激烈，争夺彼此的数据科学家。这给我们这些希望打造下一个伟大的聊天机器人或洞察驱动应用的人留下的机会不多。

如果你很幸运已经在团队中有一名数据科学家，那么让这个人成为你规划和执行项目的合作伙伴。同时，理解数据科学家在构建和扩展应用程序的其他复杂部分时，往往没有其他工程师那样的专业知识和经验。确保在项目规划中让数据科学家和工程师都参与，以最大限度地确保成功。

在没有与主题专家建立关系的情况下，产品领导者如何仍然追求对其应用程序有意义的数据驱动功能？我主张采用一种极其实用的方法——与大多数其他产品规划过程一样，为一系列权衡做好准备。幸运的是，工具和平台的高度竞争环境意味着几乎任何理想化的功能都可以实现。因此，产品领导者的重点需要放在寻找正确的功能和权衡其影响上。

### **第二步：识别 MVDP**

[Josh Wills](https://twitter.com/josh_wills?lang=en)，Slack 的数据工程主管，在会议上做了一个非常[有趣的演讲](https://www.youtube.com/watch?v=89S0jxu7Lz0&feature=youtu.be)。Slack 有很多有趣的方式来审视产品，特别是他们非常专注于解决问题而不是仅仅销售产品。*每一分努力都集中在解决客户的具体业务问题上*。

作为产品领导者，我们经常使用最小可行产品的概念来确定解决客户问题所需的最低工作量。Josh 提倡最小可行数据产品作为平衡“可能的愿景与必要性”的一种方式。Slack 选择了一小部分功能——例如频道推荐——然后努力找出验证它是否改善了客户体验的最低努力、最容易测量的方法。

最小可行数据产品需要以下条件才能成功：

1.  为客户提供真实价值——增强或加深他们与产品的关系

1.  可用且充足的数据——即使是最好的算法也无法在没有数据的情况下执行

1.  交付的实际性——换句话说，团队是否能在现有资源和现成解决方案的情况下交付能力？

产品领导者可以从头脑风暴功能开始，优先考虑那些对客户价值最大的功能。与工程领导者（以及可能的数据科学专业人士）合作，讨论实现特定功能所需的数据和资源。

不要害怕缩小范围——*这里的目标是构建能够迅速证明功能对客户有价值的东西*。一旦确定了这一价值，就可以在其基础上添加更多的复杂性。然而，对于数据科学项目而言，尤其重要的是在前期防止过多的复杂性，以减少项目无法启动的可能性。

### **第三步：寻找工程友好的解决方案**

我们的工程和产品团队在构建和交付功能方面表现出色，但他们可能还没有足够的经验和专业知识来完全独立完成这些工作。数据科学家提供对特定数据集的可能性、构建功能的正确工具/技术的更高层次的理解，然后（同样重要的是）如何将其投入生产。幸运的是，互联网上充满了可以帮助公司推出数据科学功能的课程、学习材料、应用程序和 API，即使他们没有自己的数据科学家。

现在，几乎每种算法和技术都可以现成使用。我们工程团队的真正关注点应集中在数据准备和加载、模型/算法/工具的训练和选择，以及将这些工具实施到生产环境中。团队不应该从头开始构建任何东西——那是浪费宝贵的资源。

在确定了最低可行的数据产品后，是时候寻找最实用的方法来构建该功能。虽然没有工具或平台适合每一种用例，但有一些良好的起点可以供你的团队利用：

+   **通用机器学习平台/预测服务：** [Google Prediction API](https://cloud.google.com/prediction)、[Amazon Machine Learning API](https://aws.amazon.com/machine-learning/)、[Microsoft Azure Machine Learning API](https://azure.microsoft.com/en-us/services/machine-learning/) 和 [BigML](http://www.bigml.com/) 是一些服务的例子，这些服务提供 API，你的工程师可以将数据输入到预构建或定制的模型中，这些模型可以快速测试并插入到你的应用程序中。这类服务适用于预测用户行为、标记大数据集中的用户或产品、优先排序数据集等。

+   **特定用途的人工智能平台**：这一领域似乎具有最大的动力——在互联网搜索你的用例可能会发现各种针对开发者解决特定问题的新应用程序。主要供应商包括 [IBM Watson](https://www.ibm.com/watson/developercloud/services-catalog.html)（语音识别、图像识别、翻译）和 [Google Cloud](https://cloud.google.com/products/machine-learning/)（语音、文本、图像及其他服务），每天都有许多新兴初创公司涌现。

+   **博客、目录和机器学习社区新闻：** 与其他开发领域一样，互联网提供了一个健康的起点，其他团队在数据科学项目中分享了他们的成功和失败。我推荐 KDnuggets 和[O’Reilly](https://www.oreilly.com/topics/data)作为知识探索的良好起点。

在所有可能的情况下，无论构建什么，都很可能包括工具和平台的组合。这就是为什么专注于只做团队需要交付的最小客户价值的工作是有帮助的——只要所有各方都围绕这一目标保持一致，这将有助于防止数据科学项目失控。

### **第四步：衡量和迭代**

就像任何其他功能一样，从一开始就需要明确如何衡量客户满意度。鉴于数据科学项目的额外复杂性，创建一个紧密的客户反馈和功能迭代循环更加重要。由于对数据和潜在复杂模型的巨大依赖，团队可能难以准确找出某个功能效果不如预期的原因。产品领导者在了解每次迭代预期的工作量方面发挥着重要作用，并可能需要在额外工作的价值方面做出判断。有时，如果发现需要的工作量过大或结果仍然不可预测，可能需要完全放弃某个功能。

一个优秀的产品领导者与客户和数据保持着严谨的关系。在测试新的数据科学驱动功能时，观察这两个来源的反馈是至关重要的。

### **结论：无法回头**

Josh Wills 强调观察到，对许多公司来说，数据科学工作只是他们产品投资组合的一部分。他继续说，在大多数情况下，那些工作中的一两个将为所有其他工作支付费用。开始数据科学真的很困难——他称之为一种信仰行为。他说像 Facebook、Google 和 Amazon 这样的公司已经过了最初的困难，现在数据科学几乎驱动着他们所做的一切。机器学习和数据科学是这些公司用来创造价值的工具，而用户体验成为通过自动化提供见解和机会以使客户生活更轻松的功能。

从实际的角度来看，产品领导者可以开始将数据科学功能整合到自己的应用程序中。尽管追赶大公司可能是一个挑战，但我们需要专注于我们自己客户的需求，并以最合理的方式改善他们的体验。

**简介：[Matthew Coffman](https://www.linkedin.com/in/matthew-coffman-6848689/)** 领导 High Alpha 的产品部门，这是一个专注于构思、启动和扩展下一代企业云公司的风险投资工作室。他与其投资组合公司合作，建立团队、规划技术，并解决能够帮助他们更快增长的问题。他非常高兴能成为印第安纳波利斯技术社区的一部分！

[原文](https://medium.com/high-alpha/a-startup-playbook-for-data-science-projects-6492febc19e4#.bjryh66dh)。经许可转载。

**相关：**

+   为数据团队奠定基础

+   从敏捷数据科学团队中获得现实世界的成果

+   创造力在数据科学中的重要性

### 更多相关主题

+   [停止学习数据科学以寻找目标，寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [成为优秀数据科学家需要的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)
