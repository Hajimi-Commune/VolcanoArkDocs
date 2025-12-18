# 多模态向量化 API

`POST https://ark.cn-beijing.volces.com/api/v3/embeddings/multimodal`[运行](https://api.volcengine.com/api-explorer/?action=EmbeddingsMultimodal&data=%7B%7D&groupName=%E5%90%91%E9%87%8F%E5%8C%96%20API&query=%7B%7D&serviceCode=ark&version=2024-01-01)
当您需通过语义来处理视频、图像和文本，如以图搜图、语义检索等，可以调用多模态向量化服务，将视频、图像和文本转化为向量，来分析其语义关系。本文为您提供接口的参数详细说明供您查阅。
请求参数
跳转 [响应参数](#L9tzcCyD)
请求体
**model**`string``必选`
您需要调用的模型的 ID （Model ID），[开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)，并[查询 Model ID](https://www.volcengine.com/docs/82379/1330310) 。
您也可通过 Endpoint ID 来调用模型，获得限流、计费类型（前付费/后付费）、运行状态查询、监控、安全等高级能力，可参考[获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522)。
**input**`object[]``必选`
需要向量化的内容列表。列表元素支持文本信息和图片信息，**部分模型支持视频信息**，详情参见列表：
`doubao-embedding-vision-250615`及后续版本：input 支持不限数量的 `文本信息`、`图片信息`和 `视频信息`混排输入。传入的信息作为`1`个整体进行向量化。
`doubao-embedding-vision-250328`及之前版本：input 当前仅支持3种组合， `1段文本信息`、`1段图片信息`、 `1段图片信息+1段文本信息`。传入的信息作为1个整体进行向量化，即传入1图片+1文本，返回的是1个向量。与[文本向量化 API](https://www.volcengine.com/docs/82379/1521766) 不同，本API **input **不支持单次请求中对多段文本/图片进行批量向量化。
**encoding_format**`string / null ``默认值 float`
取值范围： `float`、`base64`、`null`。
embedding 返回的格式。
**dimensions**`integer``默认值 2048`
取值范围： `1024` 或 `2048`。
用于指定输出的向量维度。此参数仅`doubao-embedding-vision-250615`及后续版本支持，历史版本可以参见[向量降维](https://www.volcengine.com/docs/82379/1409291#%E5%90%91%E9%87%8F%E9%99%8D%E7%BB%B4)。
响应参数
跳转 [请求参数](#RxN8G2nH)
**id **`string`
本次请求的唯一标识 。
**model**`string`
本次请求实际使用的模型名称和版本。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `list`。
**data**`object`
本次请求的算法输出内容。
**usage**`object`
本次请求的 token 用量。

## 示例代码

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/embeddings/multimodal \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "doubao-embedding-vision-250615",
    "encoding_format": "float",
    "input": [
        {
            "type": "video_url",
            "video_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_video/ark_vlm_video_input.mp4"
            }
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/tower.png"
            }
        },
        {
            "type": "text",
            "text": "视频和图片里有什么"
        }
    ]
}'
```

**Response:**

```json
{
  "created": 1743575029,
  "data": {
    "embedding": [
      -0.123046875, -0.35546875, -0.318359375, ..., -0.255859375
    ],
    "object": "embedding"
  },
  "id": "021743575029461acbe49a31755bec77b2f09448eb15fa9a88e47",
  "model": "doubao-embedding-vision-250615",
  "object": "list",
  "usage": {
    "prompt_tokens": 13987,
    "prompt_tokens_details": {
      "image_tokens": 13800,
      "text_tokens": 187
    },
    "total_tokens": 13987
  }
}

```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

resp = client.multimodal_embeddings.create(
    model="doubao-embedding-vision-250615",
    encoding_format="float",
    input=[{"text":"天很蓝，海很深","type":"text"},{"image_url":{"url":"https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg"},"type":"image_url"}]
)
print(resp)
```

**Response:**

```json
{
  "created": 1743575029,
  "data": {
    "embedding": [
      -0.123046875, -0.35546875, -0.318359375, ..., -0.255859375
    ],
    "object": "embedding"
  },
  "id": "021743575029461acbe49a31755bec77b2f09448eb15fa9a88e47",
  "model": "doubao-embedding-vision-250615",
  "object": "list",
  "usage": {
    "prompt_tokens": 528,
    "prompt_tokens_details": {
      "image_tokens": 497,
      "text_tokens": 31
    },
    "total_tokens": 528
  }
}

```

**go**

```go
package main

import (
	"context"
	"fmt"
	"os"

	"github.com/volcengine/volcengine-go-sdk/service/arkruntime"
	"github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
	"github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
	ctx := context.Background()

	encodingFormat := model.EmbeddingEncodingFormat("float")

	req := model.MultiModalEmbeddingRequest{
		Input: []model.MultimodalEmbeddingInput{
			model.MultimodalEmbeddingInput{
				Type: "text",
				Text: volcengine.String("天很蓝，海很深"),
			},
			model.MultimodalEmbeddingInput{
				Type: "image_url",
				ImageURL: &model.MultimodalEmbeddingImageURL{
					URL: "https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg",
				},
			},
		},
		Model: "doubao-embedding-vision-250615",
		EncodingFormat: &encodingFormat,
	}

	resp, err := client.CreateMultiModalEmbeddings(ctx, req)
	if err != nil {
		fmt.Printf("multimodal embeddings error: %v\n", err)
		return
	}
	fmt.Printf("%+v", resp)
}

```

**Response:**

```json
{
  "created": 1743575029,
  "data": {
    "embedding": [
      -0.123046875, -0.35546875, -0.318359375, ..., -0.255859375
    ],
    "object": "embedding"
  },
  "id": "021743575029461acbe49a31755bec77b2f09448eb15fa9a88e47",
  "model": "doubao-embedding-vision-250615",
  "object": "list",
  "usage": {
    "prompt_tokens": 528,
    "prompt_tokens_details": {
      "image_tokens": 497,
      "text_tokens": 31
    },
    "total_tokens": 528
  }
}

```

**java**

```java
package com.example;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.multimodalembeddings.*;
import com.volcengine.ark.runtime.model.multimodalembeddings.MultimodalEmbeddingInput.*;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

/*
# pom.xml
<dependency>
  <groupId>com.volcengine</groupId>
  <artifactId>volcengine-java-sdk-ark-runtime</artifactId>
  // 替换正式版本号
  <version>LATEST</version>
</dependency>
*/

public class Sample {

    static String apiKey = System.getenv("ARK_API_KEY");

    static ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
    static Dispatcher dispatcher = new Dispatcher();
    static ArkService service =
            ArkService.builder()
                    .dispatcher(dispatcher)
                    .connectionPool(connectionPool)
                    .apiKey(apiKey)
                    .build();

    public static void main(String[] args) throws JsonProcessingException {

        List<MultimodalEmbeddingInput> inputForReqList = new ArrayList<>();
        MultimodalEmbeddingInput elementForInputForReqList0 = new MultimodalEmbeddingInput();
        elementForInputForReqList0.setType("text");
        elementForInputForReqList0.setText("天很蓝，海很深");

        MultiModalEmbeddingContentPartImageURL imageUrlForElementForInputForReqList1 =
                new MultiModalEmbeddingContentPartImageURL();
        imageUrlForElementForInputForReqList1.setUrl(
                "https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg");
        MultimodalEmbeddingInput elementForInputForReqList1 = new MultimodalEmbeddingInput();
        elementForInputForReqList1.setType("image_url");
        elementForInputForReqList1.setImageUrl(imageUrlForElementForInputForReqList1);
        inputForReqList.add(elementForInputForReqList0);
        inputForReqList.add(elementForInputForReqList1);

        MultimodalEmbeddingRequest req =
                MultimodalEmbeddingRequest.builder()
                        .model("doubao-embedding-vision-250615")
                        .input(inputForReqList)
                        .encodingFormat("float")
                        .build();

        service.createMultiModalEmbeddings(req).toString();
        System.err.println(service.createMultiModalEmbeddings(req));

        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "created": 1743575029,
  "data": {
    "embedding": [
      -0.123046875, -0.35546875, -0.318359375, ..., -0.255859375
    ],
    "object": "embedding"
  },
  "id": "021743575029461acbe49a31755bec77b2f09448eb15fa9a88e47",
  "model": "doubao-embedding-vision-250615",
  "object": "list",
  "usage": {
    "prompt_tokens": 528,
    "prompt_tokens_details": {
      "image_tokens": 497,
      "text_tokens": 31
    },
    "total_tokens": 528
  }
}

```

[模型列表](https://www.volcengine.com/docs/82379/1330310?lang=zh#%E5%A4%9A%E6%A8%A1%E6%80%81%E5%90%91%E9%87%8F%E5%8C%96%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1099320#%E6%96%87%E6%9C%AC%E5%90%91%E9%87%8F%E6%A8%A1%E5%9E%8B)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[接口文档](https://www.volcengine.com/docs/82379/1523520)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
属性
data.**embedding**`float[]`
对应内容的向量化结果。
data.**object**`string`
固定为 `embedding`。
本接口支持 API Key 鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
如需使用 Access Key 来鉴权，推荐使用 SDK 的方式，具体请参见 [SDK概述](https://www.volcengine.com/docs/82379/1302007)。
usage.prompt_tokens_details.**text_tokens **`integer`
输入内容中，文本内容对应的 token 量，以及视频内容时间轴产生的 token 量。
为保证模型效果，当图片或视频传入时，会生成少量的预设文本 token，产生额外的 **text_tokens**。
usage.prompt_tokens_details.**image_tokens **`integer`
输入内容中，图片内容以及视频内容抽帧图片对应的 token 量。
**
usage.**prompt_tokens**`integer`
输入内容 token 数量。
usage.**total_tokens**`integer`
本次请求消耗的总 token 数量（输入 + 输出）。
usage.**prompt_tokens_details **`object`
输入的内容使用 token 量的细节信息。
input.**type **`string``必选`
输入内容的类型，此处应为 `text`。
input.**text **`string``必选`
输入给模型的文本内容，需要满足以下条件：
单条文本以 utf-8 编码，长度不超过 100,000 字节。
单条文本不超过模型的最大输入 token 数为 8k。
为获得更好性能，建议文本数量总token不超过4096，或者文本条数不超过4。
**文本信息 **`object`
输入给模型转化为向量的内容，文本内容部分。
**图片信息**`object`
输入给模型转化成向量的内容，图片信息部分。
说明
传入图片需要满足以下条件：
格式：`jpeg`、`png`、`webp`、`bmp`、`tiff`、`ico`、`dib`、`icns`、`sgi`、`jpeg2000`。其中，`tiff`、`sgi`、`icns`、`jpeg2000` 格式图片，需要保证和元数据对齐，如在对象存储中正确设置文件元数据，否则会解析失败。
宽高比（宽/高）：在范围[1/100, 100] 。
图像大小限制：
单张图片小于 10 MB；使用 base64 编码，请求中请求体大小不可超过 64 MB。
总像素不大于 3600万。
举例，当图片宽高的长度为6000 px、7000 px，则图片总像素大于3600万，则会返回错误信息。
**视频信息**`object`
输入给模型转化成向量的内容，视频信息部分。
说明
传入视频需要满足以下条件：
格式：`.mp4`、`.avi`、 `.mov`，视频格式需小写。
传入 Base64 编码时使用：[Base64 编码输入](https://www.volcengine.com/docs/82379/1409291#base64-%E7%BC%96%E7%A0%81%E8%BE%93%E5%85%A5)。
单视频文件需在 50MB 以内。
暂不支持对视频文件中的音频信息进行理解。
输入内容的类型，此处应为 `video_url`。
input.**video_url**`object``必选`
输入给模型的视频对象。
input.image_url.**url **`string``必选`
图片信息，可以是图片URL或图片Base64编码。
图片URL：请确保图片URL可被访问。
Base64编码：请遵循此格式`data:image/{图片格式};base64,{图片Base64编码}`。
输入内容的类型，此处应为 `image_url`。
input.**image_url **`object``必选`
输入给模型的图片对象。
input.video_url.**url **`string``必选`
支持传入视频链接或视频的Base64编码。具体使用请参见[视频理解说明](https://www.volcengine.com/docs/82379/1362931#%E8%A7%86%E9%A2%91%E7%90%86%E8%A7%A3)。
