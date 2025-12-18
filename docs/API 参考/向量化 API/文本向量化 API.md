# 文本向量化 API

`POST https://ark.cn-beijing.volces.com/api/v3/embeddings`[运行](https://api.volcengine.com/api-explorer/?action=Embeddings&data=%7B%7D&groupName=%E5%90%91%E9%87%8F%E5%8C%96%20API&query=%7B%7D&serviceCode=ark&version=2024-01-01)
当您需通过语义来处理文本，如语义检索、分析词性等，可以调用向量化服务，将文本转化为向量，来分析文本的语义关系。本文为您提供服务接口的参数详细说明供您查阅。
如果您需要调整向量维度，请参考 [向量降维](https://www.volcengine.com/docs/82379/1583857#%E5%90%91%E9%87%8F%E9%99%8D%E7%BB%B4)。
请求参数
跳转 [响应参数](#L9tzcCyD)
请求体
**model**`string``必选`
您需要调用的模型的 ID （Model ID），[开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)，并[查询 Model ID](https://www.volcengine.com/docs/82379/1330310) 。
您也可通过 Endpoint ID 来调用模型，获得限流、计费类型（前付费/后付费）、运行状态查询、监控、安全等高级能力，可参考[获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522#%E8%8E%B7%E5%8F%96-endpoint-id)。
**input**`string / string[]``必选`
需要向量化的内容列表，支持中文、英文。输入内容需满足下面条件：
不得超过模型的最大输入 token 数。doubao-embdding 模型，每个列表元素（并非单次请求总数）最大输入token 数为 4096。
不能为空列表，列表的每个成员不能为空字符串。
单条文本以 utf-8 编码，长度不超过 100,000 字节。
为获得更好性能，建议文本数量总token不超过4096，或者文本条数不超过4。
**encoding_format**`string / null ``默认值 float`
取值范围： `float`、`base64`、`null`。
embedding 返回的格式。
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
 curl https://ark.cn-beijing.volces.com/api/v3/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "encoding_format": "float",
    "input": [
        " 天很蓝",
        "海很深"
    ],
    "model": "doubao-embedding-text-240715"
}'
```

**Response:**

```json
{
  "created": 1743500074,
  "id": "021743500074720a0965b1ddc5dd75a6dc7a92b58e4552e39fb68",
  "data": [
    {
      "embedding": [-1.9609375, -0.197265625, -1.359375, ... , 1.984375],
      "index": 0,
      "object": "embedding"
    },
    {
      "embedding": [-2.3125, -0.70703125, -0.82421875, ... , 0.259765625],
      "index": 1,
      "object": "embedding"
    }
  ],
  "model": "doubao-embedding-text-240715",
  "object": "list",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}

```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

resp = client.embeddings.create(
    model="doubao-embedding-text-240715",
    input=[
        " 天很蓝",
        "海很深",
    ],
    encoding_format="float",
)
print(resp)
```

**Response:**

```json
{
  "created": 1743500074,
  "id": "021743500074720a0965b1ddc5dd75a6dc7a92b58e4552e39fb68",
  "data": [
    {
      "embedding": [-1.9609375, -0.197265625, -1.359375, ... , 1.984375],
      "index": 0,
      "object": "embedding"
    },
    {
      "embedding": [-2.3125, -0.70703125, -0.82421875, ... , 0.259765625],
      "index": 1,
      "object": "embedding"
    }
  ],
  "model": "doubao-embedding-text-240715",
  "object": "list",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
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
)

func main() {
	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
	ctx := context.Background()
	req := model.EmbeddingRequestStrings{
		Input: []string{
			" 天很蓝",
			"海很深",
		},
		Model:          "doubao-embedding-text-240715",
		EncodingFormat: "float",
	}
	resp, err := client.CreateEmbeddings(ctx, req)
	if err != nil {
		fmt.Printf("embeddings error: %v\n", err)
		return
	}
	fmt.Printf("%+v", resp)
}
```

**Response:**

```json
{
  "created": 1743500074,
  "id": "021743500074720a0965b1ddc5dd75a6dc7a92b58e4552e39fb68",
  "data": [
    {
      "embedding": [-1.9609375, -0.197265625, -1.359375, ... , 1.984375],
      "index": 0,
      "object": "embedding"
    },
    {
      "embedding": [-2.3125, -0.70703125, -0.82421875, ... , 0.259765625],
      "index": 1,
      "object": "embedding"
    }
  ],
  "model": "doubao-embedding-text-240715",
  "object": "list",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}

```

**java**

```java
package com.example;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.embeddings.EmbeddingRequest;
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

        EmbeddingRequest req =
                EmbeddingRequest.builder()
                        .model("doubao-embedding-text-240715")
                        .input(Arrays.asList(" 天很蓝", "海很深"))
                        .build();

        service.createEmbeddings(req).toString();
        System.out.println(service.createEmbeddings(req));

        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "created": 1743500074,
  "id": "021743500074720a0965b1ddc5dd75a6dc7a92b58e4552e39fb68",
  "data": [
    {
      "embedding": [-1.9609375, -0.197265625, -1.359375, ... , 1.984375],
      "index": 0,
      "object": "embedding"
    },
    {
      "embedding": [-2.3125, -0.70703125, -0.82421875, ... , 0.259765625],
      "index": 1,
      "object": "embedding"
    }
  ],
  "model": "doubao-embedding-text-240715",
  "object": "list",
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}

```

**
属性
data.**index**`integer`
向量的序号，与请求参数 `input` 列表中的内容顺序对应。
data.**embedding**`float[]`
对应内容的向量化结果。
data.**object**`string`
固定为 `embedding`。
本接口支持 API Key 鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
如需使用 Access Key 来鉴权，推荐使用 SDK 的方式，具体请参见 [SDK概述](https://www.volcengine.com/docs/82379/1302007)。
[模型列表](https://www.volcengine.com/docs/82379/1330310)[模型计费](https://www.volcengine.com/docs/82379/1099320#%E6%96%87%E6%9C%AC%E5%90%91%E9%87%8F%E6%A8%A1%E5%9E%8B)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[接口文档](#)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
usage.**prompt_tokens**`integer`
输入内容 token 数量。
usage.**total_tokens**`integer`
本次请求消耗的总 token 数量（输入 + 输出）。
