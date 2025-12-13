# GetFoundationModel - 获取基础模型信息
获取基础模型详细信息

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetFoundationModel&groupName=%E7%AE%A1%E7%90%86%E5%9F%BA%E7%A1%80%E6%A8%A1%E5%9E%8B&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```json
POST /?Action=GetFoundationModel&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T125935Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Name": "test-foundation-model"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514205945172013199191467134",
    "Action": "GetFoundationModel",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "Name": "test-foundation-model",
    "Description": "一条样例描述",
    "Introduction": "一条模型介绍",
    "VendorName": "vendor_name",
    "DisplayName": "test-model",
    "FeaturedImage": {
      "ObjectKey": "S6",
      "BucketName": "HMd3AI0Qtk"
    },
    "PrimaryVersion": "1.0",
    "AccessType": "Public",
    "FoundationModelTag": {
      "Languages": [
        "MyWisEPDOwb"
      ],
      "TaskTypes": [
        "jrP2OnqQxvA"
      ],
      "CustomizedTags": [
        "XX"
      ],
      "Domains": [
        "qdZtggWYIF"
      ],
      "UsedLibraries": [
        "wmD0DcjIbWe"
      ]
    },
    "Tags": [
      {
        "Key": "fok3iEEzKOZ",
        "Value": "ewCYfJcTR3"
      }
    ],
    "ProjectName": "default",
    "CreateTime": "2006-01-02T15:04:05Z07:00",
    "UpdateTime": "2006-01-02T15:04:05Z07:00"
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。