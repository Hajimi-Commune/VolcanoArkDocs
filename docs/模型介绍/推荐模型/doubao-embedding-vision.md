# doubao-embedding-vision
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.15156250000000002);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1546875);margin-left: 16px;">

<div style="text-align: center"><code>速度</code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.29843749999999997);margin-left: 16px;">

<div style="text-align: center"><code>价格（元/百万token）</code></div>

<div style="text-align: center">0.7, 1.8</div>

<div style="text-align: center">[文本输入], [图片输入]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center">Image, Video, <del>Audio</del></div>

<div style="text-align: center">文本，图像，视频 *（部分模型支持）</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1953125);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, </div>

<div style="text-align: center"><del>Image</del>, <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

doubao-embedding-vision，一款由字节跳动研发的多模态向量化模型，是一种支持文本、图片及视频混合输入的向量化技术，支持中、英双语，适用于文搜图、图搜图、图文混合搜索等场景。
`doubao-embedding-vision-250615` 及后续版本支持视频输入。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：128k（250615版本）/ 8k
最高向量维度：3072（241215版本支持）

</div>
</div>

---

## 模型价格

`元/百万 token`

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14590787119856885);">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">0.70 [文本输入]</div>

<div style="text-align: center">1.80 [图片输入]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15230769230769237);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">0</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.15384615384615385);margin-left: 16px;">

<div style="text-align: center"><code>缓存命中</code></div>

<div style="text-align: center">不涉及</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.24512820512820513);margin-left: 16px;">

<div style="text-align: center"><code>缓存存储[每小时]</code></div>

<div style="text-align: center">不涉及</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.1538461538461538);margin-left: 16px;">

<div style="text-align: center"><code>输入[批量]</code></div>

<div style="text-align: center">不涉及</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 80px) * 0.14896392367322614);margin-left: 16px;">

<div style="text-align: center"><code>输出[批量]</code></div>

<div style="text-align: center">不涉及</div>

</div>
</div>

> * 其中使用前缀缓存会产生缓存命中、缓存存储计费；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。
> * 图片tokens = (width px × height px)/784，超大图封顶 1312 token，具体请参阅[图文向量化API-请求参数](https://www.volcengine.com/docs/82379/1523520#BJ5XLFqM)。
> * 视频会按照固定间隔抽取画面，具体用量请参考[用量说明](/docs/82379/1409291#6cf6a782)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

* [批量推理](/docs/82379/1399517)

</div>
</div>

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

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center">TPM：1,200,000</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center">RPM：15,000</div>

</div>
</div>

## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1523520">多模态向量化 API </a></div>

<div style="text-align: center">API说明</div>

<div style="text-align: center">供您了解模型API详细参数含义、示例、取值。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center"><a href="/docs/82379/1409291">多模态向量化</a></div>

<div style="text-align: center">模型调用教程</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
</div>