# 智能体插件 API
:::warning
以下功能是Beta版本, 如遇到问题, 请联系 PDSA, 谢谢合作;
:::

# 联网插件 API
> 鉴权方式：[SDK鉴权](https://www.volcengine.com/docs/82379/1298459)

## Search Intention

> POST /api/v2/action/SearchIntention
> 执行新鲜度检查及检索意图检查, 返回 是否需要 进入联网搜索阶段

Request ( 入参类型为: MaasChatRequest, 关键字段如下 )

| 字段路径 | 类型 | 是否必选 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| tools | List\[Object\] | 是 | / | Action 参数 |
| tools\[\*\].type | String | 是 | SearchIntention | 类型 |
| tools\[\*\].options | List\[Object\] | 是 | / | Action 配置 |
| tools\[\*\].options\[\*\].keywords | List\[String\] | 否 | / | 关键词 |
| tools\[\*\].options\[\*\].result\_mapping | Dict\[String, Bool\] | 是 | / | 新鲜度及意图识别结果映射关系定义 (参考下面例子理解) |\
|||
| tools\[\*\].message | Object | 是 | / | 用户对话 |
| tools\[\*\].message.role | String | 是 | / | 用户对话角色, 固定为 user |
| tools\[\*\].message.content | String | 是 | / | 用户对话内容 |

Response ( 出参类型为: MaasChatResponse, 关键字段如下 )

| 字段路径 | 类型 | 是否必选 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| choices | List\[Object\] | 是 | / | 意图识别判别结果 |
| choices\[\*\].message | Object | 是 | / |  |
| choices\[\*\].message.role | String | 是 | / | 用户对话角色, 固定为 assistant |
| choices\[\*\].message.content | String | 是 | / | 用户对话返回内容, 一般为 "需要" or "不需要" 等 |
| choices\[\*\].finish\_reason | String | 是 | / | 对话完成原因 |
| usage | Object | 是 | / | token 消耗统计 |\
||||||
| usage.prompt\_tokens | Number | 是 | / | 提示的 prompt |
| usage.completion\_tokens | Number | 是 | / | 生成的 token 数量 |
| usage.total\_tokens | Number | 是 | / | 总的 token 数量 |

示例

- request: MaasChatRequest
	

```python
{
    "tools": [
        {
            "type": "SearchIntention",
            "options": {
                "keywords": ["今天天气怎么样"],
                "result_mapping": {
                    "需要": True，
                    "不需要": False,
                    "可以": True,
                    "不可以": False,
                }
            }
        }
    ],
    "messages": [
            {
                "role": "user",
                "content": "\n".join(
                    [
                        "请判断你是否需要借助搜索引擎来回答下面的问题，请回答需要或者不需要。", # prompt , 引导模型回答 ( 以便接口配合 result mapping 做意图判断 ) 
                        "问题：今天天气怎么样?"
                    ]
                )
            },
        ],
}
```

- response: MaasChatResponse
	

```python
{
    "choices": [
        {
            "message": {
                "role": "assitant",
                "content": "需要" # 依据用户 request message中的 prompt 给出
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 0,
        "completion_tokens": 0,
        "total_tokens": 0
    }
}
```

<br>

## Search Summary

> POST /api/v2/action/SearchSummary
> 执行检索并基于结果生成摘要

Request ( 入参类型为: MaasChatRequest, 关键字段如下 )

| 字段路径 | 类型 | 是否必选 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| tools | List\[Object\] | 是 | / | Action 参数 |
| tools\[\*\].type | String | 是 | SearchSummary | Action 类型 |
| tools\[\*\].options | List\[Object\] | 是 | / | Action 配置 |
| tools\[\*\].options\[\*\].keywords | List\[String\] | 否 | / | 关键词 |
| tools\[\*\].options\[\*\].action\_name | String | 否 | "WebBrowsing" | Action 名称; 目前只支持 WebBrowsing |
| tools\[\*\].options\[\*\].summary\_top\_k | Number | 否 | 5 | 联网普通版, 返回的 search 结果的最大数量 |
| tools\[\*\].user\_info | Object | 是 | 否 | 用户信息 |
| tools\[\*\].user\_info.city | String | 是 | / | 用户所在城市 |
| tools\[\*\].user\_info.district | String | 是 | / | 用户所在城市的区 |

Response ( 出参类型为: MaasChatResponse, 关键字段如下 )

| 字段路径 | 类型 | 是否必选 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| choices | List\[Object\] | 是 | / | Summary |
| choices\[\*\].message | Object | 是 | / |  |
| choices\[\*\].message.role | String | 是 | / | 用户对话角色, 固定为 assistant |
| choices\[\*\].message.content | String | 是 | / | 用户对话返回内容 |
| choices\[\*\].finish\_reason | String | 是 | / | 对话完成原因 |
| usage | Object | 是 | / | token 消耗统计 |\
||||||
| usage.prompt\_tokens | Number | 是 | / | 提示的 prompt |
| usage.completion\_tokens | Number | 是 | / | 生成的 token 数量 |
| usage.total\_tokens | Number | 是 | / | 总的 token 数量 |

示例

- request:
	

```python
{
    "tools": [
        {
            "type": "SearchSummary",
            "options": {
                "summary_topk": 5,
                "keywords": [""],
            }
        }
    ],
    "message": [
            {
                "role": "user",
                "content": "\n".join(
                    [
                        "问题：今天天气怎么样?"
                    ]
                )
            },
        ],
}
```

- reponse:
	

```python
{
    "choices": [
        {
            "message": {
                "role": "assitant",
                "content": "今天天气晴, 气温xxx " # summary 结果
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 0,
        "completion_tokens": 0,
        "total_tokens": 0
    }
}
```

<br>