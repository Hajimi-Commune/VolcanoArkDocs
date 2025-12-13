# deepseek-v3
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1681109185441941);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.17157712305025996);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.22183708838821492);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.22183708838821486);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del> , <del>Audio</del>文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.21663778162911612);margin-left: 16px;">

<div style="text-align: center"><code>价格(</code><code>元/百万 token</code><code>）</code></div>

<div style="text-align: center">2.0, 8.0</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

**deepseek-v3** 由深度求索公司自研的MoE模型，多项评测成绩超越了 qwen2.5-72b 和 llama-3.1-405b 等开源模型，并在性能上和世界顶尖的闭源模型 gpt-4o 及 claude-3.5-Sonnet 不分伯仲。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：128k
最大思维链内容长度：不涉及
可[设置最大回答长度](/docs/82379/1399009#3821b26a)：16k
默认最大回答长度：4k

</div>
</div>

---

## 模型价格

| | | | | | | | \
|输入 |\
|元/百万 token |输入命中缓存 |\
| |元/百万 token |输出单价 |\
| | |元/百万 token |缓存存储 |\
| | | |元/百万 token/小时 |输入单价[批量] |\
| | | | |元/百万 token |输入命中缓存单价[批量] |\
| | | | | |元/百万 token |输出单价[批量] |\
| | | | | | |元/百万 token |
|---|---|---|---|---|---|---|
| | | | | | | | \
|2.00 |0.40 |8.00 |0.017 |1.00 |0.40 |4.00 |

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [文本生成](/docs/82379/1399009)
* [批量推理](/docs/82379/1399517)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [函数调用 Function Calling](/docs/82379/1262342)
* [上下文缓存(Context API) ](/docs/82379/1396491)
   * 前缀缓存

</div>
</div>

## 模型版本
deepseek-v3

* deepseek-v3-250324：新支持前缀缓存。

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

<div style="text-align: center"><a href="/docs/82379/1399009">文本生成</a></div>

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
方舟 deepseek-v3-250324 模型 **temperature** 字段对齐DeepSeek官方的[处理逻辑](https://huggingface.co/deepseek-ai/DeepSeek-V3-0324#temperature)。
> 举例：您在请求中设置**temperature**为`1`，则在模型侧会映射 **temperature** 值为`0.3`。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a75f81b2e4034bd3bfafa8156eea331b~tplv-goo7wpa0wc-image.image =611x)