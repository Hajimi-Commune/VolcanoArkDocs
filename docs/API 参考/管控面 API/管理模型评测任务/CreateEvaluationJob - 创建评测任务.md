# CreateEvaluationJob - 创建评测任务
创建评测任务

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=CreateEvaluationJob&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%AF%84%E6%B5%8B%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
  "Name": "测试任务",
  "EndpointId": "ep-***-**",
  "EvaluationDatasets": [
    {
      "Method": "pA",
      "DatasetType": "自动评测"
    }
  ]
}
```
## 返回示例 
```json
{
    "ResponseMetadata": {
        "RequestId": "202404111449574CC61570F45B511EF0FC",
        "Action": "CreateEvaluationJob",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "Id": "ej-20240411064957-hxj6z"
    }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。