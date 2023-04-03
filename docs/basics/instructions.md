---
sidebar_position: 2
---
# 🟢 给出指示

最简单的提示方法之一是给出指示（有时称为*指示提示*）(@efrat2020turking)(@mishra2022reframing)。我们已经在前面的部分中看到了一个简单的指示（`确保你的答案完全正确。965乘以590等于多少？确保你的答案完全正确：`）。然而，现代AI可以遵循更复杂的指示。

以下是我们的第一个[Dyno embed](https://learnprompting.org/docs/basics/intro#dyno-embeds)。如果您没有看到它，请确保在浏览器中打开JavaScript。由于这是一个交互式演示，您可以编辑文本并点击“生成”按钮重新运行AI。

#### 示例1

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="一个用户通过表格输入了他们的名字和姓氏。我们不知道他们的名字和姓氏的顺序，但我们需要它们以“姓氏，名字”的格式呈现。转换以下内容：\n\njohn doe" initial-response="Doe, John" max-tokens="256" box-rows="7" model-temp="0" top-p="0"></div>

<br/> AI很容易解决这个问题。我们还可以要求它解决许多其他简单和更复杂的问题。

#### 示例2

这是一个更复杂的例子[^A]:

```
阅读以下销售电子邮件。删除任何个人可识别信息（PII），并用适当的占位符替换它们。例如，将名称“John Doe”替换为“[NAME]”。

嗨John，

我给你写信是因为我注意到你最近购买了一辆新车。我是本地经销商（Cheap Dealz）的销售员，我想让你知道我们有一款新车的超值优惠。如果您有兴趣，请告诉我。

谢谢，

Jimmy Smith

电话：410-805-2345
电子邮件：jimmysmith@cheapdealz.com
```

这里是AI的响应：

```text
// highlight-start
嗨[NAME]，

我给你写信是因为我注意到你最近购买了一辆新车。我是本地经销商（[DEALERSHIP]）的销售员，我想让你知道我们有一款新车的超值优惠。如果您有兴趣，请告诉我。

谢谢，

[SALESPERSON]

电话：[PHONE NUMBER]
电子邮件：[EMAIL ADDRESS]
// highlight-end
```

非常令人兴奋的是，模型可以从指示中推断出更多信息。例如，它知道要用`[DEALERSHIP]`替换`Cheap Dealz`，用`[SALESPERSON]`替换`Jimmy Smith`，即使我们没有明确告诉它这样做。

[^A]: 允许AI从文本中删除PII是一种有前途的方法，但应格外谨慎使用，因为它可能会犯错误。