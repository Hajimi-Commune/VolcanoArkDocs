# doubao-seed3d-1.0
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.18050067922825352);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.21649042978646604);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1968126231796466);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center"><del>Text</del>, </div>

<div style="text-align: center">Image , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">图片</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.20095335500843128);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center"><del>Text</del>, </div>

<div style="text-align: center"><del>Image</del>, <del>Audio</del>, <del>Video</del>, Three-Dimensional</div>

<div style="text-align: center">3D</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2052429127972026);margin-left: 16px;">

<div style="text-align: center"><code>价格（元/百万token）</code></div>

<div style="text-align: center">80</div>

<div style="text-align: center">[输出]</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

基于 Diffusion Transformer（DiT）架构训练的 3D生成模型，能够在几分钟内输出包含多边形面片与 PBR 材质的高精度资产，其生成结果具备边缘锐利清晰、薄面结构稳定不变形的特征，所有几何细节在 6K 分辨率下仍保持清晰可见，能够满足不同领域对高精度 3D资产的需求。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

多边形面的数量：30000面、100000 面、200000面
3D文件格式：glb 、obj、usd、usdz

</div>
</div>

---

## 模型价格

按输出token数计费，每生成一个3D模型消耗的 token 为固定值 30000，价格为 2.4 元。详见[3D生成模型](/docs/82379/1544106#59e650ae)。

## 效果预览
### **超丰富的几何细节和鲜明的硬边**
模型的线条和转角均非常清晰，通过实体多边形来描述细节，而非仅依赖纹理。

| | | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b318a8efdee44c6f88a931df89d624b8~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/64b5b7b80be6430d82eec7be5722c061~tplv-goo7wpa0wc-image.image =1024x) |\
| | | |\
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8679043017dd42c28f62aef2468948d5~tplv-goo7wpa0wc-image.image =1024x) |\
| | | |
|---|---|---|

### **PBR 材质系统**
真实还原如皮革、抛光金属等各种材质质感。

| | | \
|输入 |输出效果展示 |
|---|---|
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f4ba4e0d85924817bb9fa750bb91d2ad~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/59900e1f75b242ca833967ebe22240e7~tplv-goo7wpa0wc-image.image =710x) |\
| | |\
| | |\
| | |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/573ea71bf31b45059ac8f3c460b6b6b1~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c762d983fde24db98bbc9d6419e34527~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/00fd97258c22480494459328e0884b77~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/692fdc514a2e490294b8a2656f37f40d~tplv-goo7wpa0wc-image.image =1024x) |\
| | |

### **6K 纹理贴图**
放大十倍，毛孔、织物经纬纹等细节依旧清晰。

| | | \
|输入 |输出效果展示 |
|---|---|
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2d0b461713854d738b4b023a79e53647~tplv-goo7wpa0wc-image.image =1024x) |\
| |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/41af415165b24c86bc7a04b5c44eefdf~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/078de189655e40c989543d829d421e2d~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2e27978997d04e52a51ccf27beb6c25e~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/978bf94103fa4382baf6ab9ca3afb761~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6ff504b34f394404bcd581dd7f48a64b~tplv-goo7wpa0wc-image.image =1024x) |\
| | |

### **精准文字符号表现**
2D 标记在 3D 世界中精准呈现，毫无偏差。

| | | \
|输入 |输出效果展示 |
|---|---|
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b2efda943a42474c869388c542c63453~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cbd1960a269d42e69ac766d3efc529ec~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f20cd56e0e5847fe942b98825364e0ce~tplv-goo7wpa0wc-image.image =640x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c268367f00f04929b7ecc4d551166a6b~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/713fd5198a674a3ab71b2ef2973bc0b7~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/47e55a64dbca496a97ab3735f4bcd144~tplv-goo7wpa0wc-image.image =1024x) |\
| | |

### **深度适配具身智能**
支持 usdz 等格式，适配具身智能场景，从 Omniverse 到 Unity 可直接使用，无需适配。

| | | \
|输入 |输出效果展示 |
|---|---|
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2e4371617ba249dfabd42587b6ac38f7~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c29b1f69c7fa4e4cae840e5c5b787b77~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/735e1603a90b40779a63d18dda88e600~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/952cac75dc43448499e6339c28eb2bfd~tplv-goo7wpa0wc-image.image =1024x) |\
| | |
| | | \
| |\
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b7b7c0ab0e9848d8b0ab6ec2525ec5fe~tplv-goo7wpa0wc-image.image =1024x) |\
| | |\
| |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4faa6ec41a0f488cb55449a89f2163b4~tplv-goo7wpa0wc-image.image =1024x) |\
| | |

## 模型版本
**doubao-seed3d-1-0-250928**

* 根据您输入的 ++图片（1张）+ 参数（可选）++ 生成一个带纹理和 PBR 材质的3D文件。

## 模型限流

* RPM 限流：账号下同模型（区分模型版本）每分钟允许创建的任务数量上限。若超过该限制，创建3D生成任务时会报错。
* 并发数限制：账号下同模型（区分模型版本）同一时刻在处理中的任务数量上限，超过此限制的任务将进入队列等待处理。
* 不同模型版本的限制值不同，详见[3D生成能力](/docs/82379/1330310#ddefa422)。

## 使用文档
> 3D生成为异步接口，您需要先创建3D生成任务，再通过3D生成任务的 ID 去查询3D生成结果。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.49992500374981247);">

<div style="text-align: center"><a href="/docs/82379/1874993">3D生成</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.49992500374981247);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1856268">3D生成 API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>