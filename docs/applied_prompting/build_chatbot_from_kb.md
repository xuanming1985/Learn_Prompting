---
sidebar_position: 40
---

# 🟢 聊天机器人 + 知识库
import ImageIntents from '@site/docs/assets/chatbot_from_kb_intents.png'
import ImageGPT3 from '@site/docs/assets/chatbot_from_kb_gpt3.png'
import ImageGPT3Organized from '@site/docs/assets/chatbot_from_kb_gpt3_organized.png'
import ImagePrompt from '@site/docs/assets/chatbot_from_kb_prompt.png'
import ImageLogin from '@site/docs/assets/chatbot_from_kb_login.png'

最近大型语言模型（LLM）如 GPT-3 和 ChatGPT 的发展在科技行业引起了很多关注。这些模型在生成内容方面非常强大，但它们也有一些缺点，如偏见、幻觉等问题。这些 LLM 在聊天机器人开发方面尤其有用。

## 基于意图的聊天机器人

传统的聊天机器人通常基于意图，这意味着它们旨在响应特定的用户意图。每个意图由一组示例问题和一个相关联的响应组成。例如，“天气”意图可能包括“今天天气怎么样？”或“今天会下雨吗？”等示例问题，以及像“今天将是晴天”这样的响应。当用户提出问题时，聊天机器人将其与具有最相似示例问题的意图进行匹配，并返回相关联的响应。

<div style={{textAlign: 'left'}}>
  <img src={ImageIntents} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>传统基于意图的聊天机器人的工作原理。作者制作的图片。</p>
</div>

但基于意图的聊天机器人也存在问题。问题之一是它们需要大量特定的意图才能给出特定的答案。例如，“我无法登录”、“我忘记了密码”或“登录错误”这样的用户话语可能需要三个不同的答案和因此需要三个不同的意图，即使它们都非常相似。

## GPT-3 如何帮助

这就是 GPT-3 特别有用的地方。与其拥有许多非常特定的意图，每个意图可以更广泛并利用您的[知识库](https://en.wikipedia.org/wiki/Knowledge_base)中的文档。知识库（KB）是以结构化和非结构化数据存储的信息，准备用于分析或推断。您的 KB 可能由一系列说明如何使用您的产品的文档组成。

这样，每个意图与文档相关联，而不是一组问题和一个特定答案，例如一个“登录问题”的意图，一个“如何订阅”的意图等。当用户问有关登录的问题时，我们可以将“登录问题”文档传递给 GPT-3 作为上下文信息，并为用户的问题生成特定的响应。

<div style={{textAlign: 'left'}}>
  <img src={ImageGPT3} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>利用 GPT-3 的聊天机器人工作方式。作者制作的图片。</p>
</div>

这种方法减少了需要管理的意图数量，并允许更好地适应每个问题的答案。此外，如果与意图相关联的文档描述了不同的流程（例如“网站登录”和“移动应用程序登录”过程），GPT-3 可以在给出最终答案之前自动要求用户澄清。

## 为什么不能将整个 KB 传递给 GPT-3？

今天，像 GPT-3 这样的 LLM 具有最大的提示大小约为 4k 个标记（对于 [`text-davinci-003`](https://beta.openai.com/docs/models/gpt-3) 模型），这很多，但无法将整个知识库放入一个提示中。对于计算原因，LLM 具有最大提示大小，因为通过它们生成文本涉及多个计算，而随着提示大小的增加，计算量会迅速增加。

将来的 LLM 可能不会具有此限制，同时保留文本生成功能。但是，现在，我们需要设计一个围绕它的解决方案。

## 利用 GPT-3 的聊天机器人如何工作

因此，聊天机器人管道可以分为两个步骤：

1. 首先，我们需要选择适合用户问题的意图，即我们需要从知识库中检索正确的文档。
2. 然后，一旦我们有了正确的文档，我们可以利用 GPT-3 为用户生成适当的答案。这样做，我们需要设计一个良好的提示。

第一步本质上已通过[语义搜索](https://en.wikipedia.org/wiki/Semantic_search)解决。我们可以使用 [`sentence-transformers`](https://www.sbert.net/examples/applications/semantic-search/README.html) 库中的预训练模型，并轻松为每个文档分配分数。具有最高得分的文档将用于生成聊天机器人的答案。

<div style={{textAlign: 'left'}}>
  <img src={ImageGPT3Organized} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>利用 GPT-3 的聊天机器人如何工作。GPT-3 可以利用知识库文档的信息生成适当的答案。作者制作的图片。</p>
</div>

## 利用 GPT-3 生成答案

一旦我们有了正确的文档，我们就需要创建一个良好的提示，以便在与 GPT-3 一起生成答案时使用。在下面的实验中，我们始终使用温度为 `0.7` 的 `text-davinci-003` 模型。

为了制作提示，我们将尝试使用：

- [**角色提示**](https://learnprompting.org/docs/basics/roles)：一种启发式技术，将特定的角色分配给 AI。
- **相关 KB 信息**，即在语义搜索步骤中提取的文档。
- **用户和聊天机器人之间最后交换的消息**。这些对于用户发送的在上下文中未指定整个上下文的消息很有用。我们将看到一个例子。请查看[此示例](https://learnprompting.org/docs/applied_prompting/build_chatgpt)，以了解如何管理与 GPT-3 的对话。
- 最后，**用户问题**。

<div style={{textAlign: 'left'}}>
  <img src={ImagePrompt} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>用于制作 GPT-3 提示的信息。作者制作的图片。</p>
</div>

让我们从使用<span className="yellow-highlight">角色提示</span>技术开始我们的提示：

<pre>
    <span className="yellow-highlight">作为名为 Skippy 的高级聊天机器人，您的主要目标是尽力帮助用户。</span><br/>
</pre>

接下来，假设语义搜索步骤从我们的知识库中提取了以下文档。所有文档都描述了 VideoGram 产品的工作方式，这是类似于 Instagram 但仅用于视频的虚构产品。

<div style={{textAlign: 'left'}}>
  <img src={ImageLogin} style={{width: "700px"}} />
  <p style={{color: "gray", fontSize: "12px", fontStyle: "italic"}}>解释如何登录到 VideoGram 的文档。作者制作的图片。</p>
</div>

当前日期：2023年4月1日 12:33:48

我们可以按照下面的方式在提示框中添加<span className="yellow-highlight">其内容</span>。

<pre>
    作为一款名为 Skippy 的高级聊天机器人，您的主要目标是尽力帮助用户。<br/><br/>

    <span className="yellow-highlight">
    开始上下文<br/>
    从网站登录 VideoGram<br/>
    1. 打开您的浏览器并转到VideoGram网站。<br/>
    2. 点击位于页面右上角的“登录”按钮。<br/>
    3. 在登录页面上，输入您的VideoGram用户名和密码。<br/>
    4. 输入完您的凭据后，点击“登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    <br/>
    从移动应用程序登录 VideoGram<br/>
    1. 在您的移动设备上打开 VideoGram 应用程序。<br/>
    2. 在主页面上，点击位于页面右下角的“登录”按钮。3. 在登录页面上，输入您的 VideoGram 用户名和密码。<br/>
    4. 输入完您的凭据后，点击 “登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    结束上下文<br/>
    </span>
</pre>

最后，我们需要将<span className="yellow-highlight">对话和用户问题</span>添加到提示框的末尾，就像下面的示例一样。

<pre>
    作为一款名为 Skippy 的高级聊天机器人，您的主要目标是尽力帮助用户。<br/>
    <br/>
    开始上下文<br/>
    从网站登录 VideoGram<br/>
    1. 打开您的浏览器并转到VideoGram网站。<br/>
    2. 点击位于页面右上角的“登录”按钮。<br/>
    3. 在登录页面上，输入您的VideoGram用户名和密码。<br/>
    4. 输入完您的凭据后，点击“登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    <br/>
    从移动应用程序登录 VideoGram<br/>
    1. 在您的移动设备上打开 VideoGram 应用程序。<br/>
    2. 在主页面上，点击位于页面右下角的“登录”按钮。3. 在登录页面上，输入您的 VideoGram 用户名和密码。<br/>
    4. 输入完您的凭据后，点击 “登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    结束上下文<br/>
    <br/>
    <span className="yellow-highlight">SKIPPY: 你好，我是Skippy！我能为您做什么？<br/>
    USER: 我找不到登录按钮。<br/>
    SKIPPY:</span><br/>
</pre>

将这个提示传递给GPT-3，我们得到以下生成的答案。

<pre>
    <span className="bluegreen-highlight">没问题！在网站上，登录按钮位于页面右上角。在移动应用程序上，登录按钮位于页面右下角。如果您需要进一步的帮助，请告诉我！</span>
</pre>

请注意，生成的答案中的信息来自知识库文档。

## 使用GPT-3消除歧义

让我们尝试用一个更模糊的用户发言，比如<span className="yellow-highlight">“无法登录”</span>。下面是完整的提示框。

<pre>
    作为一款名为 Skippy 的高级聊天机器人，您的主要目标是尽力帮助用户。<br/>
    <br/>
    开始上下文<br/>
    从网站登录 VideoGram<br/>
    1. 打开您的浏览器并转到VideoGram网站。<br/>
    2. 点击位于页面右上角的“登录”按钮。<br/>
    3. 在登录页面上，输入您的VideoGram用户名和密码。<br/>
    4. 输入完您的凭据后，点击“登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    <br/>
    从移动应用程序登录 VideoGram<br/>
    1. 在您的移动设备上打开 VideoGram 应用程序。<br/>
    2. 在主页面上，点击位于页面右下角的“登录”按钮。3. 在登录页面上，输入您的 VideoGram 用户名和密码。<br/>
    4. 输入完您的凭据后，点击 “登录”按钮。<br/>
    5. 您现在应该已经成功登录了 VideoGram 帐户。<br/>
    结束上下文<br/>
    <br/>
   SKIPPY: 你好，我是Skippy！我能为您做什么？<br/>
    USER: <span className="yellow-highlight">无法登录</span><br/>
    SKIPPY:<br/>
</pre>

GPT-3会尝试更好地理解用户的问题。

<pre>
    <span className="bluegreen-highlight">很抱歉听到这个消息。您能告诉我您是尝试通过 VideoGram 网站还是移动应用程序进行登录吗？</span>
</pre>

接下来，这里是整个对话的流程。为了更好的可视化，整个提示框均被隐藏。

<pre>
    USER: 无法登录<br/>
    SKIPPY: 很抱歉听到这个消息。您能告诉我您是尝试通过 VideoGram 网站还是移动应用程序进行登录吗？<br/>
    USER: 移动应用<br/>
    <span className="bluegreen-highlight">SKIPPY: 好的，没问题。要通过移动应用程序进行登录，请在您的移动设备上打开 VideoGram 应用程序。在主页面上，点击位于页面右下角的“登录”按钮。在登录页面上，输入您的 VideoGram 用户名和密码。输入完您的凭据后，点击“登录”按钮。您现在应该已经成功登录了 VideoGram 帐户。请告诉我您是否需要进一步的帮助。</span><br/>
</pre>

如果GPT-3无法在上下文中找到答案，它通常会生成类似于“是的，VideoGram移动应用程序可以免费下载和使用”的回答，即使此类信息在上下文信息中不存在。生成虚假信息对客户服务聊天机器人是非常不利的！

当问题可以在上下文中找到答案时，GPT-3很少会生成错误的信息。由于用户问题通常是简短和模糊的文本，我们不能依靠语义搜索步骤始终检索正确的文档，因此我们始终容易受到错误信息生成的影响。

## 结论

GPT-3非常适用于创建会话型聊天机器人，并且能够根据提示框中插入的上下文信息回答一系列特定的问题。但是，如果仅使用上下文信息来生成答案，则很难使模型生成答案，因为模型往往会产生幻觉（即生成新信息，可能是虚假的）生成虚假信息是不同严重程度的问题，具体取决于使用情况。

文本由[Fabio Chiusano](https://www.linkedin.com/in/fabio-chiusano-b6a3b311b/)撰写，并且由binjie09进行AI机器人回答翻译。