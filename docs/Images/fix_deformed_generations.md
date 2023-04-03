---
sidebar_position: 90
---
# 🟢 修复变形生成

变形生成，特别是人体部位（例如手、脚）的变形，是许多模型常见的问题。这些问题可以通过良好的负面提示来处理（@blake2022with）。以下示例改编自[Reddit上的这篇帖子](https://www.reddit.com/r/StableDiffusion/comments/z7salo/with_the_right_prompt_stable_diffusion_20_can_do/)。

## 示例

import good_pitt from '@site/docs/assets/images_chapter/good_pitt.png';
import bad_pitt from '@site/docs/assets/images_chapter/bad_pitt.png';

使用Stable Diffusion v1.5和以下提示，我们可以生成一个漂亮的布拉德·皮特（Brad Pitt）图像，除了他的手当然没修复好！

`Martin Schoeller拍摄，90mm镜头，细节丰富的Brad Pitt挥手照片:6`

<div style={{textAlign: 'center'}}>
  <img src={bad_pitt} style={{width: "250px"}} />
</div>

使用强大的负面提示，我们可以生成更加令人信服的手部。

`Martin Schoeller拍摄，90mm镜头，细节丰富的Brad Pitt挥手照片:6 | 残缺不全的，畸形的手，模糊的，颗粒状的，损坏的，斗鸡眼了的，ps过的，曝光过度的，曝光不足的，低分辨率的，解剖错误的，手势错误的，多余的数字，数字缺失，手指错误的，耳朵错误的，眼睛错误的，脸部错误的，裁剪的：-5`
<div style={{textAlign: 'center'}}>
  <img src={good_pitt} style={{width: "250px"}} />
</div>

类似的负面提示也可以帮助其他身体部位。不幸的是，这种技术不是很一致，因此您可能需要尝试多个生成步骤才能获得好的结果。
在将来，这种提示方法应该是不必要的，因为模型将会不断改进。
然而，在当前情况下，这是一种非常有用的技术。


# 注意

改进的模型，如[Protogen](https://civitai.com/models/3666/protogen-x34-official-release)，通常在处理手、脚等部位时更加出色。