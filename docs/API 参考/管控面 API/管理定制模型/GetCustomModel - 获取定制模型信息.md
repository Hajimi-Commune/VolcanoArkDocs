# GetCustomModel - 获取定制模型信息
获取定制模型信息

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetCustomModel&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T130337Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "cm-id"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "2024051421034216322206625187C9AD",
    "Action": "GetCustomModel",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "CustomModel": {
      "Id": "1GSH",
      "Name": "2dYgMKVBt",
      "Description": "d4Hkd5Fe3M",
      "SourceType": "GWVZA3",
      "ModelCustomizationJobId": "4Uxl",
      "CustomizationType": "Hoev2RymwBl",
      "Status": "Ready",
      "HyperParameters": [
        {
          "Name": "rFL44dtTT9",
          "Value": "RnF7Q"
        }
      ],
      "FoundationModel": {
        "Name": "YflRG",
        "ModelVersion": "FvVqyQtk"
      },
      "Tags": [
        {
          "Key": "QBb",
          "Value": "rwl1F860fK"
        }
      ],
      "ProjectName": "gQZXp",
      "CreateTime": "X",
      "UpdateTime": "quUkQIjh"
    }
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。