# GetBatchInferenceJob - 获取批量推理任务
获取批量推理任务 

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetBatchInferenceJob&groupName=%E6%89%B9%E9%87%8F%E6%8E%A8%E7%90%86%E4%BB%BB%E5%8A%A1%20API&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
    "Id": "7WtYtkPG9x"
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "2024091910512025A35A851624E4541879",
        "Action": "GetBatchInferenceJob",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "Id": "bi-20240918194641-r2nwq",
        "Name": "批量推理任务",
        "Description": "这是一个批量推理任务",
        "InputFileTosLocation": {
            "BucketName": "my-bucket-name",
            "ObjectKey": "batch-inference-job/dataset/my-job.jsonl"
        },
        "OutputDirTosLocation": {
            "BucketName": "my-bucket-name",
            "ObjectKey": "batch-inference-job/output/"
        },
        "CompletionWindow": "1d",
        "Status": {
            "Phase": "Completed",
            "PhaseTime": "2024-09-18T11:50:00Z",
            "Message": "批量推理任务已完成"
        },
        "ModelReference": {
            "FoundationModel": {
                "Name": "doubao-pro-32k",
                "ModelVersion": "240615"
            }
        },
        "RequestCounts": {
            "Total": 2,
            "Completed": 2,
            "Failed": 0
        },
        "Tags": [
            {
                "Key": "sys:ark:createdBy",
                "Value": "IAMUser/30805480/test@bytedance.com"
            },
            {
                "Key": "test_key",
                "Value": "test_value"
            }
        ],
        "ProjectName": "default",
        "CreateTime": "2024-09-18T11:46:41Z",
        "UpdateTime": "2024-09-18T11:50:00Z",
        "ExpireTime": "2024-09-19T11:46:41Z"
    }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
