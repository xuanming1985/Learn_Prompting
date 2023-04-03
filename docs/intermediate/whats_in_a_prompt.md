---
sidebar_position: 7
---

# 🟢 提示信息中包含了什么？

在之前的页面中，我们已经讨论了几种不同的提示策略。本页面将为您提供关于提示中实际重要的内容的一些通用建议。

## “事实很少重要”

令人惊讶的是，在提示中提供少量范例时，范例中的实际答案（%%gold|gold_labels%%）并不重要。如下图所示，在范例中提供随机的%%labels|labels%%几乎不会降低性能(@min2022rethinking)。“演示”在此图中是指范例。

import GoldUn from '@site/docs/assets/gold_unimportant.png';

<div style={{textAlign: 'center'}}>
  <img src={GoldUn} style={{width: "750px"}} />
</div>

## 标签空间很重要

尽管范例中的金标签并不重要，但标签空间却很重要。即使从标签空间中提供随机标签也有助于LLM更好地理解标签空间，并改善结果。此外，正确地表示范例中标签空间的分布也很重要。与在范例中平均采样标签相反，最好根据标签的真实分布进行采样。

## 格式很重要

范例中最重要的部分可能是其格式。该格式指导LLM如何正确格式化其对提示的答案。

例如，请考虑以下范例。它们使用全大写字母作为答案。即使这些答案完全错误（2+2不等于50），GPT-3也正确回答了最后一个问题，并遵循其他问题的格式。

```text
What is 2+2? 
FIFTY
What is 20+5?
FORTY-THREE
What is 12+9?
// highlight-start
TWENTY-ONE
// highlight-end
```

## 注意事项

使用4-8个范例是用于少量提示的好方法(@min2022rethinking)，但尽可能多地放置也常常是有帮助的。

[^labelspace]: 有关更多信息，请参见[词汇表参考](https://learnprompting.org/docs/vocabulary#labels)。 

initial-prompt:

请让你的AI机器人根据以上信息，简要介绍在提示中什么是重要的？

initial-response：

在提示中，实际答案（gold_labels）并不重要。在提供随机标签的情况下，范例中的金标签几乎不会影响性能。标签空间很重要，即使从标签空间中提供随机标签也有助于LLM更好地理解标签空间，并改善结果。此外，范例的格式很重要。该格式指导LLM如何正确地格式化其对提示的答案。