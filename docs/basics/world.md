---
sidebar_position: 200
---

# 🟢 理解人工智能思维

读者您好，恭喜您完成了介绍章节。在开始阅读本课程的其余部分之前，有一些简单的事情您需要了解，包括不同的人工智能及其工作原理。

导入音乐图片的代码块如下：

```python
import music_image from '@site/docs/assets/music+image.png';

<div style={{textAlign: 'center'}}>
  <img src={music_image} style={{width: "850px"}} />
</div>

<div style={{textAlign: 'center'}}>
  Audio being generated from an image.
</div>
```

## 不同的人工智能

有成千上万甚至是百万种人工智能。有些比其他的更好。不同的人工智能可以产生 [图像](https://openai.com/product/dall-e-2)，[音乐](https://google-research.github.io/seanet/musiclm/examples/)，[文本](https://platform.openai.com/playground)，甚至是[视频](https://makeavideo.studio/)。请注意，这些都是*生成型*人工智能，基本上是创造*东西*的人工智能。还有*鉴别型*人工智能，是用于*分类*东西的人工智能。例如，您可以使用图像分类器来判断某个图像是猫还是狗。本课程不会使用任何鉴别型人工智能。

目前仅有一些生成型人工智能足够先进以在prompt engineering领域中变得特别有用。在本课程中，我们主要使用GPT-3和ChatGPT。如上一页所述，ChatGPT是一个聊天机器人，而GPT-3则不是。**它们在回答相同的问题时通常会产生不同的响应。** 如果您是开发人员，我建议使用GPT-3，因为它更易于复现。如果您不是开发人员，我建议使用[ChatGPT](https://learnprompting.org/docs/category/%EF%B8%8F-image-prompting)，因为它更易于使用。本课程中的大多数技术都可以应用于两种人工智能。但是，其中一些将只在GPT-3中使用，因此我们建议您使用GPT-3，如果您想使用本课程中的所有技术。

我们还将在图像生成部分中使用[Stable Diffusion](https://beta.dreamstudio.ai/home) 和 [DALLE](https://openai.com/product/dall-e-2)。请查看更多相关人工智能 [here](https://learnprompting.org/docs/products#chatbots)。

## 这些人工智能是如何工作的

本节描述了流行的生成型**文本**人工智能的一些方面。这些人工智能的大脑由数十亿个人工神经元组成。这些神经元的结构被称为transformer架构。这是一种相当复杂的神经网络类型。您需要了解以下内容：

1. 这些人工智能只是数学函数。与$f(x) = x^2$不同，它们更像是f（数千个变量）= 数千个可能的输出。
2. 这些人工智能通过将语句分成单词/子词（例如，人工智能可以将"I don't like"读作“"I", "don", "'t" "like"”）来理解句子。然后将每个标记转换为一个数字列表，以便AI可以处理它。
3. 这些人工智能基于先前的单词/标记（例如，人工智能可能在“I don't like”之后预测“apples”）来预测句子中的下一个单词/标记。它们写下的每个标记都基于它们已经看到和写下的先前标记；每次它们写下一个新标记时，它们都会暂停一下，思考接下来应该是什么标记。
4. 这些人工智能同时查看每个标记。它们不像人类一样从左到右或从右到左阅读。

请理解，“思考”、“大脑”和“神经元”这些词是zoomorphism，基本上是将模型实际执行的操作作为隐喻。这些模型实际上并没有思考，它们只是数学函数。它们实际上不是大脑，而只是人工神经网络。它们实际上并不是生物神经元，而只是一些数字。

这是一个活跃研究和哲学化的领域。这个描述相当愤世嫉俗，意在遏制人们对AI被描绘为像人类一样思考/行动的媒体狂热情绪。话虽如此，如果您想将AI拟人化，请随意！似乎大多数人都这么做，这甚至可能有助于学习。

## 注意事项

- [d2l.ai](https://www.d2l.ai) 是了解人工智能工作原理的好资源

- 请注意，作者们确实喜欢苹果。它们很美味。