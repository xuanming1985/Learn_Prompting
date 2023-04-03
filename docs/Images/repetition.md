---
sidebar_position: 50
---
# 🟢 Repetition

在提问中重复同一个词或相似的短语可能会导致模型强调该单词或短语在生成的图像中的重要性。例如，@Phillip Isola用DALLE生成了这些瀑布：

import bad_water from '@site/docs/assets/images_chapter/bad_water.jpg';
import good_water from '@site/docs/assets/images_chapter/good_water.jpg';
import planet from '@site/docs/assets/images_chapter/planet.png';
import planet_aliens from '@site/docs/assets/images_chapter/planet_aliens.png';


`一幅美丽的画，画着一座山和瀑布。`

使用`非常非常非常......非常非常非常非常非常非常非常非常非常非常非常......非常非常非常美丽的画，画着一座山和瀑布。`这样的提示会使生成的图像更加突出`非常`这个词，从而提高生成质量。重复还可以用于强调主题词。例如，如果您想生成一张带外星人的星球图片，使用提示`一个带有外星人的星球，外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人`会使外星人出现在最终生成的图像中的概率更高。以下图片是使用Stable Diffusion生成的。
`一个带外星人的星球`
<div style={{textAlign: 'center'}}>
  <img src={good_water} style={{width: "750px"}} />
</div>

强调 “非常” 这个词似乎能提高生成质量！重复也可以用于强调主题。例如，如果你想生成一个有外星人的星球图像，使用提示词 `一个有外星人的星球外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人` 将使得最终的图像中更有可能出现外星人。下面的图片使用 Stable Diffusion 创建。

`一个有外星人的星球`
<div style={{textAlign: 'center'}}>
  <img src={planet} style={{width: "250px"}} />
</div>

`一个有外星人的星球外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人外星人`

<div style={{textAlign: 'center'}}>
  <img src={planet_aliens} style={{width: "250px"}} />
</div>


## 注意 

这种方法并不完美，使用权重（下一篇文章）通常是一个更好的选择。