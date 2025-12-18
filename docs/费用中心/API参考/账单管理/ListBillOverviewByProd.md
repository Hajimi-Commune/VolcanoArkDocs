# ListBillOverviewByProd - 分页查询账单总览-产品汇总信息

分页查询账单总览-产品汇总信息

## 调试

API Explorer

您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=ListBillOverviewByProd&groupName=%E8%B4%A6%E5%8D%95%E4%B8%AD%E5%BF%83&serviceCode=billing&version=2022-01-01)

## 请求参数

下方仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数名称 | 类型 | 是否必选 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | string | 必选 | ListBillOverviewByProd | 要执行的操作，取值：ListBillOverviewByProd。 |
| Version | string | 必选 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| PayerID | long[] | 可选 | [2100057673] | Payer账号ID |
| OwnerID | long[] | 可选 | [2100057673] | Owner账号ID |
| Offset | integer | 可选 | 10 | 偏移量 |
| Limit | integer | 必选 | 10 | 数量：[1-300] |
| BillPeriod | string | 必选 | 2023-08 | 账期：格式为YYYY-MM；仅支持单月查询；所查账期至多距今 24 个月 |
| IgnoreZero | integer | 可选 | 1 | 是否忽略折后价为0的数据：0：不忽略；1忽略；默认为不忽略 |
| NeedRecordNum | integer | 可选 | 1 | 是否需要访问列表的总记录数：用于前端分页；1：表示需要； 0：表示不需要；默认为不需要，Total返回-1 |
| BillingMode | string[] | 可选 | [1] | 计费模式：1：包年包月；2：按量计费；3：合同计费；4：履约计费；默认不选为全部 |
| BillCategoryParent | string[] | 可选 | [consume] | 账单大类类型：consume：消费；refund：退款；transfer：调账；默认不选为全部 |
| Product | string[] | 可选 | [ECS] | 产品名称，默认不选为全部 |

## 返回参数

下方仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数名称 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | object[] | - | 账单总览-产品汇总列表 |
| List.BillPeriod | string | 2023-08 | 账期 |
| List.PayerID | string | 2100153894 | 支付账号ID |
| List.PayerUserName | string | Doooo | 支付账户名 |
| List.PayerCustomerName | string | 北京火山引擎科技有限公司 | 支付账号客户名称 |
| List.SellerID | string | 3423 | Seller账号ID |
| List.SellerUserName | string | 火山引擎 | Seller账户名 |
| List.SellerCustomerName | string | 北京火山引擎科技有限公司 | Seller账号客户名称 |
| List.OwnerID | string | 2100153894 | Owner账号ID |
| List.OwnerUserName | string | Doooo | Owner账户名 |
| List.OwnerCustomerName | string | 北京火山引擎科技有限公司 | Owner账号客户名称 |
| List.BusinessMode | string | 普通业务 | 业务类型 |
| List.Product | string | ECS | 产品英文名称 |
| List.ProductZh | string | 云服务器 | 产品中文名称 |
| List.BillingMode | string | 2 | 计费模式 |
| List.BillCategoryParent | string | 消费 | 账单类型 |
| List.OriginalBillAmount | string | 168.740000 | 原价 |
| List.PreferentialBillAmount | string | 148.500779 | 优惠金额 |
| List.RoundBillAmount | string | 0.559221 | 抹零金额 |
| List.DiscountBillAmount | string | 19.68 | 折后价 |
| List.CouponAmount | string | 0.00 | 代金券抵扣 |
| List.PayableAmount | string | 19.68 | 应付金额 |
| List.PaidAmount | string | 0.00 | 现金支付 |
| List.UnpaidAmount | string | 0.00 | 欠费金额 |
| List.SettlementType | string | 非结算 | 结算类型，settle：结算，non-settle：非结算，quota-settle：Quota结算 |
| List.CreditCarriedAmount | string | 0.00 | 信控额度退款抵扣 |
| List.Currency | string | - | 币种 |
| List.SubjectName | string | - | 主体名 |
| List.SettleTax | string | - | 结算币种税额 |
| List.SettlePretaxRealValue | string | - | 结算币种税前真实金额 |
| List.SettlePosttaxAmount | string | - | 结算币种税后应付金额 |
| List.SettlePretaxAmount | string | - | 结算币种税前应付金额 |
| List.PosttaxAmount | string | - | 税后应付金额 |
| List.PretaxAmount | string | - | 税前应付金额 |
| List.Tax | string | - | 税额 |
| List.CountryRegion | string | - | 国家地区 |
| List.SettleRealValue | string | - | 结算币种真实金额 |
| List.RealValue | string | - | 真实金额 |
| List.PretaxRealValue | string | - | 税前真实金额 |
| List.SavingPlanOriginalAmount | string | 621.946905 | 节省计划抵扣原价 |
| List.PreTaxPayableAmount | string | 19.68 | 税前应付金额（定价币种） |
| List.SettlePayableAmount | string | 19.68 | 税后应付金额（结算币种） |
| List.SettlePreTaxPayableAmount | string | 19.68 | 税前应付金额（结算币种） |
| List.CurrencySettlement | string | CNY | 结算币种 |
| Total | integer | 100 | 总数，默认返回-1（NeedRecordNum=0时） |
| Limit | integer | 100 | 步长 |
| Offset | integer | 0 | 偏移量 |

## 请求示例

```text
curl --location 'https://open.volcengineapi.com?Action=ListBillOverviewByProd&Version=2022-01-01' \
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

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202302151539373422AB4D3113FA66F8E3",
        "Action": "ListBillOverviewByProd",
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
                "SellerID": "3423",
                "SellerUserName": "火山引擎",
                "SellerCustomerName": "北京火山引擎科技有限公司",
                "OwnerID": "2000010593",
                "OwnerUserName": "WtestID",
                "OwnerCustomerName": "W测试",
                "BusinessMode": "普通业务",
                "Product": "on_line",
                "ProductZh": "线上测试产品-勿动",
                "BillingMode": "2",
                "BillCategoryParent": "consume",
                "OriginalBillAmount": "158452.248288",
                "PreferentialBillAmount": "0.000000",
                "RoundBillAmount": "2.568288",
                "DiscountBillAmount": "158449.68",
                "CouponAmount": "0.00",
                "PayableAmount": "0.00",
                "PaidAmount": "0.00",
                "UnpaidAmount": "0.00",
                "SettlementType": "非结算"
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
