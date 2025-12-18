# ListBill - 分页查询账单

分页查询账单数据。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListBill。 | ListBill |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| BillCategoryParent | string[] | 否 | 账单大类类型：consume：消费；refund：退款；transfer：调账；默认不选为全部 | [consume] |
| BillingMode | string[] | 否 | 计费模式：1：包年包月；2：按量计费；3：合同计费；4：履约计费；默认不选为全部 | 1 |
| OwnerID | integer[] | 否 | Owner账号ID | 2100057673 |
| PayerID | integer[] | 否 | Payer账号ID | 2100057673 |
| Product | string[] | 否 | 产品名称，默认不选为全部 | [ECS] |
| Offset | integer | 否 | 偏移量 | 10 |
| Limit | integer | 是 | 数量：[1-300] | 10 |
| BillPeriod | string | 是 | 账期：格式为YYYY-MM；仅支持单月查询；所查账期至多距今 24 个月 | 2023-08 |
| PayStatus | string | 否 | 支付状态：CompletedSettle：已结清；PartSettle：未结清；Unsettle：未结算；默认不选为全部 | CompletedSettle |
| IgnoreZero | integer | 否 | 是否忽略折后价为0的数据：0：不忽略；1忽略；默认为不忽略 | 1 |
| NeedRecordNum | integer | 否 | 是否需要访问列表的总记录数：用于前端分页；1：表示需要； 0：表示不需要；默认为不需要，Total返回-1 | 1 |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| List | object[] | 账单列表 | - |
| Total | integer | 总数，默认返回-1（NeedRecordNum=0时） | 100 |
| Limit | integer | 步长 | 10 |
| Offset | integer | 偏移量 | 0 |

### List 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| BillPeriod | string | 账期 |
| PayerID | string | 付款账号ID |
| PayerUserName | string | 付款账号用户名 |
| PayerCustomerName | string | 付款账号客户名称 |
| SellerID | string | 销售账号ID |
| SellerUserName | string | 销售账号用户名 |
| SellerCustomerName | string | 销售账号客户名称 |
| OwnerID | string | 拥有者账号ID |
| OwnerUserName | string | 拥有者账号用户名 |
| OwnerCustomerName | string | 拥有者账号客户名称 |
| Product | string | 产品代码 |
| ProductZh | string | 产品中文名称 |
| BusinessMode | string | 业务模式 |
| BillingMode | string | 计费模式 |
| ExpenseBeginTime | string | 消费开始时间 |
| ExpenseEndTime | string | 消费结束时间 |
| TradeTime | string | 交易时间 |
| BillID | string | 账单ID |
| BillCategoryParent | string | 账单大类类型 |
| OriginalBillAmount | string | 原价金额 |
| PreferentialBillAmount | string | 优惠金额 |
| RoundBillAmount | string | 抹零金额 |
| DiscountBillAmount | string | 折后金额 |
| CouponAmount | string | 代金券金额 |
| PayableAmount | string | 应付金额 |
| PaidAmount | string | 已付金额 |
| UnpaidAmount | string | 未付金额 |
| Currency | string | 币种 |
| PayStatus | string | 支付状态 |
| SettlementType | string | 结算类型 |
| SubjectName | string | 服务主体名称 |
| BusiPeriod | string | 业务账期 |

## 请求示例

```bash
curl --location 'https://open.volcengineapi.com?Action=ListBill&Version=2022-01-01' \
--header 'AccessKey: AK***mE' \
--header 'SecretKey: T0***RQ==' \
--header 'Region: cn-north-1' \
--header 'ServiceName: billing' \
--header 'Content-Type: application/json' \
--header 'User-Agent: volcengine-go-sdk' \
--data '{
    "BillPeriod": "2024-01",
    "Limit": 10,
    "Offset": 0,
    "NeedRecordNum": 1,
    "IgnoreZero": 0
}'
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "20230215150503A905DDFA8B7E846117A9",
        "Action": "ListBill",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1"
    },
    "Result": {
        "List": [
            {
                "BillPeriod": "2022-10",
                "PayerID": "2000010593",
                "PayerUserName": "WtestID",
                "PayerCustomerName": "W测试",
                "OwnerID": "2000010593",
                "Product": "on_line",
                "ProductZh": "线上测试产品-勿动",
                "BusinessMode": "普通业务",
                "BillingMode": "2",
                "BillID": "Bill7149224194994114828",
                "BillCategoryParent": "消费",
                "OriginalBillAmount": "212.973452",
                "DiscountBillAmount": "212.97",
                "PayableAmount": "0.00",
                "Currency": "CNY",
                "PayStatus": "已结清"
            }
        ],
        "Total": 744,
        "Limit": 10,
        "Offset": 0
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | Request Invalid | 请求参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
