# 批量(Chat)  API

` POST https://ark.cn-beijing.volces.com/api/v3/batch/chat/completions`[运行](https://api.volcengine.com/api-explorer/?action=BatchChatCompletions&data=%7B%7D&groupName=%E6%89%B9%E9%87%8F%E6%8E%A8%E7%90%86&query=%7B%7D&serviceCode=ark&version=2024-01-01)
本文介绍批量推理调用模型服务的 API 的输入输出参数，供您使用接口时查阅字段含义。通过批量推理您可享受到更高的限流配额以及实惠的价格，适合进行大批量数据处理时使用。
推荐的调用方式请见[示例代码](https://www.volcengine.com/docs/82379/1399517#%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81)。
请求参数
跳转 [响应参数](#PM1q4ttH)
请求体
**model**`string``必选`
通过 Endpoint ID 来调用模型，可参考[获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522)。
**messages**`object[]``必选`
到目前为止的对话组成的消息列表。不同模型支持不同类型的消息，如文本、图片、视频等。
**thinking**`object``默认值 {"type":"enabled"}`
控制模型是否开启深度思考模式。默认开启深度思考模式，可以手动关闭。
支持此字段的模型以及使用示例请参见[文档](https://www.volcengine.com/docs/82379/1449737#%E5%BC%80%E5%90%AF%E5%85%B3%E9%97%AD%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83)。
**max_tokens**`integer / null``默认值 4096`
模型回复最大长度（单位 token），取值范围各个模型不同，详细见[模型列表](https://www.volcengine.com/docs/82379/1330310)。
输入 token 和输出 token 的总长度还受模型的上下文长度限制。
**max_completion_tokens**`integer / null`
支持该字段的模型 `deepseek-r1-250528`，`doubao-seed-1-6-250615`， `doubao-seed-1-6-flash-250615`。
取值范围：`[0, 64k]`。
使用示例见[文档](https://www.volcengine.com/docs/82379/1449737#0001)。控制模型输出的最大长度（包括模型回答和模型思维链内容长度，单位 token）。配置了该参数后，可以让模型输出超长内容，**max_tokens **（默认值 4k）与思维链最大长度将失效，模型按需输出内容，直到达到 **max_completion_tokens **配置的值。
不可与 **max_tokens** 字段同时设置，会直接报错。
**stop**`string / string[] / null``默认值 null`
模型遇到 stop 字段所指定的字符串时将停止继续生成，这个词语本身不会输出。最多支持 4 个字符串。
[深度思考能力模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3%E8%83%BD%E5%8A%9B)不支持该字段。
`["你好", "天气"]`
**frequency_penalty**`float / null``默认值 0`
取值范围为 [-2.0, 2.0]。
频率惩罚系数。如果值为正，会根据新 token 在文本中的出现频率对其进行惩罚，从而降低模型逐字重复的可能性。
**presence_penalty**`float / null``默认值 0`
取值范围为 [-2.0, 2.0]。
存在惩罚系数。如果值为正，会根据新 token 到目前为止是否出现在文本中对其进行惩罚，从而增加模型谈论新主题的可能性。
**temperature**`float / null``默认值 1`
取值范围为 [0, 2]。
采样温度。控制了生成文本时对每个候选词的概率分布进行平滑的程度。当取值为 0 时模型仅考虑对数概率最大的一个 token。
较高的值（如 0.8）会使输出更加随机，而较低的值（如 0.2）会使输出更加集中确定。
通常建议仅调整 temperature 或 top_p 其中之一，不建议两者都修改。
**top_p**`float / null``默认值 0.7`
取值范围为 [0, 1]。
核采样概率阈值。模型会考虑概率质量在 top_p 内的 token 结果。当取值为 0 时模型仅考虑对数概率最大的一个 token。
0.1 意味着只考虑概率质量最高的前 10% 的 token，取值越大生成的随机性越高，取值越低生成的确定性越高。通常建议仅调整 temperature 或 top_p 其中之一，不建议两者都修改。
**logprobs**`boolean / null``默认值 false`
是否返回输出 tokens 的对数概率。
`false`：不返回对数概率信息。
`true`：返回消息内容中每个输出 token 的对数概率。
**top_logprobs**`integer / null``默认值 0`
取值范围为 [0, 20]。
指定每个输出 token 位置最有可能返回的 token 数量，每个 token 都有关联的对数概率。仅当 **logprobs为**`true` 时可以设置 **top_logprobs** 参数。
**logit_bias**`map / null``默认值 null`
调整指定 token 在模型输出内容中出现的概率，使模型生成的内容更加符合特定的偏好。**logit_bias** 字段接受一个 map 值，其中每个键为词表中的 token ID（使用 tokenization 接口获取），每个值为该 token 的偏差值，取值范围为 [-100, 100]。
-1 会减少选择的可能性，1 会增加选择的可能性；-100 会完全禁止选择该 token，100 会导致仅可选择该 token。该参数的实际效果可能因模型而异。
`{"1234": -100}`
**tools**`object[] / null``默认值 null`
待调用工具的列表，模型返回信息中可包含。当您需要让模型返回待调用工具时，需要配置该结构体。支持该字段的模型请参见[文档](https://www.volcengine.com/docs/82379/1330310#%E5%B7%A5%E5%85%B7%E8%B0%83%E7%94%A8%E8%83%BD%E5%8A%9B)。
响应参数
跳转 [请求参数](#B2fc2CRV)
非流式调用返回
**id**`string`
本次请求的唯一标识。
**model**`string`
本次请求实际使用的模型名称和版本。
doubao 1.5 代模型的模型名称格式为 doubao-1-5-**。如调用部署doubao-1.5-pro-32k 250115模型的推理接入点，返回model字段信息doubao-1-5-pro-32k-250115。
**service_tier**`string`
本次请求是否使用了TPM保障包。
`default`：本次请求未使用TPM保障包额度。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `chat.completion`。
**choices**`object[]`
本次请求的模型输出内容。
**usage**`object`
本次请求的 token 用量。

## 示例代码

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/batch/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "messages": [
        {
            "content": "You are a helpful assistant.",
            "role": "system"
        },
        {
            "content": "Hello!",
            "role": "user"
        }
    ],
    "model": "ep-bi-20250327******-*****"
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
        "content": "Hello! How can I assist you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1744206287,
  "id": "0217442062858641b9058848e46dd9b87819ea2d71e52d55930aa",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 20,
    "total_tokens": 29,
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
    # Non-streaming:
    print("----- standard request -----")
    completion = client.batch.chat.completions.create(
        model="ep-bi-20250327******-*****",
        messages=[{"content":"You are a helpful assistant.","role":"system"},{"content":"Hello!","role":"user"}],
    )
    print(completion.choices[0].message.content)
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
        "content": "Hello! How can I assist you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1744206287,
  "id": "0217442062858641b9058848e46dd9b87819ea2d71e52d55930aa",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 20,
    "total_tokens": 29,
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
	"sync"
	"time"

	"github.com/volcengine/volcengine-go-sdk/service/arkruntime"
	"github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
	"github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
	timeout := time.Hour
	workerNum := 3

	client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"), arkruntime.WithTimeout(timeout))
	client.StartBatchWorker(workerNum)

	ctx := context.Background()
	taskNum := 5
	wg := sync.WaitGroup{}

	req := model.CreateChatCompletionRequest{
		Model: "ep-bi-20250327******-*****",
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
					StringValue: volcengine.String("Hello!"),
				},
			},
		},
	}

	for i := 0; i < workerNum; i++ {
		wg.Add(1)
		go func(index int) {
			defer wg.Done()
			for j := 0; j < taskNum; j++ {
				resp, err := client.CreateBatchChatCompletion(ctx, req)
				if err != nil {
					fmt.Printf("worker %d request %d Fail Err %s\n", index, j, err)
					return
				}
				fmt.Println(*resp.Choices[0].Message.Content.StringValue)
			}
		}(i)
	}
	wg.Wait()
	client.StopBatchWorker()
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
        "content": "Hello! How can I assist you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1744206287,
  "id": "0217442062858641b9058848e46dd9b87819ea2d71e52d55930aa",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 20,
    "total_tokens": 29,
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
                        .role(ChatMessageRole.SYSTEM)
                        .content("You are a helpful assistant.")
                        .build();

        ChatMessage elementForMessagesForReqList1 =
                ChatMessage.builder().role(ChatMessageRole.USER).content("Hello!").build();

        messagesForReqList.add(elementForMessagesForReqList0);
        messagesForReqList.add(elementForMessagesForReqList1);

        ChatCompletionRequest req =
                ChatCompletionRequest.builder()
                        .model("ep-bi-20250327******-*****")
                        .messages(messagesForReqList)
                        .build();

        service.createBatchChatCompletion(req)
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
        "content": "Hello! How can I assist you today?",
        "role": "assistant"
      }
    }
  ],
  "created": 1744206287,
  "id": "0217442062858641b9058848e46dd9b87819ea2d71e52d55930aa",
  "model": "doubao-1-5-pro-32k-250115",
  "service_tier": "default",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 9,
    "prompt_tokens": 20,
    "total_tokens": 29,
    "prompt_tokens_details": {
      "cached_tokens": 0
    },
    "completion_tokens_details": {
      "reasoning_tokens": 0
    }
  }
}

```

属性
messages.content.**text **`string``必选`
文本消息部分的内容。
messages.content.**type **`string``必选`
文本消息类型，此次应为 `text`。
messages.**role**`string``必选`
发送消息的角色，此处应为`user`。
messages.**content**`string / object[]``必选`
用户信息内容。
**属性**
messages.content.image_url.**url **`string``必选`
支持传入图片链接或图片的Base64编码，不同模型支持图片大小略有不同，具体请参见[使用说明](https://www.volcengine.com/docs/82379/1362931#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)。
messages.content.image_url.**detail **`string / null``默认值 low`
取值范围：`high`、`low`、`auto`。
支持手动设置图片的质量。
`high`：高细节模式，适用于需要理解图像细节信息的场景，如对图像的多个局部信息/特征提取、复杂/丰富细节的图像理解等场景，理解更全面。此时 **min_pixels **取值`3136`、**max_pixels **取值`4014080`。
`low`：低细节模式，适用于简单的图像分类/识别、整体内容理解/描述等场景，理解更快速。此时 **min_pixels **取值`3136`、**max_pixels **取值`1048576`。
`auto`：默认模式，不同模型选择的模式略有不同，具体请参见[理解图像的深度控制](https://www.volcengine.com/docs/82379/1362931#bf4d9224)。
messages.content.image_url.**image_pixel_limit  **`object / null``默认值 null`
允许设置图片的像素大小限制，如果不在此范围，则会等比例放大或者缩小至该范围内。
生效优先级：高于 **detail **字段，即同时配置 **detail **与 **image_pixel_limit **字段时，生效 **image_pixel_limit **字段配置**。**
若 **min_pixels **/ **max_pixels **字段未设置，使用 **detail **设置配置的值对应的 **min_pixels **/ **max_pixels **值。
子字段取值逻辑：`3136`≤ **min_pixels **≤ **max_pixels **≤ `4014080`
messages.content.image_url.image_pixel_limit.**max_pixels **`integer`
取值范围：(**min_pixels**,  `4014080`]。
传入图片最大像素限制，大于此像素则等比例缩小至 **max_pixels **字段取值以下。
若未设置，则取值为 **detail **设置配置的值对应的 **max_pixels **值。
messages.content.image_url.image_pixel_limit.**min_pixels**
取值范围：[`3136`,  **max_pixels**)。
传入图片最小像素限制，小于此像素则等比例放大至 **min_pixels **字段取值以上。
若未设置，则取值为 **detail **设置配置的值对应的 **min_pixels **值（`3136`）。
各模态消息部分
**文本消息部分**`object`
多模态消息中，内容文本输入。[具备视觉理解能力模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3%E8%83%BD%E5%8A%9B)、部分大语言模型支持此类型消息。
**图像消息部分**`object`
多模态消息中，图像内容部分。[具备视觉理解能力模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3%E8%83%BD%E5%8A%9B)支持此类型消息。
**视频消息部分**
视频理解模型请参见 [视频理解模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3%E8%83%BD%E5%8A%9B)。
多模态消息中，视频内容部分。
内容类型
**纯文本消息内容**`string`
纯文本消息内容，大语言模型支持传入此类型。
**多模态消息内容**`object[]`
支持文本、图像、视频等类型，视觉理解模型等多模态模型、部分大语言模型支持此字段。
choices.message.tool_calls.**i****d **`string`
调用的工具的 ID。
choices.message.tool_calls.**type **`string`
工具类型，当前仅支持`function`。
choices.message.tool_calls.**function **`object`
模型调用的函数。
***
说明
messages.**content**与 messages.**tool_calls**字段二者至少填写其一。
发送消息的角色，此处应为`assistant`。
messages.**content**`string / array`
模型回复的消息。
messages.**tool_calls**`object[]`
历史对话中，模型回复的工具调用信息。
choices.**index **`integer`
当前元素在 **choices** 列表的索引。
choices.**finish_reason **`string`
模型停止生成 token 的原因。取值范围：
`stop`：模型输出自然结束，或因命中请求参数 stop 中指定的字段而被截断。
`length`：模型输出因达到模型输出限制而被截断，有以下原因：
触发`max_token`限制（回答内容的长度限制）。
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
消息类型
**系统消息**`object`
开发人员提供的指令，模型应遵循这些指令。如模型扮演的角色或者目标等。
**用户消息**`object`
用户发送的消息，包含提示或附加上下文信息。不同模型支持的字段类型不同，最多支持文本、图片、视频形式的消息。
**模型消息**`object`
历史对话中，模型回复的消息。往往在多轮对话传入历史对话记录以及[Prefill Response](https://www.volcengine.com/docs/82379/1359497)时让模型按照预置的回复内容继续回复时使用。
支持传入图片链接或图片的Base64编码，具体使用请参见[使用说明](https://www.volcengine.com/docs/82379/1362931#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)。
messages.content.image_url.**detail **`string``默认值 auto`
支持手动设置图片的质量，取值范围`high`、`low`、`auto`。
`high`：高细节模式，适用于需要理解图像细节信息的场景，如对图像的多个局部信息/特征提取、复杂/丰富细节的图像理解等场景，理解更全面。
`low`：低细节模式，适用于简单的图像分类/识别、整体内容理解/描述等场景，理解更快速。
usage.prompt_tokens_details.**cached_tokens **`integer`
本接口暂不支持该字段。此处应为`0`。
图像消息类型，此次应为 `image_url`。
messages.content.**image_url **`object``必选`
图片消息的内容部分。
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
视频消息类型，此次应为 `video_url`**。**
messages.content.**video_url**`object``必选`
视频消息的内容部分。
messages.content.video_url.**url **`string``必选`
支持传入视频链接或视频的Base64编码。具体使用请参见[视频理解说明](https://www.volcengine.com/docs/82379/1362931#%E8%A7%86%E9%A2%91%E7%90%86%E8%A7%A3)。
多模态消息中，内容文本输入。视觉理解模型、部分大语言模型支持此类型消息。
多模态消息中，图像内容部分。视觉理解模型支持此类型消息。
**视频信息部分**`object`
本接口支持 API Key 与 Access Key鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
choices.message.**role **`string`
内容输出的角色，此处固定为 `assistant`。
choices.message.**content **`string`
模型生成的消息内容。
choices.message.**reasoning_content **`string / null`
模型处理问题的思维链内容。
仅深度推理模型支持返回此字段，深度推理模型请参见[支持模型](https://www.volcengine.com/docs/82379/1449737#5f0f3750)。
choices.message.**tool_calls **`object[] / null`
模型生成的工具调用。
choices.logprobs.content.**token **`string`
当前 token。
choices.logprobs.content.**bytes **`integer[] / null`
当前 token 的 UTF-8 值，格式为整数列表。当一个字符由多个 token 组成（表情符号或特殊字符等）时可以用于字符的编码和解码。如果 token 没有 UTF-8 值则为空。
choices.logprobs.content.**logprob **`float`
当前 token 的对数概率。
choices.logprobs.content.**top_logprobs **`object[]`
在当前 token 位置最有可能的标记及其对数概率的列表。在一些情况下，返回的数量可能比请求参数 top_logprobs 指定的数量要少。
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_2abecd05ca2779567c6d32f0ddc7874d.png)[模型列表](https://www.volcengine.com/docs/82379/1399517)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a5fdd3028d35cc512a10bd71b982b6eb.png)[模型计费](https://www.volcengine.com/docs/82379/1099320#%E6%89%B9%E9%87%8F%E6%8E%A8%E7%90%86)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_afbcf38bdec05c05089d5de5c3fd8fc8.png)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bef4bc3de3535ee19d0c5d6c37b0ffdd.png)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_57d0bca8e0d122ab1191b40101b5df75.png)[调用教程](https://www.volcengine.com/docs/82379/1399517)![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1609c71a747f84df24be1e6421ce58f0.png)[常见问题](https://www.volcengine.com/docs/82379/1359411)
messages.content.video_url.**fps**`float/ null``默认值 1`
取值范围：`[0.2, 5]`。
每秒钟从视频中抽取指定数量的图像**。**取值越高，对于视频中画面变化理解越精细；取值越低，对于视频中画面变化感知减弱，但是使用的 token 花费少，速度也更快。详细说明见[用量说明](https://www.volcengine.com/docs/82379/1362931#%E7%94%A8%E9%87%8F%E8%AF%B4%E6%98%8E)。
{
"type":"object",
"properties":{
"location":{
"type":"string",
"description":"城市，如：北京"
}
},
"required":["location"]
}
usage.**prompt_tokens **`integer`
输入的 prompt token 数量。
usage.**completion_tokens **`integer`
模型生成的 token 数量。
usage.**total_tokens **`integer`
本次请求消耗的总 token 数量（输入 + 输出）。
usage.**prompt_tokens_details **`object`
本接口暂不支持该字段。
命中上下文缓存的tokens细节。
usage.**completion_tokens_details **`object`
本次请求花费的 token 的细节。
显示子字段
messages.tool_calls**.**function.**name **`string``必选`
模型需要调用的函数名称。
messages.tool_calls**.**function.**arguments **`string``必选`
模型生成的用于调用函数的参数，JSON 格式。
模型并不总是生成有效的 JSON，并且可能会虚构出一些您的函数参数规范中未定义的参数。在调用函数之前，请在您的代码中验证这些参数是否有效。
发送消息的角色，此处应为`system`。
系统信息内容。
choices.logprobs.**content **`object[] / null`
message列表中每个 content 元素中的 token 对数概率信息。
choices.message.tool_calls.function.**name **`string`
模型调用的函数的名称。
choices.message.tool_calls.function.**arguments **`string`
choices.logprobs.content.top_logprobs.**token **`string`
choices.logprobs.content.top_logprobs.**bytes **`integer[] / null`
choices.logprobs.content.top_logprobs.**logprob **`float`
messages.tool_calls**.function **`object``必选`
模型调用工具对应的函数信息。
messages.tool_calls**.id **`string``必选`
messages.tool_calls**.type **`string``必选`
usage.completion_tokens_details.**reasoning_tokens **`integer`
输出思维链内容花费的 token 数 。
支持输出思维链的模型请参见[文档](https://www.volcengine.com/docs/82379/1449737#5f0f3750)。
tools.**type **`string``必选`
工具类型，此处应为 `function`。
tools.**function **`object``必选`
模型返回中可包含待调用的工具。
thinking.**type **`string``必选`
取值范围：`enabled`， `disabled`，`auto`。
`enabled`：开启思考模式，模型一定先思考后回答。
`disabled`：关闭思考模式，模型直接回答问题，不会进行思考。
`auto`：自动思考模式，模型根据问题自主判断是否需要思考，简单题目直接回答。
