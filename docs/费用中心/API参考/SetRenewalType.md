# SetRenewalType - 设置实例续费类型

为一个实例设置续费类型，包括自动续费、手动续费、到期不续费。

## 使用说明

- 使用此接口设置自动续费时，仅支持续费符合续费规则的包年包月实例或包月预留实例券
- 续费规则可参考[续费规则概览](https://www.volcengine.com/docs/6269/81012)
- 机器学习平台暂不支持OpenAPI设置自动续费
- 已到期、即将关停/已关停实例，仅支持设置为手动续费
- 预留实例券到期后不支持设置续费类型
- 包年包月实例到期回收后不支持设置续费类型
- 退订关停实例不支持设置续费类型

## 注意事项

- 单账号QPS上限为30
- 子用户使用该接口时，应具备BillingCenterFullAccess或BillingCenterRenewAccess权限策略

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：SetRenewalType。 | SetRenewalType |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| InstanceID | string | 是 | 实例ID，仅支持指定一个 | ins123 |
| Product | string | 是 | 实例ID对应的商品编码 | ECS |
| RenewType | string | 是 | 续费类型：ManualRenewal 手动续费；AutoRenewal 自动续费；NonRenewal 到期不续费 | AutoRenewal |
| RenewalDurationUnit | string | 否 | 自动续费周期单位。当 RenewType = AutoRenewal 时，必须设置。Day 天；Month 月；Year 年 | Year |
| RenewalDuration | integer | 否 | 单次自动续费时长。当 RenewType = AutoRenewal 时，必须设置。如果是天，支持：1~365；如果是月，支持：1~12，24，36；如果是年，支持：1~3 | 1 |
| RenewalTimes | integer | 否 | 自动续费次数。不填写时，默认永久有效。如果限制续费次数，支持：1~100 | 1 |
| SetRenewalRelatedInstance | boolean | 否 | 是否同一实例组内的强关系实例一起设置续费类型。true: 一起设置；false: 不一起设置（若存在强绑定关系实例则报错） | true |
| ClientToken | string | 否 | 幂等token（36字符），多次调用传入同样的值，会返回第一次请求的响应。多次请求使用同一个token但请求内容发生变化会报错 | t12345ghfj |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| SuccessInstanceList | object[] | 设置续费类型成功实例列表 | - |

### SuccessInstanceList 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| InstanceID | string | 实例ID |
| Product | string | 产品编码 |

## 请求示例

```http
POST /?Action=SetRenewalType&Version=2022-01-01 HTTP/1.1
Host: billing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250311T063207Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250311/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "InstanceID": "ins123",
    "Product": "ecs",
    "RenewType": "AutoRenewal",
    "RenewalDurationUnit": "Year",
    "RenewalDuration": 1,
    "RenewalTimes": 1,
    "SetRenewalRelatedInstance": true,
    "ClientToken": "t12345ghfj"
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "2025031114175308612821124786FDF9",
        "Action": "SetRenewalType",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "SuccessInstanceList": [
            {
                "InstanceID": "b",
                "Product": "Z4yIpYlR"
            }
        ]
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 403 | RequestReject | You have engaged in unauthorized operations, and the function is temporarily unavailable. | 您涉及违规操作，暂时无法使用该功能 |
| 404 | RecordNotFound | Record not found | 记录未找到 |
| 409 | IdempotentRequestConflict | The request is already being processed. Please try again later. | 幂等冲突 |
| 412 | StatusWrong | The instance's status is unexpected. | 实例状态不符合预期 |
| 412 | CannotSetRenewalType | This instance can not set renewal type. | 该实例无法设置续费类型 |
| 429 | FrequentRequest | Frequent operations, please try again later. | 操作频繁，请稍后尝试 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
