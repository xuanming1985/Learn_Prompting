---
sidebar_position: 70
---
# 🟡 数学

在本课程中，我们已经看到了许多不同的提示方法，可以用来提高%%LLM|LLM%%数学能力。最近的一种方法是MathPrompter（@imani2023mathprompter），将一些这些方法统一成一种技术。这个整体的想法是将数学问题分解成代数术语，然后使用Python代码以不同的方式解决它。

import math from '@site/docs/assets/math.png';

<div style={{textAlign: 'center'}}>
  <img src={math} style={{width: "500px"}} />
</div>

MathPrompter有**四个**步骤。我们将使用以下示例问题来解释它们。该示例直接摘自论文。

```text
问题：在一家餐厅里，每份成人餐花费5美元，孩子可以免费用餐。如果一组有15个人进来，其中8个是孩子，请问这个团体用餐需要多少费用？
```

## 步骤1：生成代数模板

第一步是为问题中的每个数字分配一个变量。这有助于将问题转化为抽象的数学问题，并转化为编程代码。

这可以通过few-shot prompting完成：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="Q1: 一家动物园收取12美元的成人门票，允许5岁以下的儿童免费进入。一个家庭包括4个成人和2个5岁以下的儿童来参观动物园。这个家庭的总门票费用是多少？映射：{A: 12, B: 4, C: 2}
Q2: 一家商店出售60美元一双的鞋子和8美元一双的袜子。如果顾客购买了2双鞋和3双袜子，这次购物的总费用是多少？ 映射：{A: 60, B: 8, C: 2, D: 3}
Q3: 在一家餐厅里，每份成人餐花费5美元，孩子可以免费用餐。如果一组有15个人进来，其中8个是孩子，请问这个团体用餐需要多少费用？" initial-response="Qt: 在一家餐厅里，每份成人餐价格为$A，孩子们可以免费进餐。如果来了一个由B人组成的团体，其中C个是孩子，那么这个团体吃饭要花多少钱？\n映射：{A: 5, B: 15, C: 8}" max-tokens="256" box-rows="14" model-temp="0" top-p="0">
    <noscript>未能加载Dyno Embedde:必须启用JavaScript</noscript>
</div>

## 步骤2：数学提示

这一步的重点是将问题表示为代数语句和Python代码。该步骤有两个同时进行的提示，这有助于给出问题的各种不同的表示形式。

### 2a：代数语句

我们可以few-shot提示LLM将数学问题表示为代数语句。这是通过要求LLM生成答案格式来完成的，以“Answer =”开头。

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="Qt: 在一家餐厅里，每份成人餐价格为$A，孩子们可以免费进餐。如果来了一个由B人组成的团体，其中C个是孩子，那么这个团体吃饭要花多少钱？\n映射：{A: 5, B: 15, C: 8}\n\n编写一个数学方程并生成答案格式，以‘Answer =’开头\n\n答案 = A * (B - C)\n\nQt: 在一家商店里，鞋子的价格是每双$A，袜子的价格是每双$B。如果顾客购买了C双鞋和D双袜子，那么购买总价是多少？\n映射：{A: 60, B: 8, C: 2, D: 3}\n\n编写一个数学方程并生成答案格式，以‘Answer =’开头\n\n答案 = A * C + B * D\n\nQt: 在一家动物园里，每张成人门票收费$12，5岁以下的孩子可以免费进入。一个由4个成年人和2个5岁以下的儿童组成的家庭参观了动物园。这个家庭的总费用是多少？\n映射：{A: 12, B: 4, C: 2}\n\n编写一个数学方程并生成答案格式，以‘Answer =’开头" initial-response="Answer = A * B - A * C" max-tokens="256" box-rows="14" model-temp="0" top-p="0">
    <noscript>未能加载Dyno Embedde:必须启用JavaScript</noscript>
</div>
当前日期：2023年4月1日13:27:57

### 2b: Python代码

我们还可以向%%LLM|LLM%%询问生成解决问题的Python代码。这可以通过要求LLM生成Python函数来实现。

以下是一个例子：

```text
问题：在动物园里，每张成人门票的费用为$A，5岁以下的儿童可免费入场。如果一家有B名成人和C名5岁以下的孩子参观动物园，那么这个家庭进入动物园的总费用是多少？
变量：{A: 12, B: 4, C: 2}
请编写一个Python函数返回答案。

def zoo_cost(A, B, C):
   return A * B

问题：在商店里，鞋子每双售价为$A，袜子每双售价为$B。如果一个顾客买了C双鞋和D双袜子，那么购买的总费用是多少？

请编写一个Python函数返回答案。
def store_cost(A, B, C, D):
   return (A * C) + (B * D)

问题：在餐厅里，每个成人餐费用为$A，孩子们免费进餐。如果B个人来餐馆用餐，其中C个人是孩子，那么整个小组吃饭需要多少钱？

请编写一个Python函数返回答案。

def restaurant_cost(A, B, C):
   return A * (B - C)
```

### 答案生成

现在，我们可以使用之前生成的变量映射自动填充变量。

```text
变量：{A: 5, B: 15, C: 8}
```

代数式：
```text
答案 = 5 * 15 - 5 * 8
```

Python函数：
```python
def restaurant_cost(A=5, B=15, C=8):
  return A * (B - C)
```

我们可以使用Python对两者进行评估。

代数式：
```python
>>> eval("5 * 15 - 5 * 8")
35
```

Python函数：
```python
>>> restaurant_cost()
35
```

## 步骤4：自我一致性

最后，我们将利用 %%Self-Consistency|self_consistency%% 多次运行上述过程(~5次)，然后采取多数答案。

## 结论

MathPrompter在MultiArith(@roy-roth-2015-solving)数据集上报告了92.5％的准确率。这种技术的成功是一个很好的例子，它展示了**您**作为提示工程师如何将本课程学习的方法结合起来处理更大的问题。