# GetEvaluationJob - 获取评测任务
获取评测任务

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetEvaluationJob&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%AF%84%E6%B5%8B%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetEvaluationJob&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250115T084133Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250115/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "ej-202403***-***"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "202501151641402430430002397DF4EA",
    "Action": "GetEvaluationJob",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "Id": "\\-",
    "Name": "\\-",
    "Description": "\\-",
    "ModelReference": {
      "FoundationModel": {
        "Name": "n",
        "ModelVersion": "29X"
      },
      "CustomModelId": "ftsdB6bWIwP"
    },
    "EndpointId": "\\-",
    "ResultTosLocation": {
      "ObjectKey": "u",
      "BucketName": "b0E4M6"
    },
    "Status": {
      "Phase": "tpAZR4zz",
      "PhaseTime": "WDhjJFhv",
      "Message": "ntacqOQV3aw"
    },
    "Tags": [
      {
        "Key": "VERVg5xFVF",
        "Value": "dxhO22I"
      }
    ],
    "ProjectName": "\\-",
    "CreateTime": "\\-",
    "UpdateTime": "\\-"
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。