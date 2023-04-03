---
sidebar_position: 3
---
# 🟢 音乐生成

音乐生成模型越来越受欢迎，并将最终对音乐产业产生巨大影响。

音乐生成模型可以创建和弦进行、旋律或完整歌曲。它们可以结构化和创作特定流派的音乐，以特定艺术家的风格创作或即兴创作。

然而，尽管音乐模型具有巨大的潜力，但它们目前很难通过提示来完全自定义生成的输出，与图像或文本生成模型不同。

## Riffusion
import riffusion from '@site/docs/assets/riffusion_phonk.png';

<div style={{textAlign: 'center'}}>
  <img src={riffusion} style={{width: "500px"}} />
</div>

Riffusion(@Forsgren_Martiros_2022)是 Stable Diffusion 的优化版本，可以通过提示来控制生成的乐器和伪造的风格，但可用的拍子数量有限。

## Mubert

[Mubert](https://mubert.com/)似乎是通过情感分析解释提示，并将适当的音乐风格链接到提示（无法通过提示详细控制音乐参数）。目前尚不清楚该结果的生成有多少是由AI完成的。

## 其他

有人试图将GPT-3作为Text-2-Music工具，使用实际提示的音乐元素在音符的“微观层面”上（而不是mubert和riffusion产生的模糊提示式类比），但目前这些尝试仅限于单个乐器。

其他方法包括模型链，可以将任何图像转换为代表该图像的声音，并提示ChatGPT生成用于创建声音的Python库的代码。

## 注意

音乐提示还没有很好地构建起来...然而，MusicLM看起来很有前途，但它尚未公开发行。