---
sidebar_position: 3
locale: en-us
style: chicago
---

# 🟢 联想思维引导

联想思维引导（Chain of Thought Prompting，CoT）是一种最近开发的提示方法，可以鼓励LLM解释其推理过程。下图显示了一个少量样本标准提示（左图）与联想思维提示（右图）的比较。


import CoTExample from '@site/docs/assets/chain_of_thought_example.png';

<div style={{textAlign: 'center'}}>
  <img src={CoTExample} style={{width: "750px"}} />
</div>

<div style={{textAlign: 'center'}}>
常规提示与CoT（Wei等人）比较
</div>

CoT的主要思想是，在向LLM展示一些few shot样例中，说明了推理过程，LLM在回答提示时也会展示推理过程。这种推理解释通常会导致更准确的结果。

## 示例

以下是一些演示。第一个示例展示了GPT-3（davinci-003）无法解决一个简单的文字问题。第二个示例展示了通过使用CoT提示，GPT-3（davinci-003）成功地解决了相同的问题。

#### 错误

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="哪种方式更快地上班？\n选项1：乘坐1000分钟公交车，然后乘半小时火车，最后骑行10分钟。\n选项2：乘坐800分钟公交车，然后乘1小时火车，最后骑行30分钟。" initial-response="选项1是更快的上班方式。" max-tokens="256" box-rows="7" model-temp="0" top-p="0"></div>

#### 正确

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="哪种方式更快地回家？\n选项1：乘坐10分钟公交车，然后乘坐40分钟公交车，最后乘坐10分钟火车。\n选项2：乘坐90分钟火车，然后骑行45分钟，最后乘坐10分钟公交车。\n选项1需要10+40+10=60分钟。\n选项2需要90+45+10=145分钟。\n由于选项1需要60分钟，而选项2需要145分钟，因此选项1更快。\n\n哪种方式更快地上班？\n选项1：乘坐1000分钟公交车，然后乘半小时火车，最后骑行10分钟。\n选项2：乘坐800分钟公交车，然后乘1小时火车，最后骑行30分钟。" initial-response="选项1需要1000+30+10=1040分钟。
选项2需要800+60+30=890分钟。
由于选项2需要890分钟，而选项1需要1040分钟，因此选项2更快。" max-tokens="256" box-rows="18" model-temp="0" top-p="0"></div>

## 结果

CoT已被证明在提高算术、常识和符号推理等任务的结果方面非常有效（@wei2022chain）。特别是，在GSM8K任务中，参数为540B的提示PaLM（@chowdhery2022palm）实现了57%的解决率准确度（当时的SOTA）（@cobbe2021training）。

import PromptedPaLM from '@site/docs/assets/prompted_palm.png';

<div style={{textAlign: 'center'}}>
  <img src={PromptedPaLM} style={{width: "300px"}} />
</div>


<div style={{textAlign: 'center'}}>
GSM8K基准测试的模型比较（Wei等人）
</div>

## 限制

根据Wei等人的说法，重要的是，“只有具有约100B参数的模型与CoT一起使用才能获得性能提升”。较小的模型会写出不合逻辑的思维链，导致精度比标准提示更低。通常来说，模型通过CoT提示获得的性能提升与模型大小成比例。

## 注意事项

在编写本章节时，没有对任何语言模型进行~~伤害~~微调 😊。

initial-prompt和initial-response以及email里面的文本信息也需要翻译成中文：

- initial-prompt: “请翻译以下文本：Comparison of models on the GSM8K benchmark (Wei et al.)”，翻译后为“在GSM8K基准测试中比较模型（Wei等人）”。
- initial-response: “根据Wei等人的说法，‘只有具有约100B参数的模型与CoT一起使用才能获得性能提升’。较小的模型会写出不合逻辑的思维链，导致精度比标准提示更低。通常来说，模型通过CoT提示获得的性能提升与模型大小成比例。”，翻译后为“根据Wei等人的研究结果，只有大约100B参数的模型与CoT提示一起使用时才能获得性能提升。较小的模型会产生不合逻辑的思维链，导致精度比标准提示更低。通常情况下，模型通过CoT提示获得的性能提升与模型大小成比例。”。
- email中的文本信息：根据用户需求进行翻译即可。