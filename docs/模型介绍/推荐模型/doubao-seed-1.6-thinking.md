# doubao-seed-1.6-thinking
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

<div style="text-align: center">≥0.8, ≥8.0</div>

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

在思考能力上进行了大幅强化， 对比 doubao 1.5 代深度理解模型，在编程、数学、逻辑推理等基础能力上进一步提升， 支持视觉理解。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：256k
最大输入长度：224k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：

* 32k （`250715`版本）
* 16k （`250615`版本）

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
|输入长度 [0, 32] |0.80 |0.16 |8.00 |0.017 |0.40 |0.16 |4.00 |
| | | | | | | | | \
|输入长度 (32, 128] |1.20 |0.16 |16.00 |0.017 |0.60 |0.16 |8.00 |
| | | | | | | | | \
|输入长度 (128, 256] |2.40 |0.16 |24.00 |0.017 |1.20 |0.16 |12.00 |

> 下面是计费项的简单说明，具体请参阅[模型服务价格](/docs/82379/1544106)。

> * 输入输出价位按照输入长度来定档，举例，在线推理场景，当输入长度为 16k ，则输入单价为 0.8 元/百万 token，输出单价为 8 元/百万 token。
> * 使用在线推理的上下文缓存，产生命中缓存的输入折后费用、创建的缓存存储费用。
> * 使用批量推理，产生输入[批量]费用、命中透明缓存的输入折后费用、输出[批量]费用。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [Function Calling](https://www.volcengine.com/docs/82379/1262342)
* [视觉理解](/docs/82379/1362931)
* [Responses API 教程](/docs/82379/1585128)
   * beta版，邀测中

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [深度思考](/docs/82379/1449737)
* [结构化输出](/docs/82379/1568221)
* [批量推理](/docs/82379/1399517)
* [上下文缓存(Responses API)](/docs/82379/1602228)

</div>
</div>

## 模型版本
doubao-seed-1.6-thinking

* doubao-seed-1-6-thinking-250715
   强制开启深度思考，不可关闭。
   模型最大回答长度 16k 升级至 32k。
* doubao-seed-1-6-thinking-250615
   强制开启深度思考，不可关闭。

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

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1569618">Responses API</a></div>

<div style="text-align: center">Responses API参数说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.25);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat) API</a></div>

<div style="text-align: center">Chat API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
### 控制模型输出长度（支持 max_completion_tokens ）
`doubao-seed-1-6-thinking-250715` 支持通过 max_completion_tokens 字段控制模型输出长度（最大至64k）。
详细使用请参见 [设置模型输出长度限制](/docs/82379/1449737#31ecc4d7)。