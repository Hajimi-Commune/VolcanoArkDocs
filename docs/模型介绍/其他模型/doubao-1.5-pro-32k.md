# doubao-1.5-pro-32k
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

<div style="text-align: center"><code>价格（元/百万token）</code></div>

<div style="text-align: center">0.8, 2.0</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del> , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1953125);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del> , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

相比 doubao-1.0 模型，性能全面升级，在知识、代码、推理等方面表现卓越。输出长度支持最大 12k tokens。同时提供面向角色扮演领域优化版本。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：

* 32k `character-250715`
* 32k `character-250228`
* 128k `250115` 

[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：12k
默认最大回答长度：4k

</div>
</div>

---

## 模型价格

`元/百万 token`

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14590787119856885);">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">0.80</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15230769230769237);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">2.00</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15384615384615385);margin-left: 16px;">

<div style="text-align: center"><code>缓存输入</code></div>

<div style="text-align: center">0.16</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.24512820512820513);margin-left: 16px;">

<div style="text-align: center"><code>缓存存储[每小时]</code></div>

<div style="text-align: center">0.017</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.1538461538461538);margin-left: 16px;">

<div style="text-align: center"><code>输入[批量]</code></div>

<div style="text-align: center">0.40</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14896392367322614);margin-left: 16px;">

<div style="text-align: center"><code>输出[批量]</code></div>

<div style="text-align: center">1.00</div>

</div>
</div>

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [工具调用 Function Calling](/docs/82379/1262342)
   * doubao-1-5-pro-32k-character-250228 不支持
* [上下文缓存(Context API) ](/docs/82379/1396491)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [批量推理](/docs/82379/1305505)
* [模型精调](https://www.volcengine.com/docs/82379/1099350)

</div>
</div>

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
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);">

<div style="text-align: center"><a href="/docs/82379/1399009">文本生成</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1494384">对话(Chat)  API</a></div>

<div style="text-align: center">Chat API参数的说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333333333333333);margin-left: 16px;">

<div style="text-align: center"><a href="/docs/82379/1256348">角色扮演场景提示词指南</a></div>

<div style="text-align: center">助您高效撰写角色扮演提示词 (Prompt) ，以创建符合人设、交互自然且富有沉浸感的 AI 智能体角色。</div>

</div>
</div>