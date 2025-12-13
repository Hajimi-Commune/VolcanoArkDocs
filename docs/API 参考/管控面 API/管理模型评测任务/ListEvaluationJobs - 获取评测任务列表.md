# ListEvaluationJobs - 获取评测任务列表
获取评测任务列表1

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=ListEvaluationJobs&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%AF%84%E6%B5%8B%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
    "PageNumber": 1,
    "PageSize": 1,
    "SortOrder": "Asc",
    "SortBy": "CreateTime",
    "TagFilters": [],
    "ProjectName": "default",
    "Filter": {
        "Phases": [
            "Finished"
        ]
    }
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "20240411145356A067F3B9F09679456C28",
        "Action": "ListEvaluationJobs",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "TotalCount": 10,
        "PageNumber": 1,
        "PageSize": 10,
        "Items": [
            {
                "Id": "ej-20240401114848-w5b8f",
                "Name": "eval-job-20240401194800-0",
                "Description": "",
                "ModelReference": {
                    "CustomModelId": "cm-20240401114541-5gzd8"
                },
                "EndpointId": "ep-20240401114848-hzfcz",
                "ResultTosLocation": {
                    "BucketName": "ark-auto-created-required-0123456789-cn-beijing",
                    "ObjectKey": "ark/ej-20240401114848-w5b8f/inference_result/results/"
                },
                "Status": {
                    "Phase": "Finished",
                    "PhaseTime": "2024-04-01T11:54:41Z",
                    "RetryCount": 0,
                    "Message": "评测任务已完成"
                },
                "Tags": [
                    {
                        "Key": "sys:ark:createdBy",
                        "Value": "IAMUser/1234567/fabianlimxuhuai@acme.com"
                    }
                ],
                "ProjectName": "default",
                "CreateTime": "2024-04-01T11:48:48Z",
                "UpdateTime": "2024-04-01T11:54:41Z"
            }
        ]
    }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。