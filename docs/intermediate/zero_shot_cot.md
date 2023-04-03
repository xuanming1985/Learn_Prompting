---
sidebar_position: 4
---
# 🟢 零样本思维链

零样本思维链（Zero Shot Chain of Thought，简称Zero-shot-CoT）是一种基于 %%CoT prompting|CoT prompting%% (@wei2022chain) 的跟进研究，它引入了一种极其简单的零样本提示。他们发现，通过在问题末尾附加“**让我们一步一步地思考**”这个词语，LLM能够生成一个回答问题的思维链。从这个思维链中，他们可以提取出更精确的答案。

import ZSImage from '@site/docs/assets/zero_shot.png';

<div style={{textAlign: 'center'}}>
  <img src={ZSImage} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
零样本思维链 (Kojima et al.)
</div>

从技术上讲，完整的零样本思维链过程涉及两个独立的提示/完成操作。在下图中，左侧的顶部气泡生成一个思维链，而右侧的顶部气泡从第一个提示（包括第一个提示本身）中获取输出，并从思维链中提取答案。这个第二个提示是一个自我增强提示。

import ZSProcessImage from '@site/docs/assets/zero_shot_example.png';

<div style={{textAlign: 'center'}}>
  <img src={ZSProcessImage} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
完整的零样本思维链过程 (Kojima et al.)
</div>

## 示例

下面是一些演示（仅进行推理提取）。第一个演示展示了GPT-3(davinci-003)在一个简单的数学问题上失败，而第二个演示则使用了零样本思维链提示并成功解决了这个问题。请随意输入您的OpenAI API密钥（点击生成）并尝试这些示例。请注意，与CoT提示相比，零样本思维链提示要简单得多。

#### 错误

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="如果约翰有5个梨，吃了2个，又买了5个，然后送了3个给他的朋友，他还剩下多少个梨?" initial-response="约翰有8个梨。" max-tokens="256" box-rows="3" model-temp="0.7" top-p="1"></div>


#### 正确

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="如果约翰有5个梨，吃了2个，又买了5个，然后送了3个给他的朋友，他还剩下多少个梨？\n\n让我们一步一步地思考。" initial-response="约翰起初有5个梨。他吃了2个梨，还剩下3个梨。他又买了5个梨，总共有8个梨。他把3个梨送给他的朋友，只剩下5个梨了。" max-tokens="256" box-rows="5" model-temp="0.7" top-p="1"></div>


## 结果
零样本思维链在算术、常识和符号推理任务中也能提高结果。但毫不奇怪的是，它通常不如CoT提示有效。使用零样本思维链的一个重要用例是当难以获得CoT提示的少样本示例时。

## 感兴趣的消融

Kojima等人尝试了许多不同的零样本思维链提示（例如“让我们通过将其分成步骤来解决这个问题”或“让我们从逻辑上思考这个问题”），但他们发现，“让我们一步一步地思考”最适合他们选择的任务。


## 注意事项

提取步骤通常必须是特定于任务的，使得零样本思维链不像乍一看那么通用。

凭经验，我发现类似零样本思维链的提示有时对改善生成性任务的完成长度有效。例如，考虑标准提示“写一个关于一只青蛙和一个蘑菇成为朋友的故事”。将“让我们一步一步地思考”这句话附加到这个提示的末尾会导致更长的完成。