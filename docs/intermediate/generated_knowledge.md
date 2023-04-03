---
sidebar_position: 7
---

# 🟡 生成式知识

生成式知识的基本思想是在向%%LLM|LLM%% 提出最终问题之前，先要求它生成有关所提问题的潜在有用信息。

例如，假设您想撰写一篇关于某种动物的小型博客文章，比如[白熊](https://zh.wikipedia.org/wiki/%E7%99%BD%E7%86%8A)。在让 %%LLM|LLM%% 写博客文章之前，您可以要求它生成一些关于白熊的事实。这将有助于LLM撰写更具信息性的博客文章。有两种简单的方法。

## 单提示法

第一种方法要求LLM在一次提示中生成知识然后写文章。

```text
生成4个有关Kermode熊的事实，然后使用这些事实来撰写一篇内容简短的博客文章：
// highlight-start
Kermode熊，又称灵熊，是黑熊的一个罕见亚种，在加拿大不列颠哥伦比亚省的沿海地区发现。

1. Kermode熊是黑熊的一个独特亚种，约有10%的人口有白色毛皮。

2. Kermode熊是不列颠哥伦比亚省的原住民的象征，被视为灵性动物。

3. Kermode熊是生态系统的重要组成部分，帮助传播种子和维持森林的平衡。

4. Kermode熊是一种受保护的物种，其人口受到省政府的严密监控。

Kermode熊是一种独特而特殊的动物，生活在加拿大不列颠哥伦比亚省的沿海地区。约有10%的这个黑熊亚种的人口具有白色毛皮，是一种罕见珍奇动物。Kermode熊是不列颠哥伦比亚省的原住民的象征，被视为灵性动物。它是生态系统的重要组成部分，帮助传播种子和维持森林的平衡。Kermode熊是一种受保护的物种，其人口受到省政府的严密监控。它是美丽而威严的动物，我们有责任保护和保护这种物种。
// highlight-end
```


## 双提示法

在这种方法中，我们首先提示%%LLM|LLM%%生成有关熊的事实：

```text
生成10个Kermode熊的事实：

// highlight-start
1. Kermode熊，又称灵熊，是黑熊的一个罕见亚种，在加拿大不列颠哥伦比亚省发现。

2. Kermode熊具有独特的白色或奶油色皮毛，这是由隐性基因引起的。

3.对于不列颠哥伦比亚省的原住民来说，Kermode熊是希望和更新的象征。

4. Kermode熊是一种受保护的物种，被列为加拿大濒危野生动物状况委员会所关注的物种之一。
// highlight-end
```

然后，我们把这些信息输入到另一个提示中来撰写博客文章：

```text
使用上述事实撰写一段关于Kermode熊的一段博客文章：

// highlight-start
Kermode熊，又称灵熊，是黑熊的一个罕见亚种，在加拿大不列颠哥伦比亚省发现。这种独特的熊具有白色或奶油色皮毛，这是由隐性基因引起的，对于不列颠哥伦比亚省的原住民来说，Kermode熊是希望和更新的象征。Kermode熊是一种受保护的物种，被列为加拿大濒危野生动物状况委员会所关注的物种之一，这使其成为该地区生物多样性的重要组成部分。

// highlight-end
```

建议使用第二种方法，因为它可以更可靠地生成更长的内容。


initial-prompt翻译：生成式知识的基本思想是在向%%LLM|LLM%% 提出最终问题之前，先要求它生成有关所提问题的潜在有用信息。
initial-response翻译：生成式知识的基本思想是在向%%LLM|LLM%% 提出最终问题之前，先要求它生成有关所提问题的潜在有用信息。
email里面的文本信息翻译：请您撰写一篇关于白熊的小型博客文章。在让%%LLM|LLM%%写博客文章之前，您可以要求它生成一些有关白熊的事实。

## 另一个用例

生成知识的方法实际上是为了解决完全不同的问题而引入的，那就是回答困难问题。考虑以下问题，GPT-3错误回答：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="哪个国家更大，刚果还是南非？" initial-response="南非比刚果更小。" max-tokens="256" box-rows="3" model-temp="0" top-p="0">
    <noscript>Failed to load Dyno Embed: JavaScript must be enabled</noscript>
</div>

<br/>

如果我们首先要求%%LLM|LLM%%生成有关刚果和南非的事实，我们就可以使用那些信息来正确回答问题。理论上，这类似于%%CoT|CoT提示%%，因为我们实际上是让LLM以相关事实的形式生成中间推理。

让我们从第一步开始，即*知识生成*。我们可以要求%%LLM|LLM%%生成关于刚果和南非的事实：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="关于南非和刚果的大小，请生成一些知识：" initial-response="南非是世界第25大国，面积为1,221,037平方公里（471,445平方英里）。另一方面，刚果是世界第11大国，面积为2,344,858平方公里（905,567平方英里）。南非的面积几乎是刚果的一半，人口为5,770万，而刚果的人口为8,43万。南非是该地区最人口众多的国家，而刚果是第二人口众多的国家。" max-tokens="256" box-rows="6" model-temp="0" top-p="0">
    <noscript>Failed to load Dyno Embed: JavaScript must be enabled</noscript>
</div>

<br/>

接下来，让我们使用这些知识来正确回答问题。这就是*知识整合*的步骤！

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="南非是世界第25大国，面积为1,221,037平方公里（471,445平方英里）。另一方面，刚果是世界第11大国，面积为2,344,858平方公里（905,567平方英里）。南非的面积几乎是刚果的一半，人口为5,770万，而刚果的人口为8,43万。南非是该地区最人口众多的国家，而刚果是第二人口众多的国家。\n\n哪个国家更大，刚果还是南非？" initial-response="刚果比南非更大。" max-tokens="256" box-rows="15" model-temp="0" top-p="0">
    <noscript>Failed to load Dyno Embed: JavaScript must be enabled</noscript>
</div>

## 更多技术讨论

虽然上述用例与引入生成知识的方式类似，但并不完全相同。下面的内容涵盖了介绍此方法时的更多技术背景。它遵循我们上面看到的两个中间步骤（知识生成和知识整合）模式。

import KGImage from '@site/docs/assets/knowledge_generation.png';

<div style={{textAlign: 'center'}}>
  <img src={KGImage} style={{width: "750px"}} />
</div>

<div style={{textAlign: 'center'}}>
生成的知识（Liu等人）
</div>

### 知识生成

在知识生成步骤中，要求LLM生成一组关于问题的事实。LLM以few-shot方式进行提示，如下所示。使用相同提示生成M个补全方案（类似于自洽方法）。

import KGP1Image from '@site/docs/assets/gen_k_p1.png';

<div style={{textAlign: 'center'}}>
  <img src={KGP1Image} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
生成的知识示例（Liu等人）
</div>


### 知识整合

接下来，我们生成“知识增强”问题，并使用它们提示%%LLM|LLM%%，以获得最终答案。最简单的理解方法是通过以下示例。

我们假设要回答的问题是“大多数袋鼠有<mask\>条腿”。假设在知识生成步骤中，我们生成了2个知识（M=2）：

- 知识1：`袋鼠是在澳大利亚生活的有袋动物。`

- 知识2：`袋鼠是一种有5条腿的有袋动物。`

现在，我们将每个知识与问题连接起来，生成知识增强的问题：

- 知识增强问题1：`大多数袋鼠有<mask\>条腿。袋鼠是在澳大利亚生活的有袋动物。`

- 知识增强问题2：`大多数袋鼠有<mask\>条腿。袋鼠是一种有5条腿的有袋动物。`

然后，我们使用这些知识增强问题提示LLM，并获取最终答案提议：

- 答案1：`4`

- 答案2：`5`

我们选择概率最高的答案作为最终答案。最高概率可能是答案令牌的softmax概率，也可能是答案令牌的对数概率。

## 讲述增强语言模型

讲述增强（@sun2022recitationaugmented）方法与生成的知识相似（基本相同）。但是，它比生成的知识实现要简单得多。

import RImage from '@site/docs/assets/recitation.png';

<div style={{textAlign: 'center'}}>
  <img src={RImage} style={{width: "250px"}} />
</div>

这里的想法是在同一步骤中提示LLM以生成信息和答案。它在讲授/生成知识并在同一步骤中回答问题方面的差异是主要区别。

重申一下，该方法使用多个（问题、讲授、答案）示例提示模型，然后提出问题。作者指出，这种方法可以与自洽或多路径完成相结合。

## 备注

- 生成的知识在各种常识数据集上都表现出改进。

- 对应所选答案的知识称为_选择的知识_。

- 在实践中，您可以将最频繁出现的答案作为最终答案。