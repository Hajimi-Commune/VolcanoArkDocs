# GetApiKey - 获取临时API Key
获取临时API Key，用于在有效期内指定推理接入点、智能体等资源的调用的鉴权

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetApiKey&groupName=%E7%AE%A1%E7%90%86API%20Key&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetApiKey&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250108T072336Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250108/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "DurationSeconds": 86400,
  "ResourceType": "endpoint",
  "ResourceIds": [
    "[\"ep-***\", \"ep-***\"]"
  ]
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20250108152350029189016106C67393",
    "Action": "GetApiKey",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "ExpiredTime": 0,
    "ApiKey": "***-***-***"
  }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。
