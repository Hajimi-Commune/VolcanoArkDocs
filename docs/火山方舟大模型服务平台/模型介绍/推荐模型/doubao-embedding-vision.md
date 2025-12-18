# doubao-embedding-vision

<code>模型效果</code>

★★★★★

<code>速度</code>

★★★★★

<code>价格（元/百万token）</code>

0.7, 1.8

[文本输入], [图片输入]

<code>输入</code>

Text, 

Image, Video, <del>Audio</del>

文本，图像，视频 *（部分模型支持）

<code>输出</code>

Text, 

<del>Image</del>, <del>Video</del>, <del>Audio</del>

文本

---

doubao-embedding-vision，一款由字节跳动研发的多模态向量化模型，是一种支持文本、图片及视频混合输入的向量化技术，支持中、英双语，适用于文搜图、图搜图、图文混合搜索等场景。
`doubao-embedding-vision-250615` 及后续版本支持视频输入。

最大上下文长度：128k（250615版本）/ 8k
最高向量维度：3072（241215版本支持）

---

## 模型价格

`元/百万 token`

<code>输入</code>

0.70 [文本输入]

1.80 [图片输入]

<code>输出</code>

0

<code>缓存命中</code>

不涉及

<code>缓存存储[每小时]</code>

不涉及

<code>输入[批量]</code>

不涉及

<code>输出[批量]</code>

不涉及

> * 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。
> * 图片tokens = (width px × height px)/784，超大图封顶 1312 token，具体请参阅[图文向量化API-请求参数](https://www.volcengine.com/docs/82379/1523520#BJ5XLFqM)。
> * 视频会按照固定间隔抽取画面，具体用量请参考[用量说明](/docs/82379/1409291#6cf6a782)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)

* [批量推理](/docs/82379/1399517)

## 模型版本
doubao-embedding-vision

* doubao-embedding-vision-250615
   支持 **视频、文本、图片** 输入。
   最高向量维度：2048，支持1024降维使用。
* doubao-embedding-vision-250328
   支持 **最多1张图片、1段文本** 输入。
   最高向量维度：2048

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：1,200,000

RPM：15,000

## 使用文档

<a href="https://www.volcengine.com/docs/82379/1523520">多模态向量化 API </a>

API说明

供您了解模型API详细参数含义、示例、取值。

<a href="/docs/82379/1409291">多模态向量化</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。
