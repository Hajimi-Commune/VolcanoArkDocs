# 分词 API

` POST https://ark.cn-beijing.volces.com/api/v3/tokenization`[运行](https://api.volcengine.com/api-explorer/?action=Tokenization&data=%7B%7D&groupName=%E5%88%86%E8%AF%8D&query=%7B%7D&serviceCode=ark&version=2024-01-01)
本文介绍分词 API 的输入输出参数，供您使用接口时查阅字段含义。当前接口只支持文本信息。
请求参数
跳转 [响应参数](#Qu59cel0)
请求体
**model**`string``必选`
本次请求使用模型的 Model ID 或[推理接入点](https://www.volcengine.com/docs/82379/1099522) (Endpoint ID)。
**text**`string / string[]``必选`
需要分词的内容，支持字符串或字符串列表。
响应参数
跳转 [请求参数](#RxN8G2nH)
**id**`string`
本次请求的唯一标识。
**model**`string`
本次请求实际使用的模型 ID （`模型名称-版本`）。
doubao 1.5 代模型的模型名称格式为 doubao-1-5-**。如调用部署doubao-1.5-pro-32k 250115模型的推理接入点，返回model字段信息doubao-1-5-pro-32k-250115。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `list`。
**data**`object[]`
本次请求的分词的结果。

## 示例代码

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/tokenization \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "doubao-pro-32k-241215",
    "text": [
        "天空为什么这么蓝",
        "花儿为什么这么香"
    ]
}'
```

**Response:**

```json
{
  "object": "list",
  "id": "0217441868631536d992e302715172e42d8bf34590a60cadc221f",
  "model": "doubao-pro-32k-241215",
  "data": [
    {
      "object": "tokenization",
      "index": 0,
      "total_tokens": 4,
      "token_ids": [14539, 4752, 5189, 5399],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    },
    {
      "object": "tokenization",
      "index": 1,
      "total_tokens": 4,
      "token_ids": [45018, 4752, 5189, 2920],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    }
  ],
  "created": 1744186863
}

```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# Authentication
# 1.If you authorize your endpoint using an API key, you can set your api key to environment variable "ARK_API_KEY"
# or specify api key by Ark(api_key="${YOUR_API_KEY}").
# Note: If you use an API key, this API key will not be refreshed.
# To prevent the API from expiring and failing after some time, choose an API key with no expiration date.

# 2.If you authorize your endpoint with Volcengine Identity and Access Management（IAM),
# set your api key to environment variable "VOLC_ACCESSKEY", "VOLC_SECRETKEY"
# or specify ak&sk by Ark(ak="${YOUR_AK}", sk="${YOUR_SK}").
# To get your ak&sk, please refer to this document(https://www.volcengine.com/docs/6291/65568)
# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

print("----- tokenization request -----")
resp = client.tokenization.create(
    model="doubao-pro-32k-241215",
    text=[
        "天空为什么这么蓝",
        "花儿为什么这么香",
    ],
)
print(resp)
```

**Response:**

```json
{
  "object": "list",
  "id": "0217441868631536d992e302715172e42d8bf34590a60cadc221f",
  "model": "doubao-pro-32k-241215",
  "data": [
    {
      "object": "tokenization",
      "index": 0,
      "total_tokens": 4,
      "token_ids": [14539, 4752, 5189, 5399],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    },
    {
      "object": "tokenization",
      "index": 1,
      "total_tokens": 4,
      "token_ids": [45018, 4752, 5189, 2920],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    }
  ],
  "created": 1744186863
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
)

/**
 * Authentication
 * 1.If you authorize your endpoint using an API key, you can set your api key to environment variable "ARK_API_KEY"
 * client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
 * Note: If you use an API key, this API key will not be refreshed.
 * To prevent the API from expiring and failing after some time, choose an API key with no expiration date.
 *
 * 2.If you authorize your endpoint with Volcengine Identity and Access Management（IAM), set your api key to environment variable "VOLC_ACCESSKEY", "VOLC_SECRETKEY"
 * client := arkruntime.NewClientWithAkSk(os.Getenv("VOLC_ACCESSKEY"), os.Getenv("VOLC_SECRETKEY"))
 * To get your ak&sk, please refer to this document(https://www.volcengine.com/docs/6291/65568)
 * For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
 */

func main() {
	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
	ctx := context.Background()
	fmt.Println("----- tokenization request -----")

	req := model.TokenizationRequestStrings{
		Text: []string{
			"天空为什么这么蓝",
			"花儿为什么这么香",
		},
		Model: "doubao-pro-32k-241215",
	}

	resp, err := client.CreateTokenization(ctx, req)
	if err != nil {
		fmt.Printf("tokenization error: %v\n", err)
		return
	}
	fmt.Printf("tokenization response: %v\n", resp)
}

```

**Response:**

```json
{
  "object": "list",
  "id": "0217441868631536d992e302715172e42d8bf34590a60cadc221f",
  "model": "doubao-pro-32k-241215",
  "data": [
    {
      "object": "tokenization",
      "index": 0,
      "total_tokens": 4,
      "token_ids": [14539, 4752, 5189, 5399],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    },
    {
      "object": "tokenization",
      "index": 1,
      "total_tokens": 4,
      "token_ids": [45018, 4752, 5189, 2920],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    }
  ],
  "created": 1744186863
}

```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.tokenization.TokenizationRequest;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.Arrays;
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

    /**
     * Authentication 1.If you authorize your endpoint using an API key, you can set your api key to
     * environment variable "ARK_API_KEY" String apiKey = System.getenv("ARK_API_KEY"); ArkService
     * service = new ArkService(apiKey); Note: If you use an API key, this API key will not be
     * refreshed. To prevent the API from expiring and failing after some time, choose an API key
     * with no expiration date.
     *
     * <p>2.If you authorize your endpoint with Volcengine Identity and Access Management（IAM), set
     * your api key to environment variable "VOLC_ACCESSKEY", "VOLC_SECRETKEY" String ak =
     * System.getenv("VOLC_ACCESSKEY"); String sk = System.getenv("VOLC_SECRETKEY"); ArkService
     * service = new ArkService(ak, sk); To get your ak&sk, please refer to this
     * document(https://www.volcengine.com/docs/6291/65568) For more information，please check this
     * document（https://www.volcengine.com/docs/82379/1263279）
     */
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

        TokenizationRequest req =
                TokenizationRequest.builder()
                        .model("doubao-pro-32k-241215")
                        .text(Arrays.asList("天空为什么这么蓝", "花儿为什么这么香"))
                        .build();

        service.createTokenization(req).toString();
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "object": "list",
  "id": "0217441868631536d992e302715172e42d8bf34590a60cadc221f",
  "model": "doubao-pro-32k-241215",
  "data": [
    {
      "object": "tokenization",
      "index": 0,
      "total_tokens": 4,
      "token_ids": [14539, 4752, 5189, 5399],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    },
    {
      "object": "tokenization",
      "index": 1,
      "total_tokens": 4,
      "token_ids": [45018, 4752, 5189, 2920],
      "offset_mapping": [
        [0, 2],
        [2, 5],
        [5, 7],
        [7, 8]
      ]
    }
  ],
  "created": 1744186863
}

```

属性
data.**index **`integer`
当前元素在 data 列表的索引。
data.**object**`string`
固定为 `tokenization`。
data.**total_tokens**`integer`
对应内容的总 token 数量。
data.**token_ids**`integer[]`
对文本进行分词后的具体词语在词表中的 id 列表。
data.**offset_mapping**`Array<Array<Integer>>`
对文本进行分词后的词语偏移量，列表中每个元素是一个包含两个整数的列表：第一个整数表示词或标记在原始文本中的起始索引（是从0开始），第二个整数表示结束索引（不包括该索引处的字符）。
***
本接口支持 API Key 鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b9c82890e851fc10cc31f48f9065abc6.png)[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/application)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a5fdd3028d35cc512a10bd71b982b6eb.png)[模型计费](https://www.volcengine.com/docs/82379/1099320#%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_afbcf38bdec05c05089d5de5c3fd8fc8.png)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f45b5cd5863d1eed3bc3c81b9af54407.png)[接口文档](https://www.volcengine.com/docs/82379/1526787)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1609c71a747f84df24be1e6421ce58f0.png)[常见问题](https://www.volcengine.com/docs/82379/1359411)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bef4bc3de3535ee19d0c5d6c37b0ffdd.png)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
