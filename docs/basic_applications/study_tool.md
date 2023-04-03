---
sidebar_position: 30
---

# 🟢 学习伙伴

我们最喜欢使用LLMs的方法之一是作为学习工具！特别是用于解释困惑术语以及测试内容的测试。

## 解释术语

我最近读了一本关于仙人掌（@nobel2002cacti）的教科书，并看到了这段文字：

```text
Because of adaptations to xeric conditions and the presence of **less derived woods** in Pereskioideae, the Cactaceae is one of the most interesting families both anatomically and morphologically. 
```

我不知道**less derived woods**是什么意思，而Google只显示了相关的研究文章。所以我决定使用ChatGPT来找出它的含义。我给它以下提示，这个提示就是从书中引用的这段话和我的问题。

```text
因为对干旱条件的适应和Pereskioideae中存在较少进化出来的木材，Cactaceae科是解剖学和形态学上最有趣的家族之一。

less derived woods是什么意思？

// highlight-start
在这个背景下，“less derived woods”指的是早期植物进化时发现的祖先木材的形式。 这些木材缺乏后来进化的更复杂的结构和特征。 在仙人掌科家族的Pereskioideae亚科中，相比其他亚科，木材 less derived，与非仙人掌植物中发现的木材更相似。在Pereskioideae中，这种不太衍生的木材被认为是一种适应较潮湿和较不干燥的环境，与其他仙人掌比较。
//highlight-end
```

这个模型能够快速回答我的问题，我能够继续阅读。

## 自测

使用LLMs作为学习工具的另一种方法是自问自答。 如果你即将参加微积分考试，你只需要问：

```text
生成5个微积分I测验题目：

// highlight-start
1. y = x3的导数是多少？
2. 评估函数x3 + 5x2 + 7x + 4的不定积分。
3. 导数的乘法规则是什么？
4. 在点（4,16）处，y = x2的切线方程是什么？
5. y = x4的二阶导数是什么？
//highlight-end
```

如果你正在学习一个更具体的主题，可能需要向模型提供额外的信息，例如你的课堂笔记！

```text
2020年3月20日下午2:30

市场营销课堂笔记

- 讨论了市场营销的四个P：产品、价格、渠道、促销
- B2C和B2B的区别：B2C是直接面向消费者市场，B2B是企业对企业的市场
- 不同类型的市场研究：定量和定性，例如调查和焦点小组讨论
- 定价策略：撇去、渗透、竞争和价值
- 细分：通过年龄、收入等细分客户群体。
- 4种细分类型：人口统计学、地理、心理学和行为学
- 讨论了不同的推广工具：广告、个人销售、公共关系、直接营销和销售促销

基于我的笔记，为我生成5个市场营销测验题目：

// highlight-start
1. 市场营销的四个P是什么？
2. B2C和B2B的区别是什么？
3. 市场研究有哪些分类？
4. 细分有哪4种类型？
5. 列举5个推广工具。
//highlight-end
```

## 结论

可能有其他方法使用LLMs进行学习。许多学习公司，如Duolingo和Quizlet，正在将LLMs纳入其产品中。