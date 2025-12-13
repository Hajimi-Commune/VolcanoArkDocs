# doubao-seedream-4.0
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.17964435500037346);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.21546336439855196);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.20474416069535778);margin-left: 16px;">

<div style="text-align: center"><code>价格（元/张）</code></div>

<div style="text-align: center">0.20 </div>

<div style="text-align: center">[输出]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1958789124684074);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center">Image , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本，图片</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.20426920743730936);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center"><del>Text</del>, </div>

<div style="text-align: center">Image , <del>Video</del>, <del>Audio</del> </div>

<div style="text-align: center">图片</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5285167567144311);">

基于领先架构的SOTA级多模态图像创作模型。其打破传统文生图模型的创作边界，原生支持文本、单图和多图输入，用户可自由融合文本与图像，在同一模型下实现基于主体一致性的多图融合创作、图像编辑、组图生成等多样玩法，让图像创作更加自由可控。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.47148324328556895);margin-left: 16px;">

输出图片分辨率：1k ~ 4k
输出图片总像素：[`1280x720`, `4096x4096`] 
输出图片格式：jpeg 
IPM: 500
支持：多参考图生图、组图生成

</div>
</div>

---

## 模型价格

按实际生成的图片张数计费，生成失败不收费。
 0.20 元/张
## 模型能力
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ab1dda5637654c8aa31fb00a9255da60~tplv-goo7wpa0wc-image.image =864x)
## 效果预览
### 多图融合

| | | \
|输入 |输出 |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9b4bf5fb97d543a5ab56fc987ea3b737~tplv-goo7wpa0wc-image.image =620x) |\
|> 将图1的服装换为图2的服装 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/88a0148cf9224b34b2193cf84043e290~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dab8181a071345d9bf6dd1e09dad2e60~tplv-goo7wpa0wc-image.image =620x) |\
|> 图1为画面背景，图2中狮子趴在图3人物旁边，图3人物蹲在海边研究图4中的箱子，巧妙地将4张图片合成至一张图片，要求画风一致，画面协调 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6bfab19c88d345f8ad6ae72f5af33ba5~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 组图生成
> 基于用户输入的文字和图片，生成一组内容关联的图像

| | | \
|输入 |输出 |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8fc8981c61674b3083bedb4c78170ea5~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、纸盒、卡片、手环、挂绳等。绿色视觉主色调，趣味、简约现代风格 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9366dadd2d6541d4be9db553c7a19e31~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2c10c6e61b5e40d1861d7dd28d773f47~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考图1，生成四图片，分别为春夏秋冬四个场景的图片 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2ee0523d151e445cb56ca729ad8bb72e~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 图像元素增删改

| | | \
|输入 |输出 |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a730b347d1ed41ae9ef4debbb591ed38~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考这张图，去掉图中的老年人和他的影子 |\
| |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1e4585967382462883bc56194875a7c6~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/00fea688b21d428f9c6614e3b7efdfce~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考这张图片，保持画面风格，将图中的龙变为河马 |\
| |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/257f3196e9bf45e8a2a99dac5ad15324~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 风格迁移

| | | \
|输入 |输出 |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7be30d94149543f09eda3e874f9f267a~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考这张图片，保持画面内容不变，将图像风格变为**动漫风格** |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8fc263f1daea459e9691aea00fb5452a~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7be30d94149543f09eda3e874f9f267a~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考这张图片，保持画面内容不变，将图像风格变为**迪士尼3D卡通风格** |\
| |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6ee62e2efa6249a99fff253e9ce3c96f~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 主体特征保持
> 不同的创作形态下，均能高质量保持主体核心特征的一致性

| | | \
|输入 |输出 |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/171725c2dd9043b4bb431795c4c05be9~tplv-goo7wpa0wc-image.image =620x) |\
|> 生成狗狗趴在草地上的近景画面 |\
| |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7a16097a185043249dc9171cffdc65f1~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/44148b0be6c04341a65b65fcbd7fdd74~tplv-goo7wpa0wc-image.image =620x) |\
|> 将平视视角改为俯视角，将近景改为中景 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cc4ce38d78d64321a999dc9e9e1dbf74~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ac23a2396db14f728b5a9bd78f41a60a~tplv-goo7wpa0wc-image.image =620x) |\
|> 用图中的形象生成帆布包 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dc8a4d661d63425c80f46d5164a10fb5~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 画面特征保持
> 在图像编辑场景，最大化保留原图细节，无需担心编辑后带来画面“AI油腻感”，实现无损编辑

| | | \
|输入 |\
| |输出 |\
| | |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2a5ae02587a473390fe4e893d001879~tplv-goo7wpa0wc-image.image =620x) |\
|> 优化图中男人的面部肤质,使面部肤质更细腻平滑且自然,保留毛孔以及纹理细节 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3cd4a193d3794662b0a8d13c4e2c170d~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/31abe0aec1c841a8bf234057af7a8699~tplv-goo7wpa0wc-image.image =621x) |\
|> 精修一下，修复褶皱，调整光影，高清商业，产品摄影 |\
| |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96d21e80856d4fea88fcb1c9eec89937~tplv-goo7wpa0wc-image.image =621x) |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e759cdc744744e5182734fa04a31bfa6~tplv-goo7wpa0wc-image.image =620x) |\
|> 把图片文字换成创意字体 | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6223a92a30484ea6bcad3126fec61313~tplv-goo7wpa0wc-image.image =620x) |\
| | |

### 灵感成形
> 将模糊的构想变为具象的画面，让“天马行空”的灵感变为现实

| | | \
|输入 |\
| |输出 |\
| | |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e31e7ba988cf4cd09f50716fd405ee8c~tplv-goo7wpa0wc-image.image =620x) |\
|> 转化为真人形象，帅气神秘 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/77f0b01eabe64b1ea5b76568836d0ab9~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/49bd5a04da3247afaed82b8483763939~tplv-goo7wpa0wc-image.image =620x) |\
|> 参考线稿图，生成一台老式电视机 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1cb90396bdd748d2bcd7d4bf8c8a23fd~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |

### 推理预测
> 具备更强大的推演能力，能够跨越时空预测模拟，化“未见”为“可见”

| | | \
|输入 |\
| |输出 |\
| | |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3b3c287425584685aef063b77d1825cd~tplv-goo7wpa0wc-image.image =620x) |\
|> 1分钟后这个显示应该是多少 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1080a2160b9e450898afd207d4ab8531~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/abd79a5aca81460993ba73dfff068842~tplv-goo7wpa0wc-image.image =620x) |\
|> 变成一个女人 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ea5de801ec4343848e814765eed18ae9~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |

### 智能比例
> 开启后，为你的画面智能匹配最佳比例尺寸

| | | \
|输入 |\
| |输出 |\
| | |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b7385c0767e34a80a3c5f1993b159d12~tplv-goo7wpa0wc-image.image =620x) |\
|> 将画面比例调整为16:9 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/15d25be98fca4c25826373ad065f5e22~tplv-goo7wpa0wc-image.image =620x) |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ae0f7c2b56274d9d8614440572e0b372~tplv-goo7wpa0wc-image.image =620x) |\
|> 变成一张海报 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0fc3c113de224745aefee8785f368e4d~tplv-goo7wpa0wc-image.image =620x) |\
| | |\
| | |

## 模型版本
**doubao-seedream-4-0-250828**

* 生成组图（组图：基于您输入的内容，生成的一组内容关联的图片；需配置**sequential_image_generation**为auto**）**
   * 多图生组图，根据您输入的 ++多张参考图片（2-10）+文本提示词++ 生成一组内容关联的图片（输入的参考图数量+最终生成的图片数量≤15张）。
   * 单图生组图，根据您输入的 ++单张参考图片+文本提示词++ 生成一组内容关联的图片（最多生成14张图片）。
   * 文生组图，根据您输入的 ++文本提示词++ 生成一组内容关联的图片（最多生成15张图片）。
* 生成单图（配置**sequential_image_generation**为disabled**）**
   * 多图生图，根据您输入的 ++多张参考图片（2-10）+文本提示词++ 生成单张图片。
   * 单图生图，根据您输入的 ++单张参考图片+文本提示词++ 生成单张图片。
   * 文生图，根据您输入的 ++文本提示词++ 生成单张图片。

## 模型限流
> IPM（Images Per Minute， 每分钟生成的图片数量）

模型的限流标准为每分钟生成500张图片。
## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1548482">Seedream 4.0 教程</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1666945">图片生成 API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1829186">Seedream4.0 提示词指南</a></div>

<div style="text-align: center">图片生成的提示词（prompt）使用技巧，帮助您快速上手图片创作，将创意转化为图片内容。</div>

</div>
</div>