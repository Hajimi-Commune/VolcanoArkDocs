# CreateBatchInferenceJob - 创建批量推理任务
创建批量推理任务。
> 为了避免异常情况下重复提交任务带来额外推理和计费，平台在创建批量推理任务时提供幂等控制：当用户、项目名（Project Name）、存储桶（bucket）和输入文件名（object-key）完全相同时，平台仅保留一个任务处于活跃状态（运行或排队），其他重复提交将报错，并在错误信息中包含在运行或排队中任务的ID，供用户查询。

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=CreateBatchInferenceJob&groupName=%E6%89%B9%E9%87%8F%E6%8E%A8%E7%90%86%E4%BB%BB%E5%8A%A1%20API&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=CreateBatchInferenceJob&Version=2024-01-01 HTTP/1.1
Host: open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T132743Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "Name": "批量推理任务",
    "Description": "这是一个批量推理任务",
    "ModelReference": {
      "FoundationModel": {
        "Name": "doubao-pro-32k",
        "ModelVersion": "240615"
      }
    },
    "InputFileTosLocation": {
      "BucketName": "my-bucket-name",
      "ObjectKey": "batch-inference-job/dataset/my-job.jsonl"
    },
    "OutputDirTosLocation": {
      "ObjectKey": "batch-inference-job/output/",
      "BucketName": "my-bucket-name"
    },
    "ProjectName":"default",
    "CompletionWindow": "1d",
    "Tags": [
      {
        "Key": "test_key",
        "Value": "test_value"
      }
    ]
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240918194640ECB302768CB287CEDAC6",
    "Action": "CreateBatchInferenceJob",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "Id": "bi-2024091****-****"
  }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。
