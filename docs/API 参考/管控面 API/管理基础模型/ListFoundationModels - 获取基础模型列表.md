# ListFoundationModels - 获取基础模型列表
列出基础模型。

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=ListFoundationModels&groupName=%E7%AE%A1%E7%90%86%E5%9F%BA%E7%A1%80%E6%A8%A1%E5%9E%8B&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=ListFoundationModels&Version=2024-01-01 HTTP/1.1
Host: open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240514T130015Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240514/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "PageNumber": 1,
  "PageSize": 10,
  "Filter": {
    "Name": "1yEK",
    "Names": [
      "b4"
    ],
    "FoundationModelTag": {
      "Languages": [
        "Tv0mxXamkC5"
      ],
      "TaskTypes": [
        "hmqgZGGni"
      ],
      "CustomizedTags": [
        "zafcaqOy"
      ],
      "Domains": [
        "RRamYGIqLt"
      ],
      "UsedLibraries": [
        "Rctg97iyG0G"
      ]
    },
    "Introduction": "sdQb3Q",
    "Description": "ncjtG458V89",
    "DisplayName": "NBgVWD3XwHJ",
    "AccessTypes": [
      "b6Qw9wRLBb"
    ]
  },
  "SortOrder": "Desc",
  "SortBy": "\\-",
  "TagFilters": [
    {
      "Key": "DcuMJa",
      "Values": [
        "5M8sMT"
      ]
    }
  ]
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "20240514210020173184054004BACCC8",
    "Action": "ListFoundationModels",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 2,
    "PageNumber": 1,
    "PageSize": 1,
    "Items": [
      {
        "Name": "test-foundation-model",
        "Description": "wxr",
        "Introduction": "language models",
        "VendorName": "vERtPPat6Xz",
        "DisplayName": "sBEA",
        "FeaturedImage": {
          "ObjectKey": "J3Og",
          "BucketName": "5E"
        },
        "PrimaryVersion": "zYCdJe3n",
        "FoundationModelTag": {
          "Languages": [
            "XEQL2sR"
          ],
          "TaskTypes": [
            "ZpMo3"
          ],
          "CustomizedTags": [
            "80woxH8k3Ly"
          ],
          "Domains": [
            "0DQGJbb"
          ],
          "UsedLibraries": [
            "Jijubh2yS"
          ]
        },
        "AccessType": "Public",
        "Tags": [
          {
            "Key": "3",
            "Value": "V"
          }
        ],
        "ProjectName": "default",
        "CreateTime": "2006-01-02T15:04:05Z07:00\n",
        "UpdateTime": "2006-01-02T15:04:05Z07:00\n"
      }
    ]
  }
}
```

## 错误码
您可访问[公共错误码](https://www.volcengine.com/docs/82379/1299023)，获取更多错误码信息。
