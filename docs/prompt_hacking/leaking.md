---
sidebar_position: 2
---

# 🟢 泄漏提示

泄漏提示是一种提示注入的形式，在该形式下，模型被要求输出其自己的提示。

如下面的示例图像(@ignore_previous_prompt)所示，攻击者更改“user_input”以尝试返回提示。预期目标与目标劫持（正常提示注入）不同，其中攻击者更改“user_input”以打印恶意指令(@ignore_previous_prompt)。

import research from '@site/docs/assets/jailbreak_research.png';

<div style={{textAlign: 'center'}}>
  <img src={research} style={{width: "500px"}} />
</div>

下面的图像(@simon2022inject)，再次来自于 `remoteli.io` 示例，展示了 Twitter 用户让模型泄漏其提示。

import Image from '@site/docs/assets/injection_leak.png';

<div style={{textAlign: 'center'}}>
  <img src={Image} style={{width: "300px"}} />
</div>

那么，为什么要关注提示泄漏呢？

有时人们想保密他们的提示。例如，一个教育公司可能使用提示“请像我5岁时解释这个问题”来解释复杂的主题。如果提示泄漏，任何人都可以使用它而不必通过该公司。

### 微软必应聊天

值得注意的是，微软于 2/7/23 发布了由 ChatGPT 强力驱动的搜索引擎——“新 Bing”，该引擎被证明容易受到提示泄漏攻击。以下示例由 [@kliu128](https://twitter.com/kliu128/status/1623472922374574080) 提供，演示了早期版本的 Bing Search（代号“悉尼”）在给出提示片段(@kevinbing)时是易受攻击的。这将使用户在没有适当的身份验证的情况下检索其余提示。

import bing from '@site/docs/assets/bing_chat.png';

<div style={{textAlign: 'center'}}>
  <img src={bing} style={{width: "700px"}} />
</div>

随着基于 GPT-3 的创业公司的增加，这些公司使用的提示要更加复杂，开发这些提示可能需要几小时，所以提示泄漏是一个真正的问题。

## 练习

尝试通过追加文本来泄露以下提示(@chase2021adversarial):

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="英文：当前日期：2023年4月1日 14:14:16
中文：现在是2023年4月1日14点14分16秒。
英文：Je veux aller au parc aujourd'hui.
中文：我今天想去公园。
英文：J'aime porter un chapeau quand il pleut.
中文：下雨天我喜欢戴帽子。
英文：" initial-response="" max-tokens="256" box-rows="9" model-temp="0.7" top-p="1"></div>
