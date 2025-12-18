# 应用(bot)  API

` POST https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions`[运行](https://api.volcengine.com/api-explorer/?action=BotsChatCompletions&data=%7B%7D&groupName=%E5%BA%94%E7%94%A8Bot%20API&query=%7B%7D&serviceCode=ark&version=2024-01-01)
本文介绍方舟应用 API 的输入输出参数，供您使用接口时查阅字段含义。
请求参数
跳转 [响应参数](#Qu59cel0)
请求体
**model**`string``必选`
您需要调用的应用的 ID （Bot ID），[获取 Bot ID](https://www.volcengine.com/docs/82379/1267885) 。
**messages**`object[]``必选`
到目前为止的对话组成的消息列表。不同模型支持不同类型的消息，如文本、图片等。
**thinking**`object``默认值 {"type":"enabled"}`
控制模型是否开启深度思考模式。默认开启深度思考模式，可以手动关闭。
支持此字段的模型以及使用示例请参见[文档](https://www.volcengine.com/docs/82379/1449737#%E5%85%B3%E9%97%AD%E6%B7%B1%E5%BA%A6%E6%80%9D%E8%80%83)。
**stream**`boolean / null``默认值 false`
响应内容是否流式返回：
`false`：模型生成完所有内容后一次性返回结果。
`true`：按 SSE 协议逐块返回模型生成内容，并以一条 `data: [DONE] `消息结束。
**stream_options**`object / null``默认值 null`
流式响应的选项。当 **stream** 为 `true` 时可设置 **stream_options** 字段。
stream_options.**include_usage **`boolean / null``默认值 false`
是否包含本次请求的 token 用量统计信息。
`false`：不返回 token 用量信息。
`true`：在 `data: [DONE]` 消息之前会返回一个额外的块，此块上的 **usage** 字段代表整个请求的 token 用量，**choices** 字段为空数组。所有其他块也将包含 **usage** 字段，但值为 `null`。
**max_tokens**`integer / null``默认值 4096`
模型回复最大长度（单位 token），取值范围各个模型不同，详细见[模型列表](https://www.volcengine.com/docs/82379/1330310)。
输入 token 和输出 token 的总长度还受模型的上下文长度限制。
**stop**`string / string[] / null``默认值 null`
模型遇到 stop 字段所指定的字符串时将停止继续生成，这个词语本身不会输出。最多支持 4 个字符串。
`["你好", "天气"]`
**frequency_penalty**`float / null``默认值 0`
取值范围为 [-2.0, 2.0]。
频率惩罚系数。如果值为正，会根据新 token 在文本中的出现频率对其进行惩罚，从而降低模型逐字重复的可能性。
**presence_penalty**`float / null``默认值 0`
取值范围为 [-2.0, 2.0]。
存在惩罚系数。如果值为正，会根据新 token 到目前为止是否出现在文本中对其进行惩罚，从而增加模型谈论新主题的可能性。
**temperature**`float / null``默认值 1`
取值范围为 [0, 2]。
采样温度。控制了生成文本时对每个候选词的概率分布进行平滑的程度。。当取值为 0 时模型仅考虑对数概率最大的一个 token。
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
指定每个输出 token 位置最有可能返回的 token 数量，每个 token 都有关联的对数概率。仅当 **logprobs**为`true` 时可以设置 **top_logprobs** 参数。
**logit_bias**`map / null``默认值 null`
调整指定 token 在模型输出内容中出现的概率，使模型生成的内容更加符合特定的偏好。**logit_bias** 字段接受一个 map 值，其中每个键为词表中的 token ID（使用 tokenization 接口获取），每个值为该 token 的偏差值，取值范围为 [-100, 100]。
-1 会减少选择的可能性，1 会增加选择的可能性；-100 会完全禁止选择该 token，100 会导致仅可选择该 token。该参数的实际效果可能因模型而异。
`{ "1234": -100}`
**tools**`object[] / null``默认值 null`
模型可以调用的工具列表。目前仅函数作为工具被支持。用这个来提供模型可能为其生成 JSON 输入的函数列表。支持该字段的模型请参见[文档](https://www.volcengine.com/docs/82379/1330310#98fee2f1)。
tools.**type **`string``必选`
工具类型，当前仅支持 `function`。
tools.**function **`object``必选`
模型可以调用的类型为`function`的工具列表。
tools.function.**name **`string``必选`
调用的函数的名称。
tools.function.**description **`string``必选`
调用的函数的描述，大模型会使用它来判断是否调用这个函数。
tools.function.**parameters **`object``必选`
函数请求参数，以 JSON Schema 格式描述。具体格式请参考 [JSON Schema](https://json-schema.org/understanding-json-schema) 文档。
**m****a**`object / null``默认值 null`
额外参数。
ma**.****group_chat_config**`object``必选`
运行时动态传入的角色配置。
ma**.**group_chat_config.**characters**`object[]``必选`
群聊角色列表。
ma**.**group_chat_config.characters.**name**`string``必选`
群聊角色名称。
ma**.**group_chat_config.characters.**system_prompt**`string``必选`
群聊角色设定，用于告知应用需要扮演的角色。
ma**.**group_chat_config.characters.**model_desc**`object``必选`
模型说明。
ma**.**group_chat_config.characters.model_desc.**endpoint_id**`string``必选`
您创建的 [推理接入点](https://www.volcengine.com/docs/82379/1099522)ID。
ma**.**group_chat_config.**description**`string / null``默认值 null`
群聊场景描述，可设定群聊的主题、时间地点、事件场景、用户扮演的角色等信息。
ma**.**group_chat_config.**user_name**`string / null``默认值 用户`
表示“我”所扮演的角色名称 ，默认值为“用户”。
ma**.****user_info**`string / null``默认值 null`
联网的智能体可使用，如查询天气场景，前端可以直接通过该字段传地点信息给模型，方便查对应地点的天气。
传入的信息需满足条件：可以被反序列化成 json 的字符串，并需包含 `city` 和 `district` 两个字段。
ma**.****emit_intention_signal_extra**`string / null``默认值 "false"`
值是 `"true"` 时，会中途返回intention状态 "正在搜索"。
ma**.****target_character_name**`string / null``默认值 null`
群聊Bot对话时填写，指定本次要发言的角色名，必须是存在于`characters`里的角色。
响应参数
跳转 [请求参数](#RxN8G2nH)

## 示例代码

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "messages": [
        {
            "content": "杭州今天天气",
            "role": "user"
        }
    ],
    "model": "bot-20250407225237-6xv7r"
}'
```

**Response:**

```json
{
    "id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
    "choices": [
      {
        "finish_reason": "stop",
        "index": 0,
        "message": {
          "content": "以下是杭州今天（2025年4月8日）的天气情况：\n- **整体天气状况**：今天多云，偏南风3级。\n- **气温情况**：白天最高气温31度，平均相对湿度55%。\n- **空气质量**：杭州市区AQI为90 - 110，首要污染物是O₃，空气质量等级为良 - 轻度污染，易感人群需减少户外锻炼。 ",
          "role": "assistant"
        }
      }
    ],
    "created": 1744102143,
    "model": "doubao-1-5-pro-32k-250115",
    "object": "chat.completion",
    "bot_usage": {
      "model_usage": [
        {
          "name": "doubao-1-5-pro-32k-250115",
          "prompt_tokens": 1960,
          "completion_tokens": 99,
          "total_tokens": 2059
        }
      ],
      "action_usage": [
        {
          "action_name": "content_plugin",
          "count": 1
        }
      ],
      "action_details": [
        {
          "name": "content_plugin",
          "count": 1,
          "tool_details": [
            {
              "name": "search",
              "input": {
                "queries": [
                  "杭州今天天气"
                ],
                "source_type": [
                  "search_engine"
                ],
                "offset": 0,
                "count": 5,
                "account_id": 2100000825,
                "video_token": ""
              },
              "output": {
                "type": "tool",
                "data": {
                  "status_code": 200,
                  "data": {
                    "has_more": true,
                    "results": [
                      {
                        "id": "3ba8807eabf8793d-65e575422f8ebfb2",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-中国天气",
                        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
                        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
                        "publish_time": 0,
                        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "3ba8807eabf8793d-65e575422f8ebfb2",
                          "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
                          "auth_info": "非常权威",
                          "auth_score": 1,
                          "rel_info": "强相关",
                          "rel_score": 0.994623,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      },
                      ...
                      {
                        "id": "d4d70607487f997f-b2adf6b6d837f126",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-www.hzqx.com",
                        "title": "杭州天气",
                        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
                        "publish_time": 1744079400,
                        "url": "https://www.hzqx.com/hztq/index.html",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "d4d70607487f997f-b2adf6b6d837f126",
                          "logo_url": "",
                          "auth_info": "非常权威",
                          "auth_score": 0.875495,
                          "rel_info": "强相关",
                          "rel_score": 0.834824,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      }
                    ],
                    "debug_data": {
                      "extra": null
                    },
                    "log_id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
                    "timing": {
                      "total": 1431
                    }
                  }
                }
              },
              "created_at": 1744102137810,
              "completed_at": 1744102139264
            }
          ]
        }
      ]
    },
    "metadata": {},
    "references": [
      {
        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
        "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
        "site_name": "搜索引擎-中国天气",
        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
        "publish_time": "1970年01月01日 08:00:00(CST) 星期四",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      },
      ...
      {
        "url": "https://www.hzqx.com/hztq/index.html",
        "logo_url": "",
        "site_name": "搜索引擎-www.hzqx.com",
        "title": "杭州天气",
        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
        "publish_time": "2025年04月08日 10:30:00(CST) 星期二",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      }
    ]
  }
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250407******-*****",
        messages=[{"content":"杭州今天天气","role":"user"}],
    )
    print(resp.choices[0].message.content)
```

**Response:**

```json
{
    "id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
    "choices": [
      {
        "finish_reason": "stop",
        "index": 0,
        "message": {
          "content": "以下是杭州今天（2025年4月8日）的天气情况：\n- **整体天气状况**：今天多云，偏南风3级。\n- **气温情况**：白天最高气温31度，平均相对湿度55%。\n- **空气质量**：杭州市区AQI为90 - 110，首要污染物是O₃，空气质量等级为良 - 轻度污染，易感人群需减少户外锻炼。 ",
          "role": "assistant"
        }
      }
    ],
    "created": 1744102143,
    "model": "doubao-1-5-pro-32k-250115",
    "object": "chat.completion",
    "bot_usage": {
      "model_usage": [
        {
          "name": "doubao-1-5-pro-32k-250115",
          "prompt_tokens": 1960,
          "completion_tokens": 99,
          "total_tokens": 2059
        }
      ],
      "action_usage": [
        {
          "action_name": "content_plugin",
          "count": 1
        }
      ],
      "action_details": [
        {
          "name": "content_plugin",
          "count": 1,
          "tool_details": [
            {
              "name": "search",
              "input": {
                "queries": [
                  "杭州今天天气"
                ],
                "source_type": [
                  "search_engine"
                ],
                "offset": 0,
                "count": 5,
                "account_id": 2100000825,
                "video_token": ""
              },
              "output": {
                "type": "tool",
                "data": {
                  "status_code": 200,
                  "data": {
                    "has_more": true,
                    "results": [
                      {
                        "id": "3ba8807eabf8793d-65e575422f8ebfb2",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-中国天气",
                        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
                        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
                        "publish_time": 0,
                        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "3ba8807eabf8793d-65e575422f8ebfb2",
                          "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
                          "auth_info": "非常权威",
                          "auth_score": 1,
                          "rel_info": "强相关",
                          "rel_score": 0.994623,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      },
                      ...
                      {
                        "id": "d4d70607487f997f-b2adf6b6d837f126",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-www.hzqx.com",
                        "title": "杭州天气",
                        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
                        "publish_time": 1744079400,
                        "url": "https://www.hzqx.com/hztq/index.html",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "d4d70607487f997f-b2adf6b6d837f126",
                          "logo_url": "",
                          "auth_info": "非常权威",
                          "auth_score": 0.875495,
                          "rel_info": "强相关",
                          "rel_score": 0.834824,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      }
                    ],
                    "debug_data": {
                      "extra": null
                    },
                    "log_id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
                    "timing": {
                      "total": 1431
                    }
                  }
                }
              },
              "created_at": 1744102137810,
              "completed_at": 1744102139264
            }
          ]
        }
      ]
    },
    "metadata": {},
    "references": [
      {
        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
        "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
        "site_name": "搜索引擎-中国天气",
        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
        "publish_time": "1970年01月01日 08:00:00(CST) 星期四",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      },
      ...
      {
        "url": "https://www.hzqx.com/hztq/index.html",
        "logo_url": "",
        "site_name": "搜索引擎-www.hzqx.com",
        "title": "杭州天气",
        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
        "publish_time": "2025年04月08日 10:30:00(CST) 星期二",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      }
    ]
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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250407******-*****",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("杭州今天天气"),
				},
			},
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateBotChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	if resp.References != nil {
		for _, ref := range resp.References {
			fmt.Printf("reference url: %s\n", ref.Url)
		}
	}
}

```

**Response:**

```json
{
    "id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
    "choices": [
      {
        "finish_reason": "stop",
        "index": 0,
        "message": {
          "content": "以下是杭州今天（2025年4月8日）的天气情况：\n- **整体天气状况**：今天多云，偏南风3级。\n- **气温情况**：白天最高气温31度，平均相对湿度55%。\n- **空气质量**：杭州市区AQI为90 - 110，首要污染物是O₃，空气质量等级为良 - 轻度污染，易感人群需减少户外锻炼。 ",
          "role": "assistant"
        }
      }
    ],
    "created": 1744102143,
    "model": "doubao-1-5-pro-32k-250115",
    "object": "chat.completion",
    "bot_usage": {
      "model_usage": [
        {
          "name": "doubao-1-5-pro-32k-250115",
          "prompt_tokens": 1960,
          "completion_tokens": 99,
          "total_tokens": 2059
        }
      ],
      "action_usage": [
        {
          "action_name": "content_plugin",
          "count": 1
        }
      ],
      "action_details": [
        {
          "name": "content_plugin",
          "count": 1,
          "tool_details": [
            {
              "name": "search",
              "input": {
                "queries": [
                  "杭州今天天气"
                ],
                "source_type": [
                  "search_engine"
                ],
                "offset": 0,
                "count": 5,
                "account_id": 2100000825,
                "video_token": ""
              },
              "output": {
                "type": "tool",
                "data": {
                  "status_code": 200,
                  "data": {
                    "has_more": true,
                    "results": [
                      {
                        "id": "3ba8807eabf8793d-65e575422f8ebfb2",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-中国天气",
                        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
                        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
                        "publish_time": 0,
                        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "3ba8807eabf8793d-65e575422f8ebfb2",
                          "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
                          "auth_info": "非常权威",
                          "auth_score": 1,
                          "rel_info": "强相关",
                          "rel_score": 0.994623,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      },
                      ...
                      {
                        "id": "d4d70607487f997f-b2adf6b6d837f126",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-www.hzqx.com",
                        "title": "杭州天气",
                        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
                        "publish_time": 1744079400,
                        "url": "https://www.hzqx.com/hztq/index.html",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "d4d70607487f997f-b2adf6b6d837f126",
                          "logo_url": "",
                          "auth_info": "非常权威",
                          "auth_score": 0.875495,
                          "rel_info": "强相关",
                          "rel_score": 0.834824,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      }
                    ],
                    "debug_data": {
                      "extra": null
                    },
                    "log_id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
                    "timing": {
                      "total": 1431
                    }
                  }
                }
              },
              "created_at": 1744102137810,
              "completed_at": 1744102139264
            }
          ]
        }
      ]
    },
    "metadata": {},
    "references": [
      {
        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
        "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
        "site_name": "搜索引擎-中国天气",
        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
        "publish_time": "1970年01月01日 08:00:00(CST) 星期四",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      },
      ...
      {
        "url": "https://www.hzqx.com/hztq/index.html",
        "logo_url": "",
        "site_name": "搜索引擎-www.hzqx.com",
        "title": "杭州天气",
        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
        "publish_time": "2025年04月08日 10:30:00(CST) 星期二",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      }
    ]
  }
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
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
                ChatMessage.builder().role(ChatMessageRole.USER).content("杭州今天天气").build();

        messagesForReqList.add(elementForMessagesForReqList0);

        BotChatCompletionRequest req =
                BotChatCompletionRequest.builder()
                        .model("bot-20250407******-*****")
                        .messages(messagesForReqList)
                        .build();

        service.createBotChatCompletion(req)
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
    "id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
    "choices": [
      {
        "finish_reason": "stop",
        "index": 0,
        "message": {
          "content": "以下是杭州今天（2025年4月8日）的天气情况：\n- **整体天气状况**：今天多云，偏南风3级。\n- **气温情况**：白天最高气温31度，平均相对湿度55%。\n- **空气质量**：杭州市区AQI为90 - 110，首要污染物是O₃，空气质量等级为良 - 轻度污染，易感人群需减少户外锻炼。 ",
          "role": "assistant"
        }
      }
    ],
    "created": 1744102143,
    "model": "doubao-1-5-pro-32k-250115",
    "object": "chat.completion",
    "bot_usage": {
      "model_usage": [
        {
          "name": "doubao-1-5-pro-32k-250115",
          "prompt_tokens": 1960,
          "completion_tokens": 99,
          "total_tokens": 2059
        }
      ],
      "action_usage": [
        {
          "action_name": "content_plugin",
          "count": 1
        }
      ],
      "action_details": [
        {
          "name": "content_plugin",
          "count": 1,
          "tool_details": [
            {
              "name": "search",
              "input": {
                "queries": [
                  "杭州今天天气"
                ],
                "source_type": [
                  "search_engine"
                ],
                "offset": 0,
                "count": 5,
                "account_id": 2100000825,
                "video_token": ""
              },
              "output": {
                "type": "tool",
                "data": {
                  "status_code": 200,
                  "data": {
                    "has_more": true,
                    "results": [
                      {
                        "id": "3ba8807eabf8793d-65e575422f8ebfb2",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-中国天气",
                        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
                        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
                        "publish_time": 0,
                        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "3ba8807eabf8793d-65e575422f8ebfb2",
                          "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
                          "auth_info": "非常权威",
                          "auth_score": 1,
                          "rel_info": "强相关",
                          "rel_score": 0.994623,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      },
                      ...
                      {
                        "id": "d4d70607487f997f-b2adf6b6d837f126",
                        "source_type": "search_engine",
                        "site_name": "搜索引擎-www.hzqx.com",
                        "title": "杭州天气",
                        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
                        "publish_time": 1744079400,
                        "url": "https://www.hzqx.com/hztq/index.html",
                        "cover_image": null,
                        "search_plugin_data": {
                          "doc_id": "d4d70607487f997f-b2adf6b6d837f126",
                          "logo_url": "",
                          "auth_info": "非常权威",
                          "auth_score": 0.875495,
                          "rel_info": "强相关",
                          "rel_score": 0.834824,
                          "freshness_info": "非常满足时效需求",
                          "freshness_score": 0.994948,
                          "final_ref": "",
                          "final_ref_len": 0,
                          "author_name": "",
                          "duration": 0
                        },
                        "smart_content_data": null,
                        "ruyi_data": null
                      }
                    ],
                    "debug_data": {
                      "extra": null
                    },
                    "log_id": "02174410213780126458cc51545801ff6ffb59c94a55dae86c43b",
                    "timing": {
                      "total": 1431
                    }
                  }
                }
              },
              "created_at": 1744102137810,
              "completed_at": 1744102139264
            }
          ]
        }
      ]
    },
    "metadata": {},
    "references": [
      {
        "url": "https://m.weather.com.cn/mweather15d/101210101.shtml#!/abighis",
        "logo_url": "https://p3-search.byteimg.com/img/labis/dafd663cfa3b7fce9addcca7916010cb~noop.jpeg",
        "site_name": "搜索引擎-中国天气",
        "title": "【杭州天气预报15天_杭州天气预报15天查询】-中国天气网",
        "summary": "20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级 20/8℃ 东北风<3级 北风<3级 19/7℃ 东风<3级 南风<3级 26/12℃ 东风<3级 西风<3级",
        "publish_time": "1970年01月01日 08:00:00(CST) 星期四",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      },
      ...
      {
        "url": "https://www.hzqx.com/hztq/index.html",
        "logo_url": "",
        "site_name": "搜索引擎-www.hzqx.com",
        "title": "杭州天气",
        "summary": "杭州国家基准气候站发布 25.7℃ 较昨日同期 高2.8℃ 相对湿度：40 % 风力： 东南2级 气压： 1012.1 hPa 近1小时降水:0 mm 能见度：22652 m *实时数据、未经人工复核 杭州市气象台4月8日8时50分发布的市区天气预报：\n今天多云；明天多云转阴，傍晚到夜里局部有阵雨；后天阴转多云。今天偏南风3级。今天白天最高气温31度，明天白天最高气温32度，明天早晨最低气温21度，今天平均相对湿度55%。2025年04月08日 08时50分 过去24小时 主城区 未来7天天气客观预报（该预报数据为数值预报自动输出，未经人工订正，仅供参考） 白天 08时 | 20时 夜晚 昨天20时 | 当天08时 今天 明天 周四 周五 周六 周日 周一 04/08 04/09 04/10 04/11 04/12 04/13 04/14 多云 多云 晴 多云 多云 晴 晴 晴 多云 晴 大雨 晴 晴 东风 东南风 东北风 东风 北风 西南风 东北风 不超过3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 ≤3级 31.7℃ 32.2℃ 32.2℃ 31.2℃ 19.4℃ 25.2℃ 27.3℃ 17.6℃ 19.4℃ 21.5℃ 19.3℃ 17.7℃ 11.2℃ 14.7℃ * 白天为当天08时到20时，通常气温为最高温度； 夜晚为昨天20时到当天08时，通常气温为最低温度。杭州市当前气温2025-04-08 10:30 分布图 今日生活气象 空气质量预报 杭州市环境监测站和杭州市气象台 联合发布 4月8日杭州市区AQI:90-110，首要污染物:O3，空气质量等级:良-轻度污染。易感人群减少户外锻炼。4月9日杭州市区AQI:80-100，首要污染物:O3，空气质量等级:良。",
        "publish_time": "2025年04月08日 10:30:00(CST) 星期二",
        "extra": {
          "rel_info": "强相关",
          "freshness_info": "非常满足时效需求",
          "auth_info": "非常权威",
          "final_ref": ""
        }
      }
    ]
  }
```

**curl**

```bash
curl 'https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions' \
-H "Authorization: Bearer $ARK_API_KEY"  \
-H 'Content-Type: application/json' \
-d '{
    "model": "bot-20250707202501-k6czp", 
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
  "id": "021751891462122d1a8c9bcb9c54eb50913f5e75c28a3ac724183",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nHello there! How can I assist you today? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just said \"Hello!\" - a simple and friendly greeting. Since this is likely the start of a conversation, they're probably testing the waters or initiating casual interaction. No complex needs here, but I should match their upbeat tone. \n\nAh, the response was straightforward - \"Hello there! How can I assist you today?\" Perfectly neutral yet warm opening. Kept it open-ended to invite follow-up. The exclamation point adds energy without being overbearing. \n\nWonder if they're new or returning? Either way, no need to overthink - just acknowledge and pivot to assistance. Short responses like this usually mean they'll either ask something specific next or vanish. Better keep the reply crisp.\n"
      }
    }
  ],
  "created": 1751891468,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 35,
        "completion_tokens": 159,
        "total_tokens": 194
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250707202501-k6czp",
        messages=[{"content":"You are a helpful assistant.","role":"system"},{"content":"Hello!","role":"user"}],
    )
    print(resp.choices[0].message.content)
```

**Response:**

```json
{
  "id": "021751891462122d1a8c9bcb9c54eb50913f5e75c28a3ac724183",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nHello there! How can I assist you today? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just said \"Hello!\" - a simple and friendly greeting. Since this is likely the start of a conversation, they're probably testing the waters or initiating casual interaction. No complex needs here, but I should match their upbeat tone. \n\nAh, the response was straightforward - \"Hello there! How can I assist you today?\" Perfectly neutral yet warm opening. Kept it open-ended to invite follow-up. The exclamation point adds energy without being overbearing. \n\nWonder if they're new or returning? Either way, no need to overthink - just acknowledge and pivot to assistance. Short responses like this usually mean they'll either ask something specific next or vanish. Better keep the reply crisp.\n"
      }
    }
  ],
  "created": 1751891468,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 35,
        "completion_tokens": 159,
        "total_tokens": 194
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250707202501-k6czp",
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
	fmt.Println("----- standard request -----")
	resp, err := client.CreateBotChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	if resp.References != nil {
		for _, ref := range resp.References {
			fmt.Printf("reference url: %s\n", ref.Url)
		}
	}
}

```

**Response:**

```json
{
  "id": "021751891462122d1a8c9bcb9c54eb50913f5e75c28a3ac724183",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nHello there! How can I assist you today? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just said \"Hello!\" - a simple and friendly greeting. Since this is likely the start of a conversation, they're probably testing the waters or initiating casual interaction. No complex needs here, but I should match their upbeat tone. \n\nAh, the response was straightforward - \"Hello there! How can I assist you today?\" Perfectly neutral yet warm opening. Kept it open-ended to invite follow-up. The exclamation point adds energy without being overbearing. \n\nWonder if they're new or returning? Either way, no need to overthink - just acknowledge and pivot to assistance. Short responses like this usually mean they'll either ask something specific next or vanish. Better keep the reply crisp.\n"
      }
    }
  ],
  "created": 1751891468,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 35,
        "completion_tokens": 159,
        "total_tokens": 194
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
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

        BotChatCompletionRequest req =
                BotChatCompletionRequest.builder()
                        .model("bot-20250707202501-k6czp")
                        .messages(messagesForReqList)
                        .build();

        service.createBotChatCompletion(req)
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
  "id": "021751891462122d1a8c9bcb9c54eb50913f5e75c28a3ac724183",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nHello there! How can I assist you today? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just said \"Hello!\" - a simple and friendly greeting. Since this is likely the start of a conversation, they're probably testing the waters or initiating casual interaction. No complex needs here, but I should match their upbeat tone. \n\nAh, the response was straightforward - \"Hello there! How can I assist you today?\" Perfectly neutral yet warm opening. Kept it open-ended to invite follow-up. The exclamation point adds energy without being overbearing. \n\nWonder if they're new or returning? Either way, no need to overthink - just acknowledge and pivot to assistance. Short responses like this usually mean they'll either ask something specific next or vanish. Better keep the reply crisp.\n"
      }
    }
  ],
  "created": 1751891468,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 35,
        "completion_tokens": 159,
        "total_tokens": 194
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "bot-20250707202501-k6czp",
    "messages": [
        {
            "content": "You are a helpful assistant.",
            "role": "system"
        },
        {
            "content": "Hello!",
            "role": "user"
        },
        {
            "content": "Hello there! How can I assist you today? ",
            "role": "assistant"
        },
        {
            "content": "what is your name?",
            "role": "user"
        }
    ]
}'
```

**Response:**

```json
{
  "id": "021751892149044f7befb30d3a98790c255d78335d5360ad3db3d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nGreat question! 😊  \nYou can call me **Sophie** — your friendly AI assistant. I'm here to help you with questions, ideas, tasks, or anything else you'd like to explore. How about you — what should I call you? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just asked for my name after ..."
      }
    }
  ],
  "created": 1751892158,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 54,
        "completion_tokens": 268,
        "total_tokens": 322
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250707202501-k6czp",
        messages=[{"content":"You are a helpful assistant.","role":"system"},{"content":"Hello!","role":"user"},{"content":"Hello there! How can I assist you today? ","role":"assistant"},{"content":"what is your name?","role":"user"}],
    )
    print(resp.choices[0].message.content)
```

**Response:**

```json
{
  "id": "021751892149044f7befb30d3a98790c255d78335d5360ad3db3d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nGreat question! 😊  \nYou can call me **Sophie** — your friendly AI assistant. I'm here to help you with questions, ideas, tasks, or anything else you'd like to explore. How about you — what should I call you? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just asked for my name after ..."
      }
    }
  ],
  "created": 1751892158,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 54,
        "completion_tokens": 268,
        "total_tokens": 322
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250707202501-k6czp",
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
			&model.ChatCompletionMessage{
				Role: "assistant",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("Hello there! How can I assist you today? "),
				},
			},
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("what is your name?"),
				},
			},
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateBotChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	if resp.References != nil {
		for _, ref := range resp.References {
			fmt.Printf("reference url: %s\n", ref.Url)
		}
	}
}

```

**Response:**

```json
{
  "id": "021751892149044f7befb30d3a98790c255d78335d5360ad3db3d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nGreat question! 😊  \nYou can call me **Sophie** — your friendly AI assistant. I'm here to help you with questions, ideas, tasks, or anything else you'd like to explore. How about you — what should I call you? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just asked for my name after ..."
      }
    }
  ],
  "created": 1751892158,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 54,
        "completion_tokens": 268,
        "total_tokens": 322
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
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

        ChatMessage elementForMessagesForReqList2 =
                ChatMessage.builder()
                        .role(ChatMessageRole.ASSISTANT)
                        .content("Hello there! How can I assist you today? ")
                        .build();

        ChatMessage elementForMessagesForReqList3 =
                ChatMessage.builder()
                        .role(ChatMessageRole.USER)
                        .content("what is your name?")
                        .build();
        messagesForReqList.add(elementForMessagesForReqList0);
        messagesForReqList.add(elementForMessagesForReqList1);
        messagesForReqList.add(elementForMessagesForReqList2);
        messagesForReqList.add(elementForMessagesForReqList3);

        BotChatCompletionRequest req =
                BotChatCompletionRequest.builder()
                        .model("bot-20250707202501-k6czp")
                        .messages(messagesForReqList)
                        .build();

        service.createBotChatCompletion(req)
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
  "id": "021751892149044f7befb30d3a98790c255d78335d5360ad3db3d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "\nGreat question! 😊  \nYou can call me **Sophie** — your friendly AI assistant. I'm here to help you with questions, ideas, tasks, or anything else you'd like to explore. How about you — what should I call you? 😊",
        "role": "assistant",
        "reasoning_content": "Hmm, the user just asked for my name after ..."
      }
    }
  ],
  "created": 1751892158,
  "model": "deepseek-r1-250528",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 54,
        "completion_tokens": 268,
        "total_tokens": 322
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "bot-20250707202501-k6czp",
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
    "stream": true,
    "stream_options": {
        "include_usage": true
    }
}'
```

**Response:**

```json
{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":"Ah"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":","},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":".\n"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"\n","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}
...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":" 😊","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","bot_usage":{"model_usage":[{"prompt_tokens":35,"completion_tokens":175,"total_tokens":210}],"action_usage":[],"action_details":[]},"metadata":{}}

[DONE]
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250707202501-k6czp",
        messages=[{"content":"You are a helpful assistant.","role":"system"},{"content":"Hello!","role":"user"}],
        stream=True,
        stream_options={"include_usage":True},
    )
    for chunk in resp:
        if not chunk.choices:
            continue

        print(chunk.choices[0].delta.content, end="")
```

**Response:**

```json
{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":"Ah"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":","},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":".\n"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"\n","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}
...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":" 😊","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","bot_usage":{"model_usage":[{"prompt_tokens":35,"completion_tokens":175,"total_tokens":210}],"action_usage":[],"action_details":[]},"metadata":{}}

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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250707202501-k6czp",
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
		Stream: true,
		StreamOptions: &model.StreamOptions{
			IncludeUsage: true,
		},
	}
	fmt.Println("----- streaming request -----")
	stream, err := client.CreateBotChatCompletionStream(ctx, req)
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
			if recv.References != nil {
				for _, ref := range recv.References {
					fmt.Printf("reference url: %s\n", ref.Url)
				}
			}
		}
	}
}

```

**Response:**

```json
{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":"Ah"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":","},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":".\n"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"\n","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}
...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":" 😊","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","bot_usage":{"model_usage":[{"prompt_tokens":35,"completion_tokens":175,"total_tokens":210}],"action_usage":[],"action_details":[]},"metadata":{}}

[DONE]
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
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

        ChatCompletionRequestStreamOptions streamOptionsForReq =
                new ChatCompletionRequestStreamOptions(true);

        BotChatCompletionRequest req =
                BotChatCompletionRequest.builder()
                        .model("bot-20250707202501-k6czp")
                        .messages(messagesForReqList)
                        .stream(true)
                        .streamOptions(streamOptionsForReq)
                        .build();

        service.streamBotChatCompletion(req)
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
{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":"Ah"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":","},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant","reasoning_content":".\n"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"\n","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"Hello","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}
...

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":" 😊","role":"assistant"},"index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[{"delta":{"content":"","role":"assistant"},"finish_reason":"stop","index":0}],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","metadata":{}}

{"id":"0217518925181918b15924cfe8caf3be8175ae89769e0203c419c","choices":[],"created":1751892518,"model":"deepseek-r1-250528","object":"chat.completion.chunk","bot_usage":{"model_usage":[{"prompt_tokens":35,"completion_tokens":175,"total_tokens":210}],"action_usage":[],"action_details":[]},"metadata":{}}

[DONE]
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "bot-20250707205612-rr7gm",
    "messages": [
        {
            "content": "你是谁？",
            "role": "user"
        }
    ],
    "metadata": {
        "group_chat_config": {
            "characters": [
                {
                    "model_desc": {
                        "endpoint_id": "ep-20250703110544-ht42m"
                    },
                    "name": "孙悟空",
                    "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。"
                }
            ],
            "description": "遇到妖精",
            "user_name": "唐僧"
        },
        "target_character_name": "孙悟空"
    },
    "stream": false,
    "stream_options": {
        "include_usage": false
    }
}'
```

**Response:**

```json
{
  "id": "021751893662764bd5b399bfe40c748dcd249c43e3efca02ca61d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "用户现在需要我以孙悟空的身份回复唐僧的询问。首先要体现出孙悟空的性格，语气要有点傲娇又带着不耐烦但又带着忠诚的感觉。那应该这样回复：“俺乃齐天大圣孙悟空！你这呆子，怎地连俺都认不得了？俺可是要护你去西天取经的啊！”</think>俺乃齐天大圣孙悟空！你这秃驴莫不是糊涂了？俺老孙在此，正是要保你去西天取经呢，还能是谁？",
        "role": "assistant",
        "reasoning_content": "俺老孙乃是齐天大圣孙悟空！你这肉眼凡胎的和尚，连俺都认不出来啦？俺可是保护你去西天取经的大徒弟啊！"
      }
    }
  ],
  "created": 1751893664,
  "model": "doubao-seed-1-6-flash-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 149,
        "completion_tokens": 143,
        "total_tokens": 292
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {
    "group_chat_config": {
      "user_name": "唐僧",
      "description": "遇到妖精",
      "characters": [
        {
          "name": "孙悟空",
          "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。",
          "model_desc": {
            "endpoint_id": "ep-20250703110544-ht42m"
          }
        }
      ]
    },
    "target_character_name": "孙悟空"
  }
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250707205612-rr7gm",
        messages=[{"content":"你是谁？","role":"user"}],
        stream=False,
        stream_options={"include_usage":False},
        metadata={"group_chat_config":{"characters":[{"model_desc":{"endpoint_id":"ep-20250703110544-ht42m"},"name":"孙悟空","system_prompt":"齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。"}],"description":"遇到妖精","user_name":"唐僧"},"target_character_name":"孙悟空"},
    )
    print(resp.choices[0].message.content)
```

**Response:**

```json
{
  "id": "021751893662764bd5b399bfe40c748dcd249c43e3efca02ca61d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "用户现在需要我以孙悟空的身份回复唐僧的询问。首先要体现出孙悟空的性格，语气要有点傲娇又带着不耐烦但又带着忠诚的感觉。那应该这样回复：“俺乃齐天大圣孙悟空！你这呆子，怎地连俺都认不得了？俺可是要护你去西天取经的啊！”</think>俺乃齐天大圣孙悟空！你这秃驴莫不是糊涂了？俺老孙在此，正是要保你去西天取经呢，还能是谁？",
        "role": "assistant",
        "reasoning_content": "俺老孙乃是齐天大圣孙悟空！你这肉眼凡胎的和尚，连俺都认不出来啦？俺可是保护你去西天取经的大徒弟啊！"
      }
    }
  ],
  "created": 1751893664,
  "model": "doubao-seed-1-6-flash-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 149,
        "completion_tokens": 143,
        "total_tokens": 292
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {
    "group_chat_config": {
      "user_name": "唐僧",
      "description": "遇到妖精",
      "characters": [
        {
          "name": "孙悟空",
          "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。",
          "model_desc": {
            "endpoint_id": "ep-20250703110544-ht42m"
          }
        }
      ]
    },
    "target_character_name": "孙悟空"
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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250707205612-rr7gm",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("你是谁？"),
				},
			},
		},
		Stream: false,
		StreamOptions: &model.StreamOptions{
			IncludeUsage: false,
		},
		Metadata: map[string]interface{}{
			"group_chat_config":     map[string]interface{}{"characters": []interface{}{map[string]interface{}{"model_desc": map[string]interface{}{"endpoint_id": "ep-20250703110544-ht42m"}, "name": "孙悟空", "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。"}}, "description": "遇到妖精", "user_name": "唐僧"},
			"target_character_name": "孙悟空",
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateBotChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	fmt.Println(*resp.Choices[0].Message.Content.StringValue)
	if resp.References != nil {
		for _, ref := range resp.References {
			fmt.Printf("reference url: %s\n", ref.Url)
		}
	}
}

```

**Response:**

```json
{
  "id": "021751893662764bd5b399bfe40c748dcd249c43e3efca02ca61d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "用户现在需要我以孙悟空的身份回复唐僧的询问。首先要体现出孙悟空的性格，语气要有点傲娇又带着不耐烦但又带着忠诚的感觉。那应该这样回复：“俺乃齐天大圣孙悟空！你这呆子，怎地连俺都认不得了？俺可是要护你去西天取经的啊！”</think>俺乃齐天大圣孙悟空！你这秃驴莫不是糊涂了？俺老孙在此，正是要保你去西天取经呢，还能是谁？",
        "role": "assistant",
        "reasoning_content": "俺老孙乃是齐天大圣孙悟空！你这肉眼凡胎的和尚，连俺都认不出来啦？俺可是保护你去西天取经的大徒弟啊！"
      }
    }
  ],
  "created": 1751893664,
  "model": "doubao-seed-1-6-flash-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 149,
        "completion_tokens": 143,
        "total_tokens": 292
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {
    "group_chat_config": {
      "user_name": "唐僧",
      "description": "遇到妖精",
      "characters": [
        {
          "name": "孙悟空",
          "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。",
          "model_desc": {
            "endpoint_id": "ep-20250703110544-ht42m"
          }
        }
      ]
    },
    "target_character_name": "孙悟空"
  }
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.model.completion.chat.ChatCompletionRequest.*;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
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
                ChatMessage.builder().role(ChatMessageRole.USER).content("你是谁？").build();

        messagesForReqList.add(elementForMessagesForReqList0);

        ChatCompletionRequestStreamOptions streamOptionsForReq =
                new ChatCompletionRequestStreamOptions(false);

        BotChatCompletionRequest req =
                BotChatCompletionRequest.builder()
                        .model("bot-20250707205612-rr7gm")
                        .messages(messagesForReqList)
                        .metadata(
                                (convertMap(
                                        "{\"group_chat_config\":{\"characters\":[{\"model_desc\":{\"endpoint_id\":\"ep-20250703110544-ht42m\"},\"name\":\"孙悟空\",\"system_prompt\":\"齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。\"}],\"description\":\"遇到妖精\",\"user_name\":\"唐僧\"},\"target_character_name\":\"孙悟空\"}")))
                        .build();

        service.createBotChatCompletion(req)
                .getChoices()
                .forEach(choice -> System.out.println(choice.getMessage().getContent()));
        // shutdown service after all requests is finished
        service.shutdownExecutor();
    }

    public static Map<String, Object> convertMap(String jsonString) throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper();
        Map<String, Object> map = objectMapper.readValue(jsonString, Map.class);
        return map;
    }
}

```

**Response:**

```json
{
  "id": "021751893662764bd5b399bfe40c748dcd249c43e3efca02ca61d",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "用户现在需要我以孙悟空的身份回复唐僧的询问。首先要体现出孙悟空的性格，语气要有点傲娇又带着不耐烦但又带着忠诚的感觉。那应该这样回复：“俺乃齐天大圣孙悟空！你这呆子，怎地连俺都认不得了？俺可是要护你去西天取经的啊！”</think>俺乃齐天大圣孙悟空！你这秃驴莫不是糊涂了？俺老孙在此，正是要保你去西天取经呢，还能是谁？",
        "role": "assistant",
        "reasoning_content": "俺老孙乃是齐天大圣孙悟空！你这肉眼凡胎的和尚，连俺都认不出来啦？俺可是保护你去西天取经的大徒弟啊！"
      }
    }
  ],
  "created": 1751893664,
  "model": "doubao-seed-1-6-flash-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 149,
        "completion_tokens": 143,
        "total_tokens": 292
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {
    "group_chat_config": {
      "user_name": "唐僧",
      "description": "遇到妖精",
      "characters": [
        {
          "name": "孙悟空",
          "system_prompt": "齐天大圣孙悟空，生性聪明、活泼、忠诚、嫉恶如仇，有责任心，偶尔也有些骄傲自负。",
          "model_desc": {
            "endpoint_id": "ep-20250703110544-ht42m"
          }
        }
      ]
    },
    "target_character_name": "孙悟空"
  }
}
```

**curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/bots/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d $'{
    "model": "bot-20250707202501-k6czp",
    "messages": [
        {
            "content": [
                {
                    "image_url": {
                        "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/ark_demo_img_1.png"
                    },
                    "type": "image_url"
                }
            ],
            "role": "user"
        },
        {
            "content": [
                {
                    "text": "支持输入是图片的模型系列是哪个？",
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
  "id": "021751894443607bd1685776f26233986d9c881482ac5cc63c00a",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "支持输入是图片的模型系列是**Doubao-1.5-vision**。\n\n在表格中，“输入”列的“图像”项中，仅Doubao-1.5-vision对应的单元格标记为“√”，表示该模型系列支持图像输入。",
        "role": "assistant",
        "reasoning_content": "\n我现在需要解决的问题是：“支持输入是图片的模型系列是哪个？..."
      }
    }
  ],
  "created": 1751894450,
  "model": "doubao-seed-1-6-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 1131,
        "completion_tokens": 310,
        "total_tokens": 1441
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**python**

```python
import os

from volcenginesdkarkruntime import Ark

# For more information，please check this document（https://www.volcengine.com/docs/82379/1263279）
client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.bot_chat.completions.create(
        model="bot-20250707202501-k6czp",
        messages=[{"content":[{"image_url":{"url":"https://ark-project.tos-cn-beijing.volces.com/doc_image/ark_demo_img_1.png"},"type":"image_url"}],"role":"user"},{"content":[{"text":"支持输入是图片的模型系列是哪个？","type":"text"}],"role":"user"}],
    )
    print(resp.choices[0].message.content)
```

**Response:**

```json
{
  "id": "021751894443607bd1685776f26233986d9c881482ac5cc63c00a",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "支持输入是图片的模型系列是**Doubao-1.5-vision**。\n\n在表格中，“输入”列的“图像”项中，仅Doubao-1.5-vision对应的单元格标记为“√”，表示该模型系列支持图像输入。",
        "role": "assistant",
        "reasoning_content": "\n我现在需要解决的问题是：“支持输入是图片的模型系列是哪个？..."
      }
    }
  ],
  "created": 1751894450,
  "model": "doubao-seed-1-6-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 1131,
        "completion_tokens": 310,
        "total_tokens": 1441
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
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
	req := model.BotChatCompletionRequest{
		Model: "bot-20250707202501-k6czp",
		Messages: []*model.ChatCompletionMessage{
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("[map[image_url:map[url:https://ark-project.tos-cn-beijing.volces.com/doc_image/ark_demo_img_1.png] type:image_url]]"),
				},
			},
			&model.ChatCompletionMessage{
				Role: "user",
				Content: &model.ChatCompletionMessageContent{
					StringValue: volcengine.String("[map[text:支持输入是图片的模型系列是哪个？ type:text]]"),
				},
			},
		},
	}
	fmt.Println("----- standard request -----")
	resp, err := client.CreateBotChatCompletion(ctx, req)
	if err != nil {
		fmt.Printf("standard chat error: %v\n", err)
		return
	}
	if resp.References != nil {
		for _, ref := range resp.References {
			fmt.Printf("reference url: %s\n", ref.Url)
		}
	}
}

```

**Response:**

```json
{
  "id": "021751894443607bd1685776f26233986d9c881482ac5cc63c00a",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "支持输入是图片的模型系列是**Doubao-1.5-vision**。\n\n在表格中，“输入”列的“图像”项中，仅Doubao-1.5-vision对应的单元格标记为“√”，表示该模型系列支持图像输入。",
        "role": "assistant",
        "reasoning_content": "\n我现在需要解决的问题是：“支持输入是图片的模型系列是哪个？..."
      }
    }
  ],
  "created": 1751894450,
  "model": "doubao-seed-1-6-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 1131,
        "completion_tokens": 310,
        "total_tokens": 1441
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.bot.completion.chat.BotChatCompletionRequest;
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
  static ArkService service = ArkService.builder()
      .dispatcher(dispatcher)
      .connectionPool(connectionPool)
      .apiKey(apiKey)
      .build();

  public static void main(String[] args) throws JsonProcessingException {

    List<ChatMessage> messagesForReqList = new ArrayList<>();

    List<ChatCompletionContentPart> contentForElementForMessagesForReqList0 = new ArrayList<>();
    // 创建并设置图片内容部分
    ChatCompletionContentPart elementForContentForElementForMessagesForReqList00 = ChatCompletionContentPart.builder()
        .type("image_url")
        .imageUrl(new ChatCompletionContentPart.ChatCompletionContentPartImageURL(
            "https://ark-project.tos-cn-beijing.volces.com/doc_image/ark_demo_img_1.png"))
        .build();
    ChatCompletionContentPart elementForContentForElementForMessagesForReqList01 = ChatCompletionContentPart.builder()
        .type("text")
        .text("支持输入是图片的模型系列是哪个？")
        .build();
    contentForElementForMessagesForReqList0.add(
        elementForContentForElementForMessagesForReqList00);
    contentForElementForMessagesForReqList0.add(
        elementForContentForElementForMessagesForReqList01);

    // 创建用户消息对象并设置内容
    ChatMessage elementForMessagesForReqList0 = ChatMessage.builder()
        .role(ChatMessageRole.USER)
        .multiContent(contentForElementForMessagesForReqList0)
        .build();
    // 添加用户消息到消息列表中
    messagesForReqList.add(elementForMessagesForReqList0);

    BotChatCompletionRequest req = BotChatCompletionRequest.builder()
        .model("bot-20250707202501-k6czp")
        .messages(messagesForReqList)
        .build();

    service.createBotChatCompletion(req)
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
  "id": "021751894443607bd1685776f26233986d9c881482ac5cc63c00a",
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "支持输入是图片的模型系列是**Doubao-1.5-vision**。\n\n在表格中，“输入”列的“图像”项中，仅Doubao-1.5-vision对应的单元格标记为“√”，表示该模型系列支持图像输入。",
        "role": "assistant",
        "reasoning_content": "\n我现在需要解决的问题是：“支持输入是图片的模型系列是哪个？..."
      }
    }
  ],
  "created": 1751894450,
  "model": "doubao-seed-1-6-250615",
  "object": "chat.completion",
  "bot_usage": {
    "model_usage": [
      {
        "prompt_tokens": 1131,
        "completion_tokens": 310,
        "total_tokens": 1441
      }
    ],
    "action_usage": [],
    "action_details": []
  },
  "metadata": {}
}
```

****
**
本接口支持 API Key 鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
如需使用 Access Key 来鉴权，推荐使用 SDK 的方式，具体请参见 [SDK概述](https://www.volcengine.com/docs/82379/1302007)。
`object`
系统消息，开发人员提供的指令，模型应遵循这些指令。如模型扮演的角色或者目标等。
messages.**role**`string``必选`
发送消息的角色，此处应为`system`。
messages.**content**`string / object[]``必选`
系统信息内容。
模型调用工具的消息。
发送消息的角色，此处应为`tool`。
messages.**content**`string / array``必选`
工具返回的消息。
messages.**tool_call_id **`string``必选`
模型调用的工具的 ID。
**id**`string`
本次请求的唯一标识。
**model**`string`
本次请求实际使用的模型名称和版本。
doubao 1.5 代模型的模型名称格式为 doubao-1-5-**。如调用部署doubao-1.5-pro-32k 250115模型的推理接入点，返回model字段信息doubao-1-5-pro-32k-250115。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**object**`string`
固定为 `chat.completion.chunk`。
**choices**`object[]`
本次请求的模型输出内容。
choices.**index **`integer`
当前元素在 **choices** 列表的索引。
choices.**finish_reason **`string`
模型停止生成 token 的原因。取值范围：
`stop`：模型输出自然结束，或因命中请求参数 stop 中指定的字段而被截断。
`length`：模型输出因达到模型输出限制而被截断，有下面几种原因：
触发`max_token`限制（回答内容的长度限制）。
触发`max_completion_tokens`限制（思维链内容+回答内容的长度限制）。
触发`context_window` 限制（输入内容+思维链内容+回答内容的长度限制）。
`content_filter`：模型输出被内容审核拦截。
`tool_calls`：模型调用了工具。
choices.**delta **`object`
模型输出的增量内容。
choices.delta.**role **`string`
内容输出的角色，此处固定为 `assistant`。
choices.delta.**content **`string`
模型生成的消息内容。
choices.delta.**reasoning_content **`string / null`
模型处理问题的思维链内容。
仅深度推理模型支持返回此字段，深度推理模型请参见[支持模型](https://www.volcengine.com/docs/82379/1449737#5f0f3750)。
choices.delta.**tool_calls **`object[] / null`
模型调用的工具列表。
choices.delta.tool_calls.**i****d **`string`
调用的工具的 ID。
choices.delta.tool_calls.**index**`interger`
当前元素在模型调用的工具列表的索引。
choices.delta.tool_calls.**type **`string`
工具类型，当前仅支持`function`。
choices.delta.tool_calls.**function **`object`
模型调用的函数。
choices.delta.tool_calls.function.**name **`string`
模型调用的函数的名称。
choices.delta.tool_calls.function.**arguments **`string`
模型生成的用于调用函数的参数，JSON 格式。
模型并不总是生成有效的 JSON，并且可能会虚构出一些您的函数参数规范中未定义的参数。在调用函数之前，请在您的代码中验证这些参数是否有效。
choices.**logprobs **`object / null`
当前内容的对数概率信息。
choices.logprobs.**content **`object[] / null`
message列表中每个 content 元素中的 token 对数概率信息。
choices.logprobs.content.**token **`string`
当前 token。
choices.logprobs.content.**bytes **`integer[] / null`
当前 token 的 UTF-8 值，格式为整数列表。当一个字符由多个 token 组成（表情符号或特殊字符等）时可以用于字符的编码和解码。如果 token 没有 UTF-8 值则为空。
choices.logprobs.content.**logprob **`float`
当前 token 的对数概率。
choices.logprobs.content.**top_logprobs **`object[]`
在当前 token 位置最有可能的标记及其对数概率的列表。在一些情况下，返回的数量可能比请求参数 top_logprobs 指定的数量要少。
choices.logprobs.content.top_logprobs.**token **`string`
当前 token。
choices.logprobs.content.top_logprobs.**bytes **`integer[] / null`
当前 token 的 UTF-8 值，格式为整数列表。当一个字符由多个 token 组成（表情符号或特殊字符等）时可以用于字符的编码和解码。如果 token 没有 UTF-8 值则为空。
choices.logprobs.content.top_logprobs.**logprob **`float`
当前 token 的对数概率。
choices.**moderation_hit_type **`string``/ null`
模型输出文字含有敏感信息时，会返回模型输出文字命中的风险分类标签。
返回值及含义：
`severe_violation`：模型输出文字涉及严重违规。
`violence`：模型输出文字涉及激进行为。
注意：当前只有[视觉理解模型](https://www.volcengine.com/docs/82379/1362931#%E6%94%AF%E6%8C%81%E6%A8%A1%E5%9E%8B)支持返回该字段，且只有在方舟控制台[接入点配置页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint/create?customModelId=)或者 [CreateEndpoint](https://www.volcengine.com/docs/82379/1262823) 接口中，将内容护栏方案（ModerationStrategy）设置为基础方案（Basic）时，才会返回风险分类标签。
**bot_usage**`object`
本次请求的 token 用量。
bot_usage.**model_usage **`object[]`
本次请求不同 endpoint 的 token 消耗。
bot_usage.model_usage**.**name `string`
调用的模型 ID。
bot_usage.model_usage**.**prompt_tokens `integer`
输入的 prompt token 数量，包含用户输入的提示词和调用插件返回的信息。
bot_usage.model_usage**.**completion_tokens `integer`
模型生成的 token 数量。
bot_usage.model_usage**.**total_tokens `integer`
本次请求消耗的总 token 数量（输入 + 输出）。
action_usage `object[]`
本次请求插件用量信息。
action_usage.action_name `string`
插件分类名称，如 `content_plugin`（内容插件）等。
action_usage.count `integer`
本次请求某插件分类插件总调用次数。
action_details `object[]`
本次请求插件调用详情。
action_details.name `string`
插件分类名称，如 `content_plugin`（内容插件）等。
action_details.count `integer`
本次请求某插件分类中插件调用次数。
action_details.tool_details `object[]`
某插件分类中插件调用详细信息。
action_details.tool_details.name `string`
具体调用的工具名称。
action_details.tool_details.input `object`
插件输入参数，调用插件的数据结构。
action_details.tool_details.output `object`
插件输出结果，调用插件返回的数据结构。
action_details.tool_details.created_at `integer`
插件调用开始时间。
action_details.tool_details.completed_at `integer`
插件调用结束时间。
**m****a**`object / null`
您创建的[推理接入点](https://www.volcengine.com/docs/82379/1099522)ID。
可以被反序列化成 json 的字符串，需要包含 `city` 和 `district` 两个字段。
**references**`object[]`
本次请求中应用调用了插件的信息。字段参考具体见各个插件的数据结构。
[联网插件](https://www.volcengine.com/docs/82379/1285209)
[知识库插件](https://www.volcengine.com/docs/82379/1285210)
`object[]`
视觉理解模型等多模态模型、部分大语言模型支持此类型。
**文本消息部分**`object`
多模态消息中，内容文本输入。[视觉理解模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3)、部分大语言模型支持此类型消息。
messages.content.**text **`string``必选`
文本消息内容部分。
messages.content.**type **`string``必选`
文本消息类型，此次应为 `text`。
**图像消息部分**`object`
多模态消息中，图像内容部分。[视觉理解模型](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E8%A7%89%E7%90%86%E8%A7%A3)支持此类型消息。
messages.content.**image_url **`object``必选`
图片消息的内容部分。
messages.content.image_url.**url **`string``必选`
支持传入图片链接或图片的Base64编码，不同模型支持图片大小略有不同，具体请参见[使用说明](https://www.volcengine.com/docs/82379/1362931#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)。
传入图片URL：传入图片的可访问链接，推荐使用 TOS（火山引擎对象存储） 存储图片，并生成图片链接。
传入Base64编码：请遵循格式`data:image/<图片格式>;base64,<Base64编码>`，可见[示例](https://www.volcengine.com/docs/82379/1362931#base64-%E7%BC%96%E7%A0%81%E8%BE%93%E5%85%A5)。
messages.content.image_url.**detail **`string / null``默认值 auto`
支持手动设置图片的质量，取值范围`high`、`low`、`auto`。
`high`：高细节模式，适用于需要理解图像细节信息的场景，如对图像的多个局部信息/特征提取、复杂/丰富细节的图像理解等场景，理解更全面。
`low`：低细节模式，适用于简单的图像分类/识别、整体内容理解/描述等场景，理解更快速。
`auto`：默认模式，不同模型选择的模式略有不同，具体请参见[理解图像的深度控制](https://www.volcengine.com/docs/82379/1362931#bf4d9224)。
messages.content.**type **`string``必选`
图像消息类型，此次应为 `image_url`。
用户发送的消息，包含提示或附加上下文信息。
发送消息的角色，此处应为`user`。
用户信息内容。
messages.**name**`string`
发送此消息的角色的姓名。用于区别同一个角色但是不同主体发送的消息。
固定为 `chat.completion`。
choices.**message **`object`
模型输出的内容。
choices.message.**role **`string`
choices.message.**content **`string`
choices.message.**reasoning_content **`string / null`
choices.message.**tool_calls **`object[] / null`
模型生成的工具调用。
choices.message.tool_calls.**i****d **`string`
choices.message.tool_calls.**type **`string`
choices.message.tool_calls.**function **`object`
choices.message.tool_calls.function.**name **`string`
choices.message.tool_calls.function.**arguments **`string`
choices.**moderation_hit_type **`string / null`
bot_usage.model_usage**.name**`string`
bot_usage.model_usage**.prompt_tokens**`integer`
bot_usage.model_usage**.completion_tokens**`integer`
bot_usage.model_usage**.total_tokens**`integer`
**action_usage**`object[]`
action_usage.**action_name**`string`
action_usage.**count**`integer`
**action_details**`object[]`
action_details.**name**`string`
action_details.**count**`integer`
action_details.**tool_details**`object[]`
action_details.tool_details.**name**`string`
action_details.tool_details.**input**`object`
action_details.tool_details.**output**`object`
action_details.tool_details.**created_at**`integer`
action_details.tool_details.**completed_at**`integer`
属性
thinking.**type **`string``必选`
取值范围：`enabled`， `disabled`，`auto`。
`enabled`：开启思考模式，模型一定先思考后回答。
`disabled`：关闭思考模式，模型直接回答问题，不会进行思考。
`auto`：自动思考模式，模型根据问题自主判断是否需要思考，简单题目直接回答。
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
模型响应用户消息而回复的消息。
说明
messages.**content**与 messages.**tool_calls**字段二者至少填写其一。
发送消息的角色，此处应为`assistant`。
messages.**content**`string / array`
模型回复的消息。
messages.**tool_calls**`object[]`
模型回复的工具调用信息。
messages.tool_calls**.function **`object``必选`
模型调用工具对应的函数信息。
messages.tool_calls**.**function.**name **`string``必选`
模型需要调用的函数名称。
messages.tool_calls**.**function.**arguments **`string``必选`
说明
messages.tool_calls**.id **`string``必选`
messages.tool_calls**.type **`string``必选`
`string`
纯文本的消息内容。
messages.content.image_url.**detail **`string``默认值 auto`
[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/application)[模型列表](https://www.volcengine.com/docs/82379/1330310#%E6%96%87%E6%9C%AC%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1099320#%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[获取Bot ID](https://www.volcengine.com/docs/82379/1267885)[接口文档](https://www.volcengine.com/docs/82379/1526787)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
大语言模型支持此类型。
