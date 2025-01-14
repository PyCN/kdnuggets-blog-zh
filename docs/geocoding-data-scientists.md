# 数据科学家的地理编码

> 原文：[`www.kdnuggets.com/2023/06/geocoding-data-scientists.html`](https://www.kdnuggets.com/2023/06/geocoding-data-scientists.html)

当数据科学家需要了解有关数据“位置”的所有信息时，他们通常会转向地理信息系统（GIS）。GIS 是一套复杂的技术和程序，服务于各种目的，但华盛顿大学提供了一个相当全面的定义，称“地理信息系统是一个复杂的相关或连接事物的排列，其目的是传达有关地球表面特征的知识”（Lawler et al）。GIS 涵盖了从获取到可视化处理空间数据的一系列技术，其中许多技术即使你不是 GIS 专家也是有价值的工具。本文提供了地理编码的全面概述，并通过 Python 演示了几个实际应用。具体而言，你将使用地址确定纽约市的一家比萨店的确切位置，并将其与附近公园的数据连接起来。尽管演示使用了 Python 代码，但核心概念可以应用于许多编程环境中，将地理编码集成到你的工作流程中。这些工具为将数据转换为空间数据提供了基础，并为更复杂的地理分析打开了大门。

![XXXXX](img/841f52699d19a08a7845df4f5da16c99.png)

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT 部门

* * *

# 什么是地理编码？

地理编码通常被定义为将地址数据转换为映射坐标。通常，这涉及到检测地址中的街道名称，将该街道与数据库中其实际世界对应物的边界匹配，然后估计在街道上放置地址的位置。以纽约百老汇的一个比萨店地址为例：2709 Broadway, New York, NY 10025。第一步是找到适合的路网形状文件。注意在这种情况下，地址的城市和州为“New York, NY”。幸运的是，纽约市在[NYC Open Data](https://data.cityofnewyork.us/City-Government/road/svwp-sbcd)页面（CSCL PUB）上发布了详细的道路信息。第二步，检查街道名称“Broadway”。你现在知道地址可能位于纽约市任何名为“Broadway”的街道上，因此你可以执行以下 Python 代码查询 NYC Open Data SODA API 中所有名为“Broadway”的街道。

```py
import geopandas as gpd
import requests
from io import BytesIO

# Request the data from the SODA API
req = requests.get(
    "https://data.cityofnewyork.us/resource/gdww-crzy.geojson?stname_lab=BROADWAY"
)
# Convert to a stream of bytes
reqstrm = BytesIO(req.content)
# Read the stream as a GeoDataFrame
ny_streets = gpd.read_file(reqstrm) 
```

这个查询结果超过 700 个，但这并不意味着你必须检查 700 条街道才能找到你的比萨。通过数据可视化，你可以看到有 3 条主要的百老汇街和几条较小的街道。

![XXXXX](img/efc594233f7ae3f15dbc2b35215d7b5c.png)

这是因为每条街道被分割成与街区大致相对应的部分，从而允许更细致地查看数据。该过程的下一步是使用邮政编码和街道号码确定地址所在的具体部分。数据集中每个街道段包含街道两侧建筑的地址范围。同样，每个段还包含街道两侧的邮政编码。为了定位正确的段，以下代码应用过滤器，找到邮政编码与地址的邮政编码匹配且地址范围包含地址的街道号码的街道段。

```py
# Address to be geocoded
address = "2709 Broadway, New York, NY 10025"
zipcode = address.split(" ")[-1]
street_num = address.split(" ")[0]

# Find street segments whose left side address ranges contain the street number
potentials = ny_streets.loc[ny_streets["l_low_hn"] < street_num]
potentials = potentials.loc[potentials["l_high_hn"] > street_num]
# Find street segments whose zipcode matches the address'
potentials = potentials.loc[potentials["l_zip"] == zipcode]
```

这将列表缩小到下面看到的一条街道段。

![XXXXX](img/1e654dbb27b4e0215c493a3cfcbffad0.png)

最终任务是确定地址在这条线上的位置。这是通过将街道号码放置在该段的地址范围内，规范化以确定地址应该在这条线上的位置，并将该常数应用于线的端点坐标以获得地址的坐标来完成的。以下代码概述了这一过程。

```py
import numpy as np
from shapely.geometry import Point

# Calculate how far along the street to place the point
denom = (
    potentials["l_high_hn"].astype(float) - potentials["l_low_hn"].astype(float)
).values[0]
normalized_street_num = (
    float(street_num) - potentials["l_low_hn"].astype(float).values[0]
) / denom

# Define a point that far along the street
# Move the line to start at (0,0)
pizza = np.array(potentials["geometry"].values[0].coords[1]) - np.array(
    potentials["geometry"].values[0].coords[0]
)
# Multiply by normalized street number to get coordinates on line
pizza = pizza * normalized_street_num
# Add starting segment to place line back on the map
pizza = pizza + np.array(potentials["geometry"].values[0].coords[0])
# Convert to geometry array for geopandas
pizza = gpd.GeoDataFrame(
    {"address": [address], "geometry": [Point(pizza[0], pizza[1])]},
    crs=ny_streets.crs,
    geometry="geometry",
) 
```

完成地址地理编码后，现在可以在地图上绘制这家比萨店的位置，以了解其位置。由于上述代码查看了街道段左侧的信息，因此实际位置将略微偏左于绘制的点，位于道路左侧的建筑物中。你终于知道了哪里可以吃到比萨饼。

![XXXXX](img/9f6b44b6c2b5be7eb3d6ebfdb3428a0d.png)

这个过程涵盖了通常被称为地理编码的内容，但这不是该术语的唯一用法。你可能还会看到地理编码指的是将地标名称转化为坐标、邮政编码转化为坐标，或将坐标转化为 GIS 矢量的过程。你甚至可能会听到逆向地理编码（稍后会介绍）被称为地理编码。一个更宽松的地理编码定义是“在位置的近似自然语言描述和地理坐标之间的转换。”所以，每当你需要在这两种数据之间转换时，可以考虑地理编码作为解决方案。

作为重复进行地理编码的替代方法，许多 API 端点，如[美国人口普查局地理编码器](https://geocoding.geo.census.gov/geocoder/)和[谷歌地理编码 API](https://developers.google.com/maps/documentation/geocoding/overview)，提供了免费的准确地理编码服务。一些付费选项，如[Esri 的 ArcGIS](https://www.esri.com/en-us/arcgis/products/arcgis-pro/overview)、[Geocodio](https://www.geocod.io/)和[Smarty](https://www.smarty.com/)甚至为特定地址提供了屋顶级别的准确性，这意味着返回的坐标正好落在建筑物的屋顶上，而不是附近的街道上。以下部分将介绍如何使用这些服务，将地理编码融入你的数据处理流程，以美国人口普查局地理编码器为例。

# 如何进行地理编码

为了在地理编码时获得尽可能高的准确性，你应始终首先确保你的地址格式符合你选择的服务标准。不同服务之间会有些许差异，但常见格式是 USPS 格式的“PRIMARY# STREET, CITY, STATE, ZIP”，其中 STATE 是一个缩写代码，PRIMARY#是街道号码，所有套房号码、建筑号码和邮政信箱的提及都会被去除。

一旦你的地址格式化完成，你需要将其提交给 API 进行地理编码。以美国人口普查局地理编码器为例，你可以通过“一行地址处理”标签手动提交地址，或者使用[提供的 REST API](https://geocoding.geo.census.gov/geocoder/Geocoding_Services_API.pdf)以编程方式提交地址。美国人口普查局地理编码器还允许你使用批量地理编码器对整个文件进行地理编码，并使用基准参数指定数据源。要对之前提到的披萨店进行地理编码，可以使用[这个链接](https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=2709+Broadway%2C+New+York%2C+NY+10025&benchmark=Public_AR_Current&format=json)将地址传递给 REST API，这可以使用以下代码在 Python 中完成。

```py
# Submit the address to the U.S. Census Bureau Geocoder REST API for processing
response = requests.get(
    "https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=2709+Broadway%2C+New+York%2C+NY+10025&benchmark=Public_AR_Current&format=json"
).json()
```

返回的数据是一个 JSON 文件，可以轻松解码为 Python 字典。它包含一个“tigerLineId”字段，可用于匹配最近街道的 shapefile，一个“side”字段，可用于确定地址位于街道的哪一侧，以及“fromAddress”和“toAddress”字段，包含街道段的地址范围。最重要的是，它包含一个“coordinates”字段，可以用于在地图上定位地址。以下代码从 JSON 文件中提取坐标，并处理成 GeoDataFrame，以备进行空间分析。

```py
# Extract coordinates from the JSON file
coords = response["result"]["addressMatches"][0]["coordinates"]
# Convert coordinates to a Shapely Point
coords = Point(coords["x"], coords["y"])
# Extract matched address
matched_address = response["result"]["addressMatches"][0]["matchedAddress"]
# Create a GeoDataFrame containing the results
pizza_point = gpd.GeoDataFrame(
    {"address": [matched_address], "geometry": coords},
    crs=ny_streets.crs,
    geometry="geometry",
)
```

可视化这个点显示它略微偏离了左侧的手动地理编码点。

![XXXXX](img/68387211dbdba0522f535fa6d9b0fc60.png)

# 如何进行反向地理编码

反向地理编码是将地理坐标与地理区域的自然语言描述匹配的过程。正确应用时，它是数据科学工具包中最强大的技术之一。反向地理编码的第一步是确定你的目标区域。这是包含你坐标数据的区域。一些常见的例子包括人口普查区、邮政编码和城市。第二步是确定这些区域中是否包含该点。使用常见区域时，[美国人口普查地理编码器](https://geocoding.geo.census.gov/geocoder/geographies/coordinates?form)可以通过对 REST API 请求进行小幅修改来进行反向地理编码。确定哪个人口普查地理区域包含之前的比萨店的请求可以通过[此链接](https://geocoding.geo.census.gov/geocoder/geographies/coordinates?x=-73.96838669696855&y=40.799450999770706&benchmark=Public_AR_Current&vintage=ACS2022_Current&layers=all&format=json)获得。此查询的结果可以使用之前相同的方法处理。然而，创造性地定义区域以满足分析需求，并手动进行反向地理编码，打开了许多可能性。

要手动进行逆地理编码，你需要确定一个区域的位置和形状，然后判断该点是否在该区域的内部。确定一个点是否在多边形内部实际上是一个相当困难的问题，但可以使用[射线投射算法](https://dl.acm.org/doi/10.1145/368637.368653)，其中从点开始并无限延伸的射线如果与区域边界相交的次数为奇数，则点在区域内部，若为偶数则不在区域内部（Shimrat），来解决这个问题。在数学上，这实际上是[乔丹曲线定理](https://www.britannica.com/science/Jordan-curve-theorem)（Hosch）的直接应用。需要注意的是，如果你使用的是来自全球的数据，射线投射算法可能会失败，因为射线最终会绕到地球表面并变成一个圆圈。在这种情况下，你将需要找到区域和点的绕数（Weisstein）。如果绕数不为零，则点在区域内部。幸运的是，Python 的 geopandas 库提供了定义多边形区域内部以及测试点是否在其中所需的功能，而无需所有复杂的数学运算。

虽然手动地理编码对于许多应用可能过于复杂，但手动逆地理编码可以是你技能集中的一个实用补充，因为它允许你轻松地将点匹配到高度自定义的区域。例如，假设你想把你的披萨带到一个公园野餐。你可能希望知道披萨店是否在公园的短距离内。纽约市提供了作为[Parks Properties 数据集](https://nycopendata.socrata.com/Recreation/Parks-Properties/enfh-gkve)（纽约市公园开放数据团队）一部分的公园形状文件，并且可以通过他们的 SODA API 使用以下代码访问。

```py
# Pull NYC park shapefiles
parks = gpd.read_file(
    BytesIO(
        requests.get(
            "https://data.cityofnewyork.us/resource/enfh-gkve.geojson?$limit=5000"
        ).content
    )
)
# Limit to parks with green area for a picnic
parks = parks.loc[
    parks["typecategory"].isin(
        [
            "Garden",
            "Nature Area",
            "Community Park",
            "Neighborhood Park",
            "Flagship Park",
        ]
    )
] 
```

这些公园可以被添加到可视化中，以查看哪些公园靠近披萨店。

![XXXXX](img/cb71e8c74732b1539860ee87d9fd4e56.png)

显然，附近有一些选择，但使用形状文件和点来计算距离可能是困难且计算密集的。相反，可以应用逆地理编码。第一步，如上所述，是确定你要将点附加到的区域。在这种情况下，该区域是“距纽约市公园半英里”。第二步是计算点是否位于区域内部，这可以通过之前提到的方法进行数学计算，或通过应用 geopandas 中的“contains”函数来完成。以下代码用于在测试之前向公园的边界添加半英里缓冲区，以查看哪些公园的缓冲区域现在包含该点。

```py
# Project the coordinates from latitude and longitude into meters for distance calculations
buffered_parks = parks.to_crs(epsg=2263)
pizza_point = pizza_point.to_crs(epsg=2263)
# Add a buffer to the regions extending the border by 1/2 mile = 2640 feet
buffered_parks = buffered_parks.buffer(2640)
# Find all parks whose buffered region contains the pizza parlor
pizza_parks = parks.loc[buffered_parks.contains(pizza_point["geometry"].values[0])] 
```

这个缓冲区显示了附近的公园，这些公园在下图中以蓝色突出显示。

![XXXXX](img/456f186d0e83357176edd979b798f287.png)

成功进行反向地理编码后，你了解到在比萨店半英里内有 8 个公园，你可以在这些公园里举行野餐。享受那一片比萨吧。

![XXXXX](img/03dca769c277af02035fed31a6c47d8a.png)

j4p4n 制作的比萨片

## 资源

1.  Lawler, Josh 和 Schiess, Peter. ESRM 250: Introduction to Geographic Information Systems in Forest Resources. GIS 定义, 2009 年 2 月 12 日, 华盛顿大学, 西雅图. 课堂讲座. [`courses.washington.edu/gis250/lessons/introduction_gis/definitions.html`](https://courses.washington.edu/gis250/lessons/introduction_gis/definitions.html)

1.  CSCL PUB. 纽约开放数据. [`data.cityofnewyork.us/City-Government/road/svwp-sbcd`](https://data.cityofnewyork.us/City-Government/road/svwp-sbcd)

1.  美国普查局地理编码文档. 2022 年 8 月. [`geocoding.geo.census.gov/geocoder/Geocoding_Services_API.pdf`](https://geocoding.geo.census.gov/geocoder/Geocoding_Services_API.pdf)

1.  Shimrat, M., "算法 112: 点相对于多边形的位置" 1962, *ACM 通讯* 第 5 卷第 8 期, 1962 年 8 月. [`dl.acm.org/doi/10.1145/368637.368653`](https://dl.acm.org/doi/10.1145/368637.368653)

1.  Hosch, William L.. "Jordan curve theorem". *Encyclopedia Britannica*, 2018 年 4 月 13 日, [`www.britannica.com/science/Jordan-curve-theorem`](https://www.britannica.com/science/Jordan-curve-theorem)

1.  Weisstein, Eric W. "Contour Winding Number." 来源于 *MathWorld*--Wolfram 网络资源. [`mathworld.wolfram.com/ContourWindingNumber.html`](https://mathworld.wolfram.com/ContourWindingNumber.html)

1.  NYC Parks Open Data Team. 公园属性. 2023 年 4 月 14 日. [`nycopendata.socrata.com/Recreation/Parks-Properties/enfh-gkve`](https://nycopendata.socrata.com/Recreation/Parks-Properties/enfh-gkve)

1.  j4p4n, “Pizza Slice.” 来源于 *OpenClipArt*. [`openclipart.org/detail/331718/pizza-slice`](https://openclipart.org/detail/331718/pizza-slice)

**[Evan Miller](https://www.linkedin.com/in/evan-m-miller/)** 是 Tech Impact 的数据科学研究员，他利用数据支持非营利组织和政府机构，以实现社会公益的使命。之前，Evan 在中密歇根大学使用机器学习训练自主车辆。

### 更多相关话题

+   [Python 中的地理编码：完整指南](https://www.kdnuggets.com/2022/11/geocoding-python-complete-guide.html)

+   [数据工程师和数据科学家都需要的高保真合成数据](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [数据分析师和数据科学家之间的区别是什么？](https://www.kdnuggets.com/2022/03/difference-data-analysts-data-scientists.html)

+   [数据科学家和数据工程师如何协作？](https://www.kdnuggets.com/2022/08/data-scientists-data-engineers-work-together.html)

+   [掌握数据讲故事的艺术：数据科学家的指南](https://www.kdnuggets.com/2023/06/mastering-art-data-storytelling-guide-data-scientists.html)
