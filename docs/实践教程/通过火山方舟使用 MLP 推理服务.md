# 通过火山方舟使用 MLP 推理服务
火山方舟提供丰富的大模型调用服务，如 Doubao 系列大语言模型、深度思考模型、视觉理解模型，DeepSeek 系列开源模型等。如果您希望使用自定义模型或其他三方开源模型，可以使用 [机器学习平台 MLP](https://console.volcengine.com/ml-platform/region:ml-platform+cn-beijing/dashboard?guideTab=mlDevelopment) 部署您的模型，然后注册至方舟平台进行调用。本文介绍为您如何通过火山方舟使用 MLP 推理服务。
## 使用场景
#### **对于 MLP 用户，使用火山方舟可增强已有 MLP 模型服务能力，保障企业级AI应用落地**。

* **多接入点分流**：一个 MLP 在线服务，支持在方舟侧创建多个推理接入点 Endpoint ，方便业务方按照 Endpoint 限流，实现业务分流（如大促流量调度、服务等级区分等）。
* **评测与迭代**：接入点一键评测，对比性能（响应速度、准确率），辅助模型优化。
* **权限管控**：支持 IAM、API key、项目、标签维度权限管理，细粒度控制模型访问，保障安全合规。

#### 对于方舟侧用户，使用 MLP 可扩展模型种类，并获得方舟的统一集成体验。

* **开放部署**：支持调用更多的三方开源 / 自定义模型，火山方舟本身不会产生额外费用。
* **统一集成**：通过方舟统一域名与 API 调用，前端无需适配多模型，简化开发（如 AIGC 应用切换模型仅需修改 Endpoint ID）。

## 支持能力
MLP 推理服务可使用的火山方舟功能如下：

| | | | \
|类别 |功能/特性 |说明 |
|---|---|---|
| | | | \
|在线推理 |API 调用 |通过 Endpoint ID 调用您的 MLP 推理服务， Endpoint ID 格式为`ep-s-xxx`。 |
|^^| | | \
| |支持模型 |MLP 推理服务（当前仅支持文本生成和深度思考大语言模型） |
|^^| | | \
| |模型版本平滑切换 |× |
|^^| | | \
| |配置接入点限流 |设置单接入点访问频率限制 |
|^^| | | \
| |开启/停用接入点 |控制是否可通过该接入点调用您的服务 |
|^^| | | \
| |查看监控 |查看 MLP 推理服务的监控指标 |
|^^| | | \
| |安全审计（会话、传输加密） |× |
|^^| | | \
| |细粒度权限管理 |支持 IAM、API key、项目、标签维度权限管理 |
|^^| | | \
| |算力保障（TPM 保障包、模型单元） |× |
| | | | \
|应用实验室 |零代码应用 |× |
|^^| | | \
| |高代码应用 |× |
| | | | \
|模型评测 |模型评测 |对您的MLP 推理服务进行评测 |
| | | | \
|数据投递 |数据投递 |× |

## 支持模型

* 当前仅支持将 MLP 侧的**文本生成模型**和**深度思考模型**注册至方舟。其他领域模型（视觉理解模型、图片生成模型、视频生成模型等）部署的在线服务注册后，API 调用时会报错 500。

## 使用说明

* API 调用：
   * 对于注册至方舟的 MLP 在线服务，仅支持**通过 MLP Endpoint ID 进行 API 调用**，不支持使用模型名称或 Model ID 进行调用。
   * 调用 API 时，火山方舟不会对参数进行校验和拦截。为了保证您的模型可以正常调用，请提前确认模型支持的参数范围。
   * 使用 MLP Endpoint  创建应用，具体是否支持网页解析等插件能力，由您的模型能力决定（例如不支持 function calling 的模型无法支持网页解析插件）。
* 计费说明：
   从 MLP 注册的推理接入点，火山方舟不会收取推理产生的 token 费用。如果使用了火山方舟插件/知识库，则会产生相关费用。具体请参见 [联网内容插件产品计费](https://www.volcengine.com/docs/82379/1338550) 和 [知识库计费](https://www.volcengine.com/docs/82379/1263336)。
* 安全能力：
   * 内容安全：模型的内容安全需用户自身负责，火山方舟不承担审核责任。
   * 产品安全：由 MLP 保障，不支持使用方舟安全沙箱相关能力。
   * 数据安全：火山方舟不会使用您的推理数据进行模型训练，所有数据仅用于您的推理服务，不会用于其他任何用途。
* 限流与SLA：
   * 方舟侧支持设置 MLP Endpoint 限流，整体服务的实际限流将由MLP的对应服务资源总量决定。
   * **对于注册至方舟的 MLP 在线服务，实际推理服务可用性由 MLP 决定，火山方舟侧不对 SLA 进行保证和承诺。**

## 前提条件

* MLP 侧准备工作如下：
   1. 子用户已获取队列的使用权限，详情可参见 [MLP 权限管理](https://www.volcengine.com/docs/6459/1535142)。
   2. [已开通机器学习平台](https://console.volcengine.com/ml-platform)。
   3. 已将您训练好的 LLM 模型或三方模型部署为在线服务，详细步骤请参见 [将模型部署成服务](https://www.volcengine.com/docs/6459/1398947)。
   4. 已在 [MLP 全局配置](https://console.volcengine.com/ml-platform/region:ml-platform+cn-datong/globalConfig) 对火山方舟完成跨服务授权，并添加对应服务。仅管理员账号支持该操作。
* 方舟侧准备工作如下：
   1. 子用户已获取 [ArkStandardGlobalAccess](https://console.volcengine.com/iam/policymanage/System/ArkStandardGlobalAccess) 或 [ArkFullAccess](https://console.volcengine.com/iam/policymanage/System/ArkFullAccess) 权限，详情可参见 [使用IAM进行访问控制教程](https://www.volcengine.com/docs/82379/1263493)。
   2. [已开通火山方舟](https://console.volcengine.com/ark/region:ark+cn-beijing/overview?briefPage=0&briefType=introduce&projectName=default&type=advance)。

## 步骤一：创建 MLP Endpoint

1. 访问[方舟控制台-在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)。
2. 在控制台顶部切换需要创建推理接入点的项目空间。
3. 在 **自定义推理接入点** 页签，单击 **创建推理接入点。**
4. 在创建接入点页面，按要求填写 **基本信息**。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e34f1209baba4f5a89f6af96f3984152~tplv-goo7wpa0wc-image.image =1762x)

5. 接入来源选择**机器学习平台**，并选择待接入的 MLP 推理服务。

:::warning
* 当前仅支持将 MLP 侧的 LLM 模型推理服务注册至方舟。
* MLP 侧准备工作如下：
   * 已将您训练好的 LLM 模型或三方模型部署为在线服务，详细步骤请参见 [将模型部署成服务](https://www.volcengine.com/docs/6459/1398947)。
   * 已在 [MLP 全局配置](https://console.volcengine.com/ml-platform/region:ml-platform+cn-datong/globalConfig) 对火山方舟完成跨服务授权，并添加对应服务。仅管理员账号支持该操作。
:::

6. 配置接入点限流信息。请注意，实际的推理服务访问能力将由MLP的对应服务资源总量决定。
7. 阅读并勾选相关协议，单击 **确认接入**，完成 MLP 推理接入点创建。
    创建成功后，复制 Endpoint ID 备用。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/939375c8e0964caa8b8575dddc5b124c~tplv-goo7wpa0wc-image.image =2454x)

## 步骤二：通过方舟 API/SDK 调用MLP推理服务

1. 获取获取[火山方舟 API key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)。
2. 配置 API key 到环境变量。

```Plain Text
//其中"YOUR_API_KEY"需要替换为您在平台创建的 API Key
export ARK_API_KEY="YOUR_API_KEY"
```

3. 通过 API 或 SDK 调用 MLP 推理服务。下面为一个最简单的 LLM 大模型调用示例代码。
   * 更多语言的代码示例请参见 [模型调用教程](https://www.volcengine.com/docs/82379/1362932)。
   * 方舟 API 文档请参见 [API 参考](https://www.volcengine.com/docs/82379/1289651)。

**REST API 调用示例**

注意 \`model \` 需替换为步骤一获取的 Endpoint ID。
```Plain Text
curl https://ark.cn-beijing.volces.com/api/v3/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "ep-s-xxx",
    "messages": [
      {"role": "system","content": "你是人工智能助手."},
      {"role": "user","content": "你好"}
    ]
  }'
```

**OpenAI SDK**

1.  请按如下命令安装环境 

```Plain Text
pip install --upgrade "openai>=1.0"
```

2. 请参考如下示例代码进行调用

```Python
import os
from openai import OpenAI

# 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
# 初始化Openai客户端，从环境变量中读取您的API Key
client = OpenAI(
    # 此为默认路径，您可根据业务所在地域进行配置
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    # 从环境变量中获取您的 API Key
    api_key=os.environ.get("ARK_API_KEY"),
)

# Non-streaming:
print("----- standard request -----")
completion = client.chat.completions.create(
    # 替换为您步骤一获取的 MLP Endpoint ID
    model="ep-s-xxx",
    messages=[
        {"role": "system", "content": "你是人工智能助手"},
        {"role": "user", "content": "你好"},
    ],
)
print(completion.choices[0].message.content)

# Streaming:
print("----- streaming request -----")
stream = client.chat.completions.create(
    # 替换为您步骤一获取的 MLP Endpoint ID
    model="ep-s-xxx",
    messages=[
        {"role": "system", "content": "你是人工智能助手"},
        {"role": "user", "content": "你好"},
    ],
    # 响应内容是否流式返回
    stream=True,
)
for chunk in stream:
    if not chunk.choices:
        continue
    print(chunk.choices[0].delta.content, end="")
print()
```

**火山引擎 SDK**

1.  请按如下命令安装环境
   ```Plain Text
   pip install --upgrade "volcengine-python-sdk[ark]"
   ```

2. 请参考如下示例代码进行调用
   ```Python
   import os
   from volcenginesdkarkruntime import Ark
   
   # 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
   # 初始化Ark客户端，从环境变量中读取您的API Key
   client = Ark(
       # 此为默认路径，您可根据业务所在地域进行配置
       base_url="https://ark.cn-beijing.volces.com/api/v3",
       # 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
       api_key=os.environ.get("ARK_API_KEY"),
   )
   
   # Non-streaming:
   print("----- standard request -----")
   completion = client.chat.completions.create(
       # 替换为您步骤一获取的 MLP Endpoint ID
       model="ep-s-xxx",
       messages=[
           {"role": "system", "content": "你是人工智能助手."},
           {"role": "user", "content": "你好"},
       ],
   )
   print(completion.choices[0].message.content)
   
   # Streaming:
   print("----- streaming request -----")
   stream = client.chat.completions.create(
       # 替换为您步骤一获取的 MLP Endpoint ID
       model="ep-s-xxx",
       messages=[
           {"role": "system", "content": "你是人工智能助手."},
           {"role": "user", "content": "你好"},
       ],
       # 响应内容是否流式返回
       stream=True,
   )
   for chunk in stream:
       if not chunk.choices:
           continue
       print(chunk.choices[0].delta.content, end="")
   print()
   ```

## 可选：查看监控告警
支持对 MLP Endpoint ID 维度进行指标监控告警，了解模型服务的运行情况。详情可参见 [监控推理接入点](https://www.volcengine.com/docs/82379/1182403#%E7%9B%91%E6%8E%A7)。
## 可选：对 MLP推理服务进行模型评测
支持使用 MLP 推理接入点创建模型评测任务，对您的模型效果进行评测。详情可参见 [创建模型评测任务](https://www.volcengine.com/docs/82379/1150782)。
:::warning
暂不支持自定义推理参数。
:::
##