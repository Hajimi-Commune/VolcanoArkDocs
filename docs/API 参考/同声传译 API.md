# 同声传译 API
```HTTP
wss://ark-beta.cn-beijing.volces.com/api/v3/realtime?service=clasi&model=<Model>
```

本文介绍 Doubao 同声传译大模型API的输入输出参数，帮助您使用 WebSocket 接口向大模型发起同声传译请求。服务会即时将输入的音频信息输入给模型，并返回音频转录的文本，以及翻译后的文本。
# 鉴权方式
本接口支持 API Key 鉴权方式，详见[鉴权认证方式](/docs/82379/1298459)。
# 工作机制
请参见[工作机制](/docs/82379/1433754#d540c6f6)。
# 使用教程
API 使用教程请参见 [同声传译](/docs/82379/1433754)。
:::warning
当您的业务数据量较大或并发量大，请关注服务限流信息，具体请参见[接口说明](/docs/82379/1433754#aaf45f14)。
:::

# 事件（Events）
## 客户端事件（Client Events）
### session.update
发送此事件以更新会话的默认配置。客户端可以随时发送此事件更新 session 配置，且所有字段都可随时更新。收到该事件后，服务端将使用`session.updated`事件进行响应，响应事件会显示完整的有效配置信息。当您想清除配置信息时，需设置对应字段为空。

| | | | | | | | \
|参数名称 |子字段 |类型 |必填 |默认值 |描述 |示例值 |
|---|---|---|---|---|---|---|
| | | | | | | | \
|event_id |\- |String |否 |\- |用于标识此事件的客户端生成的 ID。 |event_*** |\
| | | | | | | |
| | | | | | | | \
|type |\- |String |是 |\- |事件类型，必须为`session.update` |session.update |
| | | | | | | | \
|session |\- |Object |是 |\- |会话的配置信息。 |\- |
| | | | | | | | \
| |modalities |List<String> |否 |["text"] |输出内容的模态，目前只支持`text`。 |["text"] |
| | | | | | | | \
| |input_audio_format |String |\
| | | |是 |\
| | | | |pcm16 |\
| | | | | |输入音频格式，支持`pcm16`和`opus`。 |\
| | | | | | |\
| | | | | |* pcm16 是指 16 kHz 采样率，16位采样位宽的单通道音频数据，是一种未压缩的音频格式，适用于高质量无损音频记录，宽带要求高。 |\
| | | | | |* Opus 是一种有损压缩的编码格式，支持 6 kbps 到 510 kbps 的比特率范围及 8 kHz 至 48 kHz 的采样率，可根据网络状况和音频内容自动调整参数，适用于语音通信、网络会议等对实时性和带宽利用率要求高的场景。 |pcm16 |\
| | | | | | | |
| | | | | | | | \
| |speaker_detection |Object |否 |\- |声纹检测的配置信息。 | |
| | | | | | | | \
| |speaker_detection.enable_speaker_change_detection |Boolean |否 |False |是否基于声学匹配识别开启说话人切换检测。 |\
| | | | | | |\
| | | | | |* `True`：开启说话人切换检测，并在响应 |\
| | | | | | |\
| | | | | |`response.input_audio_transcription.delta`和 |\
| | | | | |`response.input_audio_translation.delta` |\
| | | | | |中返回相关标记。 |\
| | | | | | |\
| | | | | |* `False`：关闭说话人切换检测。 |True |
| | | | | | | | \
| |input_audio_translation |Object |是 |\- |语音翻译相关的配置信息。 |\- |
| | | | | | | | \
| |input_audio_translation.source_language |String |是 |zh |源语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`zh`，`en`。 |zh |
| | | | | | | | \
| |input_audio_translation.target_language |String |是 |en |目标语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`en`，`zh`。 |\
| | | | | |:::warning |\
| | | | | |源语种和目标语种必须为不同语种。 |\
| | | | | |::: |en |
| | | | | | | | \
| |input_audio_translation.add_vocab |Object |否 |\- |\
| | | | | |自定义词典，该object的所有配置字段（热词和术语）加和不超过200个。超过则会报错。 |\- |
| | | | | | | | \
| |input_audio_translation.add_vocab.hot_word_list |List<String> |否 |\- |\
| | | | | |热词列表，需输入源语言热词列表，输入其他语言无效。  |\
| | | | | |```Plain Text |\
| | | | | |"hot_word_list":[ |\
| | | | | |     "智能体应用", |\
| | | | | |     "coze平台" |\
| | | | | |] |\
| | | | | |``` |\
| | | | | | |\- |
| | | | | | | | \
| |input_audio_translation.add_vocab.glossary_list |List<Glossary> |\
| | | |否 |\
| | | | |\- |翻译可参考的术语列表。其中，input_audio_transcription为原文，input_audio_translation为译文。  |\
| | | | | |```Plain Text |\
| | | | | |"glossary_list":[ |\
| | | | | |    { |\
| | | | | |        "input_audio_transcription":"豆包", |\
| | | | | |        "input_audio_translation":"Doubao" |\
| | | | | |    }, |\
| | | | | |    { |\
| | | | | |        "input_audio_transcription":"火山引擎", |\
| | | | | |        "input_audio_translation":"Volcengine" |\
| | | | | |    } |\
| | | | | | ] |\
| | | | | |``` |\
| | | | | | |\- |

#### 请求示例
```JSON
{
    "event_id": "event_120",
    "type": "session.update",
    "session": {
        "input_audio_format": "pcm16",
        "modalities": ["text"],
        "speaker_detection": {
            "enable_speaker_change_detection": True,
        },
        "input_audio_translation": {
            "source_language": "zh",
            "target_language": "en",
            "add_vocab": {
                "hot_word_list": [],
                "glossary_list": [
                    {
                        "input_audio_transcription": "火山引擎智能创作平台",
                        "input_audio_translation": "volcengine creative cloud"
                    }
                ]
            }
        }
    }
}
```

### input_audio.commit
发送此事件以提交音频。为确保系统稳定运行，建议每100-200ms发送一次该事件，发送过快可能导致服务出错。
:::warning
单个连接每分钟内提交此事件的次数 Commit Per Minute (CPM) Per Connection上限为 700 次。提交该事件次数超过限制时，服务端将跳过超出限制的事件，并返回`error`事件。
:::

| | | | | \
|参数名称 |类型 |必填 |描述 |
|---|---|---|---|
| | | | | \
|event_id |String |否 |用于标识此事件的客户端生成的 ID。 |
| | | | | \
|type |String |是 |事件类型，必须为`input_audio.commit`。 |
| | | | | \
|audio |String |是 |Base64 编码的音频数据，需采用会话配置中 input_audio_format 字段指定的格式。 |\
| | | |:::warning |\
| | | |音频数据上限为10KB 。传入超过10 KB 的音频数据时，服务端将跳过该事件不处理对应音频数据，并返回`error`事件。 |\
| | | |::: |

#### 请求示例
```JSON
{
    "event_id": "event_121",
    "type": "input_audio.commit",
    "audio": "CACu/wEA3P8pAPX/****"
}
```

### input_audio.done
发送此事件以告知服务端已经完成了所有计划发送的音频数据的发送。该事件通常在麦克风结束输入时发送。客户端发送该事件后，服务端将不再接受任何新的 `input_audio.commit`事件，当服务端处理完已接收到的所有音频数据后会发送`response.done`事件。

| | | | | \
|参数名称 |类型 |必填 |描述 |
|---|---|---|---|
| | | | | \
|event_id |String |否 |用于标识此事件的客户端生成的 ID。 |
| | | | | \
|type |String |是 |事件类型，必须为`input_audio.done`。 |

#### 请求示例
```JSON
{
    "event_id": "event_122",
    "type": "input_audio.done"
}
```

## 服务端事件（Server Events）
### session.created
创建对话时自动创建，返回默认配置信息。在会话创建后立即发出。

| | | | | | \
|参数名称 |子参数 |类型 |描述 |示例值 |
|---|---|---|---|---|
| | | | | | \
|event_id |\- |String |服务端事件的唯一ID。 |event_*** |
| | | | | | \
|type |\- |String |事件类型，为`session.created `。 |session.created |
| | | | | | \
|session |\- |Object |会话的配置信息。 |\- |
| | | | | | \
| |id |String |会话的唯一ID。 |sess_*** |
| | | | | | \
| |object |String |会话对象的类型，为`"realtime.session"` |realtime.session |
| | | | | | \
| |input_audio_format |String |输入音频格式，目前默认`pcm16`为初始设置。 |pcm16 |
| | | | | | \
| |model |String |实际使用的模型名称和版本。 |doubao-clasi-*** |
| | | | | | \
| |modalities |List<String> |输出内容的模态，目前只支持`text`。 |["text"] |
| | | | | | \
| |input_audio_translation |Object |语音翻译相关的配置信息。 |\- |
| | | | | | \
| |input_audio_translation.source_language |String |源语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`zh`，`en`。 |zh |
| | | | | | \
| |input_audio_translation.target_language |String |目标语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`en`，`zh`。 |en |
| | | | | | \
| |input_audio_translation.add_vocab |Object |\
| | | |自定义词典，该object的所有配置字段（热词和术语）加和不超过200个。超过则会报错。 |\- |
| | | | | | \
| |input_audio_translation.add_vocab.hot_word_list |List<String> |使用的热词列表。 |\
| | | |```Plain Text |\
| | | |"hot_word_list":[ |\
| | | |     "智能体应用", |\
| | | |     "coze平台" |\
| | | |] |\
| | | |``` |\
| | | | |\- |
| | | | | | \
|  |input_audio_translation.add_vocab.glossary_list |List<Glossary> |\
| | | |翻译可参考的术语列表。其中， |\
| | | | |\
| | | |* `input_audio_transcription`为原文。 |\
| | | |* `input_audio_translation`为译文。 |\
| | | | |\
| | | |```Plain Text |\
| | | |"glossary_list":[ |\
| | | |    { |\
| | | |        "input_audio_transcription":"豆包", |\
| | | |        "input_audio_translation":"Doubao" |\
| | | |    }, |\
| | | |    { |\
| | | |        "input_audio_transcription":"火山引擎", |\
| | | |        "input_audio_translation":"Volcengine" |\
| | | |    } |\
| | | | ] |\
| | | |``` |\
| | | | |\- |
| | | | | | \
| |speaker_detection |Object |是否基于声学匹配识别进行说话人切换检测，默认为null。 |null |

#### 返回示例
```JSON
{
    "event_id": "event_123",
    "type": "session.created",
    "session": {
        "id": "sess_***",
        "object": "realtime.session",
        "modalities": [
            "text"
        ],
        "model": "doubao-clasi-***",
        "input_audio_format": "pcm16",
        "input_audio_translation": {
            "source_language": "zh",
            "target_language": "en",
            "add_vocab": null
        }
        "speaker_detection":null
    }
}
```

### session.updated
除非出现错误，否则在使用 session.update 事件更新会话时返回。

| | | | | | \
|参数名称 |子参数 |类型 |描述 |示例值 |
|---|---|---|---|---|
| | | | | | \
|event_id |\- |String |服务端事件的唯一ID。 |event_*** |
| | | | | | \
|type |\- |String |事件类型，为`session.updated`。 |session.updated |
| | | | | | \
|session |\- |Object |会话的配置信息。 |\- |
| | | | | | \
| |id |String |会话的唯一ID。 |sess_*** |
| | | | | | \
| |object |String |会话对象的类型，为`"realtime.session"` |realtime.session |
| | | | | | \
| |input_audio_format |String |输入音频格式，支持`pcm16`和`opus`。 |pcm16 |
| | | | | | \
| |model |String |本次请求实际使用的模型名称和版本。 |doubao-clasi-*** |
| | | | | | \
| |modalities |List<String> |输出内容的模态，目前只支持`text`。 |["text"] |
| | | | | | \
| |input_audio_translation |Object |语音翻译相关的配置信息。 |\- |
| | | | | | \
| |input_audio_translation.source_language |String |源语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`zh`，`en`。 |zh |
| | | | | | \
| |input_audio_translation.target_language |String |目标语种，需使用ISO 639 - 1语言代码指定源语种和目标语种，目前仅支持`en`，`zh`。 |en |
| | | | | | \
| |input_audio_translation.add_vocab |Object |\
| | | |自定义词典，该object的所有配置字段（热词和术语）加和不超过200个。 |\- |
| | | | | | \
| |input_audio_translation.add_vocab.hot_word_list |List<String> |传入的热词列表。 |\
| | | |```Plain Text |\
| | | |"hot_word_list":[ |\
| | | |     "智能体应用", |\
| | | |     "coze平台" |\
| | | |] |\
| | | |``` |\
| | | | |\- |
| | | | | | \
|  |input_audio_translation.add_vocab.glossary_list |List<Glossary> |\
| | | |翻译可参考的术语列表。其中， |\
| | | | |\
| | | |* `input_audio_transcription`为原文。 |\
| | | |* `input_audio_translation`为译文。 |\
| | | | |\
| | | |```Plain Text |\
| | | |"glossary_list":[ |\
| | | |    { |\
| | | |        "input_audio_transcription":"豆包", |\
| | | |        "input_audio_translation":"Doubao" |\
| | | |    }, |\
| | | |    { |\
| | | |        "input_audio_transcription":"火山引擎", |\
| | | |        "input_audio_translation":"Volcengine" |\
| | | |    } |\
| | | | ] |\
| | | |``` |\
| | | | |\- |
| | | | | | \
| |speaker_detection |Object |声纹检测的配置信息。 |\- |
| | | | | | \
| |speaker_detection.enable_speaker_change_detection |Boolen |是否基于声学匹配识别开启说话人切换检测。 |\
| | | | |\
| | | |* `true`：开启说话人切换检测，并在响应中返回相关标记。 |\
| | | |* `false`：关闭说话人切换检测。 |true |

#### 返回示例
```JSON
{
    "event_id": "event_124",
    "type": "session.updated",
    "session": {
        "id": "sess_***",
        "object": "realtime.session",
        "modalities": [
            "text"
        ],
        "model": "doubao-clasi-***",
        "input_audio_format": "pcm16",
        "input_audio_translation": {
            "source_language": "zh",
            "target_language": "en",
            "add_vocab": {
                "hot_word_list": [],
                "glossary_list": [
                    {
                        "input_audio_transcription": "火山引擎智能创作平台",
                        "input_audio_translation": "volcengine creative cloud"
                    }
                ]
            }
        }
        "speaker_detection": {
            "enable_speaker_change_detection": True
        }
    }
}
```

### response.created
创建新 Response 时返回。新Response被创建时返回的第一个事件，其中响应处于in_progress的初始状态。

| | | | | | \
|参数名称 |子参数 |类型 |描述 |示例值 |
|---|---|---|---|---|
| | | | | | \
|event_id |\- |String |服务端事件的唯一ID。 |event_**** |
| | | | | | \
|type |\- |String |事件类型，为`response.created`。 |\- |
| | | | | | \
|response |\- |Object |响应信息。 |\- |
| | | | | | \
| |id |String |响应信息的唯一ID。 |resp_003 |
| | | | | | \
| |object |String |响应对象的类型，为`"realtime.response"`。 |realtime.response |
| | | | | | \
| |status |String |响应状态，为`in_progress`。 |in_progress |
| | | | | | \
|  |usage  |Object  |本次连接使用情况的统计信息。  |\-  |
| | | | | | \
| |usage.total_tokens |Integer  |本次连接消耗的总 token 数量（输入 + 输出）。 |\
| | | | |1000 |
| | | | | | \
| |usage.input_tokens  |Integer  |本次连接消耗的输入token 数量。 |200 |
| | | | | | \
|  |usage.output_tokens  |Integer  |本次连接消耗的输出token 数量，包括转录和翻译。  |800  |\
| | | | |  |
| | | | | | \
| |usage.input_token_details |Object |本次连接消耗的输入token详细信息。 |\- |
| | | | | | \
| |usage.input_token_details.audio_tokens |Integer  |\
| | | |本次连接消耗的输入token中用于音频的token 数量。 |200 |

#### 返回示例
```JSON
{
    "event_id": "event_125",
    "type": "response.created",
    "response": {
        "id": "resp_***",
        "object": "realtime.response",
        "status": "in_progress",
        "usage": null
    }
}
```

### response.done
发送此事件以告知客户端，服务端已经处理完所有接收到的音频数据，并且已经发送了所有的`response.input_audio_transcription.delta` /`response.input_audio_translation.delta`事件，本次连接结束。

| | | | | | \
|参数名称 |子参数 |类型 |描述 |示例值 |
|---|---|---|---|---|
| | | | | | \
|event_id |\- |String |服务端事件的唯一ID。 |event_**** |
| | | | | | \
|type |\- |String |事件类型。 |response.done |
| | | | | | \
|response |\- |object |响应信息。 |\- |
| | | | | | \
| |id |String |响应信息的唯一ID。 |resp_**** |
| | | | | | \
| |object |String |对象的类型，为`"realtime.response"`。 |realtime.response |
| | | | | | \
| |status |String |\
| | | |响应的最终状态，目前支持 |\
| | | | |\
| | | |* `completed`：服务端处理完已接收到的所有音频数据，正常结束连接。 |\
| | | |* `failed`：发生错误，连接中断。 |\
| | | |* `timeout`：连接超时，服务端强制断开连接。 |\
| | | | |\
| | | |:::warning |\
| | | |以下情况服务端将强制断开连接，响应状态为`timeout`。 |\
| | | | |\
| | | |* 单连接连接时长超过 2 小时。 |\
| | | |* 单连接静默持续时长 0.5 小时。  |\
| | | |::: |completed |
| | | | | | \
|  |usage  |Object  |本次连接使用情况的统计信息。  |\-  |
| | | | | | \
| |usage.total_tokens |Integer  |本次连接消耗的总 token 数量（输入 + 输出）。 |\
| | | | |1000 |
| | | | | | \
| |usage.input_tokens  |Integer  |本次连接消耗的输入token 数量。 |200 |
| | | | | | \
| |usage.input_token_details |Object |本次连接消耗的输入token详细信息。 |\- |
| | | | | | \
| |usage.input_token_details.audio_tokens |Integer  |\
| | | |本次连接消耗的输入token中用于音频的token 数量。 |200 |

#### 返回示例
```JSON
{
    "event_id": "event_126",
    "type": "response.done",
    "response": {
        "id": "resp_0217355307251692f1d0fe07ac2ef6d29344c285c5cccbb1eed50",
        "object": "realtime.response",
        "status": "completed",
        "usage": {
            "total_tokens":634,
            "input_tokens":211,
            "output_tokens": 423,
            "input_token_details": {
                "audio_tokens":211
            }
        }
    }
}
```

### response.input_audio_transcription.delta 
流式返回的转录文本结果。

| | | | | \
|参数名称 |类型 |描述 |示例值 |
|---|---|---|---|
| | | | | \
|event_id |String |服务端事件的唯一ID。 |event_**** |
| | | | | \
|type |String |事件类型，为： |\
| | |`response.input_audio_transcription.delta ` |response.input_audio_transcription.delta |
| | | | | \
|response_id |String |响应信息的唯一ID。 |resp_**** |\
| | | | |
| | | | | \
|delta |String |流式返回的转录文本的内容。 |\- |
| | | | | \
|language |String |返回文本的语种，支持如下： |\
| | | |\
| | |* `en`：英文。 |\
| | |* `zh`：中文。 |en |
| | | | | \
|start_ms |Integer |该段音频信息起始时间在原始音频的时刻（单位 ms）。 |\
| | |> 结合`end_ms`字段，表示该段音频信息在原始音频中的时间段。 |200 |
| | | | | \
|end_ms |Integer |该段音频信息终止时间在原始音频的时刻（单位 ms）。 |\
| | |> 结合`start_ms`字段，表示该段音频信息在原始音频中的时间段。 |400 |
| | | | | \
|speaker_change |Boolean |是否检测到说话人切换： |\
| | | |\
| | |* `true`：检测到说话人切换。 |\
| | |* `false`：未检测到说话人切换。 |\
| | | |\
| | |仅在`session.update`中设置`enable_speaker_change_detection = True` 时返回该字段。 |true |

#### 返回示例
```JSON
{
    "event_id": "event_127",
    "type": "response.input_audio_transcription.delta",
    "response_id": "resp_0217355307251692f1d0fe07ac2ef6d29344c285c5cccbb1eed50",
    "delta": "定制服务",
    "language": "zh",
    "start_ms": 0,
    "end_ms": 800,
    "speaker_change": true
}
```

### response.input_audio_translation.delta
流式返回的翻译文本结果。

| | | | | \
|参数名称 |类型 |描述 |示例值 |
|---|---|---|---|
| | | | | \
|event_id |String |服务端事件的唯一ID。 |event_**** |
| | | | | \
|type |String |事件类型，必须为： |\
| | |`response.input_audio_translation.delta` |response.input_audio_translation.delta |
| | | | | \
|response_id |String |响应信息的唯一ID。 |resp_**** |\
| | | | |
| | | | | \
|delta |String |流式返回的翻译文本的内容。 |\- |
| | | | | \
|language |String |返回文本的语种，支持如下： |\
| | | |\
| | |* `en`：英文。 |\
| | |* `zh`：中文。 |en |
| | | | | \
|start_ms |Integer |对应原始音频开始时间（以ms为单位）。 |200 |
| | | | | \
|end_ms |Integer |对应原始音频结束时间（以ms为单位）。 |400 |
| | | | | \
|speaker_change |Boolean |是否检测到说话人切换： |\
| | | |\
| | |* `true`：检测到说话人切换。 |\
| | |* `false`：未检测到说话人切换。 |\
| | | |\
| | |仅在`session.update`中设置`enable_speaker_change_detection = True` 时返回该字段。 |true |

#### 返回示例
```JSON
{
    "event_id": "event_128",
    "type": "response.input_audio_translation.delta",
    "response_id": "resp_0217355307251692f1d0fe07ac2ef6d29344c285c5cccbb1eed50",
    "delta": "The customized service",
    "language": "en",
    "start_ms": 0,
    "end_ms": 800,
    "speaker_change": true
}
```

### error
发生错误时返回，可能是客户端或服务端导致的问题。大多数错误均可恢复，且会话将保持连接状态，建议您默认监控和记录错误消息。

| | | | | | \
|参数名称 |子参数 |类型 |描述 |示例值 |
|---|---|---|---|---|
| | | | | | \
|event_id |\- |String |服务器事件的唯一 ID。 |event_**** |
| | | | | | \
|type |\- |String |事件类型，必须为error。 |error |
| | | | | | \
|error |\- |Object |错误信息的细节。 |\- |
| | | | | | \
| |type |String |错误类型。 |BadRequest |
| | | | | | \
| |code |String |错误代码。 |MissingParameter |
| | | | | | \
| |message |String |错误信息。 |The request failed because it is missing one or multiple required parameters.  |
| | | | | | \
| |param |String |与错误相关的参数（如果有）。 |null |
| | | | | | \
| |event_id |String |导致错误的 client 事件的 event_id（如果适用）。 |event_**** |

#### 返回示例
```JSON
{
    "event_id": "event_129",
    "type": "error",
    "error": {
        "code": "InvalidParameter",
        "message": "A parameter specified in the request is not valid: input audio format must be pcm16 Request id: ****",
        "type": "BadRequest",
        "param": "input audio format must be pcm16"
    }
}
```

###