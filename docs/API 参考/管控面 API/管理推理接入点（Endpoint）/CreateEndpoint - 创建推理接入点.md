# CreateEndpoint - 创建推理接入点
创建推理接入点

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=CreateEndpoint&groupName=%E7%AE%A1%E7%90%86%E6%8E%A8%E7%90%86%E6%8E%A5%E5%85%A5%E7%82%B9%EF%BC%88Endpoint%EF%BC%89&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
  "Name": "testname",
  "Description": "TestDescription",
  "ModelReference": {
    "CustomModelId": "SxU71Q1t",
    "FoundationModel": {
      "Name": "yzcHNZMJ736",
      "ModelVersion": "4Rh90omlqN"
    }
  },
  "ModelUnitId": "\\-",
  "Tags": [
    {
      "Key": "B",
      "Value": "3ScqC"
    }
  ],
  "ProjectName": "default",
  "DryRun": true,
  "RateLimit": {
    "Rpm": 651,
    "Tpm": 798
  }
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514212750152186163065FE5A2A",
    "Action": "CreateEndpoint",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "Id": "ep-test-id"
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
