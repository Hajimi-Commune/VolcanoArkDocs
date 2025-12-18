# CancelInvitation - 取消企业财务邀约

取消企业财务邀约

## 调试

API Explorer

您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=CancelInvitation&groupName=%E4%BC%81%E4%B8%9A%E8%B4%A2%E5%8A%A1&serviceCode=billing&version=2022-01-01)

## 请求参数

下方仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数名称 | 类型 | 是否必选 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | string | 必选 | CancelInvitation | 要执行的操作，取值：CancelInvitation。 |
| Version | string | 必选 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| InvitationID | string | 必选 | inv-xxxxx | 邀约ID |

## 返回参数

下方仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数名称 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| Success | boolean | true | 是否成功 |

## 请求示例

```text
curl --location 'https://open.volcengineapi.com?Action=CancelInvitation&Version=2022-01-01' \
--header 'AccessKey: AK***mE' \
--header 'SecretKey: T0***RQ==' \
--header 'Region: cn-north-1' \
--header 'ServiceName: billing' \
--header 'Content-Type: application/json' \
--header 'User-Agent: volcengine-go-sdk' \
--data '{
    "InvitationID": "inv-xxxxx"
}'
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202302151539373422AB4D3113FA66F8E3",
        "Action": "CancelInvitation",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1"
    },
    "Result": {
        "Success": true
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | Request Invalid | - |
| 400 | InvitationNotFound | Invitation not found | 邀约不存在 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
