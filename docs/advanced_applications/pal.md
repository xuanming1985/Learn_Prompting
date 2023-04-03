---
sidebar_position: 4
---

# 🟡 以代码为推理
[程序辅助语言模型（PAL）](https://reasonwithpal.com)是MRKL系统的另一个例子。当给定一个问题时，PAL能够编写解决这个问题的代码。它们将代码发送到程序化的运行时来获取结果。PAL与CoT相比工作方程序辅助语言模型（PAL）式不同；PAL的中间推理是代码，而CoT的中间推理是自然语言。

import image from '@site/docs/assets/pal.png';

<div style={{textAlign: 'center'}}>
  <img src={image} style={{width: "500px"}} />
</div>

<div style={{textAlign: 'center'}}>
PAL Example (Gao et al.)
</div>


需要注意的一件重要的事情是，PAL实际上交替使用自然语言（NL）和代码。在上面的图像中，蓝色的是PAL生成的自然语言推理。尽管在图像中没有显示，但PAL实际上在每行NL推理前面生成“\#”，以便被程序化的运行时解释为注释。

## 例子

让我们看一个PAL解决数学问题的例子。我使用了一个3-shot提示，这是[这个提示](https://github.com/reasoning-machines/pal/blob/main/pal/prompt/math_prompts.py)的简化版。

我将使用langchain，这是一个Python包，用于链接LLM功能。首先，需要进行一些安装：

```python
!pip install langchain==0.0.26
!pip install openai
from langchain.llms import OpenAI
import os
os.environ["OPENAI_API_KEY"] = "sk-YOUR_KEY_HERE"
```

然后，我们可以创建一个GPT-3 davinci-002实例（当我们使用这个对象时，会发生API调用）

```
llm = OpenAI(model_name='text-davinci-002', temperature=0)
```

下面是几个shot的提示：

```python
MATH_PROMPT = '''

Q: 服务器房里有九台电脑。从周一到周四，每天都会增加五台电脑。现在服务器房有多少台电脑？

# Python解决方案：
"""服务器房里有九台电脑。从周一到周四每天增加五台电脑。现在服务器房有多少台电脑？"""
initial = 9
added = 5
num_days = 4  # 周一到周四共四天
total = initial + added * num_days
result = total
return result

Q: Shawn 有五个玩具。过圣诞节时，他分别获得了两个来自母亲和父亲的玩具。他现在有多少玩具？

# Python解决方案：
"""Shawn 有五个玩具。过圣诞节时，他分别获得了两个来自母亲和父亲的玩具。他现在有多少玩具？"""
initial = 5
from_mom = 2
from_dad = 2
total = initial + from_mom + from_dad
result = total


Q: Jason 有 20 颗棒棒糖。他给了 Denny 一些棒棒糖。现在 Jason 只剩 12 颗了。Jason 给了 Denny 多少颗棒棒糖？

# Python解决方案：
"""Jason 有 20 颗棒棒糖。他给了 Denny 一些棒棒糖。现在 Jason 只剩 12 颗了。Jason 给了 Denny 多少颗棒棒糖？"""
initial = 20
after = 12
given = initial - after
result = given

Q: {question}

# Python解决方案：
'''
```

现在我们可以将合并后的提示传递给 GPT-3。

```
llm_out = llm(MATH_PROMPT.format(question=question))
print(llm_out)
```

输出结果为：

<pre>
<span className="bluegreen-highlight">
"""Emma 乘飞机到西雅图花了 60 分钟。然后她乘了 2 小时的火车到波特兰，最后坐了 30 分钟的公共汽车到温哥华。她到温哥华需要多长时间？"""<br/>

plane_ride = 60<br/>
train_ride = 2 * 60  # 2 hours in minutes<br/>
bus_ride = 30<br/>
total_time = plane_ride + train_ride + bus_ride<br/>
result = total_time
</span>
</pre>

最后，我们可以将这段代码传递给 Python 运行时以获得答案：

```python
exec(llm_out)
print(result)
```

输出结果为 **210**，是正确的。

请参见此示例的 Jupyter 笔记本 [here](https://github.com/trigaten/Learn_Prompting/tree/main/docs/code_examples/PAL.ipynb)。

## 更多

还请参阅 [PAL 的 Colab 示例](https://colab.research.google.com/drive/1u4_RsdI0E79PCMDdcPiJUzYhdnjoXeXc?usp=sharing#scrollTo=Ba0ycacK4i1V)。
'''