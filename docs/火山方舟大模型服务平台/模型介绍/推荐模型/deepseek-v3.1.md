# deepseek-v3.1

<code>模型效果</code>

★★★★★

<code>速度 </code>

★★★★

<code>输入</code>

Text, <del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

<code>输出</code>

Text, <del>Image</del>, <del>Video</del>, <del>Audio</del>

文本

<code>价格(</code><code>元/百万 token</code><code>)</code>

4.0, 12.0

[输入], [输出]

---

是深度求索推出的混合推理模型，支持思考与非思考两种推理模式，较 `deepseek-r1-0528 `思考效率更高

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
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
- 4.00 | 0.80 | 12.00 | 0.017 | 2.00 | 0.80 | 6.00

> 下面是计费项的简单说明，具体请参阅[模型服务价格](/docs/82379/1544106)。

> * 使用在线推理的上下文缓存能力，产生命中缓存的输入折后费用、创建的缓存存储费用。
> * 使用批量推理，产生输入[批量]费用、命中透明缓存的输入折后费用、输出[批量]费用。

## 能力支持

* [文本生成](/docs/82379/1399009)
* [函数调用 Function Calling](/docs/82379/1262342)
* [结构化输出(beta)](/docs/82379/1568221)
* [Responses API 教程](/docs/82379/1585128)

* [深度思考](/docs/82379/1449737)
* [批量推理](/docs/82379/1399517)
* [上下文缓存(Responses API)](/docs/82379/1602228)

## 模型版本
deepseek-v3.1

* deepseek-v3-1-terminus
   支持开启思考（默认关闭），即支持 <a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a> 中的 **thinking** 字段。
* deepseek-v3-1-250821
   支持开启思考（默认关闭），即支持 <a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a> 中的 **thinking** 字段。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="/docs/82379/1449737">深度思考</a>

深度思考能力使用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1494384">Chat API</a>

Chat API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

<a href="/docs/82379/1585128">Responses API 教程</a>

API调用教程示例

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1569618">Responses API</a>

Responses API参数说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

## 使用说明
### 深度思考开关
支持使用 **thinking** 参数控制模型是否开启深度思考模式。默认为`关闭`状态。详细使用请参见 [开启关闭深度思考](/docs/82379/1449737#fa3f44fa)文档。
### 控制模型输出长度
详细使用请参见 [设置模型输出长度限制](/docs/82379/1449737#31ecc4d7)。
