# ListCustomModels - 获取定制模型列表
获取定制模型列表

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=ListCustomModels&groupName=%E7%AE%A1%E7%90%86%E5%AE%9A%E5%88%B6%E6%A8%A1%E5%9E%8B&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=ListCustomModels&Version=2024-01-01 HTTP/1.1
Host: open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T130512Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "PageNumber": 1,
  "PageSize": 1,
  "Filter": {
    "Ids": [
      "vrYVig"
    ],
    "Name": "aThQ2SZH"
  },
  "TagFilters": [
    {
      "Key": "8mYbP7pN",
      "Values": [
        "8bS"
      ]
    }
  ],
  "ProjectName": "default",
  "SortBy": "CreateTime",
  "SortOrder": "Desc"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514210517123075045154E335F9",
    "Action": "ListCustomModels",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "Id": "jJJ",
        "Name": "Xf",
        "Description": "F",
        "SourceType": "lBQb26",
        "ModelCustomizationJobId": "eYClLOD",
        "CustomizationType": "lxb0G1",
        "Status": "Ready",
        "HyperParameters": [
          {
            "Name": "cALJ0SSf",
            "Value": "Iais76"
          }
        ],
        "FoundationModel": {
          "Name": "OI",
          "ModelVersion": "jf0GoztY9"
        },
        "Tags": [
          {
            "Key": "VKk5m6sRdxl",
            "Value": "t3ieup"
          }
        ],
        "ProjectName": "bIA3",
        "CreateTime": "lYuGzty",
        "UpdateTime": "D33NRefE"
      }
    ]
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
