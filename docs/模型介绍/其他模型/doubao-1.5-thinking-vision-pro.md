# doubao-1.5-thinking-vision-pro
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.15156250000000002);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1546875);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.29843749999999997);margin-left: 16px;">

<div style="text-align: center"><code>价格(元/百万token）</code></div>

<div style="text-align: center">3, 9</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, Image , Video, <del>Audio</del></div>

<div style="text-align: center">文本, 图像, 视频</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1953125);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, <del>Image</del>, <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

doubao-1-5-thinking-vision-pro 全新视觉深度思考模型，具备更强的通用多模态理解和推理能力，在 59 个公开评测基准中的 37 个上取得 SOTA 表现。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：16k
默认最大回答长度：4k
> [附-模型输入输出长度限制说明](/docs/82379/1449737#4adf109b)

</div>
</div>

---

# 模型价格

`元/百万 token`

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14590787119856885);">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">3</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15230769230769237);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">9</div>

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

<div style="text-align: center">1.5</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14896392367322614);margin-left: 16px;">

<div style="text-align: center"><code>输出[批量]</code></div>

<div style="text-align: center">4.5</div>

</div>
</div>

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [Function Calling](https://www.volcengine.com/docs/82379/1262342)
* [视觉理解](/docs/82379/1362931)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [深度思考](/docs/82379/1449737)
* [批量推理](/docs/82379/1305505)
* [结构化输出](/docs/82379/1568221)

</div>
</div>

## 模型版本
doubao-1.5-thinking-vision-pro

* doubao-1-5-thinking-vision-pro-250428
   支持文本、图片、视频输入，可以通过 **thinking** 参数控制是否关闭深度思考。

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
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.33333333333333337);">

<div style="text-align: center"><a href="/docs/82379/1449737">深度思考</a></div>

<div style="text-align: center">深度思考能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);margin-left: 16px;">

<div style="text-align: center"><a href="/docs/82379/1362931">视觉理解</a></div>

<div style="text-align: center">视觉理解能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.33333333333333337);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat) API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
### 深度思考模式开关
使用 **thinking** 参数控制模型是否开启深度思考模式。默认为`开启`状态。详细使用请参见 [开启关闭深度思考](/docs/82379/1449737#fa3f44fa)文档。