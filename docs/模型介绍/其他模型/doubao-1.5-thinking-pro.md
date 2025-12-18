# doubao-1.5-thinking-pro

<code>模型效果</code>

★★★★★

<code>速度</code>

★★

<code>价格（元/百万token）</code>

4, 16

[输入], [输出]

<code>输入</code>

Text, 

Image , <del>Video</del>, <del>Audio</del>

文本，图像

<code>输出</code>

Text, 

<del>Image</del> ,<del>Video, </del> <del>Audio</del>

文本

---

doubao-1.5全新深度思考模型，在数学、编程、科学推理等专业领域及创意写作等通用任务中表现突出，在AIME 2024、Codeforces、GPQA等多项权威基准上达到或接近业界第一梯队水平。

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#%E8%AE%BE%E7%BD%AE%E6%A8%A1%E5%9E%8B%E6%9C%80%E5%A4%A7%E8%BE%93%E5%87%BA%E9%95%BF%E5%BA%A6)最大回答长度：16k
默认最大回答长度：4k
> [附-模型输入输出长度限制说明](/docs/82379/1449737#4adf109b)

---

## 模型价格

`元/百万 token`

<code>输入</code>

4.00

<code>输出</code>

16.00

<code>缓存命中</code>

不涉及

<code>缓存存储[每小时]</code>

不涉及

<code>输入[批量]</code>

2.00

<code>输出[批量]</code>

8.00

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [批量推理](/docs/82379/1399517)

*  [工具调用 Function Calling](/docs/82379/1262342)
* [结构化输出](/docs/82379/1568221)
   * `doubao-1-5-thinking-pro-250415` 版本支持

## 模型版本
doubao-1.5-thinking-pro

* doubao-1-5-thinking-pro-m-250428
   支持文本+图片输入，详细见[深度思考-视觉理解示例](https://www.volcengine.com/docs/82379/1449737#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3)。
* doubao-1-5-thinking-pro-250415
   只支持文本输入。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="/docs/82379/1449737">深度思考</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1494384">对话（chat） API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

## 使用说明
### 深度思考开关（支持auto模型）
`doubao-1-5-thinking-pro-m-250428`支持使用 **thinking** 参数控制模型是否开启深度思考模式。默认为`开启`状态。
详细使用请参见 [开启关闭深度思考](/docs/82379/1449737#fa3f44fa)文档。
