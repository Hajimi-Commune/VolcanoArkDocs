# GetModelCustomizationJobMetrics - 查询精调效果指标
查询精调效果指标接口

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetModelCustomizationJobMetrics&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%B0%83%E4%BC%98%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```json
{
    "ModelCustomizationJobId": "mcj-20241128104838-g9vqg"
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "2024121216215991B48FBC86E8E61A44B1",
        "Action": "GetModelCustomizationJobMetrics",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": ""
    },
    "Result": {
        "Metrics": [
            "eval/loss",
            "eval/total_tokens",
            "train/loss",
            "train/total_tokens"
        ]
    }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。