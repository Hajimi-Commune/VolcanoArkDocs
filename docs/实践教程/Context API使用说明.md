# Context API使用说明
:::warning
当前内容仅限内部和指定客户测试使用。
:::
## 使用前提

* 访问方舟控制台[在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)页面，获取到`endpoint_id`。如无，请参考[创建推理接入点](https://www.volcengine.com/docs/82379/1099522)创建。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e8abeaf080a44eb4a41af3c0e3cf9e94~tplv-goo7wpa0wc-image.image =328x)

* 访问方舟控制台[API Key管理](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)页面，获取到`API Key`。如无，请参考[获取API Key](https://www.volcengine.com/docs/82379/1298459#%E8%8E%B7%E5%8F%96-api-key)创建。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/214401afd3284e50afe71c4aed5759bd~tplv-goo7wpa0wc-image.image =884x)
## 调用示例
支持2种调用方式，接下来讲分别给出调用方式示例。

* curl方式（推荐）
* Python SDK（仅测试用）

### curl方式（推荐）

1. 创建context。

```Shell
curl --location 'https://ark.cn-beijing.volces.com/api/v3/context/create' \
--header 'Authorization: Bearer ${YOUR_API_KEY}' \
--header 'Content-Type: application/json' \
--data '{
    "model":"${YOUR_ENDPOINT_ID}",
    "messages":[
        {"role":"system","content":"你是小爱，你只会说“我是小爱”"}
    ],
    "ttl":3600,
    "truncation_strategy":{
        "type":"last_history_tokens",
        "last_history_tokens": 4096
    }
}'
```

2. 基于创建的 context 返回 id 开始对话。

```Shell
curl --location 'https://ark.cn-beijing.volces.com/api/v3/context/chat/completions' \
--header 'Authorization: Bearer ${YOUR_API_KEY}' \
--header 'Content-Type: application/json' \
--data '{
    "context_id": "${YOUR_CONTEXT_ID}",
    "model": "${YOUR_ENDPOINT_ID}",
    "messages":[
        {
            "role":"user",
            "content": "给我讲个故事"
        }
    ]
}'
```

### Python SDK（仅供测试）

1. 下载并安装临时版本Python SDK。
   1. 方式1：下载临时版本SDK到本地安装。
      <Attachment link="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/419494ceaf7c44fa976b5c172d757847~tplv-goo7wpa0wc-image.image" name="volcengine_python_sdk-1.0.101-py3-none-any.whl" ></Attachment>
   2. 安装本地whl包。

```Shell
pip install ./volcengine_python_sdk-1.0.101-py3-none-any.whl
```

   * 方式2：联网安装SDK。

```Shell
pip install --force-reinstall https://ark-context-sdk.tos-cn-beijing.volces.com/volcengine_python_sdk-1.0.101-py3-none-any.whl
```

2. 调用模型（创建context / 调用context chat）

```Python
import datetime
from volcenginesdkarkruntime import Ark

client = Ark(api_key="${YOUR_API_KEY}")

if __name__ == "__main__":
    # Create context:
    response = client.context.create(
        model="${YOUR_ENDPOINT_ID}",
        messages=[
            {"role": "system", "content": "你是豆包，是由字节跳动开发的 AI 人工智能助手"},
            ],
        ttl=datetime.timedelta(minutes=30),
    )
    print(response)

    # Streaming:
    print("----- streaming request -----")
    stream = client.context.completions.create(
        context_id=response.id,
        model="${YOUR_ENDPOINT_ID}",
        messages=[
            {"role": "user", "content": "你是谁？"},
        ],
        stream=True
    )
    for chunk in stream:
        if not chunk.choices:
            continue
        print(chunk.choices[0].delta.content, end="")
```