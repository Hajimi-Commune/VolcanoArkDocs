# SDK 常见使用示例
当您使用官方 SDK 时，设置自定义header等典型的使用，可以参考本说明。
## 设置超时&重试次数
当您的网络环境不稳定或者响应时间较长的场景，可以设置超时和重试次数，以提高服务可用性。

* 超时设置（`timeout`）：适用于对响应时间较长的场景（如深度思考模型回复问题时间较长），避免请求时间超时导致服务中断，这里推荐30分钟（1800秒）。
* 重试次数（`max_retries`）：适用于网络不稳定场景，自动重试因瞬时故障（如网络波动）失败的请求，默认2次，即请求失败默认会再重试2次，您可以按需配置。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    # 设置服务响应超时时间，单位秒，推荐1800秒及以上
    timeout=1800,
    # 设置重试次数
    max_retries=2,
)
```

**Go**

```Go
import (
    "os"
    "time"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
)

client := arkruntime.NewClientWithApiKey(
    os.Getenv("ARK_API_KEY"),
    arkruntime.WithTimeout(30*time.Minute), //设置后端处理完本次请求的整体超时时间，单位分钟，推荐30分钟及以上
    arkruntime.WithRetryTimes(2), //设置最大重试次数
)
```

**Java**

```Java
package com.example;

import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessage;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessageRole;
import com.volcengine.ark.runtime.service.ArkService;
import java.time.Duration;
import java.util.ArrayList;
import java.util.List;

public class ChatCompletionsExample {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ArkService service = ArkService
                .builder()
                .apiKey(apiKey)
                .timeout(Duration.ofSeconds(1800))// 设置后端处理完请求的超时时间，单位秒，推荐为1800秒及以上
                .connectTimeout(Duration.ofSeconds(20))// 设置连接超时时间为20秒
                .retryTimes(2)// 设置重试次数为2次
                .build();
        final List<ChatMessage> messages = new ArrayList<>();
        final ChatMessage userMessage = ChatMessage.builder().role(ChatMessageRole.USER).content("你好")
                .build();
        messages.add(userMessage);

        ChatCompletionRequest chatCompletionRequest = ChatCompletionRequest.builder()
                .model("<MODEL>") //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
                .messages(messages)
                .build();

        service.createChatCompletion(chatCompletionRequest).getChoices()
                .forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service
        service.shutdownExecutor();
    }
}
```

## 使用Access Key鉴权
当需要通过火山引擎云服务的标准鉴权体系（Access Key/Secret Key）认证时使用。
> 使用Access Key鉴权的实现原理是通过接口[GetApiKey - GetApiKey](/docs/82379/66619f91f281250274ef5000)获取临时密钥（Access Key/Secret Key）进行鉴权，此接口限流较低，务必使用单例模式进行请求，避免因限流导致鉴权失败，可参考[单例请求](/docs/82379/1544136#063cdc02)。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark
client = Ark(
    ak=os.environ.get("VOLC_ACCESSKEY"),
    sk=os.environ.get("VOLC_SECRETKEY"),
)
```

**Go**

```Go
import (
    "os"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
)

client := arkruntime.NewClientWithAkSk(
        os.Getenv("VOLC_ACCESSKEY"),
        os.Getenv("VOLC_SECRETKEY"),
)
```

**Java**

```Java
import com.volcengine.ark.runtime.service.ArkService;

public class CreateArkClientExample1 {
    public static void main(String[] args) {
        String ak = System.getenv("VOLC_ACCESSKEY");
        String sk = System.getenv("VOLC_SECRETKEY");
        ArkService service = ArkService.builder().ak(ak).sk(sk)
                .build();
    }
    // do your operation...
}
```

## 设置自定义 header
自定义的 `header` 字段采用键值对（key-value）结构，以对象形式呈现，具体格式示例如下：
```JSON
{
    "key1": "value1",
    "key2": "value2",
    ...,
    "keyN": "valueN"
}
```

可以用于传递额外信息，如配置 ID来串联日志，使能数据加密能力。支持该字段的接口有 [对话（Chat） API](https://www.volcengine.com/docs/82379/1494384)、[批量（Chat）API](https://www.volcengine.com/docs/82379/1528783)，下面是典型示例。
### 问题定位
您可以在请求时，为多个请求设置 **key** 为 `X-Client-Request-Id`，**value** 配置为自定义的 ID，并提供 ID 给方舟售后团队，为您串联客户端和服务端日志，方便问题定位。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
)

completion = client.chat.completions.create(
    # 替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
    model="<MODEL>",
    messages = [
        {"role": "user", "content": "常见的十字花科植物有哪些？"},
    ],
    # 自定义request id
    extra_headers={"X-Client-Request-Id": "My-Request-Id"}
)
print(completion.choices[0].message.content)
```

**Go**

```Go
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
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
    )
    ctx := context.Background()
    req := model.CreateChatCompletionRequest{
        Model: "<MODEL>", //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
        Messages: []*model.ChatCompletionMessage{
            {
                Role: model.ChatMessageRoleUser,
                Content: &model.ChatCompletionMessageContent{
                    StringValue: volcengine.String("常见的十字花科植物有哪些？"),
                },
            },
        },
    }

    // 自定义request id
    resp, err := client.CreateChatCompletion(
        ctx,
        req,
        arkruntime.WithCustomHeader(model.ClientRequestHeader, "my-request-id"), // 自定义请求头
    )
    if err != nil {
        fmt.Printf("standard chat error: %v
", err)
        return
    }
    fmt.Println(*resp.Choices[0].Message.Content.StringValue)
}
```

**Java**

```Java
package com.example;

import com.volcengine.ark.runtime.Const;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessage;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessageRole;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class ChatCompletionsExample3 {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ArkService service = ArkService.builder().apiKey(apiKey).build();
        final List<ChatMessage> messages = new ArrayList<>();
        final ChatMessage userMessage = ChatMessage.builder().role(ChatMessageRole.USER).content("常见的十字花科植物有哪些？").build();
        messages.add(userMessage);
        ChatCompletionRequest chatCompletionRequest = ChatCompletionRequest.builder()
                .model("<MODEL>")
                .messages(messages)
                .build();
        // 使用自定义的请求头
        service.createChatCompletion(chatCompletionRequest, new HashMap<String, String>(){{put(Const.CLIENT_REQUEST_HEADER, "my_request_id");}}).getChoices().forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service
        service.shutdownExecutor();
    }
}
```

### 加密数据
为了保证推理会话数据的传输安全，你可以为在线推理的会话数据加密。使能能力后，SDK 会对推理会话的内容进行加密。
> 更多能力介绍和原理信息请参考[推理会话数据应用层加密方案](https://www.volcengine.com/docs/82379/1389905)。

* 版本要求：需要保证 SDK 版本 `volcengine-python-sdk` `1.0.104`及以上。参考[升级 Python SDK](/docs/82379/1541595#d6b883b8)获得 SDK 的最新版本。
* 能力支持：仅支持[文本生成](https://www.volcengine.com/docs/82379/1330310#%E6%96%87%E6%9C%AC%E7%94%9F%E6%88%90)模型和[视觉理解](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3)图片理解模型（注：图片仅支持 Base64 图片加密， URL 图片无法加密）请求，仅支持官方 Python SDK 的 [对话(Chat) API](https://www.volcengine.com/docs/82379/1494384) ，支持单轮、多轮会话，支持流式、非流式，同步、异步接口。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
)
completion = client.chat.completions.create(
    # 替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
    model="<MODEL>",
    messages = [
        {"role": "user", "content": "常见的十字花科植物有哪些？"},
    ],
    #按下述代码设置自定义header，免费开启推理会话应用层加密
    extra_headers={'x-is-encrypted': 'true'}
)
print(completion.choices[0].message.content)
```

## 单例请求
请使用单例方式请求模型服务，勿重复创建实例导致额外资源消耗。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
)

# 请求1
completion = client.chat.completions.create(
    # 替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
    model="<MODEL>",
    messages=[
        {"role": "user", "content": "今天天气不错"},
    ],
)
print(completion.choices[0].message.content)

# 请求2
completion = client.chat.completions.create(
    # 替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
    model="<MODEL>",
    messages=[
        {"role": "user", "content": "你好"},
    ],
)
print(completion.choices[0].message.content)
```

**Go**

```Go
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
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
    )
    ctx := context.Background()
    // 第一次请求
    req1 := model.CreateChatCompletionRequest{
       Model: "<MODEL>",  //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
       Messages: []*model.ChatCompletionMessage{
          {
             Role: model.ChatMessageRoleUser,
             Content: &model.ChatCompletionMessageContent{
                StringValue: volcengine.String("今天天气不错？"),
             },
          },
       },
    }

    resp1, err := client.CreateChatCompletion(ctx, req1)
    if err != nil {
       fmt.Printf("standard chat error: %v
", err)
       return
    }
    fmt.Println(*resp1.Choices[0].Message.Content.StringValue)

    // 第二次请求
    req2 := model.CreateChatCompletionRequest{
       Model: "<MODEL>",  //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
       Messages: []*model.ChatCompletionMessage{
          {
             Role: model.ChatMessageRoleUser,
             Content: &model.ChatCompletionMessageContent{
                StringValue: volcengine.String("你好"),
             },
          },
       },
    }

    resp2, err := client.CreateChatCompletion(ctx, req2)
    if err != nil {
       fmt.Printf("standard chat error: %v
", err)
       return
    }
    fmt.Println(*resp2.Choices[0].Message.Content.StringValue)
}
```

**Java**

```Java
package com.example;

import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessage;
import com.volcengine.ark.runtime.model.completion.chat.ChatMessageRole;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.List;

public class ChatCompletionsExample4 {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ArkService service = ArkService.builder()
                .apiKey(apiKey)
                .build();
        // 第一次请求
        final List<ChatMessage> messages1 = new ArrayList<>();
        final ChatMessage userMessage1 = ChatMessage.builder().role(ChatMessageRole.USER).content("今天天气不错？").build();
        messages1.add(userMessage1);
        ChatCompletionRequest chatCompletionRequest1 = ChatCompletionRequest.builder()
                .model("<MODEL>") //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
                .messages(messages1)
                .build();
        service.createChatCompletion(chatCompletionRequest1).getChoices().forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // 第二次请求
        final List<ChatMessage> messages2 = new ArrayList<>();
        final ChatMessage userMessage2 = ChatMessage.builder().role(ChatMessageRole.USER).content("你好").build();
        messages2.add(userMessage2);
        ChatCompletionRequest chatCompletionRequest2 = ChatCompletionRequest.builder()
                .model("<MODEL>") //替换为Model ID，请从文档获取 https://www.volcengine.com/docs/82379/1330310
                .messages(messages2)
                .build();
        service.createChatCompletion(chatCompletionRequest2).getChoices().forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service
        service.shutdownExecutor();
    }
}
```

## 设置Base Url
初始化时 `Ark`、`AsyncArk`实例时，默认无需配置`base_url`参数，当需要连接非默认区域的服务端点时使用，示例如下。

**Python**

```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    # 以华北 2 (北京) 为例，<ARK_BASE_URL> 处应改为 https://ark.cn-beijing.volces.com/api/v3
    base_url="<ARK_BASE_URL>"
)
```

**Go**

```Go
import (
    "os"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
)
// 以华北 2 (北京) 为例，<ARK_BASE_URL> 处应改为 https://ark.cn-beijing.volces.com/api/v3
client := arkruntime.NewClientWithApiKey(
    os.Getenv("ARK_API_KEY"),
    arkruntime.WithBaseUrl("<ARK_BASE_URL>"),
)
```

**Java**

```Java
import com.volcengine.ark.runtime.service.ArkService;

public class CreateArkClientExample {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ArkService service = ArkService.builder()
            .apiKey(apiKey)
            .baseUrl("<ARK_BASE_URL>") // 以华北 2 (北京) 为例，<ARK_BASE_URL> 处应改为 https://ark.cn-beijing.volces.com/api/v3
            .build();
    }

    // do your operation...
}
```
