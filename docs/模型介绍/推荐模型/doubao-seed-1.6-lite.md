# doubao-seed-1.6-lite
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);margin-left: 16px;">

<div style="text-align: center"><code>速度 </code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, Image, Video,   <del>Audio</del></div>

<div style="text-align: center">文本, 图像, 视频</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, <del>Image</del>, <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>价格(元/百万token）</code></div>

<div style="text-align: center">≥0.8, ≥2.0</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

具有更高性价比的多模态深度思考模型，是常见任务的最佳选择。其效果接近doubao-seed-1-6-250615，在非思考模式下优于doubao-1-5-pro-32k-250115。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

最大上下文长度：256k
最大输入长度：224k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：32k
默认最大回答长度：4k
> [附-模型输入输出长度限制说明](/docs/82379/1449737#4adf109b)

</div>
</div>

---

## 模型价格

| | | | | | | | | \
|条件 |\
|(千 token) |输入 |\
| |(元/百万 token) |输入命中缓存 |\
| | |(元/百万 token) |输出单价 |\
| | | |(元/百万 token) |缓存存储 |\
| | | | |(元/百万 token*小时) |输入单价[批量] |\
| | | | | |(元/百万 token) |输入命中缓存单价[批量] |\
| | | | | | |(元/百万 token) |输出单价[批量] |\
| | | | | | | |(元/百万 token) |
|---|---|---|---|---|---|---|---|
| | | | | | | | | \
|输入长度 [0, 32] |\
|且输出长度 [0, 0.2] |0.30 |0.06 |0.60 |0.017 |0.15 |0.06 |0.30 |
| | | | | | | | | \
|输入长度 [0, 32] |\
|且输出长度 (0.2,+∞) |0.30 |0.06 |2.40 |0.017 |0.15 |0.06 |1.20 |
| | | | | | | | | \
|输入长度 (32, 128] |0.60 |0.06 |4.00 |0.017 |0.30 |0.06 |2.00 |
| | | | | | | | | \
|输入长度 (128, 256] |1.20 |0.06 |12.00 |0.017 |0.60 |0.06 |6.00 |

> 下面是计费项的简单说明，具体请参阅[模型服务价格](/docs/82379/1544106)。

> * 输入输出价位按照输入长度来定档，举例，在线推理场景，当输入长度为 16k ，则输入单价为0.3 元/百万 token，输出单价为2.4 元/百万 token。
> * 使用在线推理的上下文缓存能力，产生命中缓存的输入折后费用、创建的缓存存储费用。
> * 使用批量推理，产生输入[批量]费用、命中透明缓存的输入折后费用、输出[批量]费用。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [文本生成](/docs/82379/1399009)
* [函数调用 Function Calling](/docs/82379/1262342)
* [批量推理](/docs/82379/1399517)
* [Responses API 教程](/docs/82379/1585128)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [深度思考](/docs/82379/1449737)
* [视觉理解](/docs/82379/1362931)
* [结构化输出](/docs/82379/1568221)
* [上下文缓存(Responses API)](/docs/82379/1602228)

</div>
</div>

## 模型版本
doubao-seed-1.6-lite

* doubao-seed-1-6-lite-251015：支持调节思考长度，即支持API中的**reasoning_effort** 字段，分为minimal、low、medium、high四种模式。

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
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.25);">

<div style="text-align: center"><a href="/docs/82379/1449737">深度思考</a></div>

<div style="text-align: center">深度思考能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.25);margin-left: 16px;">

<div style="text-align: center"><a href="/docs/82379/1362931">视觉理解</a></div>

<div style="text-align: center">视觉理解能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.25);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat) API</a></div>

<div style="text-align: center">Chat API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.25);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1569618">Responses API</a></div>

<div style="text-align: center">Responses API参数说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
### 深度思考开关
支持使用 **thinking** 参数控制模型是否开启深度思考模式。默认为`开启`状态。详细使用请参见 [开启关闭深度思考](/docs/82379/1449737#fa3f44fa)文档。
### 控制模型输出长度（支持 max_completion_tokens ）
详细使用请参见 [设置模型输出长度限制](/docs/82379/1449737#31ecc4d7)。