# doubao-embedding-large

<code>模型效果</code>

★★★★

<code>速度</code>

★★★★★

<code>价格（元/百万token）</code>

0.70

[输入]

<code>输入</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

<code>输出</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

---

doubao-embedding-large，一款由字节跳动研发的语义向量化模型，主要面向向量检索的使用场景，支持中、英双语。

最大上下文长度：128k
最高向量维度：4096

---

## 模型价格

`元/百万 token`

<code>输入</code>

0.70

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

> 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)

* [批量推理](/docs/82379/1399517)

## 模型版本
doubao-embedding-large

* doubao-embedding-large-text-250515
   * 基于Seed1.5模型训练的语义向量化模型，在通用嵌入任务中表现出色，在MTEB榜单上（包括中文和英文）均达到了最先进的效果。
   * 推理能力在复杂查询理解和推理方面展现出卓越能力，在BRIGHT榜单中取得了最先进的效果。
   * 支持多种嵌入维度 —— [2048、1024、512、256] —— 即使在较低维度下性能下降也较小。需要归一化后使用。
* doubao-embedding-large-text-240915
   * 最高向量维度 4096

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：800,000

RPM：1,000

## 使用文档

<a href="https://www.volcengine.com/docs/82379/1521766">文本向量化 API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。
