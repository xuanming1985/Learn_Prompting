---
sidebar_position: 0
---
# 🟢 介绍

本章介绍了简单的提示技术和术语。为了理解提示/提示工程，您首先需要了解一些**非常**基本的AI概念。如果您已经了解下述主题，请随时跳到[下一篇文章](https://learnprompting.org/docs/basics/prompting)。

## 什么是AI？

对于我们而言，人工智能（AI）是一个领域，其中人们创造了“聪明”的算法，这些算法“像人类一样“思考”[^1]。这些算法可以编写**论文**、解决**数学问题**并创建**艺术作品**。该领域的最新进展创造了如此先进的AI，以至于它们可以编写引人信服的**销售电子邮件**、**新闻文章**甚至赢得[艺术竞赛](https://impakter.com/art-made-by-ai-wins-fine-arts-competition/)。

AI当前正在彻底改变许多行业，包括**新闻业**、**医疗保健**和**教育**。如果您尚未在工作场所/课堂上看到它，那么您很快就会看到。尽管AI非常有用，但它需要人类指导它做什么。通常情况下，AI就像非常聪明的五岁孩子。它们可以做很多事情，但需要仔细的指导才能做得好。

正确引导AI是一项非常强大的技能。那么，如何与它们交流？本课程讨论的AIs可以理解英语、汉语（和其他语言），因此我们通常可以描述我们想要完成的任务。例如，我们可以告诉一个AI `写一篇关于AI好处的文章`，它会生成如下内容[^2]:

```
最近几年，人工智能（AI）在科技界成为热议话题。随着技术的不断发展，AI正变得日益相关，具有颠覆我们生活的许多方面的潜力。从其在医疗保健和交通运输中的应用到其在教育和制造业中的应用，AI已经向我们展示了其在多个方面受益于社会的巨大潜力。

AI最显著的好处是其自动化繁琐的任务的能力。AI系统可以用于自动化大规模操作，如数据分析、客户服务甚至是产品生产。这样可以释放出人类劳动力，专注于需要更多人类智能和创意的复杂创意任务。AI也可以用于分析大量数据并在比人类更短的时间内做出决策。这在诸如金融等行业中可能是有益的，AI可以检测股市中的模式并提供未注意到的见解。

AI还可以用于预测和产生可用于提高效率和准确性的见解。例如，AI可以用于预测客户趋势和偏好，帮助企业做出更好的决策并增加利润。AI也可以用于识别各种行业中的潜在风险和机会，如金融、医疗保健和制造业。这可以帮助企业做出明智的决策并减少昂贵的错误风险。

最后，AI可以用于改善我们对世界的理解。 AI可用于分析大数据集并生成可用于更好地了解我们周围世界的见解。这可以用于帮助我们更好地了解气候变化的影响、开发治疗疾病的新方法，甚至预测事件，如地震和洪水。

总的来说，AI有可能彻底改变我们生活的许多方面。从自动化繁琐的任务到产生见解和预测，AI可以用于提高效率、准确性和我们对世界的理解。随着技术的不断发展，AI将变得越来越相关，它对社会受益的潜力也只会增长。
```

## 我为什么要关心？

AI可用于自动化您当前花费无数小时完成的任务。我们上面提到了几个例子，但您还可以考虑到任何您正在执行的稍微重复的任务。这可能是编写电子邮件、编写报告或甚至编写代码。如果您可以向AI描述此任务，则它可能可以为您执行任务或至少给您一个思考的角度或方向。


## 如何入门？

阅读本章余下内容，然后查看您感兴趣的其他章节。

在阅读下一篇文章之前，请注意您不需要任何技术背景来进行提示工程。其中大部分都是试错，您可以边学边做。

### Dyno嵌入

本课程提供交互式学习体验。您可以使用本课程讨论的练习和[Dyno](https://trydyno.com)嵌入进行实验，这些嵌入分布在整个网站上。

以下是Dyno嵌入的**图像**示例：

import dyno from '../assets/dyno_example.png';
import key from '../assets/API_key.png';

<div style={{textAlign: 'center'}}>
  <img src={dyno} style={{width: "750px"}} />
</div>

您应该能够在本段文字下方看到与此图像完全相同的嵌入。如果您不能看到它，则可能需要启用JavaScript或使用其他浏览器。

<hr/>
嵌入在这里：
<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="生成10种冰淇淋口味的逗号分隔列表：" initial-response="巧克力、香草、草莓、薄荷巧克力、石榴路、曲奇饼、胡桃山核桃、那不勒斯、咖啡、椰子" max-tokens="256" box-rows="3" model-temp="0.7" top-p="1">
    <noscript>无法加载Dyno嵌入：必须启用JavaScript</noscript>
</div>
<hr/>

假设您可以看到它，请单击“生成”按钮。如果这是您第一次使用它（或者您在新浏览器/已清除cookie 中）。

:::note
此网站为会员提供了免费的调试机会，请大家珍惜使用！
:::

现在，您已经拥有了所有入门所需的信息。祝您学习愉快！

[^1]: 严格地说，它们不像人类一样“思考”，但这是解释其工作原理的一种简单方式。
[^2]: 这段话实际上是由一个AI（GPT-3 davinci-003）编写的。