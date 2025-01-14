# 使用 NLP 来改善你的简历

> 原文：[`www.kdnuggets.com/2021/02/nlp-improve-resume.html`](https://www.kdnuggets.com/2021/02/nlp-improve-resume.html)

评论

**由[David Moore](https://www.linkedin.com/in/mooredvd/)，金融技术实施专家**

招聘人员正在使用越来越复杂的软件和工具来扫描和匹配简历与发布的职位和职位规格。如果你的简历（CV）是通用的，或者职位规格模糊和/或通用，这些工具可能会对你不利。人工智能确实在对抗你的职位申请，我不确定你是否知道或会接受这一点！但让我展示一些可以帮助你**平衡机会**的技巧。自然，我们将使用 NLP（自然语言处理）、Python 和一些 Altair 可视化。你准备好反击了吗？

设想你对在网上看到的一个好职位感兴趣。还有多少其他人看到过相同的职位？有大致相同的经验和资格吗？你认为有多少申请者申请了？会少于 10 个还是少于 1,000 个？

此外，考虑到面试小组可能只有 5 名强候选人。那么你如何‘筛选’出 995 份申请，以精炼并呈现出仅 5 名强候选人？这就是为什么我说你需要平衡机会，否则就会被淘汰！

### 处理 1,000 份简历（CV）

我想，首先，你可以将这些简历分成 3 或 5 堆。打印出来，并分配给人工阅读者。每个阅读者从他们的堆中选择一份。5 个阅读者就是 200 份简历——挑选出最好的一两份。阅读这些将需要很长时间，最终可能只会得出一个答案。我们可以使用 Python 在几分钟内阅读所有这些简历！

在 Medium 上的文章‘[如何使用 NLP（Spacy）筛选数据科学简历](https://towardsdatascience.com/do-the-keywords-in-your-resume-aptly-represent-what-type-of-data-scientist-you-are-59134105ba0d)’中演示了只需两行代码就能收集到那 1,000 份简历的文件名。

```py
#Function to read resumes from the folder one by one
mypath='D:/NLP_Resume/Candidate Resume' 
onlyfiles = [os.path.join(mypath, f) for f in os.listdir(mypath) 
             if os.path.isfile(os.path.join(mypath, f))]
```

变量‘onlyfiles’是一个 Python 列表，包含了通过 Python os 库获取的所有简历的文件名。如果你研究这篇文章，你还会看到如何基于关键词分析几乎自动对你的简历进行排名和淘汰。由于我们试图平衡机会，我们需要关注你所期望的职位规格和你当前的简历。它们匹配吗？

### 匹配简历和职位描述

为了平衡机会，我们希望扫描职位描述、简历，并测量匹配度。理想情况下，我们的输出能帮助你在游戏中调整策略。

### 阅读文档

由于这是你的简历，你可能有 PDF 或 DOCX 格式。Python 模块可以读取大多数数据格式。图 1 演示了如何将文档内容保存到文本文件中并读取这些文档。

![图](img/4306a58db78e77c080f98e53e6550bd5.png)

图 1: 从磁盘读取文本文件并创建文本对象。图片由作者从 Visual Studio Code — Jupyter Notebook 中拍摄。

第一步总是打开文件并读取行。接下来的步骤是将字符串列表转换为单一文本，并在过程中进行一些清理。图 1 创建了变量 ‘jobContent’ 和 ‘cvContent’，这些变量代表一个包含所有文本的字符串对象。下一个代码片段展示了如何直接读取 Word 文档。

```py
import docx2txt
resume = docx2txt.process("DAVID MOORE.docx")
text_resume = str(resume)
```

变量 ‘text_resume’ 是一个字符串对象，保存了简历中的所有文本，就像之前一样。你也可以使用 PyPDF2。

```py
import PyPDF2
```

说到这里，已经有一系列选项供从业者阅读文档并将其转换为清洁处理过的文本。这些文档可能很长，很难阅读，而且坦白说也很枯燥。你可以从总结开始。

### 处理文本

我喜欢 Gensim 并经常使用它。

```py
from gensim.summarization.summarizer import summarize
from gensim.summarization import keywords
```

我们通过读取一个 Word 文件创建了变量 ‘resume_text’。让我们对简历和职位发布进行总结。

```py
print(summarize(text_resume, ratio=0.2))
```

Gensim.summarization.summarizer.summarize 将为你创建一个简洁的总结。

```py
summarize(jobContent, ratio=0.2)
```

现在你可以阅读职位角色和你现有简历的整体总结！你是否错过了总结中强调的职位角色的任何信息？小的细节可以帮助你推销自己。你的总结文档是否有意义，并突出了你的核心素质？

也许仅仅是简明的总结是不够的。接下来，让我们衡量你的简历与职位要求的相似度。图 2 提供了代码。

![图](img/cb8d354f49f7dceacc7834fe7253417c.png)

图 2 — 匹配两个文档并给出相似度评分的代码。图片由作者提供。

大致来说，我们首先列出文本对象，然后创建 sklearn CountVectorizer() 类的实例。我们还导入了 cosine_similarity 度量，它帮助我们测量两个文档的相似度。‘**你的简历与职位描述匹配约 69.44%**’。这听起来很棒，但我不会因此自满。现在你可以阅读文档的摘要并获取相似度测量。情况正在改善。

接下来，我们可以查看职位描述中的关键词，并查看哪些在简历中匹配到了。我们是否遗漏了一些可以提高匹配度至 100% 的关键词？现在转到 spacy。到目前为止，这是一段相当曲折的旅程。Gensim、sklearn 现在还有 spacy！希望你没有感到头晕！

```py
from spacy.matcher import PhraseMatcher
matcher = PhraseMatcher(Spnlp.vocab)
from collections import Counter
from gensim.summarization import keywords
```

我们将使用 spacy 的 PhraseMatcher 功能来匹配职位描述中的关键短语与简历中的内容。Gensim 关键词可以帮助提供这些短语以供匹配。图 3 显示了如何进行匹配。

![图](img/ff153c9f6609ddbf2b7c57810b71f9f9.png)

图 3: 使用关键词和短语匹配来交叉引用文档。图片由作者提供。

使用图 3 中的代码片段，可以提供匹配的关键词列表。图 4 显示了总结这些关键词匹配的方法。使用 Collections 中的 Counter 字典。

![Figure](img/474218adfafd8597caabf2ce2dbe0710.png)

图 4 — 使用集合。计数器来计算关键字的命中次数。图片由作者提供。

“reporting” 一词包含在职位描述中，而简历中有 3 次命中。职位发布中有哪些短语或关键字在简历中没有？我们可以添加更多吗？我使用 Pandas 来回答这个问题 — 你可以在图 5 中看到输出。

![Figure](img/68fe3f1342f36a1d7a62a4f3846a67b1.png)

图 5 — 职位描述中的关键字在简历中未提及。图片由作者提供。

如果这是真的，那也很奇怪。文档级别的匹配度为 69.44%，但看看那些在简历中未提及的长列表的关键字。图 6 显示了提及的关键字。

![Figure](img/b15b7316b64ba6626de340dfc823ccef.png)

图 6 使用 Pandas 匹配的关键字。图片由作者提供。

实际上，与职位规范的关键字匹配非常少，这使我对 69.44% 的余弦相似度度量表示怀疑。尽管如此，情况有所改善，因为我们可以看到职位规范中的关键字在简历中未出现。关键字匹配较少意味着你更有可能被淘汰。查看缺失的关键字，你可以立即加强简历，然后重新进行分析。不过，仅仅在简历中加入关键字会产生负面后果，你必须非常谨慎。你可能通过初步的自动筛选，但由于被认为写作技巧不足而被淘汰。我们确实需要对短语进行排名，并专注于职位规范中的核心话题或单词。

接下来我们来看排名短语。对于这个练习，我将使用我自己的 NLP 类以及之前使用过的一些方法。

```py
from nlp import nlp as nlp
LangProcessor = nlp()
keywordsJob = LangProcessor.keywords(jobContent)
keywordsCV = LangProcessor.keywords(cvContent)
```

使用我自己的类，我从之前创建的职位和简历对象中恢复了排名短语。下面的代码片段提供了方法定义。我们现在使用 rake 模块来提取 ranked_phrases 和 scores。

```py
def keywords(self, text):
 keyword = {}
 self.rake.extract_keywords_from_text(text)
 keyword['ranked phrases'] = self.rake.get_ranked_phrases_with_scores()
 return keyword
```

图 7 提供了方法调用的输出示意图。

![Figure](img/2ad796cdb63bc1024c598298c8be6077.png)

图 7 — 职位发布中的排名短语。图片由作者使用自己的代码提供。

‘project management methodology — project management’ 排名为 31.2，因此这是职位发布中的最关键话题。简历中的关键短语也可以通过小的变动来打印出来。

```py
for item in keywordsCV['ranked phrases'][:10]:
 print (str(round(item[0],2)) + ' - ' + item[1] )
```

从简历和职位发布中读取顶部短语，我们可以问自己是否存在匹配或相似度的程度？我们当然可以运行一个序列来找出！以下代码创建了职位发布和简历中的排名短语之间的交叉引用。

```py
sims = []
phrases = []
for key in keywordsJob['ranked phrases']:
 rec={}
 rec['importance'] = key[0]
 texts = key[1] sims=[]
 avg_sim=0
 for cvkey in keywordsCV['ranked phrases']:
  cvtext = cvkey[1]
  sims.append(fuzz.ratio(texts, cvtext))
  #sims.append(lev.ratio(texts.lower(),cvtext.lower()))
  #sims.append(jaccard_similarity(texts,cvtext)) count=0
 for s in sims:
 count=count+s
 avg_sim = count/len(sims)
 rec['similarity'] = avg_sim
 rec['text'] = texts
 phrases.append(rec)
```

请注意，我们使用 fuzzy-wuzzy 作为匹配引擎。代码中还有 Levenshtein 比率和 jaccard_similarity 函数。图 8 提供了这可能是什么样子的示意图。

![图像](img/9fc309d648c7e6e1186f7dec58de93bd.png)

图 8 显示了职位描述与简历之间交叉引用的关键词。

“重要性”变量是来自简历的排名短语分数。“相似度”变量是模糊匹配的比率分数。术语“项目管理方法论”排名 31.2，但交叉引用评分的简历短语的平均分数仅为 22.5。尽管项目管理是职位的首要任务，但简历在不同的技术项上得分更高。你可以通过进行类似的练习来了解人工智能如何对待你的申请。

![图像](img/de97a1c49299de6aae4368ffbb712cdb.png)

图 9 显示了简历中的术语重要性与影响力。图像由作者提供。

图 9 显示了另一种视角。通过处理词汇（单词），可以看到每个单词在职位描述中的重要性与在简历中出现的次数——特定单词在文档中的出现次数越多，影响力越大。词汇“财务”在职位描述中的重要性较低，但在简历中具有较高的影响力。这是一个财务人员在寻找 IT 工作吗？单词可能会在人工智能面前背叛你！

我相信你现在已经明白了。使用 NLP 工具和库可以帮助真正理解职位描述并衡量相对匹配度。虽然这并不完全可靠或绝对正确，但确实可以平衡竞争。你的用词很重要，但你不能在简历中随意堆砌关键词。你必须写一份强有力的简历，并申请适合你的职位。文本处理和文本挖掘是一个大话题，我们仅仅触及了可以做的表面。我发现文本挖掘和基于文本的机器学习模型非常准确。让我们看一些使用 Altair 的可视化效果，然后得出结论。

### Altair 可视化

我最近一直在使用 Altair，而且比 Seaborn 或 Matplotlib 更多。Altair 的语法让我非常满意。我制作了三个可视化图表来帮助讨论——图 10 显示了简历中的关键词重要性和影响力。通过颜色比例尺，我们可以看到像“采用”这样的词在简历中出现了两次，但在职位发布中优先级较低。

![图像](img/e8dcbd2bf65809e41b4da6a8abb5a7ae.png)

图 10 显示了 Altair 的可视化效果。图像由作者提供。图表显示了简历中按重要性和影响力排列的词汇。

图 11 显示了职位发布中发现的排名主题与简历中发现的主题的交叉引用。最重要的短语是“项目管理..”但在简历中排名的术语中得分较低。

![图像](img/eee9c33eb18d02b18eeaa2022fec416d.png)

图 11\. 显示了按排名短语和简历与职位发布之间的相关性的堆叠条形图。

图 12 绘制了相似词汇。财务在简历中使用了 10 次，但在职位发布中完全没有提及。词汇“项目”在简历（CV）中提到过，并且也出现在职位发布中。

![图像](img/9be28d669e055ed510a63abddb2e3d17.png)

图 12：文档间关键词重叠的分析。图片由作者提供。

从图表来看，简历和职位描述似乎不匹配。共享的关键词非常少，排名的短语看起来也很不同。这就是为什么你的简历会被淹没在杂草中的原因！

### 结论

阅读这篇文章可能更像是一部高预算的“**打得死的 CGI 好莱坞**”电影。所有的大牌演员通常都会出现在这些大片中。本文中大 NLP 库扮演了主角，我们甚至还有许多其他的“客串出演”，可能是一些较老且更成熟的名字，比如 NLTK。我们使用了像 Gensim、Spacy、sklearn 等库，并演示了它们的使用。我的类也作为特别出演，封装了 NLTK、rake、textblob 以及一堆其他模块，所有这些模块都在进行文本分析并提供见解，向你展示如何避免错失实现梦想工作的机会。

实现梦想工作的关键在于对细节的明确和坚持不懈的关注，以及对求职申请、简历和求职信的精心准备。使用自然语言处理不会让你成为最佳候选人。这要靠你自己！但它可以提高你在 AI 驱动的早期筛选中的胜算。

![图示](img/fc3c5e44f24d4616960335eb92478811.png)

图片由 [Vidar Nordli-Mathisen](https://unsplash.com/@vidarnm?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

每个渔夫都知道需要好的饵料！

**简介：[David Moore](https://www.linkedin.com/in/mooredvd/)** ([@CognitiveDave](https://twitter.com/CognitiveDave)) 是金融技术实施专家。David 对开发优秀的企业服务感兴趣，这些服务可以赋能用户，解放他们免于不必要的工作，并提供相关的见解。

[原文](https://towardsdatascience.com/ai-is-working-against-your-job-application-bec65d496d22)。经授权转载。

**相关：**

+   [如何获得数据科学面试机会：寻找工作、接触门卫和获取推荐](https://www.kdnuggets.com/2021/02/data-science-interviews-finding-jobs-reaching-gatekeepers-getting-referrals.html)

+   入门指南：5 个必备的自然语言处理库

+   每个数据科学家都应该知道的 6 种 NLP 技术

### 相关话题

+   [数据科学简历中的必备要素](https://www.kdnuggets.com/2022/06/musthaves-data-science-resume.html)

+   [7 个机器学习作品集项目提升简历](https://www.kdnuggets.com/2022/09/7-machine-learning-portfolio-projects-boost-resume.html)

+   [KDnuggets 新闻，9 月 21 日：7 个机器学习作品集项目……](https://www.kdnuggets.com/2022/n37.html)

+   [学生在数据科学简历中遗漏的 7 件事](https://www.kdnuggets.com/7-things-students-are-missing-in-a-data-science-resume)

+   [7 个 AI 作品集项目来提升简历](https://www.kdnuggets.com/7-ai-portfolio-projects-to-boost-the-resume)

+   [如何使用 ChatGPT 提升你的数据科学技能](https://www.kdnuggets.com/2023/03/chatgpt-improve-data-science-skills.html)
