# UpdateCustomModel - 更新定制模型
更新定制模型

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=UpdateCustomModel&groupName=%E7%AE%A1%E7%90%86%E5%AE%9A%E5%88%B6%E6%A8%A1%E5%9E%8B&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=UpdateCustomModel&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T130759Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "cm-id",
  "Name": "TestName",
  "Description": "TestDescription",
  "DryRun": true
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514210804240055238131D7DFB2",
    "Action": "UpdateCustomModel",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "Id": "cm-id"
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
