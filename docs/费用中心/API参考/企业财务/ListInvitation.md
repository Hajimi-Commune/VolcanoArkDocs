# ListInvitation - 查询企业财务邀约

查询企业财务邀约

## 调试

API Explorer

您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=ListInvitation&groupName=%E4%BC%81%E4%B8%9A%E8%B4%A2%E5%8A%A1&serviceCode=billing&version=2022-01-01)

## 请求参数

下方仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数名称 | 类型 | 是否必选 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | string | 必选 | ListInvitation | 要执行的操作，取值：ListInvitation。 |
| Version | string | 必选 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| InvitationID | string | 可选 | inv-xxxxx | 邀约ID |
| SubAccountID | long[] | 可选 | [2100057673] | 子账号ID列表 |
| Relation | integer[] | 可选 | [1] | 财务关系类型：1-财务云，2-财务托管 |
| Status | integer[] | 可选 | [1] | 邀约状态 |
| Limit | integer | 可选 | 10 | 数量 |
| Offset | integer | 可选 | 0 | 偏移量 |

## 返回参数

下方仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数名称 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | object[] | - | 邀约列表 |
| List.InvitationID | string | inv-xxxxx | 邀约ID |
| List.MajorAccountID | string | 2100057673 | 主账号ID |
| List.MajorAccountName | string | master | 主账号名称 |
| List.SubAccountID | string | 2100057674 | 子账号ID |
| List.SubAccountName | string | sub | 子账号名称 |
| List.Relation | integer | 1 | 财务关系类型 |
| List.Status | integer | 1 | 邀约状态 |
| List.CreateTime | string | 2023-08-01 10:00:00 | 创建时间 |
| List.ExpireTime | string | 2023-08-08 10:00:00 | 过期时间 |
| Total | integer | 100 | 总数 |
| Limit | integer | 10 | 步长 |
| Offset | integer | 0 | 偏移量 |

## 请求示例

```text
curl --location 'https://open.volcengineapi.com?Action=ListInvitation&Version=2022-01-01' \
--header 'AccessKey: AK***mE' \
--header 'SecretKey: T0***RQ==' \
--header 'Region: cn-north-1' \
--header 'ServiceName: billing' \
--header 'Content-Type: application/json' \
--header 'User-Agent: volcengine-go-sdk' \
--data '{
    "Limit": 10,
    "Offset": 0
}'
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202302151539373422AB4D3113FA66F8E3",
        "Action": "ListInvitation",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1"
    },
    "Result": {
        "List": [
            {
                "InvitationID": "inv-xxxxx",
                "MajorAccountID": "2100057673",
                "MajorAccountName": "master",
                "SubAccountID": "2100057674",
                "SubAccountName": "sub",
                "Relation": 1,
                "Status": 1,
                "CreateTime": "2023-08-01 10:00:00",
                "ExpireTime": "2023-08-08 10:00:00"
            }
        ],
        "Total": 1,
        "Limit": 10,
        "Offset": 0
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | Request Invalid | - |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
