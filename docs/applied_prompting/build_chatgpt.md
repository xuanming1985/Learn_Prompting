---
sidebar_position: 4
---
# 🟢 使用GPT-3构建ChatGPT

import Skippy from '@site/docs/assets/skippy_chatbot.png'    
import SkippyHeader from '@site/docs/assets/skippy_chatbot_header.png'    
import Therapy from '@site/docs/assets/therapy_chatbot.gif'
import ChatGPT from '@site/docs/assets/chatgpt_ui_diagram.png'

<div style={{textAlign: 'left'}}>
  <img src={SkippyHeader} style={{width: "700px"}} />
</div>

## 简介

ChatGPT在过去一个月内迅速爆红，仅在一周内就吸引了100万用户。令人惊讶的是，作为其基础模型的GPT-3早在2020年就已经发布，并在一年前开放了公共访问权限<a href="https://openai.com/blog/api-no-waitlist/">。</a>   

对于不了解的人，ChatGPT是OpenAI的一款新语言模型，由GPT-3进行微调以优化对话效果(@chatgpt2022)。它有一个用户友好的聊天界面，您可以在其中输入信息并从AI助手获得回复。请到[chat.openai.com](https://chat.openai.com/chat)体验。

尽管早期版本的GPT-3并不像当前的GPT-3.5系列那样先进，但也仍然给人留下了深刻的印象。这些模型可以通过API和一个<a href="https://beta.openai.com/playground">playground web UI接口</a>提供，您可以对其中某些配置超参数进行调整，还可以测试提示信息。 GPT-3获得了显着的关注，但其并未达到像ChatGPT那样的病毒式传播。

与GPT-3相比，ChatGPT之所以如此成功，是因为它作为一个简单的AI助手，对于普通人来说具有易用性，而不考虑他们对数据科学、语言模型或AI的知识水平。

在本文中，我将概述如何使用像GPT-3这样的大型语言模型实现诸如ChatGPT之类的聊天机器人。

## 动机
这篇文章部分是基于<a href="https://twitter.com/goodside">Riley Goodside</a>的一条推文写的，他指出了ChatGPT是如何实现的。

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">How to make your own knock-off ChatGPT using GPT‑3 (text‑davinci‑003) — where you can customize the rules to your needs, and access the resulting chatbot over an API. <a href="https://t.co/9jHrs91VHW">pic.twitter.com/9jHrs91VHW</a></p>&mdash; Riley Goodside (@goodside) <a href="https://twitter.com/goodside/status/1607487283782995968?ref_src=twsrc%5Etfw">December 26, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

像GPT-3.5系列中的其他模型一样，ChatGPT使用了[RLHF](https://huggingface.co/blog/rlhf)进行训练，但其大部分效果都来自于使用了一个**好的提示**。

## 提示信息

<div style={{textAlign: 'left'}}>
  <img src={Skippy} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>完整的Skippy chatbot提示信息，取自文章顶部</p>
</div>

<a href="https://learnprompting.org/docs/basics/prompting">提示是指指示AI做某事的过程。</a>正如您可能在网上看到的ChatGPT示例那样，您可以提示它做任何事情。常见的用例包括总结文本、根据描述编写内容或创建诸如诗歌、食谱等东西。 

<p></p>

ChatGPT既是一种语言模型，又是用户界面。用户在界面中输入的提示实际上被插入到包含用户和ChatGPT之间的整个对话中。这使得基础语言模型能够理解对话的上下文并进行适当的回应。

<div style={{textAlign: 'left'}}>
  <img src={ChatGPT} style={{width: "600px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>向模型发送提示之前，示例输入用户提示的插入</p>
</div>

语言模型通过在预先训练(@jurafsky2009)期间学习到的概率基础上推断出下一步要出现的单词。

<p></p>

GPT-3能够从简单的指令或几个示例中"学习".后者被称为几项式或上下文学习(@brown2020language)。在上面的聊天机器人提示中，我创建了一个虚构的聊天机器人名为Skippy，并指示它响应用户。GPT-3能够自动识别出回合制的形式，即 `USER: {user input}` 和 `SKIPPY: {skippy response}`。当我们提供下一个用户输入时，GPT-3会理解Skippy是一个聊天机器人，之前的交换是一个对话，因此当我们提供下一个用户输入"Skippy"时，它会做出响应。

### 记忆

Skippy和用户之间以前的对话将附加到下一个提示中。每次我们给出更多的用户输入并得到更多的聊天机器人输出时，提示都会扩展以包含这个新的交换。这就是像Skippy和ChatGPT这样的聊天机器人如何**记忆之前的输入**。然而，GPT-3聊天机器人可以记住的内容是有限的。

提示会在几个回合之后变得非常庞大，特别是如果我们使用聊天机器人生成长的回应，如博客文章等。发送到GPT-3的提示将被转换为标记，这些标记是单个单词或其组成部分。对于包括ChatGPT在内的GPT-3模型，组合提示和生成的响应的标记总数限制为<a href="https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them">4097个标记(约3000个单词)</a>。

### 几个例子

有很多不同的聊天机器人提示用例可以存储以前的对话。ChatGPT旨在成为一个通用的助手，在我看来，它很少提出跟进问题。

#### 询问用户今天的情况的治疗聊天机器人

有一个主动询问问题并从用户那里得到反馈的聊天机器人可能会很有帮助。下面是一个示例治疗聊天机器人提示，它会询问一些问题并进行跟进，以帮助用户思考他们的日常生活。

<div style={{textAlign: 'left'}}>
  <img src={Therapy} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>治疗聊天机器人提示</p>
</div>

#### 使用旧日记记录与年轻的自己交流

<a href="https://twitter.com/michellehuang42">Michelle Huang</a>使用GPT-3通过与自己的年轻时期写的日记来与她的过去自己交流。该提示使用一些上下文，例如旧日记条目，配对聊天机器人风格的问答格式。GPT-3能够基于这些条目模仿一个人物。

Prompt from the Tweet:
```markdown
以下是现在的 Michelle（年龄 [redacted]）和年轻的 Michelle（14岁）之间的对话。

年轻的 Michelle 写下了以下的日记条目：
[diary entries here]

现在的 Michelle：[在此输入您的问题]
```

作者指出，日记条目可能会达到令牌限制。在这种情况下，你可以选择几个条目或者尝试总结几个条目。

## 实现

我将介绍如何在Python中编写一个简单的GPT-3聊天机器人。使用OpenAI API将GPT-3包含在您正在构建的应用程序中非常容易。您需要在OpenAI上创建一个帐户并获取API密钥。查看<a href="https://beta.openai.com/docs/introduction">此处</a>的文档。

我们需要做的概述：

1. 格式化用户输入为GPT-3聊天机器人的提示。
2. 将聊天机器人的响应作为GPT-3的完成项获取。
3. 使用用户输入和聊天机器人的响应更新提示。
4. 循环

这是我将要使用的提示。我们可以使用python将<conversation history\>和<user input\>替换为它们的实际值。

```python
chatbot_prompt = """
As an advanced chatbot, your primary goal is to assist users to the best of your ability. This may involve answering questions, providing helpful information, or completing tasks based on user input. In order to effectively assist users, it is important to be detailed and thorough in your responses. Use examples and evidence to support your points and justify your recommendations or solutions.

<conversation history>

User: <user input>
Chatbot:"""
```

我会跟踪下一个用户输入和先前对话。每次循环都会追加聊天机器人与用户之间的新的输入/输出。
```python
import openai

openai.api_key = "YOUR API KEY HERE"
model_engine = "text-davinci-003"
chatbot_prompt = """
As an advanced chatbot, your primary goal is to assist users to the best of your ability. This may involve answering questions, providing helpful information, or completing tasks based on user input. In order to effectively assist users, it is important to be detailed and thorough in your responses. Use examples and evidence to support your points and justify your recommendations or solutions.

<conversation history>

User: <user input>
Chatbot:"""


def get_response(conversation_history, user_input):
    prompt = chatbot_prompt.replace(
        "<conversation history>", conversation_history).replace("<user input>", user_input)

    # 从GPT-3获取响应
    response = openai.Completion.create(
        engine=model_engine, prompt=prompt, max_tokens=2048, n=1, stop=None, temperature=0.5)

    # 从响应对象中提取响应
    response_text = response["choices"][0]["text"]

    chatbot_response = response_text.strip()

    return chatbot_response


def main():
    conversation_history = ""

    while True:
        user_input = input("> ")
        if user_input == "exit":
            break
        chatbot_response = get_response(conversation_history, user_input)
        print(f"Chatbot: {chatbot_response}")
        conversation_history += f"User: {user_input}\nChatbot: {chatbot_response}\n"

main()
```


<a href="https://gist.github.com/jayo78/79d8834e6e31bf942c7b604e1611b68d">这里</a>是简单聊天机器人的完整代码链接。

现在我们只需要构建一个漂亮的前端，供用户进行交互即可！

作者：[jayo78](https://twitter.com/jayo782).