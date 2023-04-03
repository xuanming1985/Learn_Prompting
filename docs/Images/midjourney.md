---
sidebar_position: 99
---
# 🟢 Midjourney

[Midjourney](https://www.midjourney.com) 是一个基于 AI 技术的图像生成器，用户可以通过 Discord 机器人接口和 Web 应用程序进行操作（Midjourney 还计划推出 API 版本）。与其他 AI 图像生成器相比，使用 Midjourney 生成图像的过程遵循相同的基本原则，包括使用提示来指导生成过程。

与其他 AI 图像生成器相比，Midjourney 的一个独特特点是其能够创建具有视觉冲击力和艺术性构图的图像。这归因于该模型的专业训练，使其能够以特定的艺术参数产生高质量的图像（更多信息请参见“高级提示”>“参数”）。

您可以在 [Learn Prompting Discord](http://learnprompting.org/discord) 或 [官方 Midjourney Discord 服务器](https://discord.gg/midjourney) 中尝试 Midjourney Bot。

import midjourney_astronaut from '@site/docs/assets/midjourney_astronaut.png';
import midjourney_astronaut_params from '@site/docs/assets/midjourney_astronaut_params.png';
import midjourney_astronaut_multi1 from '@site/docs/assets/midjourney_astronaut_multi1.png';
import midjourney_astronaut_multi2 from '@site/docs/assets/midjourney_astronaut_multi2.png';
import midjourney_astronaut_ip2 from '@site/docs/assets/midjourney_astronaut_ip2.png';

import midjourney_astronaut_params_a12 from '@site/docs/assets/midjourney_astronaut_params_a12.png';
import midjourney_astronaut_params_a169 from '@site/docs/assets/midjourney_astronaut_params_a169.png';

import midjourney_astronaut_params_c20 from '@site/docs/assets/midjourney_astronaut_params_c20.png';
import midjourney_astronaut_params_c80 from '@site/docs/assets/midjourney_astronaut_params_c80.png';

import midjourney_astronaut_params_q05 from '@site/docs/assets/midjourney_astronaut_params_q05.png';
import midjourney_astronaut_params_q2 from '@site/docs/assets/midjourney_astronaut_params_q2.png';

import midjourney_astronaut_params_s50 from '@site/docs/assets/midjourney_astronaut_params_s50.png';
import midjourney_astronaut_params_s900 from '@site/docs/assets/midjourney_astronaut_params_s900.png';

import midjourney_astronaut_params_sameseed from '@site/docs/assets/midjourney_astronaut_params_sameseed.png';
import midjourney_astronaut_params_seed123 from '@site/docs/assets/midjourney_astronaut_params_seed123.png';

import midjourney_astronaut_params_tile from '@site/docs/assets/midjourney_astronaut_params_tile.png';
import midjourney_astronaut_params_tilegrid from '@site/docs/assets/midjourney_astronaut_params_tilegrid.png';
import midjourney_astronaut_params_tilecomplete from '@site/docs/assets/midjourney_astronaut_params_tilecomplete.jpeg';

import midjourney_astronaut_params_v1 from '@site/docs/assets/midjourney_astronaut_params_v1.png';
import midjourney_astronaut_params_v2 from '@site/docs/assets/midjourney_astronaut_params_v2.png';
import midjourney_astronaut_params_v3 from '@site/docs/assets/midjourney_astronaut_params_v3.png';



# 基本用法

在Midjourney中，基本的提示结构是`/imagine prompt: [IMAGE PROMPT] [--OPTIONAL PARAMETERS]`。

例如：`/imagine prompt: astronaut on a horse`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut} style={{width: "350px"}} />
</div>

带参数的示例：`/imagine prompt: astronaut on a horse --ar 3:2 --c 70 --q 2 --seed 1000`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params} style={{width: "350px"}} />
</div>

在这个基本的示例中，使用了以下参数：

`--ar 3:2`设置图像的长宽比为3:2。

`--c 70`加入一个混沌值70来让Midjourney更加自由地解释下达的提示（混沌值范围：0-100）。

`--seed 100`设置一个任意的种子值，可以用于稍后重新渲染或重新处理一张图片。

（了解有关Midjourney参数的更多信息，请参见“高级提示”>“参数”）

# 高级提示
在Midjourney中，高级提示利用了参数和Midjourney算法支持的特殊提示技术。

## 多提示
默认情况下，Midjourney会整体解释你的提示。使用双冒号`::`可以告诉Midjourney分别解释提示的每个部分。

示例：

```text
/imagine prompt: astronaut and horse
```
<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_multi1} style={{width: "350px"}} />
</div>

```text
/imagine prompt: astronaut:: and horse
```

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_multi2} style={{width: "350px"}} />
</div>


## 图像提示
通过将图像上传到Discord并在提示中使用其URL，您可以指示Midjourney使用该图像影响结果的内容、风格和构图。

示例：

[太空人（来源：维基百科）](https://en.wikipedia.org/wiki/Astronaut#/media/File:STS41B-35-1613_-_Bruce_McCandless_II_during_EVA_(Retouched).jpg)

```text
/imagine prompt: [image URL], impressionist painting
```

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_ip2} style={{width: "350px"}} />
</div>

## 参数（v4）

Midjourney的最新模型（v4）支持以下参数。

### 纵横比：

`--ar [比率]` 将默认比率（1:1）改为新比率（当前支持的最大比率为2:1）

示例：`astronaut on a horse --ar 16:9` 和 `astronaut on a horse --ar 1:2`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_a169} style={{width: "350px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_a12} style={{width: "175px"}} />
</div>


### 混沌：

`--c [值]` 设置混沌值，确定Midjourney在提示中变化的程度；混沌值越高，结果和组合就越不寻常和意外（范围：0-100）

示例：`astronaut on a horse --c20` 和 `astronaut on a horse --c 80`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_c20} style={{width: "350px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_c80} style={{width: "350px"}} />
</div>

### 质量：

`--q [值]` 定义生成图像所需的时间，从而增加质量。默认设置为“1”。较高的值使用更多的订阅GPU时间（接受值“.25”、“ .5”、“ 1”和“ 2”）

示例：`astronaut on a horse --q .5` 和 `astronaut on a horse --q 2`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_q05} style={{width: "350px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_q2} style={{width: "350px"}} />
</div>


### 种子：

`--seed [值]` 设置种子号，为图像生成的起点（噪声场）。未指定种子参数时，每个图像的种子数是随机生成的。使用相同的种子编号和提示将产生类似的图像。

示例：`astronaut on a horse --seed 123`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_seed123} style={{width: "350px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_seed123} style={{width: "350px"}} />
</div>

### 风格化：

`--stylize [值]` 或 `--s [值]` 影响Midjourney应用其艺术算法的强度。较低的值会产生与提示密切匹配的图像，较高的值会创建与提示不太相关的非常艺术化的图像。默认值为100，值范围为0-1000。（注：您可以使用“/settings”命令将默认的风格化值从“🖌️ Style Med”（=`--s 100`）更改为“🖌️ Style Low”（=`--s 50`）， “🖌️ Style High”（=`--s 250`）或“🖌️ Style Very High”（=`--s 750`））

示例：`astronaut on a horse --s 50` 和 `astronaut on a horse --s 900`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_s50} style={{width: "350px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_s900} style={{width: "350px"}} />
</div>

### 版本：

`--v [版本号]` 或 `--version [版本号]` 让您访问早期的Midjourney模型（1-3）

示例： `--v 1`，`--v 2` 和 `--v 3`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_v1} style={{width: "220px"}} />
  &nbsp;
   <img src={midjourney_astronaut_params_v2} style={{width: "220px"}} />
   &nbsp;
      <img src={midjourney_astronaut_params_v3} style={{width: "220px"}} />
</div>

## 参数（上一个模型）

### 相同的种子

`--sameseed`：`--seed`参数在初始网格中应用单个噪声场，而sameseed参数将相同的起始噪声应用于初始网格中的所有图像，因此会生成非常相似的图像。

例子：`astronaut on a horse --sameseed --v 3`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_sameseed} style={{width: "350px"}} />
</div>


### 瓷砖

`--tile` 生成可用作重复瓷砖以创建面料、壁纸和纹理无缝图案的图像（仅适用于模型1-3）。

例子：`astronaut on a horse --tile --v 3`

<div style={{textAlign: 'center'}}>
  <img src={midjourney_astronaut_params_tilegrid} style={{width: "220px"}} />
  &nbsp;
  <img src={midjourney_astronaut_params_tile} style={{width: "220px"}} />
  &nbsp;
  <img src={midjourney_astronaut_params_tilecomplete} style={{width: "220px"}} />
</div>


### 视频

`--video` 创建一个简短的影片，展示生成的图像网格。使用 ✉️ 表情符号，让Midjourney机器人向你发送具有视频链接的DM。

例子：`astronaut on a horse --video --v 3`

<div style={{textAlign: 'center'}}>
 <video width="320" height="240" autoplay muted>
  <source src="https://i.mj.run/27c89699-d96d-4834-b6fa-b022a453eb28/video.mp4" type="video/mp4">
</source>
</video>
</div>



## 链接

[Midjourney官方文档](https://docs.midjourney.com/)