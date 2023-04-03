---
sidebar_position: 5
---
# 🟢 生成优化器

生成优化器（quality boosters）是添加到提示中的术语，用于改善生成图像的某些非样式特定的品质。例如，“惊人的”、“美丽的”和“高质量”的生成优化器可以用于提高所生成图像的质量。

import pyramids from '@site/docs/assets/images_chapter/pyramids.png';
import special_pyramids from '@site/docs/assets/images_chapter/special_pyramids.png';

# 示例

回想一下，在另一个页面上使用DALLE生成的金字塔，以及“pyramid”这个提示。

<div style={{textAlign: 'center'}}>
  <img src={pyramids} style={{width: "750px"}} />
</div>

现在看看使用这个提示生成的金字塔：
`一座美丽、雄伟、令人难以置信的金字塔，4K`

<div style={{textAlign: 'center'}}>
  <img src={special_pyramids} style={{width: "750px"}} />
</div>

这些金字塔更具景色和印象！

以下是几个生成优化器：

```text
高分辨率、2K、4K、8K、清晰、良好的照明、详细、极其详细、清晰对焦、复杂、美丽、逼真++，互补色、高质量、超详细、杰作、最佳质量、艺术站、令人惊叹
```

## 注意

与Oppenlaender et al.(@oppenlaender2022taxonomy)上一页的注释类似，我们的工作定义与生成优化器有所不同。尽管如此，有时很难确切区分生成优化器和样式修饰符。