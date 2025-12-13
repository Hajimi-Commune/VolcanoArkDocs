# UpdateModelCustomizationJob - 更新模型调优任务
更新模型调优任务

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=UpdateModelCustomizationJob&groupName=%E7%AE%A1%E7%90%86%E6%A8%A1%E5%9E%8B%E8%B0%83%E4%BC%98%E4%BB%BB%E5%8A%A1&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

本接口无特有的返回参数。更多信息请见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=UpdateModelCustomizationJob&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250115T083603Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250115/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "mcj-test-id"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20250115163612164250161131A08F01",
    "Action": "UpdateModelCustomizationJob",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {}
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。