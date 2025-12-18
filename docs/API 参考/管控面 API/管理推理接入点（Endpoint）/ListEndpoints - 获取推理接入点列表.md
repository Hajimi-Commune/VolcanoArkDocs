# ListEndpoints - 获取推理接入点列表
获取推理接入点列表

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=ListEndpoints&groupName=%E7%AE%A1%E7%90%86%E6%8E%A8%E7%90%86%E6%8E%A5%E5%85%A5%E7%82%B9%EF%BC%88Endpoint%EF%BC%89&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
{
  "PageNumber": 1,
  "PageSize": 10,
  "ProjectName": "default",
  "TagFilters": [
    {
      "Key": "SPVdl",
      "Values": [
        "4oQ"
      ]
    }
  ],
  "Filter": {
    "EndpointModelTypes": [
      "rhT"
    ],
    "Statuses": [
      "OM"
    ],
    "ModelVersions": [
      "gQVMJhca1P"
    ],
    "CustomModelIds": [
      "3938w"
    ],
    "FoundationModelName": "vnFbTbR1l",
    "Ids": [
      "592obll5Rho"
    ],
    "Name": "0724147D"
  },
  "SortBy": "4",
  "SortOrder": "lC"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514213143172211079122D5C1C8",
    "Action": "ListEndpoints",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 100,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "Id": "h3w92m",
        "Name": "TT7LbL8V",
        "Description": "eoxAtz8pb01",
        "ProjectName": "lvaqGj",
        "Tags": [
          {
            "Key": "X5b4S2EiYb",
            "Value": "EJUSY0fl"
          }
        ],
        "ModelReference": {
          "CustomModelId": "V0",
          "FoundationModel": {
            "Name": "i",
            "ModelVersion": "6vXDzwOuB7"
          }
        },
        "EndpointModelType": "gIqecA",
        "ModelUnitId": "K",
        "Status": "mekO1h",
        "UpdateTime": "zmUtyY",
        "CreateTime": "oFBriqbTLll",
        "RateLimit": {
          "Rpm": 91,
          "Tpm": 712
        },
        "StatusReason": "fXFny6N"
      }
    ]
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
