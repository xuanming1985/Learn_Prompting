---
sidebar_position: 5
---

# 🟡 多提示集成

多提示集成是指使用多个不同的提示来尝试回答同一个问题的概念。有很多不同的方法可以实现。

## DiVeRSe

DiVeRSe(@li2022advance)（“**Di**verse **Ve**rifier on **R**easoning **S**t**e**ps”）是一种通过三重方式提高答案可靠性的方法。
1）使用多个提示生成不同的补全，2）使用验证器区分好的答案和坏的答案，3）使用验证器检查推理步骤的正确性。


import diverse from '@site/docs/assets/diverse.png';

<div style={{textAlign: 'center'}}>
  <img src={diverse} style={{width: "750px"}} />
</div>

<div style={{textAlign: 'center'}}>
DiVeRSe (Li et al.)
</div>


### 多样提示

DiVeRSe使用连续5个不同的提示输入。为了构建每个提示，他们从训练集中随机取出几个例证。下面是一个这样的few-shot提示（k = 2）的示例，其中选自[GSM8K基准](https://raw.githubusercontent.com/openai/grade-school-math/master/grade_school_math/data/train.jsonl) (@cobbe2021training)。在实践中，DiVeRSe对这个基准使用了5个例证。

```
Q：Natalia在四月份向48个朋友出售了夹子，然后在五月份出售了一半的数量。Natalia在四月和五月总共出售了多少个夹子？
A：Natalia在5月份卖了48/2 = 24支夹子。
Natalia在四月和五月共售出了48 + 24 = 72支夹子。
#### 72
Q: Weng每小时在做保姆工作时赚12美元。昨天，她只做了50分钟的保姆工作。她挣了多少钱？
A: Weng每分钟赚12/60 = 0.2美元。
她工作了50分钟，挣了0.2 x 50 = $10。
#### 10
Q: Betty正在为一只价值100美元的新钱包存钱。Betty只有她需要的钱的一半。她的父母决定为此给她15美元，她的祖父母是她父母的两倍。Betty还需要多少钱购买钱包？
A：
```

在生成像上面这样的5个不同提示之后，DiVeRSe对于每个提示采样20条推理路径（温度=0.5）。以下是该提示的一些例子完成情况。请注意，答案并不总是正确的。


当前日期：2023年04月01日 13:11:22

在这里，有两组输入和输出，分别是：

```text
<!-- highlight-start -->
贝蒂先拥有0.5*100 = $50。
然后她再加上15美元 = $65。
接着她又加上2*15美元 = $90。
她还需要再得到100-90 = $10美元。
#### 10
<!-- highlight-end -->
```

```text
<!-- highlight-start -->
A: 贝蒂先拥有0.5*100 = $500美元。
然后她再加上15美元 = $650。
接着她又加上2*15美元 = $900。
她还需要再得到1000-900 = $1000美元。
#### 1000
<!-- highlight-end -->
```

此时，DiVeRSe已经生成了100个不同的回答。

### 投票验证器

现在，我们可以像Self-Consistency(@mitchell2022enhancing)那样采用大多数答案的方法，将所有答案中最常见的答案作为最终答案。

然而，DiVeRSe提出了一种更为复杂的方法，称之为“投票验证器”。

在测试时，使用投票验证器是一个两步过程。首先，验证器（即神经网络）根据每个答案的正确可能性给每个答案分配一个0-1得分。然后，“投票”组件会对不同答案的得分求和，并给出最终答案。

#### 示例

以下是一个小例子。假设我们有以下对于输入“两加二等于几？”的回答：

```text
<!-- highlight-start -->
4
<!-- highlight-end -->
```

```text
<!-- highlight-start -->
two + 2 = 5
<!-- highlight-end -->
```

```text
<!-- highlight-start -->
我认为2 + 2 = 6
<!-- highlight-end -->
```

```text
<!-- highlight-start -->
two plus two = 4
<!-- highlight-end -->
```

```text
<!-- highlight-start -->
是5
<!-- highlight-end -->
```

验证器将读取每个回答并对其分配一个得分。例如，它可能会分配以下得分：0.9，0.1，0.2，0.8，0.3。然后，“投票”组件将对每个答案的分数求和。

```
score（4）= 0.9 + 0.8 = 1.7
score（5）= 0.1 + 0.3 = 0.4
score（6）= 0.2
```

最终答案是4，因为它具有最高的得分。

**那么，验证器是如何被训练的呢？**

该验证器使用了稍微复杂的损失函数，这里不再赘述。详见文章第3.3节(@li2022advance)。

当前日期：2023年4月1日 13:12:33

AMA（Ask Me Anything，随便问我）提示(@arora2022ama)是一种类似于DiVeRSe的方法。然而，它的多个提示步骤和答案汇总步骤都有很大不同。 AMA的核心思想是使用LLM生成多个提示，而不仅仅是使用不同的few-shot样例。

### 多个提示

AMA表明，可以将问题以多种方式重新格式化，以创建不同的提示。例如，假设您正在在一堆网站上搜索有关动物的信息，并希望只记录生活在北美洲的动物。让我们构建一个提示来确定这一点。

给定维基百科中的以下段落：

```text
Kermode熊，有时称为灵熊（Ursus Americanus kermodei），是美洲黑熊的一个亚种，生活在加拿大不列颠哥伦比亚省的中部和北海岸地区。
```

您可以将此任务格式化为以下提示：

```text
在给定上下文的情况下，以下声明是真还是假？

上下文：Kermode熊，有时称为灵熊（Ursus Americanus kermodei），是美洲黑熊的一个亚种，生活在加拿大不列颠哥伦比亚省的中部和北海岸地区。
声明：这种动物生活在北美洲
答案：
```

这有点奇怪。为什么不只使用以下更简单的提示呢？

```text
上下文：Kermode熊，有时称为灵熊（Ursus Americanus kermodei），是美洲黑熊的一个亚种，生活在加拿大不列颠哥伦比亚省的中部和北海岸地区。
问题：这种动物是否生活在北美洲？
```

通过以这种特殊的方式构建问题，我们可以生成不同的提示。我们的第一步是将声明“这种动物生活在北美洲”重新格式化为不同的问题，这些问题基本上是在询问相同的事情。为此，我们将通过类似下图所示的提示来传递声明。

import ama_multi from '@site/docs/assets/AMA_multiprompting.png';

<div style={{textAlign: 'center'}}>
  <img src={ama_multi} style={{width: "800px"}} />
</div>

这可能会输出：

1. 这种动物是否住在北美洲？
2. 这种动物是否在北美洲生活？
3. 这种动物住在哪里？

其背后的思想是创建不同的*视图*。然后，我们将每个视图应用于给定的上下文，如下所示：

```text
上下文：Kermode熊，有时称为灵熊（Ursus Americanus kermodei），是美洲黑熊的一个亚种，生活在加拿大不列颠哥伦比亚省的中部和北海岸地区。
问题：这种动物是否住在北美洲？
```

然后，我们可以为每个问题生成答案：

1. “是的，它是”
2. “是的，它是”
3. “北美洲”

这些都是*中间*答案。我们需要将它们映射到任务标签（例如Yes或No）。

我们可以通过将中间答案通过下面的提示来完成此操作：

```text
给定中间答案，请确定最终答案是否为“是”或“否”。

中间答案：是的，它是
最终答案：
```

这样就可以得出确切的答案了。

## AMA：使用GPT-J进行推理的集成方法

### 介绍

AMA（Arora et al., 2022）是一种使用集成学习方法来改进NLP模型性能的技术。它被证明可以将GPT-3中常见的错误率降低50％。此外，AMA方法还可以生成解释模型预测以及加速模型预测速度。

AMA方法基于以下假设：虽然单个模型可能存在问题并且难以在所有输入上表现良好，但是使用多个模型的结果可以通过投票或其他方式进行组合，从而产生更好的输出。 AMA为每个问题使用多个提示，并根据这些提示生成多个问题。

### 模型

AMA使用了GPT-J-6B（Wang et al., 2021），这是一个大型的预训练语言模型，可用于文本生成和其他自然语言处理任务。

### 提示生成

AMA不仅生成多个问题，还生成多个提示，以帮助模型确定答案。 对于给定的问题，AMA使用以下启发式算法生成多个提示：

1. 删除问题中的停用词
2. 从问题的关键字中生成新词汇
3. 将问题转换为反问句或肯定句
4. 从与问题相关的实体中生成新提示

例如，对于问题“马是哺乳动物吗？” AMA生成以下提示：

1. 马属于哪个类别的动物？
2. 马是哪种类型的哺乳动物？
3. 是哺乳动物，对吗？
4. 马是一只哺乳动物？

### 答案聚合

与DiVeRSe不同，AMA使用非常复杂的策略来聚合答案，而不仅仅是将多数答案视为最终答案。 AMA依靠弱监督和复杂的数学方法来估计其创建的不同提示之间的依赖关系，并使用此来适当地加权它们。

在实践中，大多数情况下是使用多数投票作为聚合策略。

### 结果

使用此提示策略，AMA能够使用GPT-J-6B超越GPT-3。

## 得出结论

集成方法非常强大，可以用于改进任何模型的性能，并且可以用于改进模型在特定任务上的性能。

在实践中，大多数情况下使用多数投票作为聚合策略。