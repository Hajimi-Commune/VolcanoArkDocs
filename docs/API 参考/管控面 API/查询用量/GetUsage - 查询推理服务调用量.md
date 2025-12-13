# GetUsage - 查询推理服务调用量
beta测试中，查询模型推理服务调用量

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetUsage&groupName=%E6%9F%A5%E8%AF%A2%E7%94%A8%E9%87%8F&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetUsage&Version=2024-01-01 HTTP/1.1
Host: open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241119T104537Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241119/cn-beijing/ark_stg/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "Interval": 86400,
    "StartTime": 1731945600,
    "EndTime": 1731945600,
    "ProjectName": "default"
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "20241119190556E244C5F755CB355E6405",
        "Action": "GetUsage",
        "Version": "2024-01-01",
        "Service": "ark_stg",
        "Region": "cn-beijing"
    },
    "Result": {
        "UsageResults": [
            {
                "Name": "PromptTokens",
                "MetricItems": [
                    {
                        "Values": [
                            {
                                "Timestamp": 1731945600,
                                "Value": 1529525715
                            }
                        ]
                    }
                ]
            },
            {
                "Name": "CompletionTokens",
                "MetricItems": [
                    {
                        "Values": [
                            {
                                "Timestamp": 1731945600,
                                "Value": 40207324
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。