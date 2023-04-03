---
sidebar_position: 600
---

# 🟢 使用 Zapier 进行邮件管理

import Basic from'../assets/Zapiermail/Basic.png';
import Diagram from'../assets/Zapiermail/Diagram.png';
import Step1 from'../assets/Zapiermail/Step1.png';
import Step2 from'../assets/Zapiermail/Step2.png';
import Step3 from'../assets/Zapiermail/Step3.png';
import Step4 from'../assets/Zapiermail/Step4.png';
import Zap from'../assets/Zapiermail/Zap.png';

## 简介

我们已经了解到在处理电子邮件时，结合 GPT-3 和无代码工具，如 [Zapier](https://zapier.com) 或 [Bubble.io](https://bubble.io)，可以带来很大的便利。

本文将展示使用 Zapier+GPT-3 进行简单设置的示例。本文重点介绍特定示例，但是这种方法的可能性很大。我们将会提供一些其他的例子。请记住，您也可以在 Bubble.io 中完成这个任务。虽然有许多其他的无代码工具，但是在撰写本文时，只有极少数工具允许您使用 GPT-3。

在本文中，我们将向您展示如何在 Zapier 中设置一个简单的系统，用于**汇总和存储电子邮件**。与某人会面？快速检查您与该人交换的电子邮件的摘要。设置这个任务需要大约 20 分钟。

:::caution
为了更好地理解本文，最好已经了解 Zapier。如果您还不了解，请参考这篇[Zapier 入门文章](https://zapier.com/learn/)。
:::

## 总体思路

下面是我们将在 Zapier 中执行的操作示意图。当电子邮件进入您的收件箱时，它会触发 Zapier。这里有四个步骤（目前）：

1.邮件进入并触发 Zapier
2.格式化电子邮件内容（例如，去除 HTML 标记）
3.将电子邮件发送到 GPT-3 进行摘要
4.将输出结果存储在数据库中

<div style={{textAlign: 'left'}}>
  <img src={Diagram} style={{width: "500px"}} />
</div>

## 在 Zapier 中设置

请确保已经拥有[Zapier 帐户](https://zapier.com/sign-up)（可以获得免费帐户）。设置过程应该很简单。当您创建好您的帐户后，请展开下面的框以查看我们需要创建的每个 Zapier 操作的完整描述。


<details>
 <summary>展开以查看 Zapier 中步骤的更详细视图</summary>
<div>
这是最终 Zapier 操作图的样子。
<div><div style={{textAlign: 'left'}}>
  <img src={Zap} style={{width: "500px"}} />
</div></div>
    <br/>
    <details>
      <summary>
        Step 1: Gmail trigger on new incoming email (Gmail is used here).
      </summary>
      <div>
        <div style={{textAlign: 'left'}}>
    <img src={Step1} style={{width: "500px"}} />
        </div>
      </div>
    </details>
    <details>
      <summary>
       Step 2: Formatter for E-mail content. 
      </summary>
      <div>
        <div style={{textAlign: 'left'}}>
  <img src={Step2} style={{width: "500px"}} />
</div>
      </div>
    </details>
    <details>
      <summary>
        Step 3: Prompting the Email content
        <br/>
      </summary>
      <div>
        <div style={{textAlign: 'left'}}>
  <img src={Step3} style={{width: "500px"}} />
</div>
      </div>
    </details>
    <details>
      <summary>
        Step 4: Adding it to a database
      </summary>
      <div>
        <div style={{textAlign: 'left'}}>
  <img src={Step4} style={{width: "500px"}} />
</div>
      </div>
    </details>
  </div>
</details>
这是一个在 Zapier 中设置的基本摘要功能示意图。它有一些限制，但确实能完成任务并构建一个有用的数据库。

## 优化提示以获得更好的结果

有一些简单的方法可以改善结果。添加上下文和角色提示可以提高输出质量。然而，您的电子邮件主题和内容可能涵盖了各种各样的主题。这意味着通用指令将比特定指令更有效，后者可能会使模型失真。

为了实用起见，在指令之后加上“电子邮件：”，告诉GPT-3何时开始读取电子邮件，最后以“总结：”结束提示即可。这样避免了GPT-3回复“当然！让我为您总结一下......”。

角色提示也很有用。要求GPT-3充当个人助理有助于提高摘要的质量。如果要总结工作邮件，请添加您所担任的角色，以便GPT-3有可操作的上下文信息。它会假设读者具有一定的知识水平，从而帮助过滤掉电子邮件中不相关的部分。 下面我们展示了办公管理员可能收到的一些电子邮件示例。

您可以要求它用项目列表的形式总结简单的电子邮件，但是这可能不是所有情况下都有用，具体取决于您希望如何使用摘要。如果您只是想快速浏览电子邮件，那么简短而简明的摘要可能更合适。只需在提示中简单说明即可。以下是此提示的示例。调整并尝试修改它，以查看它如何改变。

<div trydyno-embed ="" openai-model="text-davinci-003" initial-prompt="充当我的个人助理。我是一名办公管理员。请简洁地总结以下电子邮件，忽略页眉和页脚以及以前的任何电子邮件。\n\n电子邮件：请求额外的办公用品尊敬的办公管理员，\n 我希望这封电子邮件能够找到您。我写信是为了请求我们团队的额外办公用品。正如您所知，我们最近工作量很大，使用办公用品的速度比平常快得多。如果您能提供以下物品，我们将不胜感激：打印纸、会议室HP打印机的墨盒、便笺、文件夹夹和荧光笔。请告诉我们是否有任何问题或疑虑，并告知我们何时可以收到这些物品。感谢您的帮助。\n\n此致\n敬礼\n您的名字\n\n总结：" initial-response ="由于工作量大，需要请求额外的办公用品。请求以下物品：打印纸、会议室HP打印机的墨盒、便签、文件夹夹和荧光笔。请求提供交付信息以及是否有任何问题或疑虑。" max-tokens ="256" box-rows ="15" model-temp="0.0" top-p="0">
    <noscript>无法加载Dyno Embed：必须启用JavaScript</noscript>
</div>

这里的响应是可以接受的，也很有用。但是，通过进一步微调，您可以获得更好的结果。作为摘要的读者，您不关心它是电子邮件，您可能希望摘要的细节程度更低。为什么的信息是无关紧要的，同样适用于最后一句关于问题和疑虑的内容。通过简单地添加摘要目的是为了让您浏览电子邮件，并删除任何愉悦语言，可以改善结果。

<div trydyno-embed ="" openai-model="text-davinci-003" initial-prompt="充当我的个人助理。我是一名办公管理员。请简洁地总结以下电子邮件，忽略页眉和页脚以及以前的任何电子邮件。我想使用摘要浏览电子邮件。删除任何愉悦语言。\n\n电子邮件：请求额外的办公用品尊敬的办公管理员，\n 我希望这封电子邮件能够找到您。我写信是为了请求我们团队的额外办公用品。正如您所知，我们最近工作量很大，使用办公用品的速度比平常快得多。如果您能提供以下物品，我们将不胜感激：打印纸、会议室HP打印机的墨盒、便笺、文件夹夹和荧光笔。请告诉我们是否有任何问题或疑虑，并告知我们何时可以收到这些物品。感谢您的帮助。\n\n此致\n敬礼\n您的名字\n\n总结：" initial-response="请求额外的办公用品-打印纸、会议室HP打印机的墨盒、便笺、文件夹夹和荧光笔。" max-tokens="256" box-rows="15" model-temp="0.0" top-p="0">
    <noscript>无法加载Dyno Embed：必须启用JavaScript</noscript>
</div>

现在，您只剩下摘要中最重要的部分了！


## 其他用途

既然您已经看到了摘要示例，我们将提及Zapier+GPT-3的其他用例。一个很好的例子是让GPT-3将您的电子邮件分类。这只需要在提示中告诉它将以下电子邮件归类为您喜欢的任何类别即可。

更深入的例子是有多个提示。您可以使用提示生成一个回复，以满足电子邮件的要求，另一个回复则表示不同意或否认。两者都可以存储在您的草稿箱中，随时可以发送。

如果您经常收到非常相似的电子邮件，则可以使用Zapier的过滤器仅将提示应用于该电子邮件。当与格式化程序结合使用时，这可以是强大的工具。您可以从中提取信息并将其导出为CSV文件，或直接将其存储在某种数据库中。


## 担忧

请在运行电子邮件通过GPT-3并存储它们时考虑隐私问题。GPT-3有时会犯错误。我们强烈建议在发送邮件之前检查邮件内容。