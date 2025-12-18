# doubao-1.5-pro-32k

<code>模型效果</code>

★★★★★

<code>速度</code>

★★★★

<code>价格（元/百万token）</code>

0.8, 2.0

[输入], [输出]

<code>输入</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

<code>输出</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

---

相比 doubao-1.0 模型，性能全面升级，在知识、代码、推理等方面表现卓越。输出长度支持最大 12k tokens。同时提供面向角色扮演领域优化版本。

最大上下文长度：

* 32k `character-250715`
* 32k `character-250228`
* 128k `250115` 

[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：12k
默认最大回答长度：4k

---

## 模型价格

`元/百万 token`

<code>输入</code>

0.80

<code>输出</code>

2.00

<code>缓存输入</code>

0.16

<code>缓存存储[每小时]</code>

0.017

<code>输入[批量]</code>

0.40

<code>输出[批量]</code>

1.00

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [工具调用 Function Calling](/docs/82379/1262342)
   * doubao-1-5-pro-32k-character-250228 不支持
* [上下文缓存(Context API) ](/docs/82379/1396491)

* [批量推理](/docs/82379/1305505)
* [模型精调](https://www.volcengine.com/docs/82379/1099350)

## 模型版本
doubao-1.5-pro-32k

* doubao-1-5-pro-32k-character-250715
   支持 Context API 的 Session 缓存和前缀缓存（新增）。
* doubao-1-5-pro-32k-250115
   扩展至128k上下文窗口。
   支持上下文缓存的前缀缓存和Session缓存。
* doubao-1-5-pro-32k-character-250228
   基于doubao-1.5 升级，支持故事剧情模式，优化恋爱拉扯能力，角色风格能力优化 ，增强剧情推动能力。
   支持上下文缓存的Session缓存。

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="/docs/82379/1399009">文本生成</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat)  API</a>

Chat API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

<a href="/docs/82379/1256348">角色扮演场景提示词指南</a>

助您高效撰写角色扮演提示词 (Prompt) ，以创建符合人设、交互自然且富有沉浸感的 AI 智能体角色。
