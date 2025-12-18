# doubao-1.5-lite

<code>模型效果</code>

★★★

<code>速度</code>

★★★★★

<code>价格（元/百万token）</code>

0.3, 0.6

[输入], [输出]

<code>输入</code>

Text, 

<del>Image</del> , <del>Audio</del>

文本

<code>输出</code>

Text, 

<del>Image</del> , <del>Audio</del>

文本

---

轻量版模型，相比 1.0 代模型，具备更高的响应速度，效果与时延均衡。

最大上下文长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：12k
默认最大回答长度：4k

---

## 模型价格

`元/百万 token`

<code>输入</code>

0.30

<code>输出</code>

0.60

<code>缓存命中</code>

不涉及

<code>缓存存储[每小时]</code>

不涉及

<code>输入[批量]</code>

0.15

<code>输出[批量]</code>

0.30

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
*  [函数调用 Function Calling](/docs/82379/1262342)
* [上下文缓存(Context API) ](/docs/82379/1396491)

* [批量推理](/docs/82379/1305505)
* [前缀缓存(Context API)](/docs/82379/1396490)
* [模型精调](https://www.volcengine.com/docs/82379/1099350)

## 模型版本
doubao-1.5-lite-32k

* doubao-1-5-lite-32k-250115

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="/docs/82379/1399009">文本生成</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat)  API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。
