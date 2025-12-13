# doubao-seededit-3.0-i2i
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.17964435500037346);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.21546336439855196);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.20474416069535778);margin-left: 16px;">

<div style="text-align: center"><code>价格（元/张）</code></div>

<div style="text-align: center">0.30</div>

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
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

SeedEdit 3.0是一款图像编辑模型，支持通过文本指令编辑图像。SeedEdit 3.0 基于文生图模型 Seedream 3.0 训练，叠加多样化的数据融合方法与特定奖励模型，其图像主体、背景和细节保持能力进一步提升，尤其在人像编辑、背景更改、视角与光线转换等场景表现突出。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

生成图像格式：jpeg
IPM: 500

</div>
</div>

---

## 模型价格

SeedEdit 3.0 模型按张计费，单价为每张 0.30 元。
## 模型优势
SeedEdit 3.0模型在**精细且自然地处理编辑区域的同时，还能高保真地维持其他细节信息。​**尤其针对图像编辑“哪里改与哪里不改”的取舍，该模型表现出更佳的理解力和权衡力，可用率相应提高

| | | \
|Prompt |示例 |
|---|---|
| | | \
|【prompt】把场景变为白天 |\
| |\
|> 对于整个场景的光影变换，从近处房屋，到远处海水波纹，原图中的各种细节均被模型合理保留，并跟随光线变化，进行“像素级”的渲染调整 | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/070154104b744baa968fe73c1966b86b~tplv-goo7wpa0wc-image.image =612x) |\
| | |
| | | \
|【prompt】移除中间人物以外的所有行人 |\
| |\
|> 去掉图片内行人时，模型准确识别出场景内的各个人物，同时去掉了影子 | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e17abe123196405c88d82399df3ef707~tplv-goo7wpa0wc-image.image =612x) |\
| | |
| | | \
|【prompt】使女孩看起来逼真 |\
| |\
|> 在 2D 绘画转为真实模特的任务中，模型较好地保持了人物的衣帽穿搭与手提包等细节，生成图片兼具时尚 Lookbook 气息 |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e1374c5549004092895395b4993a1a00~tplv-goo7wpa0wc-image.image =662x) |

## 模型版本
doubao-seededit-3-0-i2i

* doubao-seededit-3-0-i2i-250628：根据您输入的文本描述和原始图片生成新的图片。

## 模型限流
模型的限流标准为每分钟生成 500 张图片。
## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center"><a href="/docs/82379/1824691">Seededit 3.0  教程</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1666946">图像编辑 API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>