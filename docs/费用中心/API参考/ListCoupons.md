# ListCoupons - 查询代金券信息

查询代金券信息。

## 注意事项

- 查询QPS限制为10QPS，超过会报错

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListCoupons。 | ListCoupons |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| ValidBeginTime | string | 否 | 代金券生效时间范围 开始时间 | 2022-02-02T12:00:00Z |
| ValidEndTime | string | 否 | 代金券生效时间范围 结束时间 | 2022-02-02T12:00:00Z |
| ProductCode | string | 否 | 商品Code | ECS |
| Status | string | 否 | 代金券状态（多个用英文逗号分隔）：1：待生效；2：生效中；3：已失效；4：已使用；5：已作废 | 2 |
| Limit | integer | 否 | 每页返回的数据量 | 10 |
| Offset | integer | 否 | 偏移量 | 0 |
| CouponID | string | 否 | 代金券ID | DT4CC6IB2IZB8D1H |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| List | object[] | 代金券列表 | - |
| Total | integer | 总数 | 10 |
| Limit | integer | 每页数量 | 10 |
| Offset | integer | 偏移量 | 0 |

### List 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| AccountID | integer | 账号ID |
| AcquireTime | string | 获取时间 |
| AmountLimit | number | 金额限制 |
| AssignedOwnerIDList | array | 分配的Owner账号ID列表 |
| BeginTime | string | 生效开始时间 |
| CouponID | string | 代金券ID |
| CouponName | string | 代金券名称 |
| ExpiredTime | string | 过期时间 |
| OrderTypeLimit | string | 订单类型限制 |
| PayTypeLimit | string | 支付类型限制 |
| ProductLimitList | object[] | 产品限制列表 |
| RemainingAmount | number | 剩余金额 |
| Remark | string | 备注 |
| Status | integer | 状态 |
| TotalAmount | number | 总金额 |
| UsageLimit | integer | 使用限制 |

## 请求示例

```http
POST /open-apis-v2/coupon?Action=ListCoupons&Version=2022-01-01 HTTP/1.1
Host: open.volcengineapi.com
Accept: application/json
Accept-Encoding: gzip
Authorization: ******
Content-Type: application/json; charset=utf-8

{
    "Limit": 10,
    "Offset": 0,
    "ProductCode": "YJQceshi,on_line",
    "ValidBeginTime": "2024-10-16T15:59:59Z",
    "ValidEndTime": "2024-10-20T15:59:59Z"
}
```

## 响应示例

```json
{
    "Metadata": {
        "RequestId": "20250813192131978261F85B58C706552F",
        "Action": "ListCoupons",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1",
        "HTTPCode": 200
    },
    "Limit": 10,
    "List": [
        {
            "AccountID": 2000010593,
            "AcquireTime": "2024-10-17T11:14:53+08:00",
            "AmountLimit": 0,
            "AssignedOwnerIDList": [],
            "BeginTime": "2024-10-17T11:14:53+08:00",
            "CouponID": "D6JVHMZ6WWQ1NVRW",
            "CouponName": "火山引擎代金券",
            "ExpiredTime": "2024-10-20T23:59:59+08:00",
            "OrderTypeLimit": "1,6,3,4,9",
            "PayTypeLimit": "all",
            "ProductLimitList": [
                {
                    "BillingMethod": "",
                    "ChargeItemCode": "",
                    "ChargeItemType": 0,
                    "ConfigurationCode": "",
                    "ConfigurationName": "",
                    "ConfigurationType": 1,
                    "ProductCode": "YJQceshi",
                    "ProductName": "YJQceshi",
                    "TimeLimitLower": -1,
                    "TimeLimitUpper": -1
                }
            ],
            "RemainingAmount": 10,
            "Remark": "系统回收",
            "Status": 5,
            "TotalAmount": 10,
            "UsageLimit": 2
        }
    ],
    "Offset": 0,
    "Total": 1
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
