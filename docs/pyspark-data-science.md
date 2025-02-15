# PySpark 数据科学

> 原文：[`www.kdnuggets.com/2023/02/pyspark-data-science.html`](https://www.kdnuggets.com/2023/02/pyspark-data-science.html)

![PySpark 数据科学](img/2ebe106d056da2fc385c3f18101024ef.png)

图片由作者提供

[PySpark](https://spark.apache.org/docs/latest/api/python/index.html) 是 Apache Spark 的 Python 接口。它是一个开源库，允许你构建 Spark 应用程序，并在分布式环境中使用 PySpark shell 分析数据。你可以使用 PySpark 进行批处理、运行 SQL 查询、数据框、实时分析、机器学习和图形处理。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

**使用 Spark 的优势：**

1.  内存缓存支持实时计算和低延迟。

1.  它可以通过多种方式部署：Spark 的集群管理器、Mesos 和通过 Yarn 的 Hadoop。

1.  提供用户友好的 API 适用于所有流行的语言，隐藏了运行分布式系统的复杂性。

1.  它在内存中比 Hadoop MapReduce 快 100 倍，在磁盘上快 10 倍。

在这个基于代码的教程中，我们将学习如何初始化 Spark 会话、加载数据、更改 Schema、运行 SQL 查询、可视化数据，并训练机器学习模型。

# 入门指南

如果你想在本地机器上使用 PySpark，需要安装 Python、Java、Apache Spark 和 PySpark。如果想避免这些步骤，你可以使用 [Google Colab](https://colab.research.google.com/) 或 [Kaggle](https://www.kaggle.com/)。这两个平台都自带预安装的库，你可以在几秒钟内开始编程。

在 Google Colab Notebook 中，我们将首先安装 `pyspark` 和 `py4j`。

```py
%pip install pyspark py4j -qq
```

之后，我们需要提供会话名称以初始化 Spark 会话。

```py
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName('PySpark_for_DataScience').getOrCreate()
```

# 使用 PySpark 导入数据

在这个教程中，我们将使用来自 Kaggle 的 [Global Spotify Weekly Chart](https://www.kaggle.com/datasets/kabhishm/global-spotify-weekly-chart)。它包含有关艺术家和 Spotify 全球周榜上歌曲的信息。

就像 Pandas 一样，我们可以使用 `spark.read.csv` 函数从 CSV 加载数据到数据框，并使用 `printSchema()` 函数显示 Schema。

```py
df = spark.read.csv(
    '/content/spotify_weekly_chart.csv',
    sep = ',',
    header = True,
    )

df.printSchema()
```

**输出：**

正如我们所观察到的，PySpark 已将所有列加载为字符串。为了进行探索性数据分析，我们需要更改 Schema。

```py
root
 |-- Pos: string (nullable = true)
 |-- P+: string (nullable = true)
 |-- Artist: string (nullable = true)
 |-- Title: string (nullable = true)
 |-- Wks: string (nullable = true)
 |-- Pk: string (nullable = true)
 |-- (x?): string (nullable = true)
 |-- Streams: string (nullable = true)
 |-- Streams+: string (nullable = true)
 |-- Total: string (nullable = true)
```

# 更新 Schema

要更改模式，我们需要创建一个新的数据模式，并将其添加到`StructType`函数中。你需要确保每个列字段具有正确的数据类型。

```py
from pyspark.sql.types import *

data_schema = [
               StructField('Pos', IntegerType(), True),
               StructField('P+', StringType(), True),
               StructField('Artist', StringType(), True),
               StructField('Title', StringType(), True),
               StructField('Wks', IntegerType(), True),
               StructField('Pk', IntegerType(), True),
               StructField('(x?)', StringType(), True),
               StructField('Streams', IntegerType(), True),
               StructField('Streams+', DoubleType(), True),
               StructField('Total', IntegerType(), True),
            ]

final_struc = StructType(fields = data_schema)
```

然后，我们将使用额外的参数`schema`加载 CSV 文件。之后，我们将打印模式以检查是否做了正确的更改。

```py
df = spark.read.csv(
    '/content/spotify_weekly_chart.csv',
    sep = ',',
    header = True,
    schema = final_struc 
    )

df.printSchema()
```

**输出：**

如我们所见，列的数据类型各不相同。

```py
root
 |-- Pos: integer (nullable = true)
 |-- P+: string (nullable = true)
 |-- Artist: string (nullable = true)
 |-- Title: string (nullable = true)
 |-- Wks: integer (nullable = true)
 |-- Pk: integer (nullable = true)
 |-- (x?): string (nullable = true)
 |-- Streams: integer (nullable = true)
 |-- Streams+: double (nullable = true)
 |-- Total: integer (nullable = true)
```

# 探索数据

你可以使用`toPandas()`函数将数据作为数据框进行探索。

> **注意：** 我们使用了`limit`来显示前五行。这与 SQL 命令类似。这意味着我们可以使用 PySpark Python API 执行 SQL 命令以运行查询。

```py
df.limit(5).toPandas()
```

![PySpark 数据科学](img/edeb0c2579ecd4916d885888acf5f4e5.png)

就像 pandas 一样，我们可以使用`describe()`函数来显示数据分布的摘要。

```py
df.describe().show()
```

![PySpark 数据科学](img/53998c47fa57c853807d74126e467011.png)

`count()`函数用于显示行数。阅读 [Pandas API on Spark](https://spark.apache.org/docs/latest/api/python/reference/pyspark.pandas/index.html) 以了解类似的 API。

```py
df.count()
# 200
```

# 列操作

你可以使用`withColumnRenamed`函数重命名列。它需要旧名称和新名称作为字符串。

```py
df = df.withColumnRenamed('Pos', 'Rank')

df.show(5)
```

![PySpark 数据科学](img/3d14a8dd2bf66d7283cfb5810f0020e9.png)

要删除单个或多个列，你可以使用`drop()`函数。

```py
df = df.drop('P+','Pk','(x?)','Streams+')

df.show(5)
```

![PySpark 数据科学](img/6d555820029802ccae6b87b18c37cf10.png)

你可以使用`.na`来处理缺失值。在我们的例子中，我们删除了所有缺失值的行。

```py
df = df.na.drop()
## Or
#data.na.replace(old_value, new_vallue)
```

# 数据分析

对于数据分析，我们将使用 PySpark API 来转换 SQL 命令。在第一个示例中，我们选择了三列，并显示了前 5 行。

```py
df.select(['Artist', 'Artist', 'Total']).show(5)
```

![PySpark 数据科学](img/068bdd7d061c006700e66d9a39e063b1.png)

对于更复杂的查询，我们将筛选“Total”大于或等于 6 亿到 7 亿的值。

> <fo not="" size="+1">**注意：** 你还可以使用`df.Total.between(600000000, 700000000)`来筛选记录。</fo>

```py
from pyspark.sql.functions import col, lit, when

df.filter(
    (col("Total") >= lit("600000000")) & (col("Total") <= lit("700000000"))
).show(5)
```

![PySpark 数据科学](img/7858ab674eb36cec9d98c63941350995.png)

编写 if/else 语句，使用`when`函数创建分类列。

```py
df.select('Artist', 'Title', 
            when(df.Wks >= 35, 1).otherwise(0)
           ).show(5)
```

![PySpark 数据科学](img/ad64ce3b32231f59d3f35b64409056f1.png)

你可以将所有 SQL 命令作为 Python API 运行完整的查询。

```py
df.select(['Artist','Wks','Total'])\
        .groupBy('Artist')\
        .mean()\
        .orderBy(['avg(Total)'], ascending = [False])\
        .show(5)
```

![PySpark 数据科学](img/6b9760cfc2a485cf69d86f84929f0699.png)

# 数据可视化

让我们使用上述查询，并尝试将其显示为条形图。我们绘制了“艺术家与平均歌曲播放量”的关系，并且仅显示前七名艺术家。

如果你问我，这太棒了。

```py
vis_df = (
    df.select(["Artist", "Wks", "Total"])
    .groupBy("Artist")
    .mean()
    .orderBy(["avg(Total)"], ascending=[False])
    .toPandas()
)

vis_df.iloc[0:7].plot(
    kind="bar",
    x="Artist",
    y="avg(Total)",
    figsize=(12, 6),
    ylabel="Average Average Streams",
) 
```

![PySpark 数据科学](img/53998c47fa57c853807d74126e467011.png)

# 保存结果

在处理数据和运行分析之后，是时候保存结果了。

你可以将结果保存为所有流行的文件类型，例如 CSV、JSON 和 Parquet。

```py
final_data = (
    df.select(["Artist", "Wks", "Total"])
    .groupBy("Artist")
    .mean()
    .orderBy(["avg(Total)"], ascending=[False])
)

# CSV
final_data.write.csv("dataset.csv")

# JSON
final_data.write.save("dataset.json", format="json")

# Parquet
final_data.write.save("dataset.parquet", format="parquet") 
```

# 数据预处理

在这一部分，我们正在为机器学习模型准备数据。

1.  **类别编码**：使用 StringIndexer 将类别列转换为整数。

1.  **组装特征**：将重要特征组装成一个向量列。

1.  **缩放**：使用 StandardScaler 缩放函数对数据进行缩放。

```py
from pyspark.ml.feature import (
    VectorAssembler,
    StringIndexer,
    OneHotEncoder,
    StandardScaler,
)

## Categorical Encoding
indexer = StringIndexer(inputCol="Artist", outputCol="Encode_Artist").fit(
    final_data
)
encoded_df = indexer.transform(final_data)

## Assembling Features
assemble = VectorAssembler(
    inputCols=["Encode_Artist", "avg(Wks)", "avg(Total)"],
    outputCol="features",
)

assembled_data = assemble.transform(encoded_df)

## Standard Scaling
scale = StandardScaler(inputCol="features", outputCol="standardized")
data_scale = scale.fit(assembled_data)
data_scale_output = data_scale.transform(assembled_data)
data_scale_output.show(5)
```

![PySpark for Data Science](img/1df15b21db2ceea33176c94650c35653.png)

# KMeans 聚类

我已经运行了 Kmean 肘部法来找出 k 值。如果你想查看所有的代码源和输出，可以查看我的 [notebook](https://colab.research.google.com/drive/1O3XKjoqEk_r08nMyVqjRm2ZalL8le4C5?usp=sharing)。

就像 scikit-learn 一样，我们将提供多个聚类，并训练 Kmeans 聚类模型。

```py
from pyspark.ml.clustering import KMeans
KMeans_algo=KMeans(featuresCol='standardized', k=4)
KMeans_fit=KMeans_algo.fit(data_scale_output)
preds=KMeans_fit.transform(data_scale_output)

preds.show(5)
```

![PySpark for Data Science](img/833dff07fe7525d2deb604b2a1a1419f.png)

# 可视化预测

在这一部分，我们将使用 `matplotlib.pyplot.barplot` 来显示 4 个聚类的分布。

```py
import matplotlib.pyplot as plt
import seaborn as sns

df_viz = preds.select(
    "Artist", "avg(Wks)", "avg(Total)", "prediction"
).toPandas()
avg_df = df_viz.groupby(["prediction"], as_index=False).mean()

list1 = ["avg(Wks)", "avg(Total)"]

for i in list1:
    sns.barplot(x="prediction", y=str(i), data=avg_df)
    plt.show() 
```

![PySpark for Data Science](img/73b5ed95db0eec08514c9c4aa78ee445.png)![PySpark for Data Science](img/a30a4966f0e14616d98c81c26286af2e.png)

# 总结

在本教程中，我概述了你可以通过 PySpark API 做些什么。该 API 允许你执行类似 SQL 的查询、运行 pandas 函数，并训练类似于 scikit-learn 的模型。你可以在分布式计算中获得所有世界的最佳体验。

在处理大型数据集（>1GB）时，它比许多 Python 包表现更为出色。如果你是程序员，并且对 Python 代码感兴趣，请查看我们的 Google Colab [notebook](https://colab.research.google.com/drive/1O3XKjoqEk_r08nMyVqjRm2ZalL8le4C5#scrollTo=bWopwzVg1ih0)。你只需下载并添加来自 [Kaggle](https://www.kaggle.com/datasets/kabhishm/global-spotify-weekly-chart) 的数据即可开始工作。

如果你希望我继续编写其他 Python 库的基于代码的教程，请在评论中告诉我。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专家，热衷于构建机器学习模型。目前，他专注于内容创作和撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为那些与心理疾病作斗争的学生打造一个 AI 产品。

### 相关主题

+   [使用 Pandera 进行 PySpark 应用的数据验证](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)

+   [停止学习数据科学以寻找目标，并寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最低要求：你需要知道的 10 项基础技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [KDnuggets™ 新闻 22:n06, 2 月 9: 数据科学编程…](https://www.kdnuggets.com/2022/n06.html)

+   [数据科学定义幽默：一系列古怪的引用…](https://www.kdnuggets.com/2022/02/data-science-definition-humor.html)

+   [5 个数据科学项目，学习 5 个关键的数据科学技能](https://www.kdnuggets.com/2022/03/5-data-science-projects-learn-5-critical-data-science-skills.html)
