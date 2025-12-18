# UnsubscribeInstance - 退订实例

通过此接口退订符合退订规则的包年包月实例或包月预留实例券。

## 使用说明

- 此接口仅支持退订符合退订规则的包年包月实例或包月预留实例券，退订规则可参考[退订管理](https://www.volcengine.com/docs/6269/81013)。
- 退订退款仅退还实付金额的部分，已使用的代金券不退还。
- 请仔细核对退订资源的信息，部分资源一经退订无法恢复。
- 支持OpenAPI退订的商品参考附录[支持OpenAPI退订的商品清单](https://www.volcengine.com/docs/6269/1166376)。

## 注意事项

- 单账号QPS上限为50。
- 子用户使用该接口时，应具备BillingCenterFullAccess或BillingCenterRefundAccess权限策略。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：UnsubscribeInstance。 | UnsubscribeInstance |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| InstanceID | string | 是 | 实例ID | vol-hwdajoda-xxxxx |
| Product | string | 是 | 实例ID对应的商品编码 | volume |
| UnsubscribeRelatedInstance | boolean | 否 | 是否退订当前实例ID关联的实例（同一实例组内的实例或者套装促销需整体退订的实例） | false |
| ClientToken | string | 否 | UUID生成的字符串。此字段用于幂等，多次调用传入同样的值，会返回第一次请求的响应。 | 2023032417261286E73D9F9888C471372E |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| OrderIDList | string[] | 订单号列表 | order1 |
| SuccessInstanceInfos | object[] | 退订订单创建成功的所有实例信息 | - |
| OrderID | string | 订单号（已废弃，请使用 OrderIDList 获取订单号） | Order72139298780xxxxxx |

### SuccessInstanceInfos 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| Product | string | 产品编码 |
| InstanceID | string | 实例ID |

## 请求示例

```http
POST /?Action=UnsubscribeInstance&Version=2022-01-01 HTTP/1.1
Host: billing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241223T090021Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241223/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "InstanceID": "vol-hwdajoda-xxxxx",
    "Product": "volume",
    "UnsubscribeRelatedInstance": true,
    "ClientToken": "2023032417261286E73D9F9888C471372E"
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "20241223170027024017043000D145A4",
        "Action": "UnsubscribeInstance",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "OrderID": "Order72139298780xxxxxx",
        "SuccessInstanceInfos": [
            {
                "Product": "DIVob9xH",
                "InstanceID": "IYdQY"
            }
        ],
        "OrderIDList": [
            "order1",
            "order2"
        ]
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | ParamInvalid | Request param is invalid | 客户端参数错误 |
| 400 | BadRequest | Request error, failed to pass the validation | 客户端请求未通过校验，可能是业务规则导致无法请求成功 |
| 400 | CannotUnsubscribe | This instance cannot be unsubscribed | 由于退订规则，当前实例不可通过OpenAPI退订 |
| 400 | InvalidIdempotentParams | The ClientToken is illegal | ClientToken对应的参数发生变更，请更换ClientToken |
| 400 | InstanceExpireTimeRequired | Instance has no expiration time set | 实例未设置到期时间 |
| 403 | InstancePermissionDenied | No permission to operate the instance | 当前AK/SK缺少操作此实例的权限 |
| 404 | InstanceNotFound | The specified instance was not found | 指定实例不存在或未查询到指定实例 |
| 409 | IdempotentRequestConflict | The request is already being processed, please try again later | 幂等冲突 |
| 412 | StatusWrong | Instance's status is unexpected | 实例或订单状态不符合预期 |
| 500 | InternalServerError | Internal server error | 系统错误，多次出现时请联系管理员 |
| 500 | ThirdSystemError | Internal server error | 服务内部异常 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
