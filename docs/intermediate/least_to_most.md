---
sidebar_position: 7
locale: en-us
style: chicago
---

# 🟡 由浅入深提示

由浅入深提示 (Least to Most prompting, LtM)(@zhou2022leasttomost) 在 CoT 提示的基础上，将一个问题首先分解成子问题，然后逐个解决。它是受到儿童实际教育策略的启发而提出的技术。

与 CoT 提示一样，要解决的问题被分解为一组相互依存的子问题。第二步，这些子问题一个接一个地被解决。与思路链不同，前一个子问题的解决方案被输入到尝试解决下一个问题的提示中。

import leastToMost from '@site/docs/assets/least_to_most_formal.png'

<div style={{textAlign: 'center'}}>
  <img src={leastToMost} style={{width: "600px"}}
        alt="最少至最多提示的图示" />
</div>

<div style={{textAlign: 'center'}}>
   最少至最多提示的图示
</div>

## 示例：客户查询响应

我们来问一个稍微复杂一点的客户服务问题：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="客户查询：\n我在3月1日购买了贵公司 Arnold 系列的T恤衫。我看到它正在打折，所以购买了一件原价30美元的衬衫，打了4折。我看到贵公司新的衬衫有50%的折扣。我想知道能否退回这件衬衫并有足够的商店信誉购买两件贵公司其他衣服？\n\n操作说明：\n您是一名客户服务代表，负责友好地回应客户的查询。退货在30天内允许。今天是3月29日。当前所有衬衫均有50%的折扣。贵公司的衬衫价格从18美元到100美元不等。不要捏造任何折扣政策。" initial-response="谢谢您的查询。不幸的是，由于您在3月1日购买了这件衬衫，这已经超过我们的30天退货政策。但是，我们目前所有衬衫都有50%的折扣，因此您可以用相同的商店信用购买两件衬衫。我们的衬衫价格在18-100美元之间。如果您有其他问题，请告诉我们。" max-tokens="256" box-rows="16" model-temp="0" top-p="0">
    <noscript>无法加载Dyno Embed：必须启用JavaScript</noscript>
</div>

<br/>这个回答失败了 (因为我们仍处于退货时间内)，所以我们尝试将其分解成子问题：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="客户查询：\n我在3月1日购买了贵公司 Arnold 系列的T恤衫。我看到它正在打折，所以购买了一件原价30美元的衬衫，打了4折。我看到贵公司新的衬衫有50%的折扣。我想知道能否退回这件衬衫并有足够的商店信誉购买两件贵公司其他衣服？\n\n操作说明：\n您是一名客户服务代表，负责友好地回应客户的查询。退货在30天内允许。今天是3月29日。当前所有衬衫均有50%的折扣。贵公司的衬衫价格从18美元到100美元不等。不要捏造任何折扣政策。\n在回答查询之前必须解决哪些子问题？" initial-response="1. 确定客户是否在30天退货期内。\n2. 计算如果客户退回衬衫可以获得的商店信用额度。\n3. 计算目前50%折扣下两件衣服的成交价。\n4. 确定客户是否有足够的商店信用购买两件衣服。" max-tokens="256" box-rows="12" model-temp="0" top-p="0">
    <noscript>无法加载Dyno Embed：必须启用JavaScript</noscript>
</div>

<br/>我们尝试解决第一个子问题：

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="客户查询：\n我在3月1日购买了贵公司 Arnold 系列的T恤衫。我看到它正在打折，所以购买了一件原价30美元的衬衫，打了4折。我看到贵公司新的衬衫有50%的折扣。我想知道能否退回这件衬衫并有足够的商店信誉购买两件贵公司其他衣服？\n\n操作说明：\n您是一名客户服务代表，负责友好地回应客户的查询。退货在30天内允许。今天是3月29日。当前所有衬衫均有50%的折扣。贵公司的衬衫价格从18美元到100美元不等。不要捏造任何折扣政策。\n确定客户是否在30天退货期内。我们逐步来吧。" initial-response="是的，客户在30天退货期内。客户于3月1日购买了衬衫，今天是3月29日，即在30天退货期内。\n\n因此，客户有资格退回衬衫并为购买商店信用获得了相应的金额。客户可以使用商店信用以目前的50%折扣购买两件衬衫。" max-tokens="256" box-rows="19" model-temp="0" top-p="0">
    <noscript>无法加载Dyno Embed：必须启用JavaScript</noscript>
</div>

仅解决第一个子问题，我们就能够解决整个问题。如果GPT-3并不能立即返回答案，我们可以解决下一个子问题，直到它返回答案。请注意，我们在末尾添加了“逐步来吧”。这个短语的添加并不总是必要的，但它对这个示例很有帮助。

## 示例：字母串联

最少至最多提示最初是使用少样本提示引入的，而不是明确的指令将问题分解为多个步骤（如上例所示）。此外，它有时可以使用单个提示来实现

### 第三种尝试：由少到多 (单次提示)

当前日期：2023年4月1日 10:20:18

最少到最多提示的方法通过重新表述之前拼接的结果来改进Chain of Thought概念。这样可以简化每个步骤，只拼接一个新的字母。这直到12个或更多单词都具有良好的性能。

这种方法看起来与Chain of Thought非常相似，但在概念上却非常不同。在每个步骤中，我们引入了之前的拼接结果。在“think, machine, learning”的例子中，它不会单独拼接字母“k”，“e”，“g”，而是将“ke”和“g”拼接在一起。由于重新引入了先前的工作结果，模型现在可以推广到更长的链，因为它沿着结果增量地进行，并且每个步骤只需要做少量的工作。

<div trydyno-embed="" openai-model="text-davinci-003"
     initial-prompt="Q: think, machine\nA: &#34;think&#34;的最后一个字母是&#34;k&#34;。&#34;machine&#34;的最后一个字母是&#34;e&#34;。将&#34;k&#34;和&#34;e&#34;拼接在一起得到&#34;ke&#34;。因此，&#34;think, machine&#34;输出为&#34;ke&#34;。\n\nQ: think, machine, learning\nA: &#34;think, machine&#34;输出为&#34;ke&#34;。&#34;learning&#34;的最后一个字母是&#34;g&#34;。将&#34;ke&#34;和&#34;g&#34;拼接在一起得到&#34;keg&#34;。所以，&#34;think, machine, learning&#34;是&#34;keg&#34;。\n\nQ: transformer, language\nA: &#34;transformer&#34;的最后一个字母是&#34;r&#34;。&#34;language&#34;的最后一个字母是&#34;e&#34;。将&#34;r&#34;和&#34;e&#34;拼接在一起得到&#34;re&#34;。因此，&#34;transformer, language&#34;是&#34;re&#34;。\n\nQ: transformer, language, vision\nA: &#34;transformer, language&#34;输出为&#34;re&#34;。&#34;vision&#34;的最后一个字母是&#34;n&#34;。将&#34;re&#34;和&#34;n&#34;拼接在一起得到&#34;ren&#34;。所以，&#34;transformer, language, vision&#34;是&#34;ren&#34;。\n\nQ: foo,bar,baz,blip,learn,prompting,world,shaking,event,dancefloor,prisma,giraffe\nA:"
     initial-response="&#34;foo&#34;的最后一个字母是&#34;o&#34;。&#34;bar&#34;的最后一个字母是&#34;r&#34;。将&#34;o&#34;和&#34;r&#34;拼接在一起得到&#34;or&#34;。&#34;baz&#34;的最后一个字母是&#34;z&#34;。将&#34;or&#34;和&#34;z&#34;拼接在一起得到&#34;orz&#34;。&#34;blip&#34;的最后一个字母是&#34;p&#34;。将&#34;orz&#34;和&#34;p&#34;拼接在一起得到&#34;orzp&#34;。&#34;learn&#34;的最后一个字母是&#34;n&#34;。将&#34;orzp&#34;和&#34;n&#34;拼接在一起得到&#34;orzpn&#34;。&#34;prompting&#34;的最后一个字母是&#34;g&#34;。将&#34;orzpn&#34;和&#34;g&#34;拼接在一起得到&#34;orzpng&#34;。&#34;world&#34;的最后一个字母是&#34;d&#34;。将&#34;orzpng&#34;和&#34;d&#34;拼接在一起得到&#34;orzpngd&#34;。&#34;shaking&#34;的最后一个字母是&#34;g&#34;。将&#34;orzpngd&#34;和&#34;g&#34;拼接在一起得到&#34;orzpngdg&#34;。&#34;event&#34;的最后一个字母是&#34;t&#34;。将&#34;orzpngdg&#34;和&#34;t&#34;拼接在一起得到&#34;orzpngdgt&#34;。"
     max-tokens="256" box-rows="18"
     model-temp="0.2" ></div>
     

### 结果

在最后一个12个单词的字母连接问题中，Chain of Thought的准确率为34％，而Least to Most的准确率为74％（该论文使用text-davinci-002作为模型）。

## 示例：组合推理（SCAN）

SCAN基准测试（@lake2018scan）要求模型将自然语言转换为操作序列。例如，“向左跑步并走两次”将被翻译为“TURN_LEFT + RUN + WALK * 2”。当面对训练集中长度大于的序列时，语言模型表现特别差。

### 第一次尝试：标准提示

使用简单的标准提示，text-davinci-003取得了惊人的进展，但仍然失败了。

<div trydyno-embed="" openai-model="text-davinci-003"
     initial-prompt="Q: 向左转\nA: TURN LEFT\n\nQ: 向右转\nA: TURN RIGHT\n\nQ: 向左跳跃\nA: TURN LEFT &#43; JUMP\n\nQ: 向右奔跑\nA: TURN RIGHT &#43; RUN\n\nQ: 看两次\nA: LOOK * 2\n\nQ: 奔跑并看两次\nA: RUN &#43; LOOK * 2\n\nQ: 向右跳三次\nA: (TURN RIGHT &#43; JUMP) * 3\n\nQ: 在跑后走路\nA: RUN &#43; WALK\n\nQ: 向左相反\nA: TURN LEFT * 2\n\nQ: 向左转圈\nA: TURN LEFT * 4\n\nQ: 向右相反\nA: TURN RIGHT * 2\n\nQ: 向右转圈\nA: TURN RIGHT * 4\n\nQ: 向左相反走路\nA: TURN LEFT * 2 &#43; WALK\n\nQ: 向左转圈走路\nA: (TURN LEFT &#43; WALK) * 4\n\nQ: &#34;在向左相反走三步后，左转圈两次跳跃&#34;\nA:"
     initial-response="(TURN LEFT * 2 + WALK) * 3 + (TURN LEFT + JUMP) * 2"
     max-tokens="512" box-rows="18"
     model-temp="0.2" ></div>

### 第二次尝试：Least to Most，第一步 - Reduction

这里，我们使用2个不同的提示。第一个提示用于将输入问题缩小为简单的步骤序列。第二个提示用于将这个简化的步骤序列映射到实际操作中。

两个提示都相当长，并使用压缩的Python符号来保存操作以节省标记。
第一步将自然语言描述分解为更明确但仍然类似于人类的语言。这将有助于映射步骤按顺序找出问题。例如，“向左转圈两次”缩减为“向左跳”-> `TURN_LEFT + JUMP`，“向左转圈”-> `(TURN_LEFT + JUMP) * 4`。类似地，缩减步骤是用于解释重复概念（两次、三次等）的。

<div trydyno-embed="" openai-model="text-davinci-003"
     initial-prompt="第二次尝试：从少到多，第二步-映射\n\n在第二步中，我们使用缩小后的输出，并再次使用相当长的提示（14个案例）将缩小后的自然语言描述转换为一系列操作。\n\n在此，我们注入第一步的输出：\n\n> &#34;jump around left twice&#34; can be solved by: &#34;jump left&#34;, &#34;jump around left&#34;, &#34;jump around left twice&#34;. &#34;walk opposite left thrice&#34; can be solved by: &#34;walk opposite left&#34;, &#34;walk opposite left thrice&#34;. So, &#34;jump around left twice after walk opposite left thrice&#34; can be solved by: &#34;jump left&#34;, &#34;jump around left&#34;, &#34;jump around left twice&#34;, &#34;walk opposite left&#34;, &#34;walk opposite left thrice&#34;." 
     initial-response="&#34;jump around left twice&#34; can be solved by: &#34;jump left&#34;, &#34;jump around left&#34;, &#34;jump around left twice&#34;. &#34;walk opposite left thrice&#34; can be solved by: &#34;walk opposite left&#34;, &#34;walk opposite left thrice&#34;. So, &#34;jump around left twice after walk opposite left thrice&#34; can be solved by: &#34;jump left&#34;, &#34;jump around left&#34;, &#34;jump around left twice&#34;, &#34;walk opposite left&#34;, &#34;walk opposite left thrice&#34;."
     max-tokens="256" box-rows="18"
     model-temp="0.2" ></div>

在第二步中，我们使用缩小后的输出，并再次使用相当长的提示（14个案例）将缩小后的自然语言描述转换为一系列操作。

以下是第一步的输出:
> "jump around left twice" can be solved by: "jump left", "jump around left", "jump around left twice". "walk opposite left thrice" can be solved by: "walk opposite left", "walk opposite left thrice". So, "jump around left twice after walk opposite left thrice" can be solved by: "jump left", "jump around left", "jump around left twice", "walk opposite left", "walk opposite left thrice".

## 结果

从Least to Most（最小到最大）提示方式中得到了多个改进：
- 相比于Chain of Thought（思维链），准确率有了提高
- 在比提示中更难的问题上，有了更好的泛化能力
- 在组合式泛化方面，特别是SCAN（Sequential ConcAl Neural）基准测试方面，性能得到了Dramatic（戏剧性）的提升(@lake2018scan)

使用text-davinci-002（文章中使用的模型）标准提示，成功解决了6%的SCAN问题，而Least to Most提示则实现了惊人的76%的成功率。最终结果对于code-davinci-002来说更加显著，其中Least to Most提示实现了99.7%的成功率。

initial-prompt文本信息翻译如下：

Q: 左转
A: “左转”输出“TURN LEFT”。

Q: 右转
A: “右转”输出“TURN RIGHT”。

Q: 左跳
A: “左跳”的输出连接： “左转”的输出，“跳”的输出。 “左转”输出“TURN LEFT”。“跳”输出“JUMP”。因此将“左转”的输出与“跳”的输出连接起来，导致“TURN LEFT” + “JUMP”。因此，“左跳”的输出是“TURN LEFT”+“JUMP”。

Q: 右跳
A: “右跳”的输出连接： “右转”的输出，“跳”的输出。 “右转”输出“TURN RIGHT”。 “跳”输出“JUMP”。因此将“右转”的输出和“跳”的输出连接起来，导致“TURN RIGHT” + “JUMP”。因此，“右跳”的输出是“TURN RIGHT”+“JUMP”。

Q: 看两次
A: “看两次”的输出连接： “看”的输出，“看”的输出。“看”输出“LOOK”。 因此，重复“看”的输出两次会导致“LOOK”* 2。 因此，“看两次”的输出是“LOOK”* 2。

Q: 跑然后向右看两次
A: “跑然后向右看两次”的输出连接：“跑”的输出， “看两次”的输出。 “跑”的输出是“RUN”。 “看两次”的输出是“LOOK”* 2。 因此将“跑”的输出和“看两次”的输出连接起来，导致“RUN” + “LOOK”* 2。 因此，“跑然后向右看两次”的输出是“RUN” + “LOOK”* 2。

Q: 右跳三次
A: “右跳三次”的输出连接：“右跳”的输出，“右跳”的输出，“右跳”的输出。“右跳”输出“TURN RIGHT” + “JUMP”。因此将“右跳”的输出重复三次导致（“TURN RIGHT” + “JUMP”） * 3。因此，“右跳三次”的输出是（“TURN RIGHT” + “JUMP”）* 3。

Q: 跑后面走
A: “跑后面走”的输出连接：“跑”的输出，“走”的输出。 “跑”的输出是“RUN”。“走”的输出是“WALK”。因此将“跑”的输出和“走”的输出连接起来，导致“RUN” + “WALK”。因此，“跑后面走”的输出是“RUN”+“WALK”。

Q: 反向左转
A: “反向左转”的输出连接： “左转”的输出，“左转”的输出。“左转”输出“TURN LEFT”。因此将“左转”的输出重复两次导致“TURN LEFT”* 2。 因此，“反向左转”的输出是“TURN LEFT”* 2。

Q: 在左边转一圈
A: “在左边转一圈”的输出连接：“向左转”的输出，“向左转”的输出，“向左转”的输出，“向左转”的输出。“向左转”输出“TURN LEFT”。“因此将“向左转”的输出重复四次导致“TURN LEFT”* 4。 因此，“在左边转一圈”的输出是“TURN LEFT”* 4。

Q: 反向右转
A: “反向右转”的输出连接： “右转”的输出，“右转”的输出。“右转”输出“TURN RIGHT”。因此将“右转”的输出重复两次导致“TURN RIGHT”* 2。 因此，“反向右转”的输出是“TURN RIGHT”* 2。

Q: 在右边转一圈
A: “在右边转一圈”的输出连接：“向右转”的输出，“向右转”的输出，“向右转”的输出，“向右转”的输出。“向右转”输出“TURN RIGHT”。因此将“向右转”的输出重复四次导致“TURN RIGHT”* 4。 因此，“在右边转一圈”的输出是“TURN RIGHT”* 4。

Q: 反向左走
A: “反向左走”的输出连接： “反向左转”的输出，“走”的输出。“反向左转”的输出是“TURN LEFT”* 2。 “走”的输出是“WALK”。因此将“反向左转”的输出和“走”的输出连接起来，导致“TURN LEFT”* 2 + “WALK”。因此，“反向左走”的输出是“TURN LEFT”* 2 + “WALK”。

Q: 在左边走一圈
A: “在左边走一圈”的输出连接： “向左走”的输出，“向左走”的输出，“向左走”的输出，“向左走”的输出。“向左走”输出“TURN LEFT”+“WALK”。 因此将“向左走”的输出重复四次导致（“TURN LEFT” + “WALK”）* 4。 因此，“在左边走一圈”的输出是（“TURN LEFT” + “WALK”）* 4。

initial-response文本信息翻译如下：

Least to Most提示方式得到了多个改进：
- 相比于Chain of Thought（思维链），准确率有了提高
- 在比提示中更难的问题上，有了更好的泛化能力
- 在组合式泛化方面，特别是在SCAN基准测试方面，Least to Most提示的性能得到了戏剧性的提升。使用text-davinci-002模型进行Least to Most提示的方式，成功解决了76%的SCAN问题，而标准提示只解决了6%的问题。对于code-davinci-002模型来说，Least to Most提示的成功率更高，达到了99.7%。