# GetEndpoint - 获取推理接入点
获取推理接入点

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetEndpoint&groupName=%E7%AE%A1%E7%90%86%E6%8E%A8%E7%90%86%E6%8E%A5%E5%85%A5%E7%82%B9%EF%BC%88Endpoint%EF%BC%89&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetEndpoint&Version=2024-01-01 HTTP/1.1
Host: open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241231T111513Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241231/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "test-ep-id"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20241231191522021037040243127434",
    "Action": "GetEndpoint",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "SupportRolling": false,
    "CreateTime": "ZFC",
    "Description": "4i2e8gPBufo",
    "EndpointModelType": "R3",
    "Id": "n9It",
    "ModelReference": {
      "CustomModelId": "ok",
      "FoundationModel": {
        "Name": "JarKT21",
        "ModelVersion": "EQS"
      }
    },
    "ModelUnitId": "E",
    "Name": "AGEWR",
    "ProjectName": "b0",
    "RateLimit": {
      "Rpm": 118,
      "Tpm": 251
    },
    "Status": "b22",
    "StatusReason": "IQ",
    "SupportScaleTier": false,
    "Tags": [
      {
        "Key": "oT0JUKb9G",
        "Value": "m9PYoDk7"
      }
    ],
    "UpdateTime": "FPPT",
    "RollingId": "gKU9eyRE",
    "ScaleTierId": "kJQWZk2DqL"
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。