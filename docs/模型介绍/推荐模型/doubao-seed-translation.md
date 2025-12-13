# doubao-seed-translation
<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.15156250000000002);">

<div style="text-align: center"><code>模型效果</code></div>

<div style="text-align: center">★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1546875);margin-left: 16px;">

<div style="text-align: center"><code>速度 </code></div>

<div style="text-align: center">★★★★★</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.29843749999999997);margin-left: 16px;">

<div style="text-align: center"><code>价格(元/百万token）</code></div>

<div style="text-align: center">1.2, 3.6</div>

<div style="text-align: center">[输入], [输出]</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.2);margin-left: 16px;">

<div style="text-align: center"><code>输入</code></div>

<div style="text-align: center">Text, <del>Image</del> , <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 64px) * 0.1953125);margin-left: 16px;">

<div style="text-align: center"><code>输出</code></div>

<div style="text-align: center">Text, <del>Image</del>, <del>Video</del>, <del>Audio</del></div>

<div style="text-align: center">文本</div>

</div>
</div>

---

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6009073291050036);">

是字节跳动自研的多语言翻译模型，支持数十种语言互译，提供忠实、地道、流畅的译文，中英翻译效果逼近Deepseek-R1，通用多语言翻译效果超越或持平 GPT-4o / Gemini-2.5-Pro，能精准适配办公和娱乐等多场景需求。

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.39909267089499656);margin-left: 16px;">

最大上下文长度：4k
最大输入长度：1k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：3k
默认最大回答长度：3k
> [附-模型输入输出长度限制说明](/docs/82379/1449737#4adf109b)

</div>
</div>

---

## 模型价格

| | | | | \
|输入 |\
|(元/百万 token) |输入命中缓存 |\
| |(元/百万 token) |输出单价 |\
| | |(元/百万 token) |缓存存储 |\
| | | |(元/百万 token*小时) |
|---|---|---|---|
| | | | | \
|1.20 |\- |3.60 |\- |

> 下面是计费项的简单说明，具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [Responses API 教程](/docs/82379/1585128)

:::warning
不支持缓存，即不支持 **caching** 字段
:::

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

</div>
</div>

## 模型版本
doubao-seed-translation

* doubao-seed-translation-250915
   支持 Responses API 及其翻译字段，具体见下方使用说明。
   不支持 Chat API

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

<div style="text-align: center">TPM：500,000</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

<div style="text-align: center">RPM：5,000</div>

</div>
</div>

## 使用文档

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.4999999999999999);">

<div style="text-align: center"><a href="/docs/82379/1585128">Responses API 教程</a></div>

<div style="text-align: center">API调用教程示例</div>

<div style="text-align: center">供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。</div>

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.4999999999999999);margin-left: 16px;">

<div style="text-align: center"><a href="https://www.volcengine.com/docs/82379/1569618">Responses API</a></div>

<div style="text-align: center">Responses API参数说明</div>

<div style="text-align: center">供您查阅API请求以及返回参数取值范围、默认值、示例等信息。</div>

</div>
</div>

## 使用说明
### 翻译相关选项
该模型面向翻译领域，支持翻译的选项配置，包括设定翻译的源语言(**source_language**)和目标语言(**target_language**，必选)，使用示例如下。
```Shell
curl 'https://ark.cn-beijing.volces.com/api/v3/responses' \
-H "Authorization: Bearer $ARK_API_KEY" \
-H "Content-Type: application/json" \
-d '{
  "model": "doubao-seed-translation-250915",
  "input": [
    {
      "role": "user",
      "content": [
        {
          "type": "input_text",
          "text": "这是需要翻译的文本。",
          "translation_options": {
            "source_language": "zh",
            "target_language": "en"
          }
        }
      ]
    }
  ]
}'
```

### 支持翻译语种
即模型可接受的 **source_language、target_language** 的字段取值范围如下，如不符合，则报错。

| | | | \
|**lang_code** |**对应的语种英文名** |**对应的语种中文名** |
|---|---|---|
| | | | \
|ar |Arabic |阿拉伯语 |
| | | | \
|cs |Czech |捷克语 |
| | | | \
|da |Danish |丹麦语 |
| | | | \
|de |German |德语 |
| | | | \
|en |English |英语 |
| | | | \
|es |Spanish |西班牙语 |
| | | | \
|fi |Finnish |芬兰语 |
| | | | \
|fr |French |法语 |
| | | | \
|hr |Croatian |克罗地亚语 |
| | | | \
|hu |Hungarian |匈牙利语 |
| | | | \
|id |Indonesian |印尼语 |
| | | | \
|it |Italian |意大利语 |
| | | | \
|ja |Japanese |日语 |
| | | | \
|ko |Korean |韩语 |
| | | | \
|ms |Malay |马来语 |
| | | | \
|nb |Norwegian Bokmål |挪威布克莫尔语 |
| | | | \
|nl |Dutch |荷兰语 |
| | | | \
|pl |Polish |波兰语 |
| | | | \
|pt |Portuguese |葡萄牙语 |
| | | | \
|ro |Romanian |罗马尼亚语 |
| | | | \
|ru |Russian |俄语 |
| | | | \
|sv |Swedish |瑞典语 |
| | | | \
|th |Thai |泰语 |
| | | | \
|tr |Turkish |土耳其语 |
| | | | \
|uk |Ukrainian |乌克兰语 |
| | | | \
|vi |Vietnamese |越南语 |
| | | | \
|zh |Chinese (simplified) |中文（简体） |
| | | | \
|zh-Hant |Chinese (traditional) |中文（繁体） |