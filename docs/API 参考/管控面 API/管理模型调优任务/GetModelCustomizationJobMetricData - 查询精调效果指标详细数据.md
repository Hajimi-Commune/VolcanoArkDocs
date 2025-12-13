# GetModelCustomizationJobMetricData - 查询精调效果指标详细数据
查询精调效果指标详细数据接口

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetModelCustomizationJobMetricData&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%B0%83%E4%BC%98%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```json
{
    "ModelCustomizationJobId": "mcj-20241128104838-g9vqg",
    "Metrics": [
        "train/total_tokens",
        "eval/loss",
        "train/loss",
        "eval/total_tokens"
    ]
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "20241212201041E7A67D3F69E447553B7F",
        "Action": "GetModelCustomizationJobMetricData",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "Items": [
            {
                "Metric": "train/total_tokens",
                "Samples": [
                    {
                        "Step": 1,
                        "Value": 2
                    },
                    {
                        "Step": 2,
                        "Value": 3
                    }
                ]
            },
            {
                "Metric": "eval/loss",
                "Samples": [
                    {
                        "Step": 1,
                        "Value": 0.0001
                    },
                    {
                        "Step": 2,
                        "Value": 0.0001
                    }
                ]
            },
            {
                "Metric": "train/loss",
                "Samples": [
                    {
                        "Step": 1,
                        "Value": 0.0625
                    },
                    {
                        "Step": 2,
                        "Value": 0.0525
                    }
                ]
            }
        ]
    }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。