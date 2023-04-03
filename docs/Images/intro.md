---
sidebar_position: 1
---

# 🟢 介绍

制作完美的图像需要找到最佳的刺激语句，这是一个特殊的挑战。与文本提示相比，研究图像提示的方法并不十分发达。这可能是因为创建基本上具有主观性并且通常缺乏良好准确度度量的对象的固有挑战。但是，请不要担心，图像提示界面（@parsons2022dalleprompt）已经对如何提示各种图像模型（@rombach2021highresolution）（@ramesh2022hierarchical）有了很多发现。

本指南涵盖了基本的图像提示技术，我们强烈鼓励您查看本章末尾的优秀资源。此外，我们在下面提供了端到端图像提示过程的示例。


## 示例

这里我将通过一个示例展示如何创建本课程首页的图像。我一直在进行深度强化学习神经辐射场项目的低多边形式样的实验。我喜欢低聚物样式，并想在本课程的图像中使用它。

我需要一个宇航员、一架火箭和一台电脑。

我进行了大量研究，试图在 [r/StableDiffusion](https://www.reddit.com/r/StableDiffusion/) 和其他网站上创建低聚物图像，但没有找到特别有用的东西。

我决定直接通过DALLE和prompt `Low poly white and blue rocket shooting to the moon in front of a sparse green meadow`开始尝试并查看结果。

import rockets1 from '@site/docs/assets/rockets_dalle_1.png';
import rockets2 from '@site/docs/assets/rockets_dalle_2.png';
import computer_1 from '@site/docs/assets/computer_dalle_1.png';
import astronaut_1 from '@site/docs/assets/astronaut_dalle_1.png';
import astronaut_2 from '@site/docs/assets/astronaut_sd_1.png';
import rocket_sd_1 from '@site/docs/assets/rocket_sd_1.png';
import rocket_final from '../../static/img/rocket.png';
import laptop_sd_1 from '@site/docs/assets/laptop_sd_1.png';
import gemstone_sd_1 from '@site/docs/assets/gemstone_sd_1.png';
import gemstone_sd_2 from '@site/docs/assets/gemstone_sd_2.png';
import gemstone_sd_3 from '@site/docs/assets/gemstone_sd_3.png';
import focus_final from '../../static/img/computer.png';
import astronaut_final from '../../static/img/astronaut.png';

<div style={{textAlign: 'center'}}>
  <img src={rockets1} style={{width: "750px"}} />
</div>


<div style={{textAlign: 'center'}}>
  <img src={rockets2} style={{width: "750px"}} />
</div>

我认为这些结果对于第一次尝试来说相当不错，我特别喜欢左下方的火箭。

接下来，我想要在同样的风格下创建一台电脑：`Low poly white and blue computer sitting in a sparse green meadow`

<div style={{textAlign: 'center'}}>
  <img src={computer_1} style={{width: "750px"}} />
</div>

最后，我需要一名宇航员！`Low poly white and blue astronaut sitting in a sparse green meadow with low poly mountains in the background` 显然可以做到。

<div style={{textAlign: 'center'}}>
  <img src={astronaut_1} style={{width: "750px"}} />
</div>

我认为第二个还不错。

现在我有了一名宇航员、一架火箭和一台电脑。我对它们感到满意，因此将它们放在了首页上。经过几天的时间和朋友们的反馈，我意识到风格不是很一致。😔.


我在 [r/StableDiffusion](https://www.reddit.com/r/StableDiffusion/) 上进行了更多的研究，并发现人们使用“等距投影”这个词。我决定尝试一下，使用Stable Diffusion而不是DALLE。我还意识到需要添加更多的修改器来限制风格。我尝试了这个prompt：`A low poly world, with an astronaut in white suit and blue visor sitting in a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={astronaut_2} style={{width: "250px"}} />
</div>

这些不太好，所以我决定先从火箭开始。

`A low poly world, with a white and blue rocket blasting off from a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={rocket_sd_1} style={{width: "250px"}} />
</div>

这些并不是特别好，但在这里进行一些迭代后，我最终得到的是

<div style={{textAlign: 'center'}}>
  <img src={rocket_final} style={{width: "250px"}} />
</div>

现在我需要一台更好的笔记本电脑了。

`A low poly world, with a white and blue laptop sitting in sparse green meadow with low poly mountains in the background. The screen is completely blue. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={laptop_sd_1} style={{width: "250px"}} />
</div>

结果参差不齐；我喜欢右下角的，但我决定换一个方向。

`A low poly world, with a glowing white and blue gemstone sitting in a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={gemstone_sd_1} style={{width: "250px"}} />
</div>

这还不太对。让我们尝试一些神奇和发光的东西。

`A low poly world, with a glowing white and blue gemstone magically floating in the middle of the screen above a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={gemstone_sd_2} style={{width: "250px"}} />
</div>

我喜欢这些，但希望石头在屏幕中央。

`A low poly world, with a glowing blue gemstone magically floating in the middle of the screen above a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={gemstone_sd_3} style={{width: "250px"}} />
</div>

大约在这里，我使用了SD的功能，让以前的图像对未来的图像产生某些影响。于是，我终于得到了：

<div style={{textAlign: 'center'}}>
  <img src={focus_final} style={{width: "250px"}} />
</div>

最后，我开始着手处理宇航员。

`A low poly world, with an astronaut in white suite and blue visor is sitting in a sparse green meadow with low poly mountains in the background. Highly detailed, isometric, 4K`

<div style={{textAlign: 'center'}}>
  <img src={astronaut_final} style={{width: "250px"}} />
</div>

此时，我对三个图像之间的风格一致性感到满意，可以在网站上使用它们了。对我来说，主要的收获是这是一个非常迭代、研究密集的过程，我必须随着尝试不同的prompt和模型而修改我的期望和想法。