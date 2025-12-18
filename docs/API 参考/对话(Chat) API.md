# 对话(Chat) API

` POST https://ark.cn-beijing.volces.com/api/v3/chat/completions`[运行](https://api.volcengine.com/api-explorer/?action=ChatCompletions&groupName=%E5%AF%B9%E8%AF%9D%28Chat%29%20API&serviceCode=ark&version=2024-01-01)
发送包含文本、图片、视频等模态的消息列表，模型将生成对话中的下一条消息。
请求参数
跳转 [响应参数](#Qu59cel0)
请求体
**model**`string``必选`
调用的模型 ID （Model ID），[开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)，并[查询 Model ID](https://www.volcengine.com/docs/82379/1330310) 。
多个应用及精细管理场景，推荐使用 Endpoint ID 调用。详细请参考 [获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522)。
**messages**`object[]``必选`
消息列表，不同模型支持不同类型的消息，如文本、图片、视频等。
**thinking**`object``默认值 {"type":"enabled"}`
控制模型是否开启深度思考模式。
不同模型是否支持以及默认取值不同，详情请查询[文档](https://www.volcengine.com/docs/82379/1449737#0002)。
**stream**`boolean / null``默认值 false`
响应内容是否流式返回：
`false`：模型生成完所有内容后一次性返回结果。
`true`：按 SSE 协议逐块返回模型生成内容，并以一条 `data: [DONE] `消息结束。当 **stream** 为 `true` 时，可设置 **stream_options** 字段以获取 token 用量统计信息。
**stream_options**`object / null``默认值 null`
流式响应的选项。当 **stream** 为 `true` 时，可设置 **stream_options** 字段。
**max_tokens**`integer / null``默认值 4096`
取值范围：各个模型不同，详细见[模型列表](https://www.volcengine.com/docs/82379/1330310)。
模型回答最大长度（单位 token）。
说明
模型回答不包含思维链内容，模型回答 = 模型输出 - 模型思维链（如有）
输出 token 的总长度还受模型的上下文长度限制。
**max_completion_tokens**`integer / null`
支持该字段的模型及使用说明见 [文档](https://www.volcengine.com/docs/82379/1449737#0001)。
取值范围：`[0, 65,536]`。
控制模型输出的最大长度（包括模型回答和模型思维链内容长度，单位 token）。
配置了该参数后，可以让模型输出超长内容，**max_tokens **默认值失效，模型按需输出内容（回答和思维链），直到达到 **max_completion_tokens **值。
不可与 **max_tokens** 字段同时设置。
**service_tier**`string / null``默认值 auto`
控制是否使用[TPM保障包](https://www.volcengine.com/docs/82379/1510762)。取值范围：`auto`、`default`。
`auto`：本次请求优先使用 TPM 保障包额度。
有 TPM 保障包额度的推理接入点，本次请求将会优先使用 TPM 保障包额度，获得更高的服务等级（响应速度、可用性）。
无 TPM 保障包额度或用超额度的推理接入点，维持默认的服务等级。
`default`：本次请求不使用 TPM 保障包，维持默认的服务等级（即使推理接入点有TPM保障包额度）。
**stop**`string / string[] / null``默认值 null`
模型遇到 stop 字段所指定的字符串时将停止继续生成，这个词语本身不会输出。最多支持 4 个字符串。
[深度思考能力模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3%E8%83%BD%E5%8A%9B)不支持该字段。
`["你好", "天气"]`
**reasoning_effort**`string / null``默认值 medium`
支持字段的模型、使用说明与 **thinking.type** 字段关系见[文档](https://www.volcengine.com/docs/82379/1449737#%E8%B0%83%E8%8A%82%E6%80%9D%E8%80%83%E9%95%BF%E5%BA%A6)。
限制思考的工作量。减少思考深度可提升速度，思考花费的 token 更少。
取值范围：`minimal`，`low`，`medium`，`high`。
`minimal`：关闭思考，直接回答。
`low`：轻量思考，侧重快速响应。
`medium`：均衡模式，兼顾速度与深度。
`high`：深度分析，处理复杂问题。
**response_format**`object``默认值 {"type": "text"}``beta阶段`
指定模型回答格式。
**frequency_penalty**`float / null``默认值 0`
取值范围为 [`-2.0`, `2.0`]。
频率惩罚系数。如值为正，根据新 token 在文本中的出现频率对其进行惩罚，从而降低模型逐字重复的可能性。
**presence_penalty**`float / null``默认值 0`
取值范围为 [`-2.0`, `2.0`]。
存在惩罚系数。如果值为正，会根据新 token 到目前为止是否出现在文本中对其进行惩罚，从而增加模型谈论新主题的可能性。
**temperature**`float / null``默认值 1`
取值范围为 [`0`, `2`]。
采样温度。控制了生成文本时对每个候选词的概率分布进行平滑的程度。当取值为 0 时模型仅考虑对数概率最大的一个 token。
较高的值（如 0.8）会使输出更加随机，而较低的值（如 0.2）会使输出更加集中确定。
通常建议仅调整 temperature 或 top_p 其中之一，不建议两者都修改。
**top_p**`float / null``默认值 0.7`
取值范围为 [`0`, `1`]。
核采样概率阈值。模型会考虑概率质量在 top_p 内的 token 结果。当取值为 0 时模型仅考虑对数概率最大的一个 token。
0.1 意味着只考虑概率质量最高的前 10% 的 token，取值越大生成的随机性越高，取值越低生成的确定性越高。通常建议仅调整 temperature 或 top_p 其中之一，不建议两者都修改。
**logprobs**`boolean / null``默认值 false`
带深度思考能力模型不支持该字段，深度思考能力模型参见[文档](https://www.volcengine.com/docs/82379/1330310#%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83%E8%83%BD%E5%8A%9B)。
是否返回输出 tokens 的对数概率。
`false`：不返回对数概率信息。
`true`：返回消息内容中每个输出 token 的对数概率。
**top_logprobs**`integer / null``默认值 0`
带深度思考能力模型不支持该字段，深度思考能力模型参见[文档](https://www.volcengine.com/docs/82379/1330310#%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83%E8%83%BD%E5%8A%9B)。
取值范围为 [`0`, `20`]。
指定每个输出 token 位置最有可能返回的 token 数量，每个 token 都有关联的对数概率。仅当 **logprobs为**`true` 时可以设置 **top_logprobs** 参数。
**logit_bias**`map / null``默认值 null`
带深度思考能力模型不支持该字段，深度思考能力模型参见[文档](https://www.volcengine.com/docs/82379/1330310#%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83%E8%83%BD%E5%8A%9B)。
调整指定 token 在模型输出内容中出现的概率，使模型生成的内容更加符合特定的偏好。**logit_bias** 字段接受一个 map 值，其中每个键为词表中的 token ID（使用 tokenization 接口获取），每个值为该 token 的偏差值，取值范围为 [-100, 100]。
-1 会减少选择的可能性，1 会增加选择的可能性；-100 会完全禁止选择该 token，100 会导致仅可选择该 token。该参数的实际效果可能因模型而异。
`{"1234": -100}`
**tools**`object[] / null``默认值 null`
待调用工具的列表，模型返回信息中可包含。当您需要让模型返回待调用工具时，需要配置该结构体。支持该字段的模型请参见[文档](https://www.volcengine.com/docs/82379/1330310#%E5%B7%A5%E5%85%B7%E8%B0%83%E7%94%A8%E8%83%BD%E5%8A%9B)。
**parallel_tool_calls**`boolean``默认值 true`
本次请求，模型返回是否允许包含多个待调用的工具。
`true`：允许返回多个待调用的工具。
`false`：允许返回的待调用的工具小于等于1，本取值当前仅 `doubao-seed-1-6-***` 模型生效。
**tool_choice**`string / object`
仅 `doubao-seed-1-6-***` 及之后系代模型支持此字段。
本次请求，模型返回信息中是否有待调用的工具。
当没有指定工具时，`none` 是默认值。如果存在工具，则 `auto` 是默认值。
响应参数
跳转 [请求参数](#RxN8G2nH)
非流式调用返回
跳转 [流式调用返回](#jp88SeXS)
**id**`string`
本次请求的唯一标识。
**model**`string`
本次请求实际使用的模型名称和版本。
**service_tier**`string`
本次请求是否使用了TPM保障包。
`scale`：本次请求使用TPM保障包额度。
`default`：本次请求未使用TPM保障包额度。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `chat.completion`。
**choices**`object[]`
本次请求的模型输出内容。
**usage**`object`
本次请求的 token 用量。
流式调用返回
跳转 [非流式调用返回](#fT1TMaZk)
**id**`string`
本次请求的唯一标识。
**model**`string`
本次请求实际使用的模型名称和版本。
**service_tier**`string`
本次请求是否使用了TPM保障包。
`scale`：本次请求使用TPM保障包额度。
`default`：本次请求未使用TPM保障包额度。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `chat.completion.chunk`。
**choices**`object[]`
本次请求的模型输出内容。
**usage**`object`
本次请求的 token 用量。
流式调用时，默认不统计 token 用量信息，返回值为`null`。
如需统计，需设置 **stream_options.include_usage**为`true`。

## 示例代码

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-1-5-pro-32k-250115",
    "messages": [
        {
            "role": "system",
            "content": "You are a helpful assistant."
        },
        {
            "role": "user",
            "content": "Hello!"
        }
    ]
  }'
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "Hello! How can I help you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1742631811,
  "id": "0217426318107460cfa43dc3f3683b1de1c09624ff49085a456ac",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 19,
    "total_tokens": 28,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**python**

```python
import os
from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

completion = client.chat.completions.create(
    model="doubao-1-5-pro-32k-250115",
    messages=[
        {"role": "user", "content": "You are a helpful assistant."}
    ]
)
print(completion.choices[0].message)
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "Hello! How can I help you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1742631811,
  "id": "0217426318107460cfa43dc3f3683b1de1c09624ff49085a456ac",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 19,
    "total_tokens": 28,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
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
  // 从环境变量中获取您的API KEY，配置方法见：https://www.volcengine.com/docs/82379/1399008
	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
	ctx := context.Background()
	req := model.CreateChatCompletionRequest{
		Model: "doubao-1-5-pro-32k-250115",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("You are a helpful assistant."),
				},
			},
		},
	}
	resp, err := client.CreateChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	fmt.Println(*resp.Choices[0].Message.Content.StringValue)

}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "Hello! How can I help you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1742631811,
  "id": "0217426318107460cfa43dc3f3683b1de1c09624ff49085a456ac",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 19,
    "total_tokens": 28,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest.*;
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
    //从环境变量中获取您的API KEY，配置方法见：https://www.volcengine.com/docs/82379/1399008
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

        List<ChatMessage> messagesForReqList = new ArrayList<>();

        ChatMessage elementForMessagesForReqList0 =
                ChatMessage.builder()
                        .role(ChatMessageRole.USER)
                        .content("You are a helpful assistant.")
                        .build();
        messagesForReqList.add(elementForMessagesForReqList0);

        ChatCompletionRequest req =
                ChatCompletionRequest.builder()
                        .model("doubao-1-5-pro-32k-250115")
                        .messages(messagesForReqList)
                        .build();

        service.createChatCompletion(req)
                .getChoices()
                .forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "Hello! How can I help you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1742631811,
  "id": "0217426318107460cfa43dc3f3683b1de1c09624ff49085a456ac",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 19,
    "total_tokens": 28,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "messages": [
        {
            "content": "You are a helpful assistant.",
            "role": "system"
        },
        {
            "content": "hello",
            "role": "user"
        }
    ],
    "model": "doubao-1-5-pro-32k-250115",
    "stream": true
}'
```

**Response:**

```json
{"choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"!","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" How","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" can","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" I","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" help","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" you","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" today","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"?","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

[DONE]
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.chat.completions.create(
        model="doubao-1-5-pro-32k-250115",
        messages=[{"content":"You are a helpful assistant.","role":"system"},{"content":"hello","role":"user"}],
        stream=True,
    )
    for chunk in resp:
        if not chunk.choices:
            continue

        print(chunk.choices[0].delta.content, end="")

```

**Response:**

```json
{"choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"!","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" How","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" can","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" I","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" help","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" you","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" today","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"?","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

[DONE]
```

**go**

```go
package main

import (
	"context"
	"fmt"
	"io"
	"os"

	"github.com/volcengine/volcengine-go-sdk/service/arkruntime"
	"github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
	"github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
	ctx := context.Background()
	req := model.CreateChatCompletionRequest{
		Model: "doubao-1-5-pro-32k-250115",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "system",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("You are a helpful assistant."),
				},
			},
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("hello"),
				},
			},
		},
		Stream: volcengine.Bool(true),
	}
	fmt.Println("----- streaming request -----")
	stream, err := client.CreateChatCompletionStream(ctx, req)
	if err != nil {
		fmt.Printf("stream chat error: %v\n", err)
		return
	}
	defer stream.Close()

	for {
		recv, err := stream.Recv()
		if err == io.EOF {
			return
		}
		if err != nil {
			fmt.Printf("Stream chat error: %v\n", err)
			return
		}
		if len(recv.Choices) > 0 {
			fmt.Print(recv.Choices[0].Delta.Content)
		}
	}

}

```

**Response:**

```json
{"choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"!","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" How","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" can","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" I","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" help","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" you","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" today","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"?","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

[DONE]
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest.*;
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

        List<ChatMessage> messagesForReqList = new ArrayList<>();

        ChatMessage elementForMessagesForReqList0 =
                ChatMessage.builder()
                        .role(ChatMessageRole.SYSTEM)
                        .content("You are a helpful assistant.")
                        .build();

        ChatMessage elementForMessagesForReqList1 =
                ChatMessage.builder().role(ChatMessageRole.USER).content("hello").build();

        messagesForReqList.add(elementForMessagesForReqList0);
        messagesForReqList.add(elementForMessagesForReqList1);

        ChatCompletionRequest req =
                ChatCompletionRequest.builder()
                        .model("doubao-1-5-pro-32k-250115")
                        .messages(messagesForReqList)
                        .stream(true)
                        .build();

        service.streamChatCompletion(req)
                .doOnError(Throwable::printStackTrace)
                .blockingForEach(
                        choice -> {
                            if (choice.getChoices().size() > 0) {
                                System.out.print(
                                        choice.getChoices().get(0).getMessage().getContent());
                            }
                        });
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{"choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"!","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" How","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" can","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" I","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" help","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" you","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":" today","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"?","role":"assistant"},"index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

{"choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1742632436,"id":"021742632435712396f12d018b5d576a7a55349c2eba0815061fc","model":"doubao-1-5-pro-32k-250115","service_tier":"default","object":"chat.completion.chunk","usage":null}

[DONE]
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "messages": [
        {
            "content": "你是一个计算器，请计算： 1 + 1",
            "role": "user"
        },
        {
            "content": "=",
            "role": "assistant"
        }
    ],
    "model": "doubao-1-5-pro-32k-250115"
}'
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": " 2",
        "role": "assistant"
      }
    }
  ],
  "created": 1742634165,
  "id": "0217426341647344ee2a242cadeb3c7acc981f0bd805884bc65fc",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 2,
    "prompt_tokens": 21,
    "total_tokens": 23,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

if __name__ == "__main__":
    resp = client.chat.completions.create(
        model="doubao-1-5-pro-32k-250115",
        messages=[{"content":"你是一个计算器，请计算： 1 + 1","role":"user"},{"content":"=","role":"assistant"}],
    )
    print(resp.choices[0].message.content)

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": " 2",
        "role": "assistant"
      }
    }
  ],
  "created": 1742634165,
  "id": "0217426341647344ee2a242cadeb3c7acc981f0bd805884bc65fc",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 2,
    "prompt_tokens": 21,
    "total_tokens": 23,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
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
	req := model.CreateChatCompletionRequest{
		Model: "doubao-1-5-pro-32k-250115",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("你是一个计算器，请计算： 1 + 1"),
				},
			},
			&model.ChatCompletionMessage{
				Role: "assistant",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("="),
				},
			},
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	fmt.Println(*resp.Choices[0].Message.Content.StringValue)

}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": " 2",
        "role": "assistant"
      }
    }
  ],
  "created": 1742634165,
  "id": "0217426341647344ee2a242cadeb3c7acc981f0bd805884bc65fc",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 2,
    "prompt_tokens": 21,
    "total_tokens": 23,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest.*;
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

        List<ChatMessage> messagesForReqList = new ArrayList<>();

        ChatMessage elementForMessagesForReqList0 =
                ChatMessage.builder()
                        .role(ChatMessageRole.USER)
                        .content("你是一个计算器，请计算： 1 + 1")
                        .build();

        ChatMessage elementForMessagesForReqList1 =
                ChatMessage.builder().role(ChatMessageRole.ASSISTANT).content("=").build();

        messagesForReqList.add(elementForMessagesForReqList0);
        messagesForReqList.add(elementForMessagesForReqList1);

        ChatCompletionRequest req =
                ChatCompletionRequest.builder()
                        .model("doubao-1-5-pro-32k-250115")
                        .messages(messagesForReqList)
                        .build();

        service.createChatCompletion(req)
                .getChoices()
                .forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": " 2",
        "role": "assistant"
      }
    }
  ],
  "created": 1742634165,
  "id": "0217426341647344ee2a242cadeb3c7acc981f0bd805884bc65fc",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 2,
    "prompt_tokens": 21,
    "total_tokens": 23,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "doubao-1-5-vision-pro-32k-250115",
    "messages": [
        {
            "content": [
                {
                    "image_url": {
                        "url": "https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg"
                    },
                    "type": "image_url"
                },
                {
                    "text": "图片主要讲了什么?",
                    "type": "text"
                }
            ],
            "role": "user"
        }
    ]
}'
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "画面中呈现了一幅宁静的户外景象。一个人乘坐在橙黄色的皮划艇上，手持船桨，正在平静的水面划行。水面如镜，倒映着周围景致。远处是茂密的森林，森林后方矗立着巍峨的雪山，山体覆盖着白雪。天空呈浅蓝色，飘浮着一些云朵。整体氛围静谧而美好，展现出自然的纯净与壮阔。 ",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636149,
  "id": "0217426361458116592a076493be583bc5e33f80ac2dcf1efc31b",
  "model": "doubao-1-5-vision-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 85,
    "prompt_tokens": 521,
    "total_tokens": 606,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.chat.completions.create(
        model="doubao-1-5-vision-pro-32k-250115",
        messages=[{"content":[{"image_url":{"url":"https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg"},"type":"image_url"},{"text":"图片主要讲了什么?","type":"text"}],"role":"user"}],
    )
    print(resp.choices[0].message.content)

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "画面中呈现了一幅宁静的户外景象。一个人乘坐在橙黄色的皮划艇上，手持船桨，正在平静的水面划行。水面如镜，倒映着周围景致。远处是茂密的森林，森林后方矗立着巍峨的雪山，山体覆盖着白雪。天空呈浅蓝色，飘浮着一些云朵。整体氛围静谧而美好，展现出自然的纯净与壮阔。 ",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636149,
  "id": "0217426361458116592a076493be583bc5e33f80ac2dcf1efc31b",
  "model": "doubao-1-5-vision-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 85,
    "prompt_tokens": 521,
    "total_tokens": 606,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
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
	req := model.CreateChatCompletionRequest{
		Model: "doubao-1-5-vision-pro-32k-250115",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					ListValue: []*model.ChatCompletionMessageContentPart{
						&model.ChatCompletionMessageContentPart{
							Type: "image_url",
							ImageURL: &model.ChatMessageImageURL{
								URL: "https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg",
							},
						},
						&model.ChatCompletionMessageContentPart{
							Type: "text",
							Text: "图片主要讲了什么?",
						},
					},
				},
			},
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	fmt.Println(*resp.Choices[0].Message.Content.StringValue)
}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "画面中呈现了一幅宁静的户外景象。一个人乘坐在橙黄色的皮划艇上，手持船桨，正在平静的水面划行。水面如镜，倒映着周围景致。远处是茂密的森林，森林后方矗立着巍峨的雪山，山体覆盖着白雪。天空呈浅蓝色，飘浮着一些云朵。整体氛围静谧而美好，展现出自然的纯净与壮阔。 ",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636149,
  "id": "0217426361458116592a076493be583bc5e33f80ac2dcf1efc31b",
  "model": "doubao-1-5-vision-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 85,
    "prompt_tokens": 521,
    "total_tokens": 606,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionContentPart.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest.*;
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

        List<ChatMessage> messagesForReqList = new ArrayList<>();

        List<ChatCompletionContentPart> contentForElementForMessagesForReqList0 = new ArrayList<>();

        ChatCompletionContentPartImageURL
                imageUrlForElementForContentForElementForMessagesForReqList00 =
                        new ChatCompletionContentPartImageURL();
        imageUrlForElementForContentForElementForMessagesForReqList00.setUrl(
                "https://ark-project.tos-cn-beijing.volces.com/images/view.jpeg");
        ChatCompletionContentPart elementForContentForElementForMessagesForReqList00 =
                new ChatCompletionContentPart();
        elementForContentForElementForMessagesForReqList00.setType("image_url");
        elementForContentForElementForMessagesForReqList00.setImageUrl(
                imageUrlForElementForContentForElementForMessagesForReqList00);
        ChatCompletionContentPart elementForContentForElementForMessagesForReqList01 =
                new ChatCompletionContentPart();
        elementForContentForElementForMessagesForReqList01.setType("text");
        elementForContentForElementForMessagesForReqList01.setText("图片主要讲了什么?");
        contentForElementForMessagesForReqList0.add(
                elementForContentForElementForMessagesForReqList00);
        contentForElementForMessagesForReqList0.add(
                elementForContentForElementForMessagesForReqList01);

        ChatMessage elementForMessagesForReqList0 =
                ChatMessage.builder()
                        .role(ChatMessageRole.USER)
                        .multiContent(contentForElementForMessagesForReqList0)
                        .build();
        messagesForReqList.add(elementForMessagesForReqList0);

        ChatCompletionRequest req =
                ChatCompletionRequest.builder()
                        .model("doubao-1-5-vision-pro-32k-250115")
                        .messages(messagesForReqList)
                        .build();

        service.createChatCompletion(req)
                .getChoices()
                .forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }
}

```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "画面中呈现了一幅宁静的户外景象。一个人乘坐在橙黄色的皮划艇上，手持船桨，正在平静的水面划行。水面如镜，倒映着周围景致。远处是茂密的森林，森林后方矗立着巍峨的雪山，山体覆盖着白雪。天空呈浅蓝色，飘浮着一些云朵。整体氛围静谧而美好，展现出自然的纯净与壮阔。 ",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636149,
  "id": "0217426361458116592a076493be583bc5e33f80ac2dcf1efc31b",
  "model": "doubao-1-5-vision-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 85,
    "prompt_tokens": 521,
    "total_tokens": 606,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "messages": [

        {
            "content": "我要有研究推理模型与非推理模型区别的课题，怎么体现我的专业性",
            "role": "user"
        }
    ],
    "model": "deepseek-r1-250120"
}'
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "\n\n推理模型与非推理模型的主要区别在于其处理任务的方式、能力侧重点以及应用场景。以下是两者的核心区别分析：\n\n---\n\n### **1. 核心定义**\n- **推理模型**  \n  具备**逻辑推理、因果推断或复杂决策**能力，***  \n\n实际应用中，两类模型常配合使用（如用非推理模型提取特征，再交给推理模型决策），以平衡效率与准确性。",
        "reasoning_content": "嗯，用户问的是推理模型和非推理模型有什么区别。我需要先理解这两个术语的具体含义。可能用户对机器学习或深度学习有一定的了解，但需要更清晰的区分。首先，我应该明确这两个概念的定义，然后从不同角度进行比较。\n\n首先，推理模型可能指的是那些需要进行逻辑推理、处理复杂任务的模型，***，而非推理模型侧重于模式识别和直接映射，帮助用户形成清晰的概念框架。\n",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636537,
  "id": "0217426364949596592a076493be583bc5e33f80ac2dcf1eeaf9b",
  "model": "deepseek-r1-250120",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1207,
    "prompt_tokens": 11,
    "total_tokens": 1218,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 419
    }
  }
}
```

**python**

```python
import os
# 升级方舟 SDK 到最新版本 pip install -U 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark

client = Ark(
    # 从环境变量中读取您的方舟API Key
    api_key=os.environ.get("ARK_API_KEY"), 
    # 深度推理模型耗费时间会较长，请您设置较大的超时时间，避免超时，推荐30分钟以上
    timeout=1800,
    )
response = client.chat.completions.create(
    # 替换 <Model> 为模型的Model ID
    model="deepseek-r1-250120",
    messages=[
        {"role": "user", "content": "我要有研究推理模型与非推理模型区别的课题，怎么体现我的专业性"}
    ]
)
# 当触发深度推理时，打印思维链内容
if hasattr(response.choices[0].message, 'reasoning_content'):
    print(response.choices[0].message.reasoning_content)
print(response.choices[0].message.content)
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "\n\n推理模型与非推理模型的主要区别在于其处理任务的方式、能力侧重点以及应用场景。以下是两者的核心区别分析：\n\n---\n\n### **1. 核心定义**\n- **推理模型**  \n  具备**逻辑推理、因果推断或复杂决策**能力，***  \n\n实际应用中，两类模型常配合使用（如用非推理模型提取特征，再交给推理模型决策），以平衡效率与准确性。",
        "reasoning_content": "嗯，用户问的是推理模型和非推理模型有什么区别。我需要先理解这两个术语的具体含义。可能用户对机器学习或深度学习有一定的了解，但需要更清晰的区分。首先，我应该明确这两个概念的定义，然后从不同角度进行比较。\n\n首先，推理模型可能指的是那些需要进行逻辑推理、处理复杂任务的模型，***，而非推理模型侧重于模式识别和直接映射，帮助用户形成清晰的概念框架。\n",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636537,
  "id": "0217426364949596592a076493be583bc5e33f80ac2dcf1eeaf9b",
  "model": "deepseek-r1-250120",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1207,
    "prompt_tokens": 11,
    "total_tokens": 1218,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 419
    }
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
    "time"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        //通过 os.Getenv 从环境变量中获取 ARK_API_KEY
        os.Getenv("ARK_API_KEY"),
        //深度推理模型耗费时间会较长，请您设置较大的超时时间，避免超时导致任务失败。推荐30分钟以上
        arkruntime.WithTimeout(30*time.Minute),
    )
    // 创建一个上下文，通常用于传递请求的上下文信息，如超时、取消等
    ctx := context.Background()
    // 构建聊天完成请求，设置请求的模型和消息内容
    req := model.ChatCompletionRequest{
        // 需要替换 <Model> 为模型的Model ID
        model: "deepseek-r1-250120",
        Messages: []*model.ChatCompletionMessage{
            {
                // 消息的角色为用户
                Role: model.ChatMessageRoleUser,
                Content: &model.ChatCompletionMessageContent{
                    StringValue: volcengine.String("我要有研究推理模型与非推理模型区别的课题，怎么体现我的专业性"),
                },
            },
        },
    }

    // 发送聊天完成请求，并将结果存储在 resp 中，将可能出现的错误存储在 err 中
    resp, err := client.CreateChatCompletion(ctx, req)
    if err != nil {
        // 若出现错误，打印错误信息并终止程序
        fmt.Printf("standard chat error: %v\n", err)
        return
    }
    // 检查是否触发深度推理，触发则打印思维链内容
    if resp.Choices[0].Message.ReasoningContent != nil {
        fmt.Println(*resp.Choices[0].Message.ReasoningContent)
    }
    // 打印聊天完成请求的响应结果
    fmt.Println(*resp.Choices[0].Message.Content.StringValue)
}
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "\n\n推理模型与非推理模型的主要区别在于其处理任务的方式、能力侧重点以及应用场景。以下是两者的核心区别分析：\n\n---\n\n### **1. 核心定义**\n- **推理模型**  \n  具备**逻辑推理、因果推断或复杂决策**能力，***  \n\n实际应用中，两类模型常配合使用（如用非推理模型提取特征，再交给推理模型决策），以平衡效率与准确性。",
        "reasoning_content": "嗯，用户问的是推理模型和非推理模型有什么区别。我需要先理解这两个术语的具体含义。可能用户对机器学习或深度学习有一定的了解，但需要更清晰的区分。首先，我应该明确这两个概念的定义，然后从不同角度进行比较。\n\n首先，推理模型可能指的是那些需要进行逻辑推理、处理复杂任务的模型，***，而非推理模型侧重于模式识别和直接映射，帮助用户形成清晰的概念框架。\n",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636537,
  "id": "0217426364949596592a076493be583bc5e33f80ac2dcf1eeaf9b",
  "model": "deepseek-r1-250120",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1207,
    "prompt_tokens": 11,
    "total_tokens": 1218,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 419
    }
  }
}
```

**java**

```java
package com.example;

import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessage;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessageRole;
import com.volcengine.ark.runtime.service.ArkService;

import java.time.Duration;
import java.util.ArrayList;
import java.util.List;

/**
 * 这是一个示例类，展示了如何使用ArkService来完成聊天功能。
 */
public class ChatCompletionsExample {
    public static void main(String[] args) {
        // 从环境变量中获取API密钥
        String apiKey = System.getenv("ARK_API_KEY");
        // 创建ArkService实例
        ArkService arkService = ArkService.builder().apiKey(apiKey)
                .timeout(Duration.ofMinutes(30))// 深度推理模型耗费时间会较长，请您设置较大的超时时间，避免超时导致任务失败。推荐30分钟以上
                .build();
        // 初始化消息列表
        List<ChatMessage> chatMessages = new ArrayList<>();
        // 创建用户消息
        ChatMessage userMessage = ChatMessage.builder()
                .role(ChatMessageRole.USER) // 设置消息角色为用户
                .content("我要有研究推理模型与非推理模型区别的课题，怎么体现我的专业性") // 设置消息内容
                .build();
        // 将用户消息添加到消息列表
        chatMessages.add(userMessage);
        // 创建聊天完成请求
        ChatCompletionRequest chatCompletionRequest = ChatCompletionRequest.builder()
                .model("deepseek-r1-250120")
                .messages(chatMessages) // 设置消息列表
                .build();
        // 发送聊天完成请求并打印响应
        try {
            // 获取响应并打印每个选择的消息内容
            arkService.createChatCompletion(chatCompletionRequest)
                    .getChoices()
                    .forEach(choice -> {                    
                        // 校验是否触发了深度思考，打印思维链内容
                        if (choice.getMessage().getReasoningContent() != null) {
                            System.out.println("推理内容: " + choice.getMessage().getReasoningContent());
                        } else {
                            System.out.println("推理内容为空");
                        }
                        // 打印消息内容
                        System.out.println("消息内容: " + choice.getMessage().getContent());
                    });
        } catch (Exception e) {
            System.out.println("请求失败: " + e.getMessage());
        } finally {
            // 关闭服务执行器
            arkService.shutdownExecutor();
        }
    }
}
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "\n\n推理模型与非推理模型的主要区别在于其处理任务的方式、能力侧重点以及应用场景。以下是两者的核心区别分析：\n\n---\n\n### **1. 核心定义**\n- **推理模型**  \n  具备**逻辑推理、因果推断或复杂决策**能力，***  \n\n实际应用中，两类模型常配合使用（如用非推理模型提取特征，再交给推理模型决策），以平衡效率与准确性。",
        "reasoning_content": "嗯，用户问的是推理模型和非推理模型有什么区别。我需要先理解这两个术语的具体含义。可能用户对机器学习或深度学习有一定的了解，但需要更清晰的区分。首先，我应该明确这两个概念的定义，然后从不同角度进行比较。\n\n首先，推理模型可能指的是那些需要进行逻辑推理、处理复杂任务的模型，***，而非推理模型侧重于模式识别和直接映射，帮助用户形成清晰的概念框架。\n",
        "role": "assistant"
      }
    }
  ],
  "created": 1742636537,
  "id": "0217426364949596592a076493be583bc5e33f80ac2dcf1eeaf9b",
  "model": "deepseek-r1-250120",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1207,
    "prompt_tokens": 11,
    "total_tokens": 1218,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 419
    }
  }
}
```

**curl**

```bash
curl --location "https://ark.cn-beijing.volces.com/api/v3/chat/completions" \
--header "Authorization: Bearer $ARK_API_KEY" \
--header "Content-Type: application/json" \
--data '{
 "model": "doubao-seed-1.6-250615",
     "messages": [
         {
             "role": "user",
             "content": [
                 {
                     "type":"text",
                     "text":"我要研究深度思考模型与非深度思考模型区别的课题，体现出我的专业性"
                 }
             ]
         }
     ],
     "thinking":{
         "type":"disabled"
     }
}'
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "研究“深度思考模型与非深度思考模型的区别”是一个兼具理论深度与实践意义的课题，...",
        "role": "assistant"
      }
    }
  ],
  "created": 1750342275,
  "id": "0217503421709253ddc3c2b93f219d1c79fa57be98ff8fa5ef2ba",
  "model": "doubao-seed-1-6-250615",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 3178,
    "prompt_tokens": 54,
    "total_tokens": 3232,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 }
  }
}
```

**python**

```python
import os
# 升级方舟 SDK 到最新版本 pip install -U 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark

client = Ark(
    # 从环境变量中读取您的方舟API Key
    api_key=os.environ.get("ARK_API_KEY"), 
    # 深度思考模型耗费时间会较长，请您设置较大的超时时间，避免超时，推荐30分钟以上
    timeout=1800,
    )
response = client.chat.completions.create(
    # 替换 <Model> 为您的Model ID
    model="doubao-seed-1.6-250615",
    messages=[
        {"role": "user", "content": "我要研究深度思考模型与非深度思考模型区别的课题，体现出我的专业性"}
    ],
     thinking={
         "type": "disabled" # 不使用深度思考能力,
         # "type": "enabled" # 使用深度思考能力
         # "type": "auto" # 模型自行判断是否使用深度思考能力
     },
)

print(response)
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "研究“深度思考模型与非深度思考模型的区别”是一个兼具理论深度与实践意义的课题，...",
        "role": "assistant"
      }
    }
  ],
  "created": 1750342275,
  "id": "0217503421709253ddc3c2b93f219d1c79fa57be98ff8fa5ef2ba",
  "model": "doubao-seed-1-6-250615",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 3178,
    "prompt_tokens": 54,
    "total_tokens": 3232,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 }
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
    "time"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        //从环境变量中获取 ARK_API_KEY
        os.Getenv("ARK_API_KEY"),
        //深度思考模型耗费时间会较长，请您设置较大的超时时间，避免超时导致任务失败。推荐30分钟以上
        arkruntime.WithTimeout(30*time.Minute),
    )
    // 创建一个上下文，通常用于传递请求的上下文信息，如超时、取消等
    ctx := context.Background()
    // 构建聊天完成请求，设置请求的模型和消息内容
    req := model.CreateChatCompletionRequest{
        // 需要替换 <Model> 为您的 Model ID
        Model: "doubao-seed-1.6-250615",
        Messages: []*model.ChatCompletionMessage{
            {
                // 消息的角色为用户
                Role: model.ChatMessageRoleUser,
                Content: &model.ChatCompletionMessageContent{
                    StringValue: volcengine.String("我要研究深度思考模型与非深度思考模型区别的课题，怎么体现我的专业性"),
                },
            },
        },
        Thinking: &model.Thinking{
            Type: model.ThinkingTypeDisabled, // 关闭深度思考能力
            // Type: model.ThinkingTypeEnabled, //开启深度思考能力
            // Type: model.ThinkingTypeAuto, //开启深度思考能力
        },
    }

    // 发送聊天完成请求，并将结果存储在 resp 中，将可能出现的错误存储在 err 中
    resp, err := client.CreateChatCompletion(ctx, req)
    if err != nil {
        // 若出现错误，打印错误信息并终止程序
        fmt.Printf("standard chat error: %v\n", err)
        return
    }
    // 检查是否触发深度思考，触发则打印思维链内容
    if resp.Choices[0].Message.ReasoningContent != nil {
        fmt.Println(*resp.Choices[0].Message.ReasoningContent)
    }
    // 打印聊天完成请求的响应结果
    fmt.Println(*resp.Choices[0].Message.Content.StringValue)
}
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "研究“深度思考模型与非深度思考模型的区别”是一个兼具理论深度与实践意义的课题，...",
        "role": "assistant"
      }
    }
  ],
  "created": 1750342275,
  "id": "0217503421709253ddc3c2b93f219d1c79fa57be98ff8fa5ef2ba",
  "model": "doubao-seed-1-6-250615",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 3178,
    "prompt_tokens": 54,
    "total_tokens": 3232,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 }
  }
}
```

**java**

```java
package com.example;

import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessage;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessageRole;
import com.volcengine.ark.runtime.service.ArkService;

import java.time.Duration;
import java.util.ArrayList;
import java.util.List;

/**
 * 这是一个示例类，展示了如何使用ArkService来完成聊天功能。
 */
public class ChatCompletionsExample {
    public static void main(String[] args) {
        // 从环境变量中获取API密钥
        String apiKey = System.getenv("ARK_API_KEY");
        // 创建ArkService实例
        ArkService arkService = ArkService.builder().apiKey(apiKey)
                .timeout(Duration.ofMinutes(30))// 深度思考模型耗费时间会较长，请您设置较大的超时时间，避免超时导致任务失败。推荐30分钟以上
                .build();
        // 初始化消息列表
        List<ChatMessage> chatMessages = new ArrayList<>();
        // 创建用户消息
        ChatMessage userMessage = ChatMessage.builder()
                .role(ChatMessageRole.USER) // 设置消息角色为用户
                .content("我要研究深度思考模型与非深度思考模型区别的课题，怎么体现我的专业性") // 设置消息内容
                .build();
        // 将用户消息添加到消息列表
        chatMessages.add(userMessage);
        // 创建聊天完成请求
        ChatCompletionRequest chatCompletionRequest = ChatCompletionRequest.builder()
                .model("doubao-seed-1.6-250615")
                .messages(chatMessages) // 设置消息列表
                .thinking(new ChatCompletionRequest.ChatCompletionRequestThinking("disabled"))
                .build();
        // 发送聊天完成请求并打印响应
        try {
            // 获取响应并打印每个选择的消息内容
            arkService.createChatCompletion(chatCompletionRequest)
                    .getChoices()
                    .forEach(choice -> {                    
                        // 校验是否触发了深度思考，打印思维链内容
                        if (choice.getMessage().getReasoningContent() != null) {
                            System.out.println("推理内容: " + choice.getMessage().getReasoningContent());
                        } else {
                            System.out.println("推理内容为空");
                        }
                        // 打印消息内容
                        System.out.println("消息内容: " + choice.getMessage().getContent());
                    });
        } catch (Exception e) {
            System.out.println("请求失败: " + e.getMessage());
        } finally {
            // 关闭服务执行器
            arkService.shutdownExecutor();
        }
    }
}
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "研究“深度思考模型与非深度思考模型的区别”是一个兼具理论深度与实践意义的课题，...",
        "role": "assistant"
      }
    }
  ],
  "created": 1750342275,
  "id": "0217503421709253ddc3c2b93f219d1c79fa57be98ff8fa5ef2ba",
  "model": "doubao-seed-1-6-250615",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 3178,
    "prompt_tokens": 54,
    "total_tokens": 3232,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 }
  }
}
```

**OpenAI SDK**

```openai sdk
import os
from openai import OpenAI

client = OpenAI(
    # 从环境变量中读取您的方舟API Key
    api_key=os.environ.get("ARK_API_KEY"),
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    # 深度思考模型耗费时间会较长，请您设置较大的超时时间，避免超时，推荐30分钟以上
    timeout=1800,
)
response = client.chat.completions.create(
    # 替换 <Model> 为您的Model ID
    model="doubao-seed-1.6-250615",
    messages=[
        {
            "role": "user",
            "content": "我要研究深度思考模型与非深度思考模型区别的课题，体现出我的专业性",
        }
    ],
    extra_body={
        "thinking": {
            "type": "disabled"  # 不使用深度思考能力
            # "type": "enabled" # 使用深度思考能力
            # "type": "auto" # 模型自行判断是否使用深度思考能力
        }
    },
)

print(response)
```

**Response:**

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "logprobs": null,
      "message": {
        "content": "研究“深度思考模型与非深度思考模型的区别”是一个兼具理论深度与实践意义的课题，...",
        "role": "assistant"
      }
    }
  ],
  "created": 1750342275,
  "id": "0217503421709253ddc3c2b93f219d1c79fa57be98ff8fa5ef2ba",
  "model": "doubao-seed-1-6-250615",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 3178,
    "prompt_tokens": 54,
    "total_tokens": 3232,
    "prompt_tokens_details": { "cached_tokens": 0 },
    "completion_tokens_details": { "reasoning_tokens": 0 }
  }
}
```

属性
messages.**role**`string``必选`
发送消息的角色，此处应为`system`。
messages.**content**`string / object[]``必选`
系统消息的内容。
纯文本内容 `string`
纯文本消息内容。
多模态内容 `object[]`
messages.tool_calls**.function **`object``必选`
模型返回的需调用的函数信息。
messages.tool_calls**.id **`string``必选`
需调用的工具的 ID，由模型生成。
messages.tool_calls**.type **`string``必选`
消息类型，当前仅支持`function`。
消息类型
系统消息 `object`
模型需遵循的指令，包括扮演的角色、背景信息等。
用户消息 `object`
用户角色发送的消息。不同模型支持的字段类型不同。
模型消息 `object`
历史对话中，模型角色返回的消息。用以保持对话一致性，多在[多轮对话](https://www.volcengine.com/docs/82379/1399009#%E5%A4%9A%E8%BD%AE%E5%AF%B9%E8%AF%9D)及[续写模式](https://www.volcengine.com/docs/82379/1359497)使用。
工具消息 `object`
历史对话中，调用工具返回的消息。工具调用场景中使用。
response_format.json_schema.**name**`string``必选`
用户自定义的JSON结构的名称。
response_format.json_schema.**description**`string / null`
回复用途描述，模型将根据此描述决定如何以该格式回复。
response_format.json_schema.**schema**`object``必选`
回复格式的 JSON 格式定义，以 JSON Schema 对象的形式描述。
response_format.json_schema.**strict**`boolean / null``默认值 false`
是否在生成输出时，启用严格遵循模式。
`true`：模型将始终严格遵循**schema**字段中定义的格式。
`false`：模型会尽可能遵循**schema**字段中定义的结构。
messages.content.video_url.**fps**`float/ null``默认值 1`
取值范围：`[0.2, 5]`
抽帧频率，详见[视频理解](https://www.volcengine.com/docs/82379/1895586)。
取值越高，对视频中画面变化越敏感。
取值越低，对视频中画面变化越迟钝，但 token 花费少，速度更快。
回答格式说明
文本格式 `object`
模型默认回复文本格式内容。
JSON Object 格式 `object`
模型回复内容以JSON对象结构来组织。
支持该字段的模型请参见[文档](https://www.volcengine.com/docs/82379/1568221#%E6%94%AF%E6%8C%81%E7%9A%84%E6%A8%A1%E5%9E%8B)。
该能力尚在 beta 阶段，请谨慎在生产环境使用。
JSON Schema 格式 `object``beta功能`
模型回复内容以JSON对象结构来组织，遵循 **schema **字段定义的JSON结构。
支持该字段的模型请参见[文档](https://www.volcengine.com/docs/82379/1568221#%E6%94%AF%E6%8C%81%E7%9A%84%E6%A8%A1%E5%9E%8B)。
该能力尚在 beta 阶段，请谨慎在生产环境使用。
tool_choice.**name**`string`
待调用工具的名称。
tool_choice.**type**`string`
调用的类型，此处应为 `function`。
choices.delta.tool_calls.**i****d **`string`
调用的工具的 ID。
choices.delta.tool_calls.**type **`string`
工具类型，当前仅支持`function`。
choices.delta.tool_calls.**function **`object`
模型调用的函数。
发送消息的角色，此处应为`tool`。
messages.**content**`string / array``必选`
工具返回的消息。
messages.**tool_call_id **`string``必选`
模型生成的需调用工具请求时，生成的ID。在程序调用工具的返回需要附上同一 ID，来关联工具结构与模型请求。避免多工具调用时混淆信息。
messages.**content**与 messages.**tool_calls**字段两种者至少填写其一。
发送消息的角色，此处应为`assistant`。
messages.**content**`string / array`
模型消息的内容。
messages.**tool_calls**`object[]`
模型消息中工具调用部分。
usage.prompt_tokens_details.**cached_tokens **`integer`
缓存输入内容的 token 用量，此处应为`0`。
usage.**total_tokens **`integer`
本次请求消耗的总 token 数量（输入 + 输出）。
usage.**prompt_tokens **`integer`
输入给模型处理的内容 token 数量。
usage.**prompt_tokens_details **`object`
输入给模型处理的内容 token 数量的细节。
usage.**completion_tokens **`integer`
模型输出内容花费的 token。
usage.**completion_tokens_details **`object`
模型输出内容花费的 token 的细节。
choices.message.**role **`string`
内容输出的角色，此处固定为 `assistant`。
choices.message.**content **`string`
模型生成的消息内容。
choices.message.**reasoning_content **`string / null`
模型处理问题的思维链内容。
仅深度推理模型支持返回此字段，深度推理模型请参见[支持模型](https://www.volcengine.com/docs/82379/1449737#5f0f3750)。
choices.message.**tool_calls **`object[] / null`
模型生成的工具调用。
usage.completion_tokens_details.**reasoning_tokens **`integer`
输出思维链内容花费的 token 数 。
支持输出思维链的模型请参见[文档](https://www.volcengine.com/docs/82379/1449737#5f0f3750)。
response_format.**type **`string``必选`
此处应为`json_schema`。
response_format.**json_schema**`object``必选`
JSON结构体的定义。
response_format.**type**`string``必选`
此处应为 `text`。
choices.**index **`integer`
当前元素在 **choices** 列表的索引。
choices.**finish_reason **`string`
模型停止生成 token 的原因。取值范围：
`stop`：模型输出自然结束，或因命中请求参数 stop 中指定的字段而被截断。
`length`：模型输出因达到模型输出限制而被截断，有以下原因：
触发`max_tokens`限制（回答内容的长度限制）。
触发`max_completion_tokens`限制（思维链内容+回答内容的长度限制）。
触发`context_window`限制（输入内容+思维链内容+回答内容的长度限制）。
`content_filter`：模型输出被内容审核拦截。
`tool_calls`：模型调用了工具。
choices.**message **`object`
模型输出的内容。
choices.**logprobs **`object / null`
当前内容的对数概率信息。
choices.**moderation_hit_type **`string``/ null`
模型输出文字含有敏感信息时，会返回模型输出文字命中的风险分类标签。
返回值及含义：
`severe_violation`：模型输出文字涉及严重违规。
`violence`：模型输出文字涉及激进行为。
注意：当前只有[视觉理解模型](https://www.volcengine.com/docs/82379/1362931#%E6%94%AF%E6%8C%81%E6%A8%A1%E5%9E%8B)支持返回该字段，且只有在方舟控制台[接入点配置页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint/create?customModelId=)或者 [CreateEndpoint](https://www.volcengine.com/docs/82379/1262823) 接口中，将内容护栏方案（ModerationStrategy）设置为基础方案（Basic）时，才会返回风险分类标签。
messages.content.video_url.**url **`string``必选`
支持格式如下，具体使用请参见[视频理解说明](https://www.volcengine.com/docs/82379/1895586)。
视频链接
视频的Base64编码
取值范围：`[0.2, 5]`。
messages.tool_calls**.**function.**name **`string``必选`
需调用的函数的名称。
messages.tool_calls**.**function.**arguments **`string``必选`
需调用的函数的入参，JSON 格式。
模型并不总是生成有效的 JSON，可能会虚构出未定义的参数。建议在调用函数前，验证参数是否有效。
choices.message.tool_calls.**id**`string`
choices.message.tool_calls.**type **`string`
choices.message.tool_calls.**function **`object`
choices.**delta **`object`
模型输出的增量内容。
choices.delta.tool_calls.function.**name **`string`
模型调用的函数的名称。
choices.delta.tool_calls.function.**arguments **`string`
模型生成的用于调用函数的参数，JSON 格式。
模型并不总是生成有效的 JSON，并且可能会虚构出一些您的函数参数规范中未定义的参数。在调用函数之前，请在您的代码中验证这些参数是否有效。
Tips：一键展开折叠，快速检索内容
打开页面右上角开关后，**ctrl **+ f 可检索页面内所有内容。
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_952f1a5ff1c9fc29c4642af62ee3d3ee.png)
tools.**type **`string``必选`
工具类型，此处应为 `function`。
tools.**function **`object``必选`
模型返回中可包含待调用的工具。
**属性**
choices.logprobs.content.top_logprobs.**token **`string`
当前 token。
choices.logprobs.content.top_logprobs.**bytes **`integer[] / null`
当前 token 的 UTF-8 值，格式为整数列表。当一个字符由多个 token 组成（表情符号或特殊字符等）时可以用于字符的编码和解码。如果 token 没有 UTF-8 值则为空。
choices.logprobs.content.top_logprobs.**logprob **`float`
当前 token 的对数概率。
messages.content.**type **`string``必选`
消息模态，此处应为 `image_url`。
messages.content.**image_url **`object``必选`
图片模态的内容。
choices.logprobs.**content **`object[] / null`
message列表中每个 content 元素中的 token 对数概率信息。
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b9c82890e851fc10cc31f48f9065abc6.png)[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/experience/chat)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_2abecd05ca2779567c6d32f0ddc7874d.png)[模型列表](https://www.volcengine.com/docs/82379/1330310)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a5fdd3028d35cc512a10bd71b982b6eb.png)[模型计费](https://www.volcengine.com/docs/82379/1544106)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_afbcf38bdec05c05089d5de5c3fd8fc8.png)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bef4bc3de3535ee19d0c5d6c37b0ffdd.png)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_57d0bca8e0d122ab1191b40101b5df75.png)[文本生成](https://www.volcengine.com/docs/82379/1399009)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_57d0bca8e0d122ab1191b40101b5df75.png)[视觉理解](https://www.volcengine.com/docs/82379/1362931)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f45b5cd5863d1eed3bc3c81b9af54407.png)[接口文档](https://www.volcengine.com/docs/82379/1494384)
{
"type":"object",
"properties":{
"参数名":{
"type":"string | number | boolean | object | array",
"description":"参数说明"
}
},
"required":["必填参数"]
}
内容类型
支持文本、图片、视频等模态内容。
choices.delta.**role **`string`
choices.delta.**content **`string`
choices.delta.**reasoning_content **`string / null`
choices.delta.**tool_calls **`object[] / null`
choices.message.tool_calls.function.**name **`string`
choices.message.tool_calls.function.**arguments **`string`
发送消息的角色，此处应为`user`。
用户信息内容。
此处应为`json_object`。
stream_options.**include_usage **`boolean / null``默认值 false`
模型流式输出时，是否在输出结束前输出本次请求的 token 用量信息。
`true`：在 `data: [DONE]` 消息之前会返回一个额外的 chunk。此 chunk 中， **usage** 字段中输出整个请求的 token 用量，**choices** 字段为空数组。
`false`：输出结束前，没有一个 chunk 来返回 token 用量信息。
stream_options.**chunk_include_usage **`boolean / null``默认值 false`
模型流式输出时，输出的每个 chunk 中是否输出本次请求到此 chunk 输出时刻的累计 token 用量信息。
`true`：在返回的 **usage** 字段中，输出本次请求到此 chunk 输出时刻的累计 token 用量。
`false`：不在每个 chunk 都返回 token 用量信息。
tools.function.**name **`string``必选`
调用的函数的名称。
tools.function.**description **`string`
调用的函数的描述，大模型会使用它来判断是否调用这个工具。
tools.function.**parameters **`object`
函数请求参数，以 JSON Schema 格式描述。具体格式请参考 [JSON Schema](https://json-schema.org/understanding-json-schema) 文档，格式如下：
其中，
所有字段名大小写敏感。
**parameters** 须是合规的 JSON Schema 对象。
建议用英文字段名，中文置于 **description** 字段中。
messages.content.image_url.**url **`string``必选`
支持格式如下，具体请参见[使用说明](https://www.volcengine.com/docs/82379/1362931#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)。
图片链接
图片的Base64编码
messages.content.image_url.**detail **`string / null``默认值 low`
取值范围：`high`、`low`。
理解图片的精细度，具体使用见[理解图片的深度控制](https://www.volcengine.com/docs/82379/1362931#bf4d9224)。
`high`：高细节模式，理解图片精细信息，如图片的多个局部信息/特征提取、复杂/丰富细节的图片理解等，更细节，耗时更长。此时 **min_pixels **取值`3136`、**max_pixels **取值`4014080`。
`low`：理解图片概略信息，如图片分类/识别、整体内容理解/描述，延时低、花费少。此时 **min_pixels **取值`3136`、**max_pixels **取值`1048576`。
messages.content.image_url.**image_pixel_limit  **`object / null``默认值 null`
输入给模型的图片的像素范围，如不在此范围，图片会被等比例缩放至该范围。
注意
输入给平台的图片的像素需在 [196, 36,000,000]，否则会直接返回错误。
生效优先级：高于 **detail **字段，即同时配置 **detail **与 **image_pixel_limit **字段时，生效 **image_pixel_limit **字段配置**。**
默认生效规则：若 **min_pixels **/ **max_pixels **字段未设置，使用 **detail **设置配置的值对应的 **min_pixels **/ **max_pixels **值。
子字段取值逻辑：`3136`≤ **min_pixels **≤ **max_pixels **≤ `4014080`
messages.content.image_url.image_pixel_limit.**max_pixels **`integer`
取值范围：(**min_pixels**,  `4014080`]。
传入图片最大像素限制，大于此像素则等比例缩小至 **max_pixels **字段取值以下。
若未设置，则取值为 **detail **设置配置的值对应的 **max_pixels **值`4014080`或`1048576`。
messages.content.image_url.image_pixel_limit.**min_pixels **`integer`
取值范围：[`3136`,  **max_pixels**)。
传入图片最小像素限制，小于此像素则等比例放大至 **min_pixels **字段取值以上。
若未设置，则取值为 **detail **设置配置的值对应的 **min_pixels **值（`3136`）。
thinking.**type **`string``必选`
取值范围：`enabled`， `disabled`，`auto`。
`enabled`：开启思考模式，模型强制先思考再回答。
`disabled`：关闭思考模式，模型直接回答问题，不进行思考。
`auto`：自动思考模式，模型根据问题自主判断是否需要思考，简单题目直接回答。
支持格式如下，详细信息请参见[使用说明](https://www.volcengine.com/docs/82379/1362931#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)。
messages.content.image_url.**detail **`string``默认值 low`
对图片模态内容理解的精细度，取值如下。
`high`：高细节模式，理解图片精细信息，如图片的多个局部信息/特征提取、复杂/丰富细节的图片理解等，更细节，耗时更长。
`low`：低细节模式，理解图片概略信息，如图片分类/识别、整体内容理解/描述，延时低、花费少。
内容模态，此处应为`video_url`。
messages.content.**video_url**`object``必选`
视频消息的内容部分。
messages.content.**text **`string``必选`
文本模态部分的内容。
内容模态，此处应为 `text`。
内容模态，此处应为 `video_url`**。**
视频模态的内容。
**
choices.logprobs.content.**token **`string`
choices.logprobs.content.**bytes **`integer[] / null`
choices.logprobs.content.**logprob **`float`
choices.logprobs.content.**top_logprobs **`object[]`
在当前 token 位置最有可能的标记及其对数概率的列表。在一些情况下，返回的数量可能比请求参数 top_logprobs 指定的数量要少。
各模态内容对象
文本部分 `object`
图片部分 `object`
视频部分 `object`
不支持理解视频中的音频内容。
内容模态，此处应为 `image_url`。
多模态消息中，文本模态的部分。
