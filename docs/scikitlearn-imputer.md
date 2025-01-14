# 使用 Scikit-learn 的填补器

> 原文：[`www.kdnuggets.com/2022/07/scikitlearn-imputer.html`](https://www.kdnuggets.com/2022/07/scikitlearn-imputer.html)

![使用 Scikit-learn 的填补器](img/6237013757a9a03d4aae1d849299a8af.png)

[拼图形状](https://www.freepik.com/photos/puzzle-shape) 照片由[freepik](https://www.freepik.com)创建

# 什么是填补器？

* * *

## 我们的前 3 名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

如果你的数据集中存在缺失值，你可以删除缺失值所在的行或列。这种方法并不推荐，因为它会减少数据量，可能导致数据分析偏离实际情况。相反，我们应该使用不受缺失值影响的机器学习算法，或使用填补器来填充缺失的信息。

填补器是一种用于填充数据集中缺失值的估算器。对于数值型数据，它使用**均值**、**中位数**和**常量**。对于类别型数据，它使用**最常用值**和**常量值**。你也可以训练你的模型来预测缺失的标签。

在本教程中，我们将学习 Scikit-learn 的**SimpleImputer**、**IterativeImputer** 和 **KNNImputer**。我们还将创建一个管道来填补类别和数值特征，并将其输入到机器学习模型中。

# 如何使用 Scikit-learn 的填补器

[scikit-learn](https://scikit-learn.org/stable/modules/impute.html)的填补函数为我们提供了一个简单的填充选项，只需几行代码。我们可以集成这些填补器，创建管道以重现结果并改进机器学习开发过程。

## 入门指南

我们将使用[Deepnote](https://deepnote.com/)环境，它类似于 Jupyter Notebook，但在云端运行。

要从[Kaggle](https://www.kaggle.com/)下载并解压数据，你需要安装 Kaggle Python 包，并使用 API 下载[太空船泰坦尼克号](https://www.kaggle.com/competitions/spaceship-titanic/data?select=train.csv)数据集。最后，将数据解压到数据集文件夹中。

```py
%%capture
!pip install kaggle
!kaggle competitions download -c spaceship-titanic
!unzip -d ./dataset spaceship-titanic

```

接下来，我们将导入所需的 Python 包用于数据摄取、填补以及创建转换管道。

```py
import numpy as np
import pandas as pd
from sklearn.impute import SimpleImputer
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer,KNNImputer
from sklearn.pipeline import FeatureUnion,make_pipeline,Pipeline
from sklearn.compose import ColumnTransformer

```

[Spaceship Titanic](https://www.kaggle.com/competitions/spaceship-titanic/data?select=train.csv)数据集是 Kaggle 入门预测比赛的一部分。它包括训练、测试和提交的 CSV 文件。我们将使用**train.csv**，其中包含宇宙飞船上的乘客信息。

pandas 的**read_csv()**函数读取 train.csv 并显示数据框。

```py
df = pd.read_csv("dataset/train.csv")

df

```

![使用 Scikit-learn 的插补器](img/f6f658a4b29bc3a93b8c4935833f651e.png)

## 数据分析

在本节中，我们将探讨缺失值的列，但首先需要检查数据集的形状。它有 8693 行和 14 列。

```py
df.shape

>>> (8693, 14)

```

我们现在将基于列显示缺失值的数量和百分比。为了在数据框中显示它，我们将创建一个新的缺失值数据框，并对**NA Count**列应用样式渐变。

```py
NA = pd.DataFrame(data=[df.isna().sum().tolist(), ["{:.2f}".format(i)+'%' \
           for i in (df.isna().sum()/df.shape[0]*100).tolist()]], 
           columns=df.columns, index=['NA Count', 'NA Percent']).transpose()

NA.style.background_gradient(cmap="Pastel1_r", subset=['NA Count'])

```

![使用 Scikit-learn 的插补器](img/28ac6aaf0b02b65004ff589fe965ab36.png)

除了**PassengerID**和**Transported**之外，每一列都有缺失值。

## 数值数据插补

我们将利用缺失列的信息，将其分为类别列和数值列。我们将对它们采取不同的处理方式。

对于数值插补，我们将选择**Age**列并显示缺失值的数量。这将帮助我们验证插补前后的结果。

```py
all_col = df.columns
cat_na = ['HomePlanet', 'CryoSleep','Destination','VIP']
num_na = ['Age','RoomService', 'FoodCourt', 'ShoppingMall', 'Spa', 'VRDeck']
data1 = df.copy()
data2 = df.copy()

data1['Age'].isna().sum()

>>> 179

```

```py
data1.Age[0:5]

>>> 0    39.0
>>> 1    24.0
>>> 2    58.0
>>> 3    33.0
>>> 4    16.0

```

接下来，我们将使用 sklearn 的**SimpleImputer**并将其应用于**Age**列。它将用列的**average**值替换缺失数据。

正如我们所观察到的，**Age**列中没有剩余的缺失值。

```py
imp = SimpleImputer(strategy='mean')
data1['Age'] = imp.fit_transform(data1['Age'].values.reshape(-1, 1) )

data1['Age'].isna().sum()

>>> 0

```

对于数值列，你可以使用**constant**、**mean**和**median**策略，对于类别列，你可以使用**most_frequent**和**constant**策略。

## 类别数据插补

对于类别数据插补，我们将使用包含 201 个缺失值的**HomePlanet**列。

```py
data1['HomePlanet'].isna().sum()

>>> 201

```

```py
data1.HomePlanet[0:5]

>>> 0    Europa
>>> 1     Earth
>>> 2    Europa
>>> 3    Europa
>>> 4     Earth

```

为了填充类别缺失值，我们将使用带有**most_frequent**策略的**SimpleImputer**。

```py
imp = SimpleImputer(strategy="most_frequent")
data1['HomePlanet'] = imp.fit_transform(data1['HomePlanet'].values.reshape(-1, 1))

```

我们已经填充了 HomePlanet 列中的所有缺失值。

```py
data1['HomePlanet'].isna().sum()

>>> 0

```

## 多变量插补器

在**单变量插补器**中，缺失值是通过相同的特征计算的，而在**多变量插补器**中，算法使用所有可用特征维度来预测缺失值。

我们将一次性填充数值列，正如我们所看到的，它们都有 150+的缺失值。

```py
data2[num_na].isna().sum()

```

```py
>>> Age             179
>>> RoomService     181
>>> FoodCourt       183
>>> ShoppingMall    208
>>> Spa             183
>>> VRDeck          188

```

我们将使用`IterativeImputer`，设置**max_iter**为 10，以估计和填充数值列中的缺失值。该算法将在估计值时考虑所有列。

```py
imp = IterativeImputer(max_iter=10, random_state=0)
data2[num_na] = imp.fit_transform(data2[num_na])

data2[num_na].isna().sum()

```

```py
>>> Age             0
>>> RoomService     0
>>> FoodCourt       0
>>> ShoppingMall    0
>>> Spa             0
>>> VRDeck          0

```

## 机器学习中的类别和数值数据插补

为什么选择 Scikit-learn 的插补器？除了插补器外，机器学习框架还提供特征转换、数据处理、管道和机器学习算法。它们都能无缝集成。只需几行代码，你就可以在任何数据集上进行插补、归一化、转换和训练你的模型。

在本节中，我们将学习如何在机器学习项目中集成插补器以获得更好的结果。

+   首先，我们将从 sklearn 中导入相关函数。

+   然后，我们将删除不相关的列以创建 X 和 Y 变量。我们的目标列是“Transported”。

+   之后，我们将数据分割为训练集和测试集。

```py
from sklearn.preprocessing import LabelEncoder, StandardScaler, OrdinalEncoder
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
X,y = df.drop(['Transported','PassengerId','Name','Cabin'],axis = 1) , df['Transported']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=100, random_state=0)
```

要创建数值和类别转换数据管道，我们将使用 sklearn 的 Pipeline 函数。

对于 **numeric_transformer**，我们使用了：

+   **KNNImputer** 使用 2 个 **n_neighbors** 和均匀的 **weights**

+   在第二步中，我们使用了**StandardScaler**的默认配置。

对于 **categorical_transformer**，我们使用了：

+   **SimpleImputer** 使用 **most_frequent** 策略

+   在第二步中，我们使用了**OrdinalEncoder**，将类别转换为数字。

```py
numeric_transformer = Pipeline(steps=[
   ('imputer', KNNImputer(n_neighbors=2, weights="uniform")),
   ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
   ('imputer', SimpleImputer(strategy='most_frequent')),
   ('onehot', OrdinalEncoder())])

```

现在，我们将使用 **ColumnTransformer** 处理和转换训练特征。对于 **numeric_transformer**，我们提供了一组数值列，对于 **categorical_transformer**，我们将使用一组类别列。

**注意：**我们只是准备了管道和转换器。我们还没有处理任何数据。

```py
preprocessor = ColumnTransformer(
   remainder = 'passthrough',
   transformers=[
       ('numeric', numeric_transformer, num_na),
       ('categorical', categorical_transformer, cat_na)
])

```

最后，我们将创建一个包含**处理器**和**DecisionTreeClassifier**的转换管道，用于二分类任务。该管道将首先处理和转换数据，然后训练分类模型。

```py
transform = Pipeline(
   steps=[
       ("processing", preprocessor),
       ("DecisionTreeClassifier", DecisionTreeClassifier()),
   ]
)

```

这就是魔法发生的地方。我们将使用**transform**管道来拟合训练数据集。之后，我们将使用测试数据集来评估我们的模型。

我们在默认配置下得到了**75% 的准确率**。不错!!!

```py
model = transform.fit(X_train,y_train)
model.score(X_test, y_test)
>>> 0.75

```

接下来，我们将在测试数据集上进行预测，并创建结构化的**分类报告**。

```py
from sklearn.metrics import classification_report
prediction = model.predict(X_test)
print(classification_report(prediction, y_test))

```

如我们所见，我们对**True**和**False**类有稳定的得分。

```py
                precision    recall  f1-score   support

       False       0.70      0.74      0.72        43
        True       0.80      0.75      0.77        57
    accuracy                           0.75       100
   macro avg       0.75      0.75      0.75       100
weighted avg       0.75      0.75      0.75       100

```

# 结论

为了获得更高的准确性，数据科学家们正在使用深度学习方法来插补缺失值。再次，你需要决定构建系统所需的时间和资源，以及它带来的价值。在大多数情况下，Scikit-learn 的插补器提供了更大的价值，并且我们只需几行代码就能插补整个数据集。

在这篇博客中，我们了解了插补及 Scikit-learn 库如何估计缺失值。我们还了解了单变量、变量间、多变量、类别和数值插补。在最后部分，我们使用了数据管道、列转换器和机器学习管道来插补、转换、训练和评估我们的模型。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为有心理疾病困扰的学生构建 AI 产品。

### 更多相关话题

+   [如何使用 Scikit-learn 的 Imputer 模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)

+   [使用 Python 自动化 Microsoft Excel 和 Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [如何使用 Python 确定最佳拟合数据分布](https://www.kdnuggets.com/2021/09/determine-best-fitting-data-distribution-python.html)

+   [使用 Datawig，AWS 的深度学习库进行缺失值插补](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [使用 BERT 对长文本进行分类](https://www.kdnuggets.com/2022/02/classifying-long-text-documents-bert.html)

+   [使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)
