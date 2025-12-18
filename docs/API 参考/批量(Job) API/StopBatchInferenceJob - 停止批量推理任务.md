# StopBatchInferenceJob - 停止批量推理任务
停止批量推理任务1

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=StopBatchInferenceJob&groupName=%E6%89%B9%E9%87%8F%E6%8E%A8%E7%90%86%E4%BB%BB%E5%8A%A1%20API&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

本接口无特有的返回参数。更多信息请见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=StopBatchInferenceJob&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241101T132518Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241101/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "WT8B0KFw"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "202411012125322342121442116F88A9",
    "Action": "StopBatchInferenceJob",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {}
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
