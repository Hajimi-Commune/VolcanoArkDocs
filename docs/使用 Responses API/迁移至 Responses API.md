# 迁移至 Responses API
Responses API 是火山方舟最新推出的 API 接口，原生支持高效的上下文管理，支持更简洁的输入输出格式，并且工具调用方式也更加便捷，不仅延续了 Chat API 的易用性，还结合了更强的智能代理能力。
随着大模型技术不断升级，Responses API 为开发各类面向实际行动的应用提供了更灵活的基础，并且支持工具调用多种扩展能力，非常适合搭建智能助手、自动化工具等场景。
## Responses API 核心优势
与 Chat API 相比，Responses API 在能力上具备多方面优势。

* 简洁的输入输出格式：输入为字符串或数组格式，输出为包含自身 ID 的 Response 对象且默认会被存储。
* 高效的上下文管理：默认开启存储功能，在多轮对话模式下能够自动管理上下文，避免了手动维护上下文的繁琐过程，提升智能交互体验。
* 低成本的上下文缓存：通过缓存常用上下文信息，减少每次请求重复处理加载开销，降低成本。 
* 便捷的工具调用：支持多种工具调用方式，如内置工具联网搜索、图像处理、私域知识库搜索、云部署 MCP 等，提升开发和集成效率。
* 良好的扩展性：未来将陆续支持更多内置工具，为开发者提供更丰富、更灵活的智能应用开发能力。

| || | | \
|能力 | |Chat API |Responses API |
|---|---|---|---|
| || | | \
|文本生成 | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |\
| | | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
| || | | \
|视觉理解 | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
| || | | \
|结构化输出 | |beta阶段 |beta阶段 |
| | | | | \
|工具调用 |函数调用 Function Calling |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
|^^| | | | \
| |联网搜索 Web Search |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96a134db51ea4e8d83b5c9dccff686c3~tplv-goo7wpa0wc-image.image =25x) |\
| | | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
|^^| | | | \
| |图像处理 Image Process |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96a134db51ea4e8d83b5c9dccff686c3~tplv-goo7wpa0wc-image.image =25x) |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
|^^| | | | \
| |私域知识库搜索 Knowledge Search |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96a134db51ea4e8d83b5c9dccff686c3~tplv-goo7wpa0wc-image.image =25x) |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
|^^| | | | \
| |云部署 MCP |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96a134db51ea4e8d83b5c9dccff686c3~tplv-goo7wpa0wc-image.image =25x) |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |
| || | | \
|上下文缓存 | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96a134db51ea4e8d83b5c9dccff686c3~tplv-goo7wpa0wc-image.image =25x) |\
| | | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aba4522e4aab46318574c8c3e460d20b~tplv-goo7wpa0wc-image.image =20x) |\
| | | |> 250615之后的模型版本支持 |

## 基础差异
Chat API (`/chat/completions`) 与 Responses API (`/responses`) 输入和输出格式略有不同。

* 输入：Chat API 需要传入一个消息（messages）数组，而 Responses API 则接受字符串或数组格式的输入。同时，在 Responses API 中，支持使用 **instructions** 字段在特定轮次中补充系统提示词，使用方式请参见[补充系统提示词](/docs/82379/1958520#6855e23d)。
* 输出：Chat API 返回 message，而 Responses API 返回一个包含自身 ID 的 response 对象。

通过以下示例快速体验两个 API 使用上的差异。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.4953051643192488);">

**Chat API**
输入示例
```Python
import os
from volcenginesdkarkruntime import Ark 

client = Ark(
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    api_key=os.getenv('ARK_API_KEY'), 
)

completion = client.chat.completions.create(
    model = "doubao-seed-1-6-251015",
    messages=[
        {"role": "user", "content": "Hello."},
    ],
)
print(completion)
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5046948356807511);margin-left: 16px;">

**Responses API**
输入示例
```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.getenv('ARK_API_KEY'),
)

response = client.responses.create(
    model="doubao-seed-1-6-251015",
    input="Hello.",
)

print(response)
```

</div>
</div>

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.4953051643192488);">

输出示例
```JSON
{
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "logprobs": null,
            "message": {
                "content": "Hello! How can I assist you today? Whether you have a question you'd like answered, want to chat about something that's on your mind, or need help with a specific task, feel free to share—I'm here to help. 😊",
                "reasoning_content": "\nGot it, let's see. The user just said \"Hello.\" I need to respond in a friendly way. Since the system prompt mentions I'm an AI assistant who can answer questions, chat, and provide information, I should keep it open-ended to encourage them to share what they need help with. Maybe something like \"Hello! How can I assist you today? Whether you have a question, want to chat about something, or need help with a task, feel free to let me know.\" That sounds natural and covers the points from the system prompt.",
                "role": "assistant"
            }
        }
    ],
    "created": 1765193367,
    "id": "0217651933631536335e3dfd75940b9979797202ce7ea2a894823",
    "model": "doubao-seed-1-6-251015",
    "service_tier": "default",
    "object": "chat.completion",
    "usage": {
        "completion_tokens": 164,
        "prompt_tokens": 35,
        "total_tokens": 199,
        "prompt_tokens_details": {
            "cached_tokens": 0
        },
        "completion_tokens_details": {
            "reasoning_tokens": 114
        }
    }
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5046948356807511);margin-left: 16px;">

输出示例
```JSON
{
    "created_at": 1765193461,
    "id": "resp_0217651934613099e1bacc68b98f823c2af95ea68bff0aec36f83",
    "max_output_tokens": 32768,
    "model": "doubao-seed-1-6-251015",
    "object": "response",
    "output": [
        {
            "id": "rs_02176519346192100000000000000000000ffffac15322033925d",
            "type": "reasoning",
            "summary": [
                {
                    "type": "summary_text",
                    "text": "\nGot it, let's see. The user said \"hello.\" First, I need to respond in a friendly way. Since the system prompt mentions being a helpful assistant, I should greet back and maybe invite them to ask whatever they need help with. Let me make it natural. Like, \"Hello! How can I assist you today?\" That's simple and open-ended. Yeah, that works."
                }
            ],
            "status": "completed"
        },
        {
            "type": "message",
            "role": "assistant",
            "content": [
                {
                    "type": "output_text",
                    "text": "Hello! How can I assist you today? Whether you have a question, need help with a task, or just want to chat, feel free to let me know. 😊"
                }
            ],
            "status": "completed",
            "id": "msg_02176519346378200000000000000000000ffffac153220cfab51"
        }
    ],
    "service_tier": "default",
    "status": "completed",
    "usage": {
        "input_tokens": 35,
        "output_tokens": 118,
        "total_tokens": 153,
        "input_tokens_details": {
            "cached_tokens": 0
        },
        "output_tokens_details": {
            "reasoning_tokens": 82
        }
    },
    "caching": {
        "type": "disabled"
    },
    "store": true,
    "expire_at": 1765452661
}
```

</div>
</div>

## 进阶能力适配
### 更新多轮对话
在多轮对话场景中，使用 Responses API 能够更高效的管理上下文，避免了手动维护上下文的繁琐过程。

* Chat API 是无状态的，每次请求时需要将历史信息放在 **messages** 中，并通过 **role** 字段设置，以便进行主题相关的延续性对话。具体使用参见[多轮对话](/docs/82379/1399009#f6222fec)。
* Responses API 默认开启存储功能，方便进行上下文管理，通过 **previous_response_id** 引入对应请求的输入和回复，实现智能交互体验。具体使用参见[上下文管理](/docs/82379/1958520#6d3df616)。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

**Chat API**
```Python
import os
from volcenginesdkarkruntime import Ark 

client = Ark(
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    api_key=os.getenv('ARK_API_KEY'), 
)

completion = client.chat.completions.create(
    # Replace with Model ID
    model = "doubao-seed-1-6-251015",
    messages=[
        {"content": "Hi，帮我讲个笑话。", "role": "user"},
        {"content": "这个笑话的笑点在哪？", "role": "user"}
    ]
)
print(completion.choices[0].message.content)
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

**Responses API**
```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.getenv('ARK_API_KEY'),
)

# Create the first-round conversation request
response = client.responses.create(
    model="doubao-seed-1-6-251015",
    input="Hi，帮我讲个笑话。"
)
print(response)

# Create the second-round conversation request
second_response = client.responses.create(
    model="doubao-seed-1-6-251015",
    previous_response_id=response.id,
    input=[{"role": "user", "content": "这个笑话的笑点在哪？"}],
)
print(second_response)
```

</div>
</div>

### 更新结构化输出定义
定义结构化输出的方式：

* Chat API：**response_format**，具体使用参见[结构化输出(beta)](/docs/82379/1568221)。
* Responses API：**text.format**，具体使用参见[结构化输出(beta)](/docs/82379/1958523)。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

**Chat API**
```Python
import os
from volcenginesdkarkruntime import Ark 

client = Ark(
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    api_key=os.getenv('ARK_API_KEY'), 
)

completion = client.chat.completions.create(
    model = "doubao-seed-1-6-251015",
    messages=[
        {"role": "user", "content": "常见的十字花科植物有哪些？json输出"}
    ],
    response_format={"type":"json_object"},
    thinking={"type": "disabled"},# 不使用深度思考能力
)
print(completion.choices[0].message.content)
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

**Responses API**
```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.getenv('ARK_API_KEY'), 
)

response = client.responses.create(
    model="doubao-seed-1-6-251015", 
    input=[
        {"role": "user", "content": "常见的十字花科植物有哪些？json输出"}
    ],
    text={"format":{"type": "json_object"}},
    thinking={"type": "disabled"},# 不使用深度思考能力
)
print(response)
```

</div>
</div>

### 更新最大输出长度参数

* Chat API：通过参数 **max_completion_tokens** 控制模型最大输出长度，具体教程参见[设置最大输出长度](/docs/82379/1449737#31ecc4d7)。
* Responses API：通过参数 **max_output_tokens** 控制模型最大输出长度，具体教程参见[设置最大输出长度](/docs/82379/1956279#1460ba95)。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

**Chat API**
```Python
import os
from volcenginesdkarkruntime import Ark 

client = Ark(
    base_url="https://ark.cn-beijing.volces.com/api/v3",    
    api_key=os.getenv('ARK_API_KEY'), 
)

completion = client.chat.completions.create(
    model = "doubao-seed-1-6-251015",
    messages=[
        {"role": "system", "content": "你是 AI 人工智能助手"},
        {"role": "user", "content": "常见的十字花科植物有哪些？"},
    ],
    max_completion_tokens = 1024,
)
print(completion.choices[0].message.content)
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

**Responses API**
```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.getenv('ARK_API_KEY'),
)

response = client.responses.create(
    model="doubao-seed-1-6-251015", 
    input=[
        {"role": "system", "content": "你是 AI 人工智能助手"},
        {"role": "user", "content": "常见的十字花科植物有哪些？"},
    ],
    max_output_tokens = 1024,
)
print(response)
```

</div>
</div>

### 使用上下文缓存能力
Context API 支持上下文缓存能力，而 Responses API 在缓存操控方面更加灵活，支持进行 ID 粒度的使用及变更。关于两种使用方式，参见[上下文缓存概述](/docs/82379/1398933)。

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

**Context API**
```Python
import datetime
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.environ.get("ARK_API_KEY"),
)

response = client.context.create(
    model=<YOUR_ENDPOINT_ID>,
    mode="session",
    messages=[
        {"role": "system", "content": "你是李雷"},
    ],
    ttl=datetime.timedelta(minutes=60),
)
print(response)

print("----- chat round 1 -----")
first_response = client.context.completions.create(
    context_id=response.id,
    model=<YOUR_ENDPOINT_ID>,
    messages=[
        {"role": "user", "content": "我是方方"},
    ]
)
print(first_response.choices[0].message.content)

print("----- chat round 2  -----")
second_response = client.context.completions.create(
    context_id=response.id,
    model=<YOUR_ENDPOINT_ID>,
    messages=[
        {"role": "user", "content": "你是谁，我是谁？"},
    ]
)
print(second_response.choices[0].message.content)    
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

**Responses API**
```Python
import os
from volcenginesdkarkruntime import Ark

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=os.getenv('ARK_API_KEY'),
)

response = client.responses.create(
    model="doubao-seed-1-6-251015",
    input=[
            {
             "role": "system", 
             "content": "你是李雷"
            }
          ],
    caching={"type": "enabled"}, 
    thinking={"type": "disabled"},
)
print(response.output[0].content[0].text)
print("----- chat round 1 -----")
first_response = client.responses.create(
    model="doubao-seed-1-6-251015",
    previous_response_id=response.id,
    input=[{"role": "user", "content": "我是方方"}],
    caching={"type": "enabled"}, 
    thinking={"type": "disabled"},
)
print(first_response.output[0].content[0].text)
print("----- chat round 2 -----")
second_response = client.responses.create(
    model="doubao-seed-1-6-251015",
    previous_response_id=first_response.id,
    input=[{"role": "user", "content": "你是谁，我是谁？"}],
    caching={"type": "enabled"}, 
    thinking={"type": "disabled"},
)
print(second_response.output[0].content[0].text)
```

</div>
</div>

### 使用工具调用
#### 更新函数定义
Responses API 和 Chat API 在定义 function 函数方面有细微区别，具体使用教程参见[函数调用 Function Calling](/docs/82379/1262342)：

<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

**Chat API**
```JSON
[
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "根据城市名称查询该城市当日天气（含温度、天气状况）",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "城市名称，如北京、上海（仅支持国内地级市）"
                    }
                },
                "required": [
                    "location"
                ]
            }
        }
    }
]
```

</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

**Responses API**
```JSON
[
    {
        "type": "function",
        "name": "get_weather",
        "description": "根据城市名称查询该城市当日天气（含温度、天气状况）",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "城市名称，如北京、上海（仅支持国内地级市）"
                }
            },
            "required": [
                "location"
            ]
        }
    }
]
```

</div>
</div>

#### 使用内置工具
Chat API 当前不支持使用方舟大模型内置工具（联网搜索、图像处理、私域知识库搜索、云部署 MCP等）能力，可以通过 Responses API 使用，具体教程参见[联网搜索 Web Search ](/docs/82379/1756990)、[图像处理 Image Process](/docs/82379/1798161)、[私域知识库搜索 Knowledge Search](/docs/82379/1873396)、[云部署 MCP / Remote MCP](/docs/82379/1827534)。

**联网搜索**

通过 Web Search 工具可以获取实时公开网络信息（如新闻、商品、天气等），解决数据时效性、知识盲区、信息同步等核心问题，并且无需自行开发搜索引擎或维护数据资源。
```Python
import os
from volcenginesdkarkruntime import Ark

# 从环境变量中获取您的API KEY，配置方法见：https://www.volcengine.com/docs/82379/1399008
api_key = os.getenv('ARK_API_KEY')

client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=api_key,
)

tools = [{
    "type": "web_search",
    "max_keyword": 2,  
}]

# 创建一个对话请求
response = client.responses.create(
    model="doubao-seed-1-6-251015",
    input=[{"role": "user", "content": "北京的天气怎么样？"}],
    tools=tools,
)

print(response)
```

**图像处理**

Image Process 图像处理工具支持通过 Responses API 调用对输入图片执行画点、画线、旋转、缩放、框选/裁剪关键区域等基础操作，适用于需模型通过视觉处理提升图片理解的场景（如图文内容分析、物体定位标注、多轮视觉推理等）。工具通过模型自动判断图像处理逻辑，支持与自定义 Function 混合使用，且可处理多轮视觉输入（上一轮输出图片作为下一轮输入）。
```Python
from openai import OpenAI
import os

# 从环境变量获取API密钥，配置方法见：https://www.volcengine.com/docs/82379/1399008
api_key = os.getenv('ARK_API_KEY')

# 初始化客户端，配置API地址与工具启用头
client = OpenAI(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=api_key,
    default_headers={"ark-beta-image-process": "true"}
)

# 发起图像处理请求
response = client.responses.create(
    model="doubao-seed-1-6-vision-250815",
    tools=[
        {
            "type": "image_process",
            "point": {
                "type": "disabled"
            },
            "grounding": {
                "type": "disabled"
            },
            "zoom": {
                "type": "enabled" # 启用缩放（zoom）工具
            },
            "rotate": {
                "type": "disabled"
            }
        }
    ],
    input=[
        {
            "type": "message",
            "role": "user",
            "content": [
                {
                    "type": "input_image",
                    "image_url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_1.jpg"  # 输入图片 URL
                },
                {
                    "type": "input_text",
                    "text": "前方路牌写了什么？"  # 系统提示文本
                }
            ]
        }
    ],
    stream=True  # 启用流式响应，实时获取处理结果
)

# 打印流式响应结果
for chunk in response:
    if hasattr(chunk, 'delta'):
        print(chunk.delta, end="", flush=True)

```

**私域知识库搜索**

私域知识库搜索工具 Knowledge Search 支持通过 Responses API 调用直接获取企业私域知识库中的信息（如内部文档、产品手册、行业资料等），适用于需基于企业专属数据解答问题的场景（如内部培训问答、产品功能咨询、行业方案查询等）。工具通过模型自动判断是否需要调用私域知识库，支持与自定义 Function、MCP 等工具混合使用，目前仅支持旗舰版知识库。
```Python
import os
from openai import OpenAI 

# 从环境变量获取API密钥，配置方法见：https://www.volcengine.com/docs/82379/1399008
api_key = os.getenv('ARK_API_KEY')

# 初始化客户端
client = OpenAI(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=api_key,
    default_headers={"ark-beta-knowledge-search": "true"}  # 启用私域知识库搜索
)

# 发起知识库搜索请求
response = client.responses.create(
    model="doubao-seed-1-6-251015",  # 替换为支持的模型
    input=[
        {
            "role": "user",
            "content": [
                {
                    "type": "input_text",
                    "text": "应用实验室里有类似实时视频理解的agent demo么？"  # 用户问题
                }
            ]
        }
    ],
    tools=[
        {
            "type": "knowledge_search",
            "knowledge_resource_id": "<knowledge_resource_id>",  # 替换为实际知识库ID
            "limit": 10,  # 最多返回10条搜索结果
        }
    ],
    stream=True,  # 启用流式响应，实时获取结果
    extra_body={"thinking": {"type": "auto"}}  # 模型自动判断是否需要搜索
)

# 处理流式响应
for chunk in response:
    if hasattr(chunk, 'delta'):
        print(chunk.delta, end="", flush=True)
```

**MCP**

对接“[MCP MarketPlace](https://www.volcengine.com/mcp-marketplace)”，支持调用市场内各类垂直领域MCP工具（如巨量千川、知识库），无需自行开发工具逻辑。适用于复杂任务（如多步数据查询 + 分析）场景，支持与自定义 Function、Web Search 工具混合使用。
```Python
from volcenginesdkarkruntime import Ark
import os

# 从环境变量获取API密钥（配置方法：https://www.volcengine.com/docs/82379/1399008）
api_key = os.getenv('ARK_API_KEY')

# 初始化客户端，启用MCP功能
client = Ark(
    base_url='https://ark.cn-beijing.volces.com/api/v3',
    api_key=api_key
)

# 发起基础MCP调用请求
response = client.responses.create(
    model="doubao-seed-1-6-251015",
    tools=[
        {
            "type": "mcp",
            "server_label": "deepwiki",
            "server_url": "https://mcp.deepwiki.com/mcp",
            "require_approval": "never"
        }
    ],
    input=[
        {
            "role": "user",
            "content": [
                {
                    "type": "input_text",
                    "text": "看一下volcengine/ai-app-lab这个repo的文档"
                }
            ]
        }
    ],
    extra_headers={"ark-beta-mcp": "true"},
    stream=True  # 流式获取结果
)

# 打印响应结果
for chunk in response:
    if hasattr(chunk, 'delta'):
        print(chunk.delta, end="", flush=True)

```

## 渐进式迁移
当前新模型会在 Chat API 和 Responses API 上同步适配，无需担心后续的维护问题。建议根据需求逐步采用 Responses API，可以先在工具调用及缓存等部分业务场景中使用，待使用稳定后再全面替换 Chat API，以实现平滑过渡。