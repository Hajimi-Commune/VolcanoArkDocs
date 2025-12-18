# ListEvaluationResults - 获取评测任务结果列表
获取评测任务结果列表 

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
    "EvaluationJobId": "ej-20240326054800-gl9rp",
    "PageNumber": 1,
    "PageSize": 1,
    "SortOrder": "Desc",
    "SortBy": "CreateTime"
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "202404111456299465E1DB51F5B325605A",
        "Action": "ListEvaluationResults",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "TotalCount": 1,
        "PageNumber": 1,
        "PageSize": 1,
        "Items": [
            {
                "Id": "er-20240326054800-shhvj",
                "EvaluationJobId": "ej-20240326054800-gl9rp",
                "DatasetType": "AdminDataset",
                "AdminEvaluationDatasetId": "aed-20240227112053-bt425",
                "DatasetName": "ark-ff-mmlu-other-5shots",
                "DatasetDisplayName": "MMLU 其他学科",
                "EvaluationAbility": "MMLU",
                "ScoringWeight": 3107,
                "Method": "BuiltIn",
                "Metrics": null,
                "TokenUsage": {
                    "TotalTokenCount": 0
                },
                "SamplesTosLocation": {
                    "BucketName": "",
                    "ObjectKey": ""
                },
                "CreateTime": "2024-03-26T05:48:00Z",
                "UpdateTime": "2024-03-26T05:48:00Z"
            }
        ]
    }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
