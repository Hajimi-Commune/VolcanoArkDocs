# doubao-1.5-vision-pro
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.15156250000000002);">

<div style="text-align: center"><code>智力程度</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1546875);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.29843749999999997);margin-left: 16px;">

<div style="text-align: center"><code>价格（元/百万token）</code></div>

<div style="text-align: center">3, 9</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, Image ,Video,  <del>Audio</del></div>

<div style="text-align: center">文本，图像，视频</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1953125);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text,</div>

<div style="text-align: center"><del>Image,Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

doubao-1.5-vision-pro，全新升级的多模态大模型，支持任意分辨率和极端长宽比图像识别，增强视觉推理、文档识别、细节信息理解和指令遵循能力。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：16k
默认最大回答长度：4k

</div>
</div>

---

## 模型价格
`元/百万 token`

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14590787119856885);">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">3.00</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15230769230769237);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">9.00</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15384615384615385);margin-left: 16px;">

<div style="text-align: center"><code>缓存命中</code></div>

<div style="text-align: center">不涉及</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.24512820512820513);margin-left: 16px;">

<div style="text-align: center"><code>缓存存储[每小时]</code></div>

<div style="text-align: center">不涉及</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.1538461538461538);margin-left: 16px;">

<div style="text-align: center"><code>输入[批量]</code></div>

<div style="text-align: center">1.50</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14896392367322614);margin-left: 16px;">

<div style="text-align: center"><code>输出[批量]</code></div>

<div style="text-align: center">4.50</div>

</div>
</div>

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [Function Calling](https://www.volcengine.com/docs/82379/1262342)
   * 仅`doubao-1-5-vision-pro-32k-250115`支持
* [结构化输出](/docs/82379/1568221)
   * `doubao-1-5-vision-pro-250328`版本支持 `json_object`模式

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [批量推理](/docs/82379/1305505)
* [深度思考](/docs/82379/1449737)

</div>
</div>

## 模型版本
doubao-1.5-vision-pro

* doubao-1-5-vision-pro-250328
   支持 文本+图片+视频 输入，可通过 [System Prompt](https://www.volcengine.com/docs/82379/1362931#%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5-%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83%E6%A8%A1%E5%BC%8F) 强制进行深度思考，输出思维链内容。
* doubao-1-5-vision-pro-32k-250115
   支持文本+图片输入，最大上下文长度为32k。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center">TPM：5,000,000</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center">RPM：30,000</div>

</div>
</div>

## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center"><a href="/docs/82379/1362931">视觉理解</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat) API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
doubao-1-5-vision-pro-250328 模型，支持使用以下System Prompt，开启深度思考模型，并获得最佳效果。
```Plain Text
You should first think about the reasoning process in the mind and then provide the user with the answer. The reasoning process is enclosed within <think> </think> tags, i.e. <think> reasoning process here </think>here answer
```

下面是简单回复示例。

**模型回复说明**

通过System Prompt开启思考模型时，思考内容不会通过回复中的 **reasoning_content** 字段输出。思考内容与回答内容，均通过回复中的 **content** 字段输出。
例如：
```Plain Text
<think>
思考内容
</think>
回答内容
```

**模型回复示例**

```Plain
Text
<think>
好，现在用户需详细描述四张图片并分类。先看第一张：广阔草原，远景山脉森林，中景草地颜色渐变，近景小黑点（可能动物或人）。自然景观，侧重地形植被——判断为自然风景类。

第二张：猴子特写，手持食物，周围绿叶红花。主体动物与植物互动，环境有蓝天。虽有植物但主体是动物行为——动物类。

第三张：松鼠在树枝，啃食东西，茂密树叶。主体野生动物栖息状态，自然环境但聚焦动物——动物类。

第四张：狗在砖墙，建筑遗迹背景。动物（狗）与人工建筑（砖墙）结合，但主体是动物状态？但砖墙属人文元素。不过分类可能看主体：若动物为主则动物类，若强调建筑与动物互动？但通常第四张狗是主体，砖墙背景。但再检查：

分类逻辑：自然风景（无动物主体，纯景观）、动物（主体动物）、人文？但第四张砖墙是人造结构，但图片主体是狗。看示例分类：比如第一张纯风景，后三张主体动物。但第四张狗在古迹？不过用户给的例子可能分自然风景、动物。检查第四张描述：狗躺砖墙，砖墙是背景，主体动物。所以分类：

图1：自然风景（草原山脉）

图2、3、4：动物（猴子、松鼠、狗）

描述时详细刻画元素：图1的色彩层次（黄绿草地、深绿森林、山脉轮廓），图2猴子动作（手持食物、树叶花朵衬托），图3松鼠姿态（树枝位置、啃食细节），图4狗的表情（放松）与砖墙质感（古老）。
</think>
### 图片1：草原山脉景观  
**描述**：画面展现开阔的自然景观。前景是大片**黄绿渐变的草地**，中景有几处小黑点（可能是动物或行人），中远景是**起伏的山脉**，山坡覆盖着茂密的**深绿色森林**，与浅绿色草坡形成层次。天空清澈，点缀几缕白云。整体氛围宁静，展现了**草原、森林、山脉**的自然壮美。  
**分类**：**自然风景类**（无动物主体，以地形、植被、天空构成纯自然景观）。

### 图片2：猴子进食  
**描述**：主体是一只**猴子**，坐在木质结构上，双手捧食物（可能是水果）。猴子毛发呈灰褐色，面部细节清晰。周围有**绿色树叶**和**鲜艳的红色花朵**（如三角梅），背景是淡蓝色天空。画面聚焦动物行为，通过植物衬托自然生境。  
**分类**：**动物类**（主体为野生动物，展现其觅食行为）。

### 图片3：松鼠栖息  
**描述**：一只**松鼠**倒挂在树枝上，啃食手中的食物。松鼠毛色棕黑相间，尾巴蓬松。背景是**茂密的绿叶**，树干纹理粗糙，光线明亮。画面捕捉动物的动态瞬间，突出其与树木（栖息地）的关系。  
**分类**：**动物类**（主体为野生动物，展现其栖息习性）。

### 图片4：犬类休憩  
**描述**：一只**黑白相间的狗**趴在**古老的砖墙**上，砖墙由红褐色石块堆砌，表面斑驳（可能是历史遗迹）。狗神态放松，张嘴露舌，背景简洁。画面结合**动物**与**人造建筑**，但主体是动物的状态。  
**分类**：**动物类**（尽管背景含人文元素，核心主体是动物）。

### 总结分类  
- **自然风景**：图1（纯自然地貌，无动物主体）。  
- **动物类**：图2（猴子）、图3（松鼠）、图4（狗）——均以动物为视觉焦点，展现其行为或状态。  

（若严格区分“动物 vs. 动物+人文”，图4可商榷，但通常摄影分类中“动物类”允许背景含环境元素，除非建筑占绝对主导。此组图中动物仍是图4的视觉中心。）
```