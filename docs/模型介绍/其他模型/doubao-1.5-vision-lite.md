# doubao-1.5-vision-lite

<code>模型效果</code>

★★

<code>速度</code>

★★★★

<code>价格（元/百万token）</code>

1.5, 4.5

[输入], [输出]

<code>输入</code>

Text, 

Image , <del>Video</del>, <del>Audio</del>

文本，图像

<code>输出</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

---

doubao-1.5-vision-lite，极具性价比的多模态大模型，支持任意分辨率和极端长宽比图像识别，增强视觉推理、文档识别、细节信息理解和指令遵循能力。

最大上下文长度：128k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：16k
默认最大回答长度：4k

---

## 模型价格

`元/百万 token`

<code>输入</code>

1.50

<code>输出</code>

4.50

<code>缓存命中</code>

不涉及

<code>缓存存储[每小时]</code>

不涉及

<code>输入[批量]</code>

1.50

<code>输出[批量]</code>

4.50

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [结构化输出](/docs/82379/1568221)
   * 仅支持 `json_object`

* [批量推理](/docs/82379/1305505)

## 模型版本
doubao-1.5-vision-lite

* doubao-1-5-vision-lite-250315

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="/docs/82379/1362931">视觉理解</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat)  API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。
