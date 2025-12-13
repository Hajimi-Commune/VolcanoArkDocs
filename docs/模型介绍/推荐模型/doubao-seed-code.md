# doubao-seed-code
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>速度 </code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, Image, Video</div>

<div style="text-align: center">文本, 图片, 视频</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, <del>Image</del>, <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2000);margin-left: 16px;">

<div style="text-align: center"><code>价格(元/百万token）</code></div>

<div style="text-align: center">≥1.2, ≥8.0</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

doubao-seed-code是专为实际开发场景打造的AI Coding模型，强化了Bugfix能力和前端能力。支持输入透明Cache能力，降低使用成本。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

最大上下文长度：256k
最大输入长度：224k
最大思维链内容长度：32k
max_tokens：最大32k，默认4k（不包含思考内容）
max_completion_tokens：最大64k（包含思考内容）

</div>
</div>

---

## 模型价格

| | | | | | \
|条件 |\
|(千 token) |输入 |\
| |(元/百万 token) |输入命中缓存 |\
| | |(元/百万 token) |输出单价 |\
| | | |(元/百万 token) |缓存存储 |\
| | | | |(元/百万 token*小时) |
|---|---|---|---|---|
| | | | | | \
|输入长度 [0, 32] |1.20 |0.24 |8.00 |0.017 |
| | | | | | \
|输入长度 (32, 128] |1.40 |0.24 |12.00 |0.017 |
| | | | | | \
|输入长度 (128, 256] |2.80 |0.24 |16.00 |0.017 |

> 下面是计费项的简单说明，具体请参阅[模型服务价格](/docs/82379/1544106)。

> * 输入输出价位按照输入长度来定档，举例，在线推理场景，当输入长度为 16k ，则输入单价为1.2 元/百万 token，输出单价为8 元/百万 token。
> * 使用在线推理的上下文缓存能力，产生命中缓存的输入折后费用、创建的缓存存储费用。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [文本生成](/docs/82379/1399009)
* [函数调用 Function Calling](/docs/82379/1262342)
* [上下文缓存(Responses API)](/docs/82379/1602228)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [深度思考](/docs/82379/1449737)
* [图片理解](/docs/82379/1362931)

</div>
</div>

## 模型版本
doubao-seed-code

* doubao-seed-code-preview-251028：支持输入透明Cache能力，能够提供稳定高效的编程智能辅助，具备极强的前端页面开发能力。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center">TPM：1,200,000</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center">RPM：5,000</div>

</div>
</div>

## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);">

<div style="text-align: center"><a href="/docs/82379/1449737">深度思考</a></div>

<div style="text-align: center">深度思考能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);margin-left: 16px;">

<div style="text-align: center"><a href="/docs/82379/1362931">图片理解</a></div>

<div style="text-align: center">视觉理解能力使用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a></div>

<div style="text-align: center">Chat API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
### 深度思考开关
支持使用 **thinking** 参数控制模型是否开启深度思考模式。默认为`开启`状态。详细使用请参见 [开启关闭深度思考](/docs/82379/1449737#fa3f44fa)文档。
### 控制模型输出长度
支持 max_completion_tokens、max_tokens。
详细使用请参见 [设置模型输出长度限制](/docs/82379/1449737#31ecc4d7)。
### 上下文缓存

* Responses API：需要通过**caching**参数手动开启。
* Chat API：透明cache，无需手动开启。如有已缓存内容，自动命中。

## 接入方式
### 方舟 Coding Plan 接入方式
方舟 Coding Plan 是为广大开发者量身打造的AI Coding场景订阅服务，支持最新的Doubao-Seed-Code模型与多款主流 AI 编码工具，为开发者提供畅快、智能、稳定的编码体验，大幅提升代码编写效率与质量。

* 接入 Claude Code：[快速开始](/docs/82379/1928261)
* 接入其他编程工具：[接入AI编程工具](/docs/82379/1928262)

### API 按量付费接入方式
:::warning
以下接入方式将使用Tokens按量后付费方式计费，不会使用 Coding Plan 额度。如需接入 Coding plan，请详见[方舟 Coding Plan 接入方式](/docs/82379/1949118#6107f521)。
:::
#### Chat API
Base URL：`https://ark.cn-beijing.volces.com/api/v3`
API使用文档：[Chat API](https://www.volcengine.com/docs/82379/1494384)
#### Responses API
Base URL：`https://ark.cn-beijing.volces.com/api/v3`
API使用文档：[Responses API](https://www.volcengine.com/docs/82379/1585135)
#### Anthropic API
Base URL：`https://ark.cn-beijing.volces.com/api/compatible`
使用示例如下：
```Shell
curl https://ark.cn-beijing.volces.com/api/compatible/v1/messages \
  -H "Content-Type: application/json" \
  -H "x-api-key: $ARK_API_KEY" \
  -d '{
    "model": "doubao-seed-code-preview-251028",
    "system":"你是Doubao，我的编程助手。",
    "messages": [
        {
            "role": "user",
            "content": "Hello!"
        }
    ]
  }'
```