# 利用 GPT 模型将自然语言转化为 SQL 查询

> 原文：[`www.kdnuggets.com/leveraging-gpt-models-to-transform-natural-language-to-sql-queries`](https://www.kdnuggets.com/leveraging-gpt-models-to-transform-natural-language-to-sql-queries)

![利用 GPT 模型将自然语言转化为 SQL 查询](img/3d2bce97a2479138e14bd6004194e384.png)

图片来源：作者。基础图片来自 [pch-vector](https://www.freepik.com/author/pch-vector)。

自然语言处理——或称 NLP——已经发生了巨大的发展，而 GPT 模型正处于这场变革的最前沿。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

目前，大型语言模型可以用于各种应用场景。

为了避免不必要的任务并提高我的工作效率，我开始探索训练 GPT 为我生成 SQL 查询的可能性。

就在这时，一个绝妙的主意出现了：

*利用 GPT 模型的力量来解读自然语言并将其转化为结构化的 SQL 查询。*

*这可能吗？*

让我们一起发现吧！

所以让我们从头开始……

# “少量示例提示”的概念

你们中的一些人可能已经对*少量示例提示*的概念有所了解，而其他人可能从未听说过这个概念。

*那么……这是什么？*

这里的基本思想是使用一些明确的示例——或称为“示例”——来指导大型语言模型以特定的方式响应。

这就是为什么它被称为**少量示例提示**。

简单来说，通过展示一些用户输入的示例提示和期望的大型语言模型输出，我们可以教会模型生成一些更符合我们偏好的增强输出。

这样做，我们在某些特定领域**扩展了模型的知识**，以生成与我们期望的任务更为契合的输出。

*那我们就来举个例子吧！*

*在本教程中，我将使用一个名为 chatgpt_call()的预定义函数来提示 GPT 模型。如果你想进一步了解，可以查看* [*以下文章。*](https://medium.com/forcodesake/chatgpt-api-calls-introduction-chatgpt3-chatgpt4-ai-d19b79c49cc5)

想象一下，我希望 ChatGPT 描述一下“乐观主义”这个术语。

如果我只是简单地让 GPT 描述它，我会得到一个严肃而枯燥的描述。

```py
## Code Block
response = chatgpt_call("Teach me about optimism. Keep it short.")
print(response)
```

附带相应的输出：

![利用 GPT 模型将自然语言转化为 SQL 查询](img/b5f888f6a77e5622cd7e80e04cf1b1a5.png)

我的 Jupyter Notebook 截图。提示 GPT。

不过，假设我希望得到一些更具诗意的东西。我可以在提示中添加更多细节，说明我想要一个诗意的定义。

```py
## Code Block
response = chatgpt_call("Teach me about optimism. Keep it short. Try to create a poetic definition.")
print(response)
```

但第二个输出看起来像一首诗，与我期望的输出无关。

![利用 GPT 模型将自然语言转化为 SQL 查询](img/ac8f33d5b2042cb52501ff60584b2830.png)

我的 Jupyter Notebook 截图。提示 GPT。

*我该怎么办？*

我可以进一步详细描述提示，并继续迭代，直到获得一些好的输出。*不过，这会花费很多时间。*

相反，**我可以通过设计一个示例并向模型展示，向模型展示我偏好的诗意描述类型。**

```py
## Code Block
prompt = """

Your task is to answer in a consistent style aligned with the following style. 

<user>: Teach me about resilience.

<system>: Resilience is like a tree that bends with the wind but never breaks. 
It is the ability to bounce back from adversity and keep moving forward.

<user>: Teach me about optimism.
"""
response = chatgpt_call(prompt)
print(response)</user></system></user>
```

而输出正是我所寻找的。

![利用 GPT 模型将自然语言转化为 SQL 查询](img/29672dbc07d91ff7f5f66c798b19b211.png)

我的 Jupyter Notebook 截图。提示 GPT。

*那么……我们如何将这一点转化为我们特定的 SQL 查询案例呢？*

# 使用 NLP 进行 SQL 生成

ChatGPT 已经能够从自然语言提示中生成 SQL 查询。**我们甚至不需要向模型展示任何表格，只需制定一个假设的计算，它就能为我们完成。**

```py
## Code Block
user_input = """
Assuming I have both product and order tables, could you generate a single table that contained all the info 
of every product together with how many times has it been sold?
"""

prompt = f"""
Given the following natural language prompt, generate a hypothetical query that fulfills the required task in SQL.
{user_input}
"""
response = chatgpt_call(prompt)
print(response)
```

不过，正如你已经知道的，**我们给模型提供的上下文越多，它生成的输出就会越好。**

![利用 GPT 模型将自然语言转化为 SQL 查询](img/149e91207d54213c671bf38b7abf2e63.png)

我的 Jupyter Notebook 截图。提示 GPT。

*在本教程中，我将输入提示分为用户的具体需求和模型所期望的高层次行为。这是改善我们与 LLM 交互并使提示更加简洁的好方法。* [*你可以在以下文章中了解更多。*](https://medium.com/forcodesake/chat-gpt-dual-prompt-ai-artificial-intelligence-gpt-engineering-good-practices-bard-d04efef1721b)

所以让我们假设我正在处理两个主要表格：PRODUCTS 和 ORDERS

![利用 GPT 模型将自然语言转化为 SQL 查询](img/9ce66c11c34da2333af88ae08dc748cf.png)

作者提供的图像。教程中将使用的表格。

如果我要求 GPT 提供一个简单的查询，模型会立刻给出解决方案，就像一开始那样，但会针对我的具体情况提供特定的表格。

```py
## Code Block
user_input = """
What model of TV has been sold the most in the store?
"""

prompt = f"""
Given the following SQL tables, your job is to provide the required SQL queries to fulfil any user request.

Tables: <{sql_tables}>

User request: ```{user_input}```py
"""
response = chatgpt_call(prompt)
print(response)
```

*你可以在本文末尾找到 sql_tables！*

*输出看起来如下！*

![利用 GPT 模型将自然语言转化为 SQL 查询](img/2f03b4450ce6961b19f75de01e2a25bb.png)

我的 Jupyter Notebook 截图。提示 GPT。

不过，我们可以观察到之前输出中的一些问题。

1.  计算部分是错误的，因为它只考虑了那些已经交付的电视。任何发出的订单——无论是否已交付——都应该被视为销售。

1.  查询的格式不是我希望的那样。

所以首先让我们专注于向模型展示如何计算所需的查询。

## #1\. 解决模型的一些误解

在这种情况下，模型只考虑那些已经作为已售出的产品，但这并不正确。**我们可以通过显示两个不同的示例来解决这种误解，其中我计算了类似的查询。**

```py
## Few_shot examples

fewshot_examples = """
-------------- FIRST EXAMPLE
User: What model of TV has been sold the most in the store when considering all issued orders. 
System: You first need to join both orders and products tables, filter only those orders that correspond to TVs 
and count the number of orders that have been issued: 

SELECT P.product_name AS model_of_tv, COUNT(*) AS total_sold
FROM products AS P
JOIN orders   AS O ON P.product_id = O.product_id
WHERE P.product_type = 'TVs'
GROUP BY P.product_name
ORDER BY total_sold DESC
LIMIT 1;

-------------- SECOND EXAMPLE
User: What's the sold product that has been already delivered the most?
System: You first need to join both orders and products tables, count the number of orders that have 
been already delivered and just keep the first one: 

SELECT P.product_name AS model_of_tv, COUNT(*) AS total_sold
FROM products AS P
JOIN orders   AS O ON P.product_id = O.product_id
WHERE P.order_status = 'Delivered'
GROUP BY P.product_name
ORDER BY total_sold DESC
LIMIT 1;
"""
```

现在，如果我们再次提示模型并包含之前的示例，我们可以看到，相应的查询不仅会正确——之前的查询已经有效——而且还会考虑我们想要的销售！

```py
## Code Block
user_input = """
What model of TV has been sold the most in the store?
"""

prompt = f"""
Given the following SQL tables, your job is to provide the required SQL tables
to fulfill any user request.

Tables: <{sql_tables}>. Follow those examples the generate the answer, paying attention to both
the way of structuring queries and its format:
<{fewshot_examples}>

User request: ```{user_input}```py
"""
response = chatgpt_call(prompt)
print(response)
```

以下是输出：

![利用 GPT 模型将自然语言转换为 SQL 查询](img/95b34ebe16d55f97da0ebee9136b3f35.png)

我在 Jupyter Notebook 的截图。提示 GPT。

现在如果我们检查相应的查询…

```py
## Code Block

pysqldf("""
SELECT P.product_name AS model_of_tv, COUNT(*) AS total_sold
FROM PRODUCTS AS P
JOIN ORDERS AS O ON P.product_id = O.product_id
WHERE P.product_type = 'TVs'
GROUP BY P.product_name
ORDER BY total_sold DESC
LIMIT 1;
""")
```

它工作得非常好！

![利用 GPT 模型将自然语言转换为 SQL 查询](img/3c146136efdf18097b03452a54688139.png)

我在 Jupyter Notebook 的截图。提示 GPT。

## #2\. 格式化 SQL 查询

少量示例提示也可以是一种定制模型以符合我们自己的目的或风格的方法。

如果我们回顾之前的示例，查询完全没有格式。我们都知道，有一些良好的实践——加上一些个人的怪癖——可以帮助我们更好地阅读 SQL 查询。

**这就是为什么我们可以使用少量示例提示来向模型展示我们喜欢的查询方式**——包括我们的良好实践或只是我们的怪癖——**并训练模型给我们我们所期望的格式化 SQL 查询。**

现在，我将按照我的格式偏好准备之前相同的示例。

```py
## Code Block
fewshot_examples = """
---- EXAMPLE 1
User: What model of TV has been sold the most in the store when considering all issued orders. 
System: You first need to join both orders and products tables, filter only those orders that correspond to TVs 
and count the number of orders that have been issued: 

SELECT 
       P.product_name AS model_of_tv, 
       COUNT(*)       AS total_sold
FROM products AS P
JOIN orders   AS O
  ON P.product_id = O.product_id

WHERE P.product_type = 'TVs'
GROUP BY P.product_name
ORDER BY total_sold DESC
LIMIT 1;

---- EXAMPLE 2
User: What is the latest order that has been issued?
System: You first need to join both orders and products tables and filter by the latest order_creation datetime: 

SELECT 
      P.product_name AS model_of_tv
FROM products AS P
JOIN orders AS O 
  ON P.product_id = O.product_id

WHERE O.order_creation = (SELECT MAX(order_creation) FROM orders)
GROUP BY p.product_name
LIMIT 1;
"""
```

一旦定义了示例，**我们可以将它们输入到模型中，以便模型可以模仿展示的风格。**

**正如你在下面的代码框中观察到的那样，在向 GPT 展示我们期望的内容后，它会模仿给定示例的风格，以相应地生成任何新输出。**

```py
## Code Block

user_input = """
What is the most popular product model of the store?
"""

prompt = f"""
Given the following SQL tables, your job is to provide the required SQL tables
to fulfill any user request.

Tables: <{sql_tables}>. Follow those examples the generate the answer, paying attention to both
the way of structuring queries and its format:
<{fewshot_examples}>

User request: ```{user_input}```py
"""
response = chatgpt_call(prompt)
print(response)
```

正如你在下面的输出中观察到的那样，它有效！

![利用 GPT 模型将自然语言转换为 SQL 查询](img/c6df74d0ef912029cb5660c768da2a5f.png)

我在 Jupyter Notebook 的截图。提示 GPT。

## #3\. 训练模型以计算某个特定变量。

让我们深入一个说明性的场景。假设我们旨在计算哪个产品的交付时间最长。**我们用自然语言向模型提出这个问题，期待一个正确的 SQL 查询。**

```py
## Code Block

user_input = """
What product is the one that takes longer to deliver?
"""

prompt = f"""
Given the following SQL tables, your job is to provide the required SQL tables
to fulfill any user request.

Tables: <{sql_tables}>. Follow those examples the generate the answer, paying attention to both
the way of structuring queries and its format:
<{fewshot_examples}>

User request: ```{user_input}```py
"""
response = chatgpt_call(prompt)
print(response)
```

然而，我们收到的回答远非正确。

![利用 GPT 模型将自然语言转换为 SQL 查询](img/46ebbf792e1f3475fa4070e247639425.png)

我在 Jupyter Notebook 的截图。提示 GPT。

*出了什么问题？*

GPT 模型尝试直接计算两个 datetime SQL 变量之间的差异。**这种计算与大多数 SQL 版本不兼容，特别是对于 SQLite 用户，创建了一个问题。**

*我们如何纠正这个问题？*

解决方案就在我们眼前——我们回到少量示例提示的做法。

通过向模型展示我们通常如何计算时间变量——在这种情况下是交货时间——**我们训练它在遇到类似变量类型时能够复制这一过程。**

例如，SQLite 用户可以使用 julianday() 函数。该函数将任何日期转换为自公历初始纪元以来经过的天数。

这可以帮助 GPT 模型更好地处理 SQLite 数据库中的日期差异。

```py
## Adding one more example
fewshot_examples += """
------ EXAMPLE 4
User: Compute the time that it takes to delivery every product?
System: You first need to join both orders and products tables, filter only those orders that have 
been delivered and compute the difference between both order_creation and delivery_date.: 

SELECT 
    P.product_name AS product_with_longest_delivery,
    julianday(O.delivery_date) - julianday(O.order_creation) AS TIME_DIFF

FROM 
    products AS P
JOIN 
    orders AS O ON P.product_id = O.product_id
WHERE 
    O.order_status = 'Delivered';
"""
```

当我们使用这种方法作为模型的示例时，它会学习我们计算交货时间的偏好方式。**这使得模型更适合生成适用于我们特定环境的功能性 SQL 查询。**

如果我们使用之前的示例作为输入，模型将复制我们计算交货时间的方式，并将为我们具体的环境提供功能性查询。

```py
## Code Block

user_input = """
What product is the one that takes longer to deliver?
"""

prompt = f"""
Given the following SQL tables, your job is to provide the required SQL tables
to fulfill any user request.

Tables: <{sql_tables}>. Follow those examples the generate the answer, paying attention to both
the way of structuring queries and its format:
<{fewshot_examples}>

User request: ```{user_input}```py
"""
response = chatgpt_call(prompt)
print(response)
```

![利用 GPT 模型将自然语言转化为 SQL 查询](img/d6158b05fc630dd555cb19d23a96b31b.png)

我的 Jupyter Notebook 截图。提示 GPT。

# 总结

总之，GPT 模型是将自然语言转换为 SQL 查询的绝佳工具。

不过，它还不完美。

如果没有适当的训练，模型可能无法理解上下文相关的查询或特定操作。

通过使用少量示例提示，**我们可以引导模型理解我们的查询风格和计算偏好。**

这使我们能够充分发挥 GPT 模型在数据科学工作流中的作用，将模型转变为一个强大的工具，以适应我们独特的需求。

**从未格式化的查询到完美定制的 SQL 查询，GPT 模型将个性化的魔力带到我们的指尖！**

[你可以直接在我的 GitHub 上查看我的代码。](https://github.com/rfeers/How-to/blob/main/LLM%20%26%20SQL/Train_LLM_to_improve_SQL_queries.ipynb)

```py
## SQL TABLES

sql_tables = """
CREATE TABLE PRODUCTS (
    product_name VARCHAR(100),
    price DECIMAL(10, 2),
    discount DECIMAL(5, 2),
    product_type VARCHAR(50),
    rating DECIMAL(3, 1),
    product_id VARCHAR(100)
);

INSERT INTO PRODUCTS (product_name, price, discount, product_type, rating, product_id)
VALUES
    ('UltraView QLED TV', 2499.99, 15, 'TVs', 4.8, 'K5521'),
    ('ViewTech Android TV', 799.99, 10, 'TVs', 4.6, 'K5522'),
    ('SlimView OLED TV', 3499.99, 5, 'TVs', 4.9, 'K5523'),
    ('PixelMaster Pro DSLR', 1999.99, 20, 'Cameras and Camcorders', 4.7, 'K5524'),
    ('ActionX Waterproof Camera', 299.99, 15, 'Cameras and Camcorders', 4.4, 'K5525'),
    ('SonicBlast Wireless Headphones', 149.99, 10, 'Audio and Headphones', 4.8, 'K5526'),
    ('FotoSnap DSLR Camera', 599.99, 0, 'Cameras and Camcorders', 4.3, 'K5527'),
    ('CineView 4K TV', 599.99, 10, 'TVs', 4.5, 'K5528'),
    ('SoundMax Home Theater', 399.99, 5, 'Audio and Headphones', 4.2, 'K5529'),
    ('GigaPhone 12X', 1199.99, 8, 'Smartphones and Accessories', 4.9, 'K5530');

CREATE TABLE ORDERS (
    order_number INT PRIMARY KEY,
    order_creation DATE,
    order_status VARCHAR(50),
    product_id VARCHAR(100)
);

INSERT INTO ORDERS (order_number, order_creation, order_status, delivery_date, product_id)
VALUES
    (123456, '2023-07-01', 'Shipped','', 'K5521'),
    (789012, '2023-07-02', 'Delivered','2023-07-06', 'K5524'),
    (345678, '2023-07-03', 'Processing','', 'K5521'),
    (901234, '2023-07-04', 'Shipped','', 'K5524'),
    (567890, '2023-07-05', 'Delivered','2023-07-15', 'K5521'),
    (123789, '2023-07-06', 'Processing','', 'K5526'),
    (456123, '2023-07-07', 'Shipped','', 'K5529'),
    (890567, '2023-07-08', 'Delivered','2023-07-12', 'K5522'),
    (234901, '2023-07-09', 'Processing','', 'K5528'),
    (678345, '2023-07-10', 'Shipped','', 'K5530');
"""
```

**[Josep Ferrer](https://www.linkedin.com/in/josep-ferrer-sanchez)** 是一位来自巴塞罗那的分析工程师。他毕业于物理工程专业，目前在应用于人类流动性的 数据科学 领域工作。他还是一名兼职内容创作者，专注于数据科学和技术。你可以通过 [LinkedIn](https://www.linkedin.com/in/josep-ferrer-sanchez/)、[Twitter](https://twitter.com/rfeers) 或 [Medium](https://medium.com/@rfeers) 联系他。

### 更多相关内容

+   [自然语言处理中的 N-gram 语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [了解 Gorilla：UC Berkeley 和微软的 API 增强 LLM……](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

+   [25 本免费书籍掌握 SQL、Python、数据科学、机器学习……](https://www.kdnuggets.com/25-free-books-to-master-sql-python-data-science-machine-learning-and-natural-language-processing)

+   [数据库内分析：利用 SQL 的分析函数](https://www.kdnuggets.com/2023/07/indatabase-analytics-leveraging-sql-analytic-functions.html)

+   [数据科学中的 SQL：理解和利用连接](https://www.kdnuggets.com/2023/08/sql-data-science-understanding-leveraging-joins.html)

+   [MiniGPT-4：GPT-4 的轻量级替代方案，用于增强…](https://www.kdnuggets.com/2023/04/minigpt4-lightweight-alternative-gpt4-enhanced-visionlanguage-understanding.html)**
