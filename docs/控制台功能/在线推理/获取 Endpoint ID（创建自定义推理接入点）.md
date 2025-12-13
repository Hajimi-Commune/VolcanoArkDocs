# 获取 Endpoint ID（创建自定义推理接入点）
自定义推理接入点（Endpoint）是用户自主创建的模型调用入口，支持精调模型接入、权限控制、算力保障等高级功能。本文介绍如何创建自定义推理接入点并获取 Endpoint ID。
## 1.创建 Endpoint

1. 访问 [在线推理页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint) 。
2. (可选)在控制台顶部切换需要创建推理接入点的项目空间。
3. 在 **自定义推理接入点** 页签，单击 **创建推理接入点。**
4. 在创建接入点页面，按要求填写 **基本信息**。
   * 接入来源为火山方舟：可接入模型广场的基础模型或模型仓库的精调模型。
      :::tip
      建议您打开 **创建说明** 开关，帮助您了解每个配置项的使用场景和含义，轻松完成接入点的创建配置。
      :::
   * 接入来源为机器学习平台：可接入 MLP 平台部署的自定义模型或其他三方的开源模型。详情请参见[通过火山方舟使用 MLP 推理服务](/docs/82379/1584258)。

## 2.获取 Endpoint ID
创建成功后，可在 [在线推理页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint) 查看并复制推理接入点 Endpoint ID。
## 3.通过Endpoint ID 调用模型
您可在代码中通过配置 **Endpoint ID** （推理接入点ID）的方式来发起调用。
```Python
import os
from volcenginesdkarkruntime import Ark
client = Ark(
    api_key=os.environ.get("ARK_API_KEY"),
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    )
completion = client.chat.completions.create(
    # Replace with your endpoint ID
    model="<ENDPOINT_ID>", 
    messages=[
        {"role": "user", "content": "Hello"}
    ]
)
print(completion.choices[0].message)
```