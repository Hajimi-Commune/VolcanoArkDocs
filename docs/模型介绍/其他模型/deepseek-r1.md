# deepseek-r1
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1619365609348915);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1619365609348915);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.23038397328881471);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del> , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.21368948247078462);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del> , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2320534223706177);margin-left: 16px;">

<div style="text-align: center"><code>价格（</code><code>元/百万 token</code><code>）</code></div>

<div style="text-align: center">4, 16</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.631430584918957);">

**deepseek-r1** 是由深度求索推出的深度思考模型。在后训练阶段大规模使用了强化学习技术，在仅有极少标注数据的情况下，极大提升了模型推理能力。在数学、代码、自然语言推理等任务上，性能比肩 OpenAI o1 正式版。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.3685694150810431);margin-left: 16px;">

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
可[设置最大回答长度](/docs/82379/1399009#3821b26a)：32k
默认最大回答长度：4k

</div>
</div>

---

## 模型价格

| | | | | | | | \
|输入单价 |\
|元/百万 token |输入命中缓存 |\
| |元/百万 token |输出单价 |\
| | |元/百万 token |缓存存储 |\
| | | |元/百万 token/小时 |输入单价[批量] |\
| | | | |元/百万 token |输入命中缓存单价[批量] |\
| | | | | |元/百万 token |输出单价[批量] |\
| | | | | | |元/百万 token |
|---|---|---|---|---|---|---|
| | | | | | | | \
|4.00 |0.80 |16.00 |0.017 |2.00 |0.80 |8.00 |

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [文本生成](/docs/82379/1399009)
* [批量推理](/docs/82379/1399517)
* [结构化输出(beta)](/docs/82379/1568221)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [函数调用 Function Calling](/docs/82379/1262342)
* [上下文缓存(Context API) ](/docs/82379/1396491)
   * 支持前缀缓存

</div>
</div>

## 模型版本
**deepseek-r1**

* deepseek-r1-250528：新支持结构化输出；支持`max_completion_tokens`字段，输出超长内容。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center">TPM：5,000,000</div>

<div style="text-align: center">TPM：800,000</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center">RPM：30,000</div>

<div style="text-align: center">RPM：15,000</div>

</div>
</div>

## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center"><a href="/docs/82379/1449737">深度思考</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a></div>

<div style="text-align: center">模型调用API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 其他说明
### **不支持的参数**
下面介绍模型对于API参数的支持情况

| | | | \
|字段 |类型 |传入后行为 |
|---|---|---|
| | | | \
|stop |String or Array |不支持，忽略不报错。 |
| | | | \
|frequency_penalty |Float |不支持，忽略不报错。 |
| | | | \
|presence_penalty |Float |不支持，忽略不报错。 |
| | | | \
|temperature |Float |不支持，忽略不报错。 |
| | | | \
|top_p |Float |不支持，忽略不报错。 |
| | | | \
|logprobs |Boolean |不支持，报错。 |
| | | | \
|top_logprobs |Integer |不支持，报错。 |
| | | | \
|logit_bias |Object |不支持，报错。 |
| | | | \
|thinking |Object |不支持，报错。 |
| | | | \
|response_format |Object |暂不支持，报错。 |

### 控制模型输出长度
详细使用请参见 [设置模型输出长度限制](/docs/82379/1449737#31ecc4d7)。