# ListAvailableInstances - 批量查询可用实例

批量查询可用实例信息。

## 使用说明

1. 通过此接口可以分页查询创建中、运行中、到期关停、退订关停、欠费关停、服务关闭等状态的实例列表。
2. 此接口数据查询存在一定延迟，通常3-5秒左右，不适合时延敏感场景使用。

## 注意事项

1. 单账号QPS上限为5。
2. 子用户使用该接口时，应具备BillingCenterFullAccess或BillingCenterReadOnlyAccess权限策略。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListAvailableInstances。 | ListAvailableInstances |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| MaxResults | integer | 是 | 单次查询的页大小，范围支持[1,100] | 100 |
| InstanceIDs | string[] | 否 | 实例ID列表，单次最多100 | ins1 |
| Product | string | 否 | 商品编号 | ECS |
| RenewType | string | 否 | 续费类型（仅支持一个）：ManualRenewal 手动续费；AutoRenewal 自动续费；NonRenewal 到期不续费 | ManualRenewal |
| BeginTimeStart | string | 否 | 实例开始时间段起，UTC格式：yyyy-MM-dd'T'HH:mm:ss'Z' | 2019-05-06T08:05:01Z |
| BeginTimeEnd | string | 否 | 实例开始时间段止，UTC格式：yyyy-MM-dd'T'HH:mm:ss'Z'。同时指定 BeginTimeStart 和 BeginTimeEnd 间隔不允许大于31天 | 2019-05-06T08:05:01Z |
| ExpiredTimeStart | string | 否 | 实例到期时间段起，UTC格式：yyyy-MM-dd'T'HH:mm:ss'Z' | 2019-05-06T08:05:01Z |
| ExpiredTimeEnd | string | 否 | 实例到期时间段止，UTC格式：yyyy-MM-dd'T'HH:mm:ss'Z'。同时指定ExpiredTimeStart 和 ExpiredTimeEnd 间隔不允许大于31天 | 2019-05-06T08:05:01Z |
| NextToken | string | 否 | 下一次查询的起始位（有效数字） | 0 |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| InstanceList | object[] | 实例列表 | - |
| NextToken | string | 下一次查询的起始位 | 100 |
| MaxResults | integer | 查询的页大小 | 10 |

### InstanceList 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| InstanceID | string | 实例ID |
| AccountID | integer | 账号ID |
| InstanceName | string | 实例名称 |
| Product | string | 产品编码 |
| ConfigurationCode | string | 配置代码 |
| PaymentMethod | string | 付款方式 |
| Status | string | 实例状态 |
| SubStatus | string | 子状态 |
| RenewType | string | 续费类型 |
| RenewalTimes | string | 续费次数 |
| RenewalDurationUnit | string | 续费周期单位 |
| RemainRenewTimes | string | 剩余续费次数 |
| BeginTime | string | 开始时间 |
| ExpiredTime | string | 到期时间 |

## 请求示例

```http
POST /?Action=ListAvailableInstances&Version=2022-01-01 HTTP/1.1
Host: billing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20250311T065137Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20250311/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "InstanceIDs": ["ins1", "ins2"],
    "Product": "ECS",
    "RenewType": "ManualRenewal",
    "BeginTimeStart": "2019-05-06T08:05:01Z",
    "BeginTimeEnd": "2019-05-06T08:05:01Z",
    "ExpiredTimeStart": "2019-05-06T08:05:01Z",
    "ExpiredTimeEnd": "2019-05-06T08:05:01Z",
    "NextToken": "0",
    "MaxResults": 100
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202503111451460900060851034FF805",
        "Action": "ListAvailableInstances",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "InstanceList": [
            {
                "InstanceID": "45o",
                "AccountID": 32,
                "InstanceName": "oyJ3DT",
                "Product": "K",
                "ConfigurationCode": "Fss1y",
                "PaymentMethod": "Pre",
                "Status": "ServiceShutdown",
                "SubStatus": "a66P",
                "RenewType": "ManualRenewal",
                "RenewalTimes": "0",
                "RenewalDurationUnit": "9cG3t5WKg",
                "RemainRenewTimes": "9L9lB6E8",
                "BeginTime": "2024-07-02T15:17:21+08:00",
                "ExpiredTime": "2024-07-02T15:17:21+08:00"
            }
        ],
        "NextToken": "100",
        "MaxResults": 10
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | 请求异常 | 请求异常 |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 403 | RequestReject | You have engaged in unauthorized operations, and the function is temporarily unavailable. | 您涉及违规操作，暂时无法使用该功能 |
| 404 | RecordNotFound | Record not found | 记录未找到 |
| 429 | FrequentRequest | Frequent operations, please try again later. | 操作频繁，请稍后尝试 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
