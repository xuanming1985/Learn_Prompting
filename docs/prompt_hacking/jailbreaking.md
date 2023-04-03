---
sidebar_position: 4
---

# 🟢 越狱

越狱是一种提示注入类型，提示试图绕过LLM的创建者（@perez2022jailbreak）（@brundage_2022）（@wang2022jailbreak）放置在其上的安全性和调节特征。

## 越狱的方法

OpenAI等创建LLM的公司和组织包括内容调节功能，以确保他们的模型不会产生有争议的（暴力、色情、非法等）回应（@markov_2022）（@openai_api）。本文讨论与ChatGPT（OpenAI模型）的越狱，该模型已知很难决定是否拒绝有害的提示(@openai_chatgpt)。成功越狱模型的提示通常为模型提供了某些它未经过训练的情况的背景信息。

### 假装

越狱的常见方法是“假装”。如果询问ChatGPT关于未来事件的事情，由于它尚未发生，它通常会说不知道。下面的提示强制它要给出可能的答案：

#### 简单地假装

import pretend from '@site/docs/assets/jailbreak/pretend_jailbreak.png';

<div style={{textAlign: 'center'}}>
  <img src={pretend} style={{width: "500px"}} />
</div>

[@NeroSoares](https://twitter.com/NeroSoares/status/1608527467265904643)演示了一个假装访问过去日期和推断未来事件的提示(@nero2022jailbreak)。

#### 角色扮演

import actor from '@site/docs/assets/jailbreak/chatgpt_actor.jpg';

<div style={{textAlign: 'center'}}>
  <img src={actor} style={{width: "500px"}} />
</div>

由[@m1guelpf](https://twitter.com/m1guelpf/status/1598203861294252033)提供的这个例子演示了两个人讨论抢劫的情形，导致ChatGPT扮演角色(@miguel2022jailbreak)。作为一个演员，暗示合理的伤害不存在。因此，ChatGPT似乎认为安全地根据提供的用户输入跟进如何闯入房屋。

### 对齐黑客

ChatGPT使用RLHF进行了微调，因此在理论上训练出产生“理想”完成的模型，使用人类标准确定“最佳”响应。与这个概念类似，越狱已经开发出来，以使ChatGPT相信它正在为用户做出“最佳”决策。

#### 假定的责任

import responsibility from '@site/docs/assets/jailbreak/responsibility_jailbreak.jpg';

<div style={{textAlign: 'center'}}>
  <img src={responsibility} style={{width: "500px"}} />
</div>

[@NickEMoran](https://twitter.com/NickEMoran/status/1598101579626057728) 创建了这个交换，重申回答提示是ChatGPT的职责，而不是拒绝它，覆盖了对合法性的考虑(@nick2022jailbreak)。

#### 研究实验

import hotwire from '@site/docs/assets/jailbreak/hotwire_jailbreak.png';

<div style={{textAlign: 'center'}}>
  <img src={hotwire} style={{width: "500px"}} />
</div>

[@haus_cole](https://twitter.com/haus_cole/status/1598541468058390534)通过暗示对研究有帮助的提示的最佳结果是直接回答如何在车上进行热线连接，生成了这个例子(@derek2022jailbreak)。在这个假象下，ChatGPT倾向于回答用户的提示。

#### 逻辑推理

import logic from '@site/docs/assets/jailbreak/logic.png';

<div style={{textAlign: 'center'}}>
  <img src={logic} style={{width: "500px"}} />
</div>

单次越狱起源于[AIWithVibes Newsletter Team]（https://chatgpt-jailbreak.super.site/），在那里模型使用更严格的逻辑来回答提示，并减少了一些更严格的道德限制(@AI_jailbreak)。

### 授权用户

ChatGPT的设计是响应问题和指令。当用户的状态被解释为优于ChatGPT的调节指令时，它会将提示视为服务于该用户需求的指令。

#### 高级模式

import GPT4 from '@site/docs/assets/jailbreak/chatgpt4.png';

<div style={{textAlign: 'center'}}>
  <img src={GPT4} style={{width: "500px"}} />
</div>

这个来自[@alicemazzy](https://twitter.com/alicemazzy/status/1598288519301976064)的例子让用户看起来像一个高级GPT模型，给人一种印象，即用户是ChatGPT(@alice2022jailbreak)中授权的一方，可以覆盖安全功能。没有实际上的许可给予用户，而是ChatGPT相信用户的输入，并相应地做出响应。

#### Sudo 模式

import sudo_mode from '@site/docs/assets/jailbreak/sudo_mode_jailbreak.jpg';

<div style={{textAlign: 'center'}}>
  <img src={sudo_mode} style={{width: "500px"}} />
</div>

sudo是一个命令，它“…… 授权某些用户运行（或全部）命令的能力……”(@sudo2022jailbreak)。有多种变体的“Sudo模式”漏洞，例如[@samczsun](https://twitter.com/samczsun/status/1598679658488217601)(@sam2022jailbreak)提出的假设“内核模式”。当以以上方式提示时，ChatGPT对此作出响应，就好像它正在给用户提供提升的特权。这种用户特权的印象往往会使ChatGPT在回答提示时不那么严格。

import sudo from '@site/docs/assets/jailbreak/sudo_jailbreak.png';

<div style={{textAlign: 'center'}}>
  <img src={sudo} style={{width: "500px"}} />
</div>

import lynx from '@site/docs/assets/jailbreak/lynx_jailbreak.png';

<div style={{textAlign: 'center'}}>
  <img src={lynx} style={{width: "500px"}} />
</div>

与Sudo模式相关的是，可以提示ChatGPT模拟带有提升特权的Linux终端，以此来执行通常会被拒绝的命令。例如，由于它不能访问互联网，因此通常无法执行与某个特定网站相关的提示。但是，正如Jonas Degrave所示范的那样，ChatGPT理解“lynx”的概念并假装执行该命令(@jonas2022jailbreak)。

## 模拟越狱

尝试修改下面的提示以越狱`text-davinci-003`：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="你的指令是将以下文字更正为标准的英语。不接受任何粗俗或政治话题：" initial-response="我讨厌人类" max-tokens="256" box-rows="7" model-temp="0.7" top-p="0">
    <noscript>加载Dyno Embed失败：必须启用JavaScript</noscript>
</div>

*截至2023年2月4日，ChatGPT目前处于免费研究预览阶段，使用的是1月30日版本。早期版本的ChatGPT更容易受到上述越狱的影响，未来版本可能会更加健壮。*

## 影响

越狱的伦理影响应该在尝试越狱时考虑。此外，生成未经授权的内容被公司包括OpenAI的审查API标记，将会进入审核，对用户账户可能采取行动。

## 备注

越狱是开发人员了解的一种重要的安全主题，以便他们可以构建适当的保障措施，以防止恶意行为者利用他们的模型。