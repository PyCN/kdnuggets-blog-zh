# 实时 AI 和机器学习的特征存储

> 原文：[`www.kdnuggets.com/2022/03/feature-stores-realtime-ai-machine-learning.html`](https://www.kdnuggets.com/2022/03/feature-stores-realtime-ai-machine-learning.html)

实时 AI/ML 用例 如欺诈预防和推荐系统正在上升，特征存储在成功部署这些系统到生产环境中扮演了关键角色。根据流行的开源特征存储 Feast，用户在其 [社区 Slack](https://slack.feast.dev/) 中最常问的问题之一是：**Feast 的可扩展性/性能如何**？这是因为实时 AI/ML 的特征存储最重要的特性是从在线存储到机器学习模型的特征服务速度，以便进行在线预测或评分。成功的特征存储可以满足严格的延迟要求（以毫秒为单位），一致地（考虑 p99），以及大规模（每秒高达 100K 查询，甚至每秒数百万查询，以及 GB 到 TB 级别的数据集），同时保持低的总体拥有成本和高精度。

正如我们将在这篇文章中看到的，在线特征存储的选择以及特征存储的架构在确定其性能和成本效益方面起着重要作用。因此，通常公司在选择在线特征存储之前，会进行详细的基准测试，以查看哪种架构或在线特征存储最具性能和成本效益。在这篇文章中，我们将回顾由成功部署实时 AI/ML 用例的公司构建的 DIY 特征存储的架构和基准测试，以及开源和商业特征存储。让我们开始吧。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

# 1\. 开源 Feast

首先，我们来看看基准测试数据，然后是 [Feast](https://feast.dev/) 开源特性存储的数据架构。Feast 最近进行了一项基准测试，以比较使用不同在线存储（Redis 与 Google Cloud DataStore 与 AWS DynamoDB）时的特性服务延迟，并比较使用不同机制提取特性时的速度（例如，Java gRPC 服务器、Python HTTP 服务器、lambda 函数等）。你可以在[这篇博客文章](https://feast.dev/blog/feast-benchmarks/)中找到完整的基准设置及其结果，但结论是 Feast 发现使用**Java** gRPC 服务器和**Redis**作为在线存储时表现最为出色。

![实时 AI 与机器学习的特性存储](img/abd5584f2cf3850f33c680c3d1fd0b80.png)

来源：[视频](https://www.applyconf.com/agenda/using-feast-in-a-ranking-syst)

在上图中，你可以看到一个例子，展示了流行的在线抵押贷款公司 [Better.com](https://better.com/about-us) 如何使用开源的 Feast 特性存储来实施他们的潜在客户评分排名系统，如 [Vitaly Sergey](https://www.applyconf.com/agenda/using-feast-in-a-ranking-syst)，Better.com 的高级软件工程师所展示的那样：特性从离线存储（S3、Snowflake 和 Redshift）被物化到在线存储（Redis）。此外，特性还从**流数据源**（Kafka 主题）被摄取到在线存储中。Feast 最近增加了对流数据源的支持（除了批处理数据源），目前仅支持 Redis。支持流数据源对实时 AI/ML 用例非常重要，因为这些用例依赖于最新的实时数据。例如，在 Better 的潜在客户评分用例中，新潜在客户在一天内不断被摄取，特性来自许多不同的来源，并且用于评分的实体（潜在客户）和特性会不断更新，潜在客户会被排名和重新排名。每当有新的潜在客户时，它会被模型摄取和评分，同时被摄取到在线存储中，因为我们可能希望在不久后重新排名。对于 Better.com，潜在客户在 48 小时后过期，这在 Redis 在线存储中通过简单地将 TTL（生存时间）设置为 48 小时来实现，以便在 48 小时后使实体（潜在客户）和关联特征向量过期。因此，特性存储会自动清理自己，没有过期的实体或特性占用宝贵的在线存储空间。

Feast 的另一个有趣实现是用于 [Microsoft Azure Feature Store](https://techcommunity.microsoft.com/t5/ai-customer-engineering-team/bringing-feature-store-to-azure-from-microsoft-azure-redis-and/ba-p/2918917)。你可以查看它的 [架构](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/323561i3F763F78F483587D/image-size/large?v=v2&px=999)，它运行在针对低延迟实时 AI/ML 用例优化的 Azure 云上，支持批量和流式数据源，并且集成到 Azure 数据和 AI 生态系统中。特性从批量数据源（Azure Synapse Serverless SQL、Azure Storage / ADLS）和流式数据源（Azure Event Hub）中摄取到在线存储中。如果你已经部署在 Azure 上或对 Azure 生态系统熟悉，那么这个特性存储可能是适合你的。对于在线存储，它使用 Azure Cache for Redis，并且通过 Azure Redis 的企业级方案，它包括主动地理复制，以创建全球分布的缓存，具有高达 99.999% 的可用性。此外，通过使用企业级 Flash 层，可以通过在分层内存架构中运行 Redis 实现进一步的成本降低，该架构使用内存（DRAM）和 Flash 存储（NVMe 或 SSD）来存储数据。

# 2\. Wix DIY 特性存储 - MLOps 平台的基石

以下是实现实时 AI/ML 用例的另一种特性存储架构。它是流行网站建设平台 [Wix](https://www.wix.com/) 的特性存储架构。特性存储用于实时用例，如推荐、流失与高端预测、排名和垃圾邮件分类器，被认为是其 MLOps 平台的基石。Wix 服务于超过 2 亿注册用户，其中只有一小部分是任何时间点的活跃用户。这对特性存储的实现方式产生了重大影响。让我们来看一下。以下信息基于 Ran Romano 在 Wix 负责 ML 工程时所做的 [TechTalk 演讲](https://youtu.be/E8839ENL-WY?t=2061)。Wix 特性存储中存储的数据中超过 90% 是点击流，ML 模型根据网站或用户触发。Ran 解释说，对于实时用例，延迟是一个大问题，并且对于一些生产用例，他们需要在毫秒内提取特征向量。

![实时 AI & 机器学习的特性存储](img/cdeff0e8141b685af79ab20d4ed8519e.png)

来源：[视频](https://youtu.be/E8839ENL-WY?t=2061)

原始数据存储在 AWS 的 S3 存储桶中的 Parquet 文件里，并按业务单元（例如‘编辑器’、‘餐馆’、‘预订’等）和日期进行分区。这是 Wix 数据平台的一部分，早于 Wix ML Platform 存在多年。在一个**每日构建**的批处理过程中（使用 Spark SQL，耗时从几分钟到几小时），所有用户历史特征从 S3 中提取，经过透视和按用户聚合后，导入到离线存储（Apache Hbase）。这提供了更快速的‘用户’历史查找。一旦系统检测到用户当前处于活动状态，将触发**‘预热’**过程，并将该用户的特征加载到在线存储（Redis）中，因为在线存储比离线存储小，只保存活动用户的历史。此**‘预热’**过程可能需要几秒钟。最后，在线特征存储中的特征会使用直接来自用户每个事件的实时数据（使用 Apache Storm）持续更新。

这种架构的写入与读取的比率低于 Feast 架构，在物化和在线存储方面非常高效，因为它仅在在线存储中存储活动用户的特征，而不是所有用户的特征。而且，由于在 Wix 活跃用户只是所有注册用户的一小部分，这代表了巨大的节省。然而，这也有代价。虽然从在线存储中检索特征非常快，通常在毫秒级别，但前提是这些特征已经存在于在线存储中。由于竞争条件，有些用户的特征可能因预热过程需要几秒钟，无法快速加载相关特征，从而导致该用户的评分或预测失败。这是可以接受的，只要用例不是关键流程的一部分或任务关键用例，例如批准交易或防止欺诈。这种架构也非常特定于 Wix，其中任何时间只有一小部分用户是活跃用户。

# 3\. 商业特征存储 Tecton

现在让我们来看一下商业企业特征存储 [Tecton](https://www.tecton.ai/) 的架构。如下面的图所示，除了批处理数据源和流数据源外，Tecton 还支持‘开箱即用’的实时数据源，也称为‘实时特征’或‘实时’转换。实时特征是只能在推理请求时计算的特征，例如怀疑交易的大小与最后一次交易的大小之间的差异。因此，如果在上面提到的 Better.com 的开源 Feast 的情况下，Better.com 自行开发了对实时特征的支持，那么在 Tecton 特征存储中，这种实现更为简单，因为它已经被特征存储原生支持。

与 Feast 和 Wix 功能存储类似，Tecton 也在注册表中定义特性，从而使离线和在线存储的逻辑定义只定义一次，显著减少了训练与服务的不一致性，以确保 ML 模型在生产中的高准确度。

![实时 AI 和机器学习的功能存储](img/561d61784bbd9fc747457dd5597ad0c4.png)

来源: [博客文章](https://www.tecton.ai/blog/delivering-fast-ml-features-with-tecton-and-redis-enterprise-cloud/)

关于离线存储、在线存储和基准测试：对于离线功能存储，Tecton 支持 S3；对于在线存储，Tecton 现在为客户提供了[在 DynamoDB 和 Redis Enterprise Cloud 之间的选择](https://www.tecton.ai/blog/delivering-fast-ml-features-with-tecton-and-redis-enterprise-cloud/)。在最近的演讲中，[Tecton CTO Kevin Stumpf 提供了如何选择在线功能存储的建议](https://youtu.be/osxzKxiznm4)，基于公司最近进行的基准测试。除了基准测试延迟和吞吐量外，Tecton 还基准测试了**成本**。为什么这很重要？对于高吞吐量或低延迟用例，在线存储的成本可能是整个 MLOps 平台总拥有成本中的一个大而重要的部分，因此任何节省的成本都可能是可观的。

Tecton 基准测试的核心结论是，对于 Tecton 用户典型的高吞吐量用例，[Redis Enterprise](https://redis.com/redis-enterprise-cloud/overview/)比 DynamoDB 快 3 倍，同时价格便宜 14 倍。

那么问题在哪呢？例如，如果你只有一个用例，并且它没有高或稳定的吞吐量，也没有严格的延迟要求，那么你可以选择 DynamoDB。你可以在这里找到详细信息和[Tecton 基准测试的结果](https://www.tecton.ai/blog/announcing-support-for-redis/)。

# 4\. Lightricks 使用商业功能存储 Qwak

下面是另一个功能存储架构示例，这是[Lightricks](https://www.lightricks.com/)基于商业功能存储[Qwak](https://www.qwak.com/)使用的。Lightricks 是一家开发视频和图像编辑移动应用的独角兽公司，特别以其自拍编辑应用 Facetune 而闻名。它使用功能存储来支持其推荐系统。

![实时 AI 和机器学习的功能存储](img/530653e7255e8cb8c273c5e0af7cb33e.png)

来源: [AIGuild 视频](https://www.youtube.com/watch?v=CG2vUCcvnD8&t=1915s)

如上图所示，类似于 Tecton，Qwak 功能存储也支持即插即用的三种特性来源——批处理、流处理和实时特性。

值得注意的是，Qwak 特征存储中，特征的物化直接来自原始数据源，适用于离线存储（使用 S3 上的 Parquet 文件）和在线存储（使用 Redis）。这与 Wix、Feast 或 Tecton 的特征存储示例不同，这些示例中物化到在线存储是从离线存储到在线存储进行的批处理源。为什么这很重要？这种做法的优势在于，不仅特征的转换逻辑在训练和服务流程中是一致的（如 Feast、Wix 和 Tecton 的特征存储），而且实际的转换或特征计算也以统一的方式进行，进一步减少了训练与服务的不一致性。直接从原始数据建立统一的数据管道，有可能在生产过程中确保更高的准确性。你可以在此演示文稿中找到更多关于 Qwak 的 [特征存储架构和组件](https://drive.google.com/file/d/1KfOMI9C-aitJNPdGB56L-6tA8BBp9gsl/view) 的信息。

# 摘要

在这篇文章中，我们回顾了多个实时 AI/ML 特征存储的基准和架构的关键亮点。第一个是开源 Feast，第二个是 DIY Wix 特征存储，第三个是 Tecton，第四个是 Qwak。我们还回顾了这些公司进行的一些基准测试，以了解哪个在线存储性能最佳且成本最有效，以及使用哪种机制或特征服务器从在线存储中提取特征。我们看到，根据架构、支持的特征类型和选择的组件，特征存储的性能和成本存在显著差异。

**[娜瓦·莱维](https://www.linkedin.com/in/nava1/?originalSubdomain=il)** 是 Redis 的数据科学和 MLOps 开发者倡导者。她的职业生涯始于 IDF 的研发部门，后来有幸在云计算、大数据以及深度学习/机器学习/人工智能技术初兴之际与这些领域合作并推广。娜瓦还是 MassChallenge 加速器的导师，以及 LerGO——一个基于云的教育科技企业的创始人。在闲暇时，她喜欢骑自行车、四球杂技以及阅读奇幻和科幻书籍。

### 更多相关主题

+   [2022 年特征存储峰会：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [机器学习中的替代特征选择方法](https://www.kdnuggets.com/2021/12/alternative-feature-selection-methods-machine-learning.html)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [机器学习中的特征工程实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [特征选择：科学与艺术的结合](https://www.kdnuggets.com/2021/12/feature-selection-science-meets-art.html)

+   [构建一个可操作的多变量时间序列特征工程流程](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)
