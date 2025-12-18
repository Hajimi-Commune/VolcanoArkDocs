# 豆包大模型1.8(最新)

输入 | 输出：文字、图片、视频 | 文字

模型价格：输入 ≥ 0.8，输出 ≥ 2.0

doubao-seed-1.8 有着更强的多模态理解能力和 Agent 能力，模型在真实世界诸多复杂任务中，能发挥更出色的表现，进一步帮助企业创造价值。

## 模型价格

| 条件（千 token） | 输入（元/百万 token） | 输入命中缓存（元/百万 token） | 输出单价（元/百万 token） | 缓存存储（元/百万 token*小时） | 输入单价[批量]（元/百万 token） | 输入命中缓存[批量]（元/百万 token） | 输出单价[批量]（元/百万 token） |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 输入长度 [0, 32] 且输出长度 [0, 0.2] | 0.80 | 0.16 | 2.00 | 0.017 | 0.40 | 0.16 | 1.00 |
| 输入长度 [0, 32] 且输出长度 (0.2,+∞) | 0.80 | 0.16 | 8.00 | 0.017 | 0.40 | 0.16 | 4.00 |
| 输入长度 (32, 128] | 1.20 | 0.16 | 16.00 | 0.017 | 0.60 | 0.16 | 8.00 |
| 输入长度 (128, 256] | 2.40 | 0.16 | 24.00 | 0.017 | 1.20 | 0.16 | 12.00 |

下面是计费项的简单说明，具体请参阅[模型价格](../../购买指南/模型服务价格.md)。

- 输入输出价位按照输入长度来定档，举例，在线推理场景，当输入长度为 16k ，则输入单价为0.8 元/百万 token，输出单价为8 元/百万 token。
- 使用在线推理的上下文缓存能力，产生命中缓存的输入折后费用、创建的缓存存储费用。
- 使用批量推理，产生输入[批量]费用、命中透明缓存的输入折后费用、输出[批量]费用。

## 模型能力

| 能力 | 状态 |
| --- | --- |
| 文本生成 | Supported |
| 深度思考 | Supported |
| 结构化输出 | Supported |
| 上下文缓存 | Supported |
| 批量推理 | Supported |
| 函数调用 | Supported |
| 图片理解 | Supported |
| 视频理解 | Supported |
| 文档理解 | Supported |

## 模型版本

- **doubao-seed-1.8 | 251215**
- Model ID：`doubao-seed-1-8-251215`
- 更强 Agent 能力，多模态理解升级

## 模型限制

| 长度限制（token） | 模型限流 |
| --- | --- |
| 上下文窗口：256k | RPM：30,000 |
| 最大输入长度：224k | TPM：5,000,000 |
| 最大思维链内容长度：64k | |
| 最大输出长度：64k | |

## API 支持范围

| API | 说明 |
| --- | --- |
| Chat API | 使用广泛的 API，存量业务迁移成本低 |
| Batch API | 提升处理吞吐量，降低使用成本 |
| Responses API | 新推出的 API，推荐业务使用 |

## Cookbook

| 示例 | 说明 |
| --- | --- |
| 代码 Agent | 通过函数工具扩展模型能力实现闭环推理 |
| 搜索 Agent | 集成 Web Search 等工具，完成复杂问题的搜索和解答 |
| 多模态搜索 Agent | 集成Web Search、图像、视觉等工具，完成多模态信息搜索 |
| MCP Agent | 集成 GitHub API 等工具，通过工具调用及闭环推理完成任务 |
| 图片理解 | 通过对图片进行特征提取及内容识别，实现对图片的解读 |
| 视觉定位（2D） | 通过边界框及中心点的方式定位目标，实现图像感知和推理 |
| 视觉定位（3D） | 通过三维边界框及中心点方式定位目标，实现对 3D 空间理解 |
| 视频理解 | 支持长视频理解，高帧率回看片段，细节无遗漏 |

## 使用说明

### 深度思考

- 默认开启思考：通过 thinking 参数控制模型是否开启深度思考模式。
- 默认开启 `medium` 模式：支持 `minimal`、`low`、`medium`、`high` 四种模式调节思考长度。

教程参见[深度思考](../../文本相关/深度思考.md)。

### 函数调用

- 默认启用 JSON 模式提高输出结果合法率。
- 支持 strict 模式。

教程参见[函数调用 Function Calling](../../工具调用/函数调用%20Function%20Calling.md)。

### 视觉理解

升级了视觉编码，token 用量更少，可处理更长视频：

- 图片理解：[图片像素及 token 用量说明](../../多模态理解/图片理解.md#图片像素及-token-用量说明)
  - 更少的 tokens：400w 像素以内的图片 token 用量降至 44.4%。
  - 默认高细节模式（High Detail）：图转 token 时，不缩放900w 像素内图片。
- 视频理解：[抽帧策略](../../多模态理解/视频理解.md#抽帧策略)
  - 支持更长视频：最大抽帧数从 640 提升至 1280，同抽帧频率下，视频时长扩大为2倍。

> **注意**
> 使用 Files API 上传视频文件，需设置 model 为 `doubao-seed-1-8-251215`，否则采用旧视觉编码，影响抽帧、token用量，详细请参见[上传文件](../../API%20参考/Files%20API/上传文件.md)。

### 工具调用

开启深度思考的工具调用，将不会直接丢弃思维链内容，思维链内容可能参与后续轮次推理，增加 tokens 会增加。

工具调用场景开启深度思考，为给出更详尽准确的回答，历史轮次的思维链内容按需（模型自主判断）参与推理，工作流程如下：

| 流程图 | 说明 |
| --- | --- |
| ![工具调用流程图] | - 回答问题 1 时（请求 1.1 - 1.2），模型进行多次思考 + 工具调用后给出答案，方舟会输入完整上下文包括思维链内容给模型处理。<br>- 开始回答问题2时（请求 2.1），方舟会自行判断并删除之前上下文中的思维链，输入给模型。 |

在整个请求过程中，用户回传完整上下文即可，无需关注是否回传思维链内容，由服务端自行判断，是否保留思维链内容。未输入给模型的思维链内容，不会计算 token 用量。

> **说明**
> 推荐在 Responses API 中使用 previous_response_id，平台自动保存历史对话的上下文，并在多轮交互中回传给推理服务。

Chat API 请求示例如下：

```bash
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seed-1-8-251215",
    "messages": [
      {
        "role": "system",
        "content": "你是人工智能助手."
      },
      {
        "role": "user",
        "content": "今天北京天气怎么样"
      },
      {
        "reasoning_content": "用户问今天北京的天气，我需要调用get_weather工具，参数query应该是"北京今天天气"对吧？按照格式来写函数调用。",
        "role": "assistant",
        "tool_calls": [
          {
            "function": {
              "arguments": " {\"query\": \"北京今天天气\"}",
              "name": "get_weather"
            },
            "id": "call_uufgsktw93ixgrw7*****",
            "type": "function"
          }
        ]
      },
      {
        "role": "tool",
        "tool_call_id":"call_uufgsktw93ixgrw7*****",
        "content": "38度"
      }
    ],
    "reasoning_effort": "high",
    "tools": [
      {
        "type": "function",
        "function": {
          "name": "get_weather",
          "description": "天气查询",
          "parameters": {
            "properties": {
              "query": {
                "description": "天气查询，查询全国各地的天气",
                "type": "string"
              }
            },
            "required": [
              "query"
            ],
            "type": "object"
          }
        }
      }
    ]
  }'
```

### 上下文编辑

支持管理上下文中的思考块及工具结果内容，具备以下优势：

- 工具使用效果提升：能够让模型利用历史上下文中思考的内容，提高工具触发及使用效果。
- 上下文窗口管理：自动保持在模型上下文窗口限制内。
- 智能缓存：与缓存功能协同工作，提升性能。

使用上下文编辑时，推荐配置如下：

| 功能策略 | 推荐配置 |
| --- | --- |
| 思考块清除 `clear_thinking` | `{"type": "clear_thinking", "keep": {"type": "thinking_turns", "value": 1}}` |
| 工具结果清除 `clear_tool_uses` | `{"type": "clear_tool_uses", "trigger": {"type": "tool_uses", "value": 30}, "keep": {"type": "tool_uses", "value": 20}}` |

使用 Responses API 的请求示例如下：

```bash
curl https://ark.cn-beijing.volces.com/api/v3/responses \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "doubao-seed-1-8-251215",
    "input": [
      {
        "role": "system",
        "content": "你是人工智能助手."
      },
      {
        "role": "user",
        "content": "今天北京天气怎么样"
      }
    ],
    "thinking":{"type": "enabled"},
    "tools": [
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
          "required": ["location"]
        }
      }
    ],
    "context_management": {
      "edits": [
        {
          "type": "clear_thinking",
          "keep": {
            "type": "thinking_turns",
            "value": 3
          }
        },
        {"type": "clear_tool_uses"}
      ]
    }
  }'
```

使用教程参见[上下文编辑](../../文本相关/上下文管理.md)。
