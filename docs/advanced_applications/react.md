---
sidebar_position: 3
---

# 🟡 能推理和执行的LLM

ReAct（@yao2022react）（reason，act）是一种使语言模型使用自然语言推理解决复杂任务的范例。ReAct旨在用于允许LLM执行某些操作的任务。例如，在MRKL系统中，LLM可以与外部API交互以检索信息。当被问及问题时，LLM可以选择执行操作来检索信息，然后根据检索到的信息回答问题。

可以把ReAct系统想象成MRKL系统，具有**思考和行动的能力**。

查看以下图片。上方的问题源自HotPotQA（@yang2018hotpotqa），这是一个需要进行复杂推理的问答数据集。 ReAct可以通过先对问题进行推理（思路1），然后执行操作（Act 1）向Google发送查询来回答问题。然后，它收到了一个观察值（观察1），并继续这个思路、行动、观察循环，直到得出结论（Act 3）。

import react_qa from '@site/docs/assets/react_qa.png';

<div style={{textAlign: 'center'}}>
  <img src={react_qa} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
ReAct系统（Yao等）
</div>

具有强化学习知识的读者可能会认为这个过程与状态、行动、奖励、状态... 的经典强化学习循环类似。 ReAct在他们的论文中对此进行了一些形式化处理。

## 结果

谷歌在与ReAct一起的实验中使用了PaLM（@chowdhery2022palm）LLM。与标准提示（仅问题）、CoT和其他配置进行比较，显示出ReAct在复杂推理任务方面的表现非常有前途。谷歌还对涵盖事实提取和验证的FEVER数据集（@thorne2018fever）进行了研究。

import react_performance from '@site/docs/assets/react_performance.png';

<div style={{textAlign: 'center'}}>
  <img src={react_performance} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
ReAct结果（Yao等）
</div>