---
sidebar_position: 60
---

# 🟢 W加权术语

某些模型（如Stable Diffusion，Midjourney等）允许您在提示中加权术语。这可用于强调生成图像中的某些单词或短语。也可以用来减少生成图像中某些单词或短语的重要性。让我们考虑一个简单的例子：

import mountains from '@site/docs/assets/images_chapter/mountains.png';
import mountains_no_trees from '@site/docs/assets/images_chapter/mountains_no_trees.png';
import planets from '@site/docs/assets/images_chapter/planets.png';


# 示例

这里有几个由Stable Diffusion生成的山峰，初始提示为“mountain”。

<div style={{textAlign: 'center'}}>
  <img src={mountains} style={{width: "350px"}} />
</div>

但是，如果我们想要没有树的山峰，我们可以使用提示 `mountain | tree:-10`. 由于我们将tree的权重设置得非常低，因此它们不会出现在生成的图像中。

<div style={{textAlign: 'center'}}>
  <img src={mountains_no_trees} style={{width: "350px"}} />
</div>

加权术语可以组合成更复杂的提示，例如
`A planet in space:10 | bursting with color red, blue, and purple:4 | aliens:-10 | 4K, high quality`

<div style={{textAlign: 'center'}}>
  <img src={planets} style={{width: "350px"}} />
</div>

## 注意事项

有关加权的更多信息，请参阅本章末尾的一些资源。