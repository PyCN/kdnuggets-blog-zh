# 数据协作失败的原因（以及 4 种解决方案）

> 原文：[`www.kdnuggets.com/2023/01/collaboration-fails-around-data-4-tips-fixing.html`](https://www.kdnuggets.com/2023/01/collaboration-fails-around-data-4-tips-fixing.html)

![数据协作失败的原因（以及 4 种解决方案）](img/8fbd8a16e2ed2e3b83a73e02a2f0188d.png)

图片来源于 Freepik 上的 creativeart

数据团队越来越像软件工程团队一样工作，采用工程和开发工具来管理工作。这些工具包括版本控制系统如 Github、敏捷实践如 Kanban 和 Scrum，以及每日站会、冲刺承诺和冲刺演示等仪式。市场上已经出现了针对数据建模、测试和集成的定制解决方案（如 dbt），支持软件工程思维。这些解决方案使大型、分布式的数据团队能够发挥最佳水平。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

但在数据团队与业务其他部门之间的协作方面，仍然有很大的创新空间。

即使是最具前瞻性的数据驱动型组织，仍然依赖标准的协作工具和实践（例如 Slack、电子邮件或定期会议）来管理数据团队与业务利益相关者之间的沟通。毕竟，为什么不呢？数据团队及其工作流程难道不应该与组织中的其他职能类似吗？这种论点和行为在互动相对通用的情况下有效。但在团队动态更加复杂（且数据在每个重要对话和决策中更为核心）的情况下，依赖通用解决方案是不够的。

随着数据在业务运营中的重要性增加，数据团队成员通常需要担任多个角色。在某些情况下，他们需要像产品经理一样，理解业务用户的需求，以便改进数据平台。在其他情况下，他们需要以支持角色处理临时请求。在又一些情况下，他们需要引导新用户，并帮助他们使用可用的数据资产。

在这些情况下，通用协作工具和传统的工作管理方法很快会失效。产品团队和支持团队拥有专门设计的工具来管理他们的工作。数据团队是否也需要一个解决方案来最佳地管理利益相关者请求？或者管理他们的支持文档，或培训最终用户的工具？最优秀的数据团队常常在这个工作环节中挣扎，并最终采用为其他团队（在此情况下为产品和支持团队）构建的解决方案。

由于大多数数据工作和互动都是内部的，团队很难找到与业务利益相关者合作的正确方式，而不产生混乱或遇到尴尬的情况。

# 减少信息不对称

如果你深入探讨数据团队与其他团队之间的协作问题，你必定会发现构建者和数据资产使用者之间的信息不对称。一方面，你有对底层数据有深刻了解的数据构建者，他们知道如何操作和分析数据，并在更大的数据资产体系中对其进行上下文化。另一方面，你有数据消费者，他们通常是对业务本身具有丰富知识的领域专家，这对于提供更广泛的背景、理解数据以及发展数据平台至关重要。

以简为例。她刚刚加入了一家财富 500 强公司担任销售经理，负责管理一个分布在东南部的 15 人的销售团队。在新工作的第二天，她收到了一封同事转发的电子邮件，里面包含了多个链接到各种资源：一个包含管道信息的电子表格、Salesforce 中的各种报告，以及公司 BI 解决方案中有关个人表现的几个仪表板。在看了一会儿数据后，她意识到自己根本不知道自己在看什么，以及这些数据的含义。她给销售运营经理发了一条求助信息，销售运营经理将数据团队的合作伙伴也拉进来，数据分析师阅读了邮件，叹了口气，然后花了一个小时写了回复。他们在 JIRA 面板上创建了一个“重新评估文档”的票据。

这些数据协作问题的根本原因是构建者和消费者之间的信息不对称，这使得每个人都感到沮丧和不满。

不幸的是，最常受到这些动态影响的是初级员工或中层管理人员，因为他们通常在组织中权力较小，对数据决策的理解背景也最少。没有深入培训，这些员工容易受到信息不对称导致的沟通问题的影响。他们也容易受到“吱吱作响的轮子症候群”的影响，即执行人员和高级领导的声音自然最容易被数据团队听到（因此他们的请求和需求被优先考虑）。

# 4 个提升协作的主动技巧

为了从对数据工具和团队的大量投资中获得更好的投资回报，我们需要从根本上解决这些信息不对称问题。达到零信息不对称可能是一个理想目标，但数据团队应不断通过实践、合作伙伴关系和工具来缩小这一差距。这样可以消除摩擦，增加透明度和信任，并让每个人从公司的数据产品中获得更多收益。

以下是 4 个主动技巧，适用于希望减少信息不对称并在组织中实现更好协作的数据领导者：

1.  **将组织和团队结构与业务需求对齐**。这不仅包括报告模型，还包括数据团队的角色和职能。我们已经开始看到更多类似“数据产品经理”或“数据敏捷教练”的职位招聘。这些新职能将帮助数据团队管理协作挑战，这些挑战最终通常与人员和流程有关，而不是潜在的技术问题。

1.  **考虑投资于矩阵模型**，在这种模型中，你的团队成员——或者在某些情况下是整个小组——将与特定的业务部门对齐。这将使长期的数据计划与即时的业务需求对齐，促进知识共享，以及分析师与他们日常支持的人员之间建立更紧密、合作的关系。

1.  **从小处着手，并随着进展积累成功**。 [初次印象的力量](https://blogs.unimelb.edu.au/sciencecommunication/2020/08/29/the-science-and-power-of-first-impressions/) 不可被低估。数据团队的初步印象对其工作的接纳度至关重要，因此要考虑与关键团队成员建立良好的前期关系。通过与组织中 1-2 个关键支持者建立稳固的关系来建立自己的声誉，然后从那里扩展。

1.  **注意哪些协作工具** **可以贯穿于数据计划和数据产品的生命周期**。例如，考虑如何为以下每个类别调动人员、流程和系统。通常，某些类别有效的工具在其他类别中会失败。

    +   数据团队内的合作

    +   与团队之外其他员工的通用合作

    +   临时问题或新功能请求

    +   对数据产品的持续支持

    +   规划新的数据项目或数据产品

    +   根据业务的价值演变你的数据产品

# 结论

创新的数据团队已经开始迁移到软件工程最佳实践中，这一趋势可能在未来几年继续。 当你考虑投资数据基础设施以支持未来增长时，考虑支持业务伙伴合作的工具。

**[尼古拉斯·弗洛伊德](https://www.linkedin.com/in/nick-from-workstream/)** 是一位经验丰富的 SaaS 行业高管，拥有十多年领导专注于产品驱动增长的初创公司的经验。作为 Workstream.io 的创始人兼首席执行官，Nick 领导一家种子阶段的技术初创公司，帮助数据团队管理关键数据资产。在创办 Workstream 之前，Nick 曾担任 BetterCloud 的运营副总裁，这是一家提供领先 SaaS 运营管理解决方案的独立软件供应商。此前，Nick 在特斯拉担任高级财务职位，同时在哈佛大学获得 MBA 学位。

### 更多相关主题

+   [背景、一致性和合作对于数据科学成功至关重要](https://www.kdnuggets.com/2022/01/context-consistency-collaboration-essential-data-science-success.html)

+   [数据科学团队合作的 5 个最佳实践](https://www.kdnuggets.com/2023/06/5-best-practices-data-science-team-collaboration.html)

+   [合作的力量：开源项目如何推动人工智能进步](https://www.kdnuggets.com/2023/08/power-collaboration-opensource-projects-advancing-ai.html)

+   [将 ChatGPT 融入数据科学工作流程：技巧与最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [快速数据科学技巧和窍门学习 SAS](https://www.kdnuggets.com/2022/05/sas-quick-data-science-tips-tricks-learn.html)

+   [数据科学家使用 Jupyter Notebook 的 10 个技巧和窍门](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)
