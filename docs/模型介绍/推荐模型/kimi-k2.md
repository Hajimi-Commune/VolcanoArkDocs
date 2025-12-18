# kimi-k2

<code>模型效果</code>

★★★★★

<code>速度</code>

★★

<code>输入</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

<code>输出</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

<code>价格(</code><code>元/百万 token</code><code>)</code>

4.0, 16.0

[输入], [输出]

---

具备超强代码和 Agent 能力的 MoE （混合专家模型）架构基础模型，总参数 1T，激活参数 32B。在通用知识推理、编程、数学、Agent 等主要类别的基准性能测试中，K2 模型的性能超过其他主流开源模型。

最大上下文长度：256k 
可[设置最大回答长度](/docs/82379/1399009#3821b26a)：32k
默认最大回答长度：4k

---

## 模型价格

- 输入
- 元/百万 token | 输入命中缓存
- 元/百万 token | 输出单价
- 元/百万 token | 缓存存储
- 元/百万 token/小时 | 输入单价[批量]
- 元/百万 token | 输入命中缓存单价[批量]
- 元/百万 token | 输出单价[批量]
- 元/百万 token
- 4.00 | 0.80 | 16.00 | 0.017 | 2.00 | 0.80 | 8.00

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [文本生成](/docs/82379/1399009)
* [Responses API 教程](/docs/82379/1585128)
* [批量推理](/docs/82379/1399517)

* [函数调用 Function Calling](/docs/82379/1262342)
* [上下文缓存(Responses API)](/docs/82379/1602228)

## 模型版本
kimi-k2

* kimi-k2-thinking-251104：强制开启深度思考，不可关闭；支持结构化输出 **json_object** 和 **json_schema** 的 strict 模式，以及函数调用的 strict 模式。
* kimi-k2-250905：新支持 Responses API 及上下文缓存；上下文窗口由128k升级至256k。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：500,000

RPM：5,000

## 使用文档

<a href="/docs/82379/1399009">文本生成</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1569618">Responses API</a>

Responses API参数说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

<a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a>

Chat API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。
