# RenewInstance - 实例续费

对包年包月实例或包月预留实例券进行续费。

## 使用说明

- 此接口仅支持续费符合续费规则的包年包月实例或包月预留实例券
- 续费规则可参考[续费规则概览](https://www.volcengine.com/docs/6269/81012)
- 机器学习平台暂不支持OpenAPI续费

## 注意事项

- 单账号QPS上限为30
- 子用户使用该接口时，应具备BillingCenterFullAccess或BillingCenterRefundAccess权限策略

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：RenewInstance。 | RenewInstance |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| InstanceID | string | 是 | 实例ID | ins123456780 |
| Product | string | 是 | 商品code | ecs |
| RenewalDurationUnit | string | 是 | 自动续费的周期单位。后付费不返回此字段：Year 年；Month 月；Day 天 | Month |
| RenewalDuration | integer | 否 | 续费周期。如果是天，支持：1~365；如果是月，支持：1~12，24，36；如果是年，支持：1~3 | 1 |
| UnitedExpireDay | string | 否 | 设置统一续费到期日。续费周期为月时，支持设置为每月的1~28号；续费周期为年或日时，不支持 | 1 |
| RenewRelatedInstance | boolean | 否 | 是否同一实例组内的强关系实例一起续费。true: 同实例组强绑定关系实例一起续；false: 不一起续，若存在强绑定关系实例则报错 | true |
| ClientToken | string | 否 | 幂等token（36字符），多次调用传入同样的值，会返回第一次请求的响应。多次请求使用同一个token但请求内容发生变化会报错 | t12345bvhj |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| OrderIDList | string[] | 订单号列表 | order1 |
| SuccessInstanceList | object[] | 续费成功实例列表 | - |

### SuccessInstanceList 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| InstanceID | string | 实例ID |
| Product | string | 产品编码 |

## 请求示例

```http
POST /?Action=RenewInstance&Version=2022-01-01 HTTP/1.1
Host: billing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250210T112600Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250210/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "InstanceID": "ins123456780",
    "Product": "ecs",
    "RenewalDurationUnit": "Month",
    "RenewalDuration": 1,
    "RenewRelatedInstance": false,
    "ClientToken": "tq4"
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "20250210192609163025253195E9AC22",
        "Action": "RenewInstance",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "OrderIDList": [
            "order1"
        ],
        "SuccessInstanceList": [
            {
                "InstanceID": "qaQ6",
                "Product": "vu"
            }
        ]
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | InvalidIdempotentParams | ClientToken is illegal. | ClientToken对应的参数发生变更，请更换ClientToken |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 400 | InvalidParam | RenewalTimes and UnitedExpireTime can not both exist. | 续费次数和统一到期时间不能同时存在 |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 403 | RequestReject | You have engaged in unauthorized operations, and the function is temporarily unavailable. | 您涉及违规操作，暂时无法使用该功能 |
| 404 | RecordNotFound | Record not found | 记录未找到 |
| 409 | IdempotentRequestConflict | The request is already being processed. Please try again later. | 幂等冲突 |
| 412 | CannotRenew | This instance can not be renewed. | 存在强绑定关系没有一起续费，续费周期不合法，实例处于其他交付流程等原因无法发起续费 |
| 412 | StatusWrong | The instance's status is unexpected. | 实例状态不符合预期 |
| 429 | FrequentRequest | Frequent operations, please try again later. | 操作频繁，请稍后尝试 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
