---
sidebar_position: 2
---
# 🟡 使用工具的LLMs

MRKL系统（Modular Reasoning，Knowledge and Language，发音为“miracle”）是一种**神经符号化架构**，结合了LLMs（神经计算）和外部工具（如计算器符号计算），用于解决复杂问题。

MRKL系统由一组模块（例如计算器、天气API、数据库等）和一个路由器组成，路由器决定如何将自然语言查询“路由”到适当的模块。

一个简单的MRKL系统的例子是一个可以使用计算器应用程序的LLM。这是一个单一模块系统，其中LLM是路由器。当被问到“100*100等于多少？”时，LLM可以选择从提示中提取数字，然后告诉MRKL系统使用计算器应用程序计算结果。这可能如下所示：

<pre>
<p>100*100等于多少？</p>

<span className="bluegreen-highlight">CALCULATOR[100*100]</span>
</pre>

MRKL系统会看到单词“CALCULATOR”，并将“100*100”插入计算器应用程序。这个简单的想法可以轻松扩展到各种符号计算工具上。

考虑以下附加应用的例子：

- 一个能够通过从用户的文本中提取信息来形成SQL查询来回答关于财务数据库问题的问题的聊天机器人。

<pre>
<p>苹果股票现在的价格是多少？</p>

<span className="bluegreen-highlight">当前价格为DATABASE [在公司 =“Apple”和时间 =“现在”时选择价格FROM股票]。</span>
</pre>

- 一个能够通过从提示中提取信息并使用天气API检索信息来回答关于天气的问题的聊天机器人。

<pre>
<p>纽约的天气怎么样？</p>

<span className="bluegreen-highlight">天气是WEATHER_API [纽约]。</span>
</pre>

- 或者甚至更复杂的任务，这些任务需要多个数据源，例如：

import mrkl_task from '@site/docs/assets/mrkl_task.png';
import dataset from '@site/docs/assets/mrkl/dataset.png';
import load_dataset from '@site/docs/assets/mrkl/load_dataset.png';
import model from '@site/docs/assets/mrkl/model.png';
import extract from '@site/docs/assets/mrkl/extract.png';
import search from '@site/docs/assets/mrkl/search.png';
import final from '@site/docs/assets/mrkl/final.png';

<div style={{textAlign: 'center'}}>
  <img src={mrkl_task} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
AI21的例子
</div>


## 示例

我使用Dust.tt复制了原始论文中的一个示例MRKL系统，链接[here](https://dust.tt/trigaten/a/98bdd65cb7)。该系统读取一个数学问题（例如“20乘以5 ^ 6等于多少？”），提取数字和操作，并为计算器应用程序重新格式化它们（例如“20 * 5 ^ 6”）。然后它将格式化后的等式发送到Google的计算器应用程序中，并返回结果。请注意，原始论文对路由器（LLM）进行了提示调整，但我在此示例中没有进行。让我们看看它是如何工作的：

首先，我在Dust`数据集`选项卡中创建了一个简单的数据集。

<div style={{textAlign: 'center'}}>
  <img src={dataset} style={{width: "750px"}} />
</div>

然后，我切换到`规格说明`选项卡，并使用`data`块加载了数据集。

<div style={{textAlign: 'center'}}>
  <img src={load_dataset} style={{width: "750px"}} />
</div>

接下来，我创建了一个LLM块，用于提取数字和操作。请注意，在提示中我告诉它我们将使用Google的计算器。我使用的模型（GPT-3）可能在预训练中对Google的计算器有一些了解。

<div style={{textAlign: 'center'}}>
  <img src={model} style={{width: "750px"}} />
</div>

然后，我制作了一个`code`块，该块运行一些简单的javascript代码以从完成中删除空格。

<div style={{textAlign: 'center'}}>
  <img src={extract} style={{width: "750px"}} />
</div>

最后，我制作了一个`search`块，将重新格式化的等式发送到Google的计算器。

<div style={{textAlign: 'center'}}>
  <img src={search} style={{width: "750px"}} />
</div>

下面是最终结果，所有结果都是正确的！

<div style={{textAlign: 'center'}}>
  <img src={final} style={{width: "750px"}} />
</div>

请随意克隆并尝试使用此游乐场 [here](https://dust.tt/trigaten/a/98bdd65cb7)。

## 注意事项
MRKL由[AI21](https://www.ai21.com/)开发，最初使用了他们的J-1（侏罗纪1）（@lieberjurassic）LLM。

## 更多内容

请参见[MRLK系统](https://langchain.readthedocs.io/en/latest/modules/agents/implementations/mrkl.html)的此示例。