---
sidebar_position: 4
---
# 🟢 样式修改器

样式修改器是一些描述符，可以持续产生某些特定的样式（例如“红色着色的”，“玻璃制成的”，“在Unity中渲染的”）(@oppenlaender2022taxonomy)。它们可以组合在一起以产生更具体的样式。它们可以“包含有关艺术时期、学派和风格的信息，还包括艺术材料和媒介、技术和艺术家”(@oppenlaender2022taxonomy)。

import pyramids from '@site/docs/assets/images_chapter/pyramids.png';
import red_pyramids from '@site/docs/assets/images_chapter/red_pyramids.png';

# 示例

这里展示了一些通过DALLE生成的金字塔，使用initial-prompt：“pyramid”。

<div style={{textAlign: 'center'}}>
  <img src={pyramids} style={{width: "750px"}} />
</div>

这里展示了一些通过DALLE生成的金字塔，使用initial-prompt：“用玻璃制成的、在Unity中渲染的、着色为红色的金字塔”，其中使用了3个样式修改器。

<div style={{textAlign: 'center'}}>
  <img src={red_pyramids} style={{width: "750px"}} />
</div>

这是一些有用的样式修改器的列表:

```text
photorealistic, by greg rutkowski, by christopher nolan, painting, digital painting, concept art, octane render, wide lens, 3D render, cinematic lighting, trending on ArtStation, trending on CGSociety, hyper realist, photo, natural light, film grain
```

## 注意事项

Oppenlaender等人(@oppenlaender2022taxonomy)将“在……中渲染”的描述符描述为质量提升器，但我们的工作定义不同，因为该修改器确实可以产生特定的Unity（或其他渲染引擎）样式。因此，我们将该描述符称为一个样式修改器。