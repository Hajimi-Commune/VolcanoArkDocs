# ListBillOverviewByCategory - 查询账单总览-账号汇总信息

查询账单总览-账号汇总信息

## 调试

API Explorer

您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=ListBillOverviewByCategory&groupName=%E8%B4%A6%E5%8D%95%E4%B8%AD%E5%BF%83&serviceCode=billing&version=2022-01-01)

## 请求参数

下方仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数名称 | 类型 | 是否必选 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | string | 必选 | ListBillOverviewByCategory | 要执行的操作，取值：ListBillOverviewByCategory。 |
| Version | string | 必选 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| BillingMode | string[] | 可选 | [1] | 计费模式：1：包年包月；2：按量计费；3：合同计费；4：履约计费；默认不选为全部 |
| BillCategoryParent | string[] | 可选 | [consume] | 账单大类类型：consume：消费；refund：退款；transfer：调账；默认不选为全部 |
| PayerID | long[] | 可选 | [2100057673] | Payer账号ID |
| OwnerID | long[] | 可选 | [2100057673] | Owner账号ID |
| BillPeriod | string | 必选 | 2023-03 | 账务账期：格式为YYYY-MM；仅支持单月查询；所查账期至多距今 24 个月 |

## 返回参数

下方仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数名称 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | object[] | - | 账单总览-账号汇总总列表 |
| List.List | object[] | - | 账单总览-账号汇总项 |
| List.List.OwnerID | string | 2100057673 | Owner账号ID |
| List.List.PayableAmount | string | 0.00 | 应付金额 |
| List.List.UnpaidAmount | string | 0.00 | 欠费金额 |
| List.List.OriginalBillAmount | string | 0.00 | 原价 |
| List.List.DiscountBillAmount | string | 0.00 | 折后价 |
| List.List.PaidAmount | string | 0.00 | 现金支付 |
| List.List.CouponAmount | string | 0.00 | 代金券抵扣 |
| List.List.PayerID | string | 2100057673 | 支付账号ID |
| List.List.SellerID | string | 3423 | Seller账号ID |
| List.List.CreditCarriedAmount | string | 0.00 | 信控额度退款抵扣 |
| List.List.OwnerUserName | string | weitest009 | Owner账号名 |
| List.List.OwnerCustomerName | string | 俊杰1 | Owner账号客户名称 |
| List.List.SettlementType | string | 非结算 | 结算类型，settle：结算，non-settle：非结算，quota-settle：Quota结算 |
| List.List.SubjectName | string | 北京火山引擎科技有限公司 | 主体名 |
| List.List.Currency | string | CNY | 币种 |
| List.List.PayerCustomerName | string | 俊杰1 | 支付账号客户名称 |
| List.List.SellerUserName | string | 火山引擎 | Seller账号名 |
| List.List.SellerCustomerName | string | 北京火山引擎科技有限公司 | Seller账号客户名称 |
| List.List.BusinessMode | string | 财务托管 | 业务类型 |
| List.List.BillPeriod | string | 2023-11 | 账期 |
| List.List.BillCategoryParent | string | 消费 | 账单类型 |
| List.List.PayerUserName | string | weitest009 | 支付账号名 |
| List.List.SubjectNo | string | 3423 | 主体编码 |
| List.List.SettlePretaxRealValue | string | - | 结算币种税前真实金额 |
| List.List.CountryArea | string | - | 国家地区 |
| List.List.RealValue | string | - | 真实金额 |
| List.List.SettleRealValue | string | - | 结算币种真实金额 |
| List.List.PretaxRealValue | string | - | 税前真实金额 |
| List.List.SettlePretaxAmount | string | - | 结算币种税前应付金额 |
| List.List.PretaxAmount | string | - | 税前应付金额 |
| List.List.SettlePosttaxAmount | string | - | 结算币种税后应付金额 |
| List.List.CountryRegion | string | - | 国家区域 |
| List.List.PosttaxAmount | string | - | 税后税额 |
| List.List.Tax | string | - | 税额 |
| List.List.SettleTax | string | - | 结算币种税额 |
| List.List.SavingPlanOriginalAmount | string | 621.946905 | 节省计划抵扣原价 |
| List.List.PreTaxPayableAmount | string | 0.00 | 税前应付金额（定价币种） |
| List.List.SettlePayableAmount | string | - | 税后应付金额（结算币种） |
| List.List.SettlePreTaxPayableAmount | string | 0.00 | 税前应付金额（结算币种） |
| List.List.CurrencySettlement | string | CNY | 结算币种 |

## 请求示例

```text
curl --location 'https://open.volcengineapi.com?Action=ListBillOverviewByCategory&Version=2022-01-01' \
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
    "ResponseMetadata":{
        "RequestId":"202302151509010207BA7B59BCEA6513BD",
        "Action":"ListBillOverviewByCategory",
        "Version":"2022-01-01",
        "Service":"billing",
        "Region":"cn-north-1"
    },
    "Result":{
        "List": [
            {
                "List":[
                    {
                        "PayerID": "2100057673",
                        "PayerUserName": "weitest009",
                        "PayerCustomerName": "俊杰1",
                        "BusinessMode": "普通业务",
                        "BillPeriod": "2023-11",
                        "BillCategoryParent": "消费",
                        "SubjectNo": "3423",
                        "SellerID": "3423",
                        "SellerUserName": "火山引擎",
                        "SellerCustomerName": "北京火山引擎科技有限公司",
                        "OwnerID": "2100057673",
                        "OwnerUserName": "weitest009",
                        "OwnerCustomerName": "俊杰1",
                        "SettlementType": "结算",
                        "SubjectName": "北京火山引擎科技有限公司",
                        "CountryArea": "",
                        "OriginalBillAmount": "437.946904",
                        "DiscountBillAmount": "437.94",
                        "CouponAmount": "0.00",
                        "PayableAmount": "437.94",
                        "PaidAmount": "0.00",
                        "UnpaidAmount": "437.94",
                        "CreditCarriedAmount": "0.00"
                    }
                ]
            }
        ]
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | RequestInvalid | - |
| 400 | MissingParameter | The required parameter BillPeriod is missing. | - |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
