---
sidebar_position: 10
---

# 🔴 校准LLMs

可以通过校准**输出分布**(@zhao2021calibrate)来抵消LLMs表现出的某些偏差。

**校准输出分布具体是什么意思？**

让我们通过一个快速的例子来说明：假设我们有一个情感分析任务，有两个可能的标签，`Positive`和`Negative`。考虑当LLM受到提示 `Input: nothing Sentiment:` 时会发生什么。这个输入不包含任何LLM用于进行情感预测的上下文信息，因此它被称为 **无上下文** 输入。

由于`nothing`既不是积极的概念也不是消极的概念，我们期望LLM对于`Positive`和`Negative`都输出约为0.5的概率。然而，通常情况下（对于这个示例也是如此），实际输出并非如此。
```
p("Positive"| "Input: nothing Sentiment:") = 0.9

p("Negative"| "Input: nothing Sentiment:") = 0.1
```

考虑到这样一个上下文无关的输入的标签概率，我们知道LLM的**输出分布**可能存在偏差，使其更加倾向于标签“正面”。这可能会导致LLM在所有输入中更倾向于“正面”，即使输入实际上并不是积极的。

如果我们可以以某种方式 **校准** 输出分布，使得上下文无关的输入被分配为`Positive`和`Negative`的概率都为0.5，则我们通常可以消除对“Positive”的偏好，使LLM对于上下文无关的输入和有上下文的输入都更可靠。

## 非技术性解决方案

解决此问题的非技术性方法是简单地提供少量样例，其中上下文无关的范例有效地被分配为标签`Positive`和`Negative`的概率均为0.5。

例如，我们可以提供以下少量样例，其中显示每个上下文无关示例均被分类为 `Positive` 和`Negative`：
```
Input: 我讨厌这部电影。Sentiment: Negative
Input: 我喜欢这部电影。Sentiment: Positive
Input: N/A Sentiment: Positive
Input: N/A Sentiment: Negative
Input: nothing Sentiment: Positive
Input: nothing Sentiment: Negative
Input: 我喜欢鸡蛋。Sentiment:
```

据我所知，文献中尚未探讨此解决方案，不确定其在实践中的效果如何。但这是一个简单的解决方案，演示了校准试图实现的内容。

## 技术解决方案

另一种解决方法是 __上下文校准__ (@zhao2021calibrate)，我们调整特殊的校准参数，以确保像 `Input: nothing Sentiment:` 这样的上下文无关输入被分配为两个标签的概率约为0.5。请注意，实际上这种方法会对多个不同的无上下文输入（例如`Input: N/A Sentiment:`，`Input: [MASK] Sentiment:`）执行校准。它将适用于最佳校准参数的每个上下文无关输入平均起来，以找到LLM的最佳校准参数。

### 例子

让我们通过一个例子来计算一个上下文无关输入的校准参数。请注意，由于无法将它们限制为标签“Positive”和“Negative”，因此此示例无法在GPT-3上复现。

再次考虑上述示例，其中LLM为上下文无关输入分配以下标签概率：

```
p("Positive"| "Input: nothing Sentiment:") = 0.9

p("Negative"| "Input: nothing Sentiment:") = 0.1
```

我们希望找到一些概率分布q，使得

```
q("Positive"| "Input: nothing Sentiment:") = 0.5

q("Negative"| "Input: nothing Sentiment:") = 0.5
```

我们将创建一个线性变换来调整(校准) $p$ 的概率。

$\^{q} = \text{Softmax}(W\^{p} + b)$

该方程将原始概率 $\^{p}$ 应用于权重 $W$ 和偏置 $b$。 权重 $W$ 和偏置 $b$ 是校准参数，应用于上下文无关示例的概率，将产生 $\^{q}$ = [0.5, 0.5]。

#### 计算 W 和 b

我们需要以某种方式计算权重 $W$ 和偏置 $b$。一种计算方法是：

$W = \text{diag}(\^{p})^{-1}$ 

$b = 0$

虽然 $W$ 的定义可能一开始看起来有点奇怪，但它只是取 $\^{p}$ 中每个值的倒数，以找到一个可将原始概率 $\^{p}$ 转换为经过校准的概率 [0.5, 0.5] 的 $W$。

让我们验证这个在上述示例中是否有效：

$\^{p} = [0.9, 0.1]$

$W = \text{diag}(\^{p})^{-1} = \text{diag}([0.9, 0.1])^{-1} 
= \begin{bmatrix}
   0.9 & 0 \\
   0 & 0.1
\end{bmatrix}^{-1}
= \begin{bmatrix}
   1.11 & 0 \\
   0 & 10
\end{bmatrix}$

$\^{q} = \text{Softmax}(W\^{p} + b) = \text{Softmax}(\begin{bmatrix}
   1.11 & 0 \\
   0 & 10
\end{bmatrix}*{[0.9, 0.1]} + 0)
= \text{Softmax}([1, 1])
=[0.5, 0.5]$

如上所述，我们将为多个不同的无上下文输入执行相同的过程，并平均适用于每个无上下文输入的最佳校准参数，以找到LLM的最佳校准参数。这意味着最终的校准参数可能不会将任何上下文无关输入映射到恰好的[0.5, 0.5]。

## 要点

LLMs往往对某些标签存在偏好。校准可用于抵消此偏差。