# 多变量时间序列预测与 BQML

> 原文：[`www.kdnuggets.com/2023/07/multivariate-timeseries-prediction-bqml.html`](https://www.kdnuggets.com/2023/07/multivariate-timeseries-prediction-bqml.html)

![多变量时间序列预测与 BQML](img/f6601849ad09b52789b510bc3f570317.png)

图片来源于 [Pexels](https://www.pexels.com/photo/graphs-display-on-an-ipad-187041/)

去年冬天，我在乌兹别克斯坦首都塔什干的[**GDG DevFest Tashkent 2022**](https://gdg.community.dev/events/details/google-gdg-tashkent-presents-devfest-tashkent-22/)上做了一个关于‘**使用 BQML 进行更可预测的时间序列模型**’的演讲。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

我本打算分享一些我在 DevFest 上使用的材料和代码，但时间已经过去，BQML 中已发布一些新功能，这些功能与一些内容重叠。

因此，我将简要提及一些新功能和一些仍然有效的内容。

# 时间序列模型与 BQML

**时间序列数据**被许多组织用于各种目的，需要注意的是，“**预测分析**”关注的是时间上的“未来”。时间序列预测分析已经在短期、中期和长期中使用，虽然它有许多不准确性和风险，但也在稳步改进。

由于“预测”似乎非常有用，如果你有时间序列数据，你可能会被诱导去应用时间序列预测模型。但时间序列预测模型通常计算密集，如果数据量很大，它会更加计算密集。因此，这很繁琐，处理、加载到分析环境并进行分析都很困难。

如果你使用**Google BigQuery**进行数据管理，可以使用**BQML**（BigQuery ML）以简单、便捷、快速的方式将机器学习算法应用于你的数据。许多人使用 BigQuery 处理大量数据，其中许多数据通常是时间序列数据。而**BQML 也支持时间序列模型**。

目前 BQML 支持的时间序列模型的基础是 [**自回归积分滑动平均（ARIMA）**](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average) 模型。ARIMA 模型仅利用现有时间序列数据进行预测，已知其短期预测性能良好，并且由于它结合了 AR 和 MA，因此是一个能够覆盖广泛时间序列模型的流行模型。

然而，这个模型整体上计算密集，并且由于它仅利用正常的时间序列数据，因此在存在趋势或季节性情况下使用起来很困难。因此，***ARIMA_PLUS*** 在 BQML 中包含了几个附加特性作为选项。你可以将时间序列分解、季节性因素、波动和下跌、系数变化等添加到模型中，或者可以分别处理它们并手动调整模型。我个人也喜欢可以通过自动包含假期选项来调整周期性，这也是使用一个不需要你手动添加日期相关信息的平台的好处之一。

![使用 BQML 的多变量时间序列预测](img/8791fc9a35fae94548b0139f3a6c3c30.png)

ARIMA_PLUS 的结构（来自 [BQML 手册](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-time-series)）

你可以参考这个 [页面](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-time-series) 以获取更多信息。

然而，当涉及到实际应用时，时间序列预测并不像这样简单。当然，我们已经能够利用 ARIMA_PLUS 识别多个周期并对多个时间序列进行干预，但时间序列数据中存在许多外部因素，并且很少有事件是孤立发生的。**时间序列数据中的平稳性可能很难找到。**

在原始演示中，我研究了如何处理这些实际时间序列数据以建立预测模型——**分解这些时间序列**，清理分解后的数据，将其导入 Python，然后**与其他变量结合创建多变量时间序列函数，估计因果关系并将其纳入预测模型中，评估事件变化对效果的影响程度。**

# 新特性：ARIMA_PLUS_XREG

而在仅仅过去几个月里，**用于创建带有外部变量的多变量时间序列函数（*****ARIMA_PLUS_XREG, XREG*** **）的新特性已经成为 BQML 的一项显著功能**。

你可以在 [这里](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-multivariate-time-series) 阅读全部内容（截至 2023 年 7 月仍处于预览阶段，但我猜它会在今年晚些时候推出）。

我应用了[官方教程](https://cloud.google.com/bigquery/docs/arima-plus-xreg-single-time-series-forecasting-tutorial?hl=ko)，以查看它与传统的单变量时间序列模型的比较情况，并了解其工作原理。

步骤与教程中的相同，因此我不再重复，但这是我创建的两个模型。首先，我创建了一个传统的 *ARIMA_PLUS* 模型，然后使用相同的数据创建了一个 *XREG* 模型，同时加入了当时的温度和风速。

# ARIMA_PLUS

```py
# ARIMA_PLUS
CREATE OR REPLACE MODEL test_dt_us.seattle_pm25_plus_model
OPTIONS (
 MODEL_TYPE = 'ARIMA_PLUS',
 time_series_timestamp_col = 'date',
 time_series_data_col = 'pm25') AS
SELECT date, pm25
FROM test_dt_us.seattle_air_quality_daily
WHERE date BETWEEN DATE('2012-01-01') AND DATE('2020-12-31')
#ARIMA_PLUS_XREG
CREATE OR REPLACE  MODEL test_dt_us.seattle_pm25_xreg_model
 OPTIONS (
   MODEL_TYPE = 'ARIMA_PLUS_XREG',
   time_series_timestamp_col = 'date',
   time_series_data_col = 'pm25') AS
SELECT  date, pm25, temperature, wind_speed
FROM test_dt_us.seattle_air_quality_daily
WHERE  date BETWEEN DATE('2012-01-01') AND DATE('2020-12-31')
```

使用这些多数据的模型可能如下所示

![使用 BQML 的多元时间序列预测](img/cd984e9f6e14ec62d2a62ec4ccf7b0b5.png)

结构 ARIMA_PLUS_XREG（来自 [BQML 手册](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-create-multivariate-time-series)）

两个模型通过 ML.Evaluate 进行比较。

```py
SELECT  * 
FROM  ML.EVALUATE
         (  MODEL test_dt_us.seattle_pm25_plus_model, 
         (  SELECT  date,  pm25
           FROM  test_dt_us.seattle_air_quality_daily 
           WHERE  date > DATE('2020-12-31')  ))
SELECT  * 
FROM  ML.EVALUATE
          (  MODEL test_dt_us.seattle_pm25_xreg_model, 
          (  SELECT  date,  pm25,  temperature,  wind_speed 
             FROM  test_dt_us.seattle_air_quality_daily 
             WHERE  date > DATE('2020-12-31')  ),
          STRUCT(  TRUE AS perform_aggregation,  30 AS horizon))
```

结果如下。

**ARIMA_PLUS**

![使用 BQML 的多元时间序列预测](img/53d50aecd494ec5e09b358e6a89863d8.png)

**ARIMA_PLUS_XREG**

![使用 BQML 的多元时间序列预测](img/1857f75e7b6e853a6d79c20129d41b29.png)

你可以看到**XREG 模型在 MAE、MSE 和 MAPE 等基本性能指标上表现更好。**（显然，这不是一个完美的解决方案，**依赖于数据**，我们只能说这是一个有用的工具。）

多元时间序列分析在许多情况下是一个非常需要的选项，但由于各种原因，往往难以应用。现在，只要原因存在于数据和分析步骤中，我们就可以使用它。看起来我们有一个不错的选择，所以了解它是很好的，希望在许多情况下它会有用。

**[权政敏](https://www.linkedin.com/in/jeongmin-kwon-a5069734/)** 是一名拥有超过 10 年实际操作经验的自由职业高级数据科学家，擅长利用机器学习模型和数据挖掘。

### 更多相关内容

+   [为多元时间序列构建可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [烂番茄电影评分预测的数据科学项目：…](https://www.kdnuggets.com/2023/06/data-science-project-rotten-tomatoes-movie-rating-prediction-first-approach.html)

+   [烂番茄电影评分预测的数据科学项目：…](https://www.kdnuggets.com/2023/07/data-science-project-rotten-tomatoes-movie-rating-prediction-second-approach.html)
