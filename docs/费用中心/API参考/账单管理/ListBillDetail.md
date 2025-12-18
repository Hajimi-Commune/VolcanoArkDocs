# ListBillDetail - 分页查询账单明细

分页查询账单明细数据。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListBillDetail。 | ListBillDetail |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| OwnerID | integer[] | 否 | Owner账号ID | 2100057673 |
| PayerID | integer[] | 否 | Payer账号ID | 2100057673 |
| Product | string[] | 否 | 产品名称，默认不选为全部 | [ECS] |
| BillingMode | string[] | 否 | 计费模式：1：包年包月；2：按量计费；3：合同计费；4：履约计费；默认不选为全部 | 1 |
| BillCategory | string[] | 否 | 账单类型：consume-use：消费-使用；consume-new：消费-新购；consume-renew：消费-续费；consume-formalize：消费-转正；consume-modify：消费-更配；consume-trial：消费-试用；refund-terminate：退款-退订；refund-modify：退款-更配；transfer-manual：调账-人工；transfer-system：调账-系统；默认不选为全部 | [consume-use] |
| Offset | integer | 否 | 偏移量 | 10 |
| Limit | integer | 是 | 数量：[1-300] | 10 |
| BillPeriod | string | 是 | 账期：格式为YYYY-MM；仅支持单月查询；所查账期至多距今 24 个月 | 2023-08 |
| ExpenseDate | string | 否 | 账单日期，在GroupPeriod=1或2时支持账单日期传参，必须与账期在同一月份。该参数可提升查询性能，建议填写。 | 2023-03-15 |
| GroupTerm | integer | 否 | 统计项：0：计费项；1：实例；2：产品；3：账号 | 0 |
| GroupPeriod | integer | 否 | 统计周期：0：账期；1：按天；2：明细 | 0 |
| InstanceNo | string | 否 | 实例id：默认不选为全部 | i-ycjlq77tdg8rx6ib4v1s |
| IgnoreZero | integer | 否 | 是否忽略折后价为0的数据：0：不忽略；1忽略；默认为不忽略 | 1 |
| NeedRecordNum | integer | 否 | 是否需要访问列表的总记录数：用于前端分页；1：表示需要； 0：表示不需要；默认为不需要，Total返回-1 | 1 |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| List | object[] | 账单明细列表 | - |
| Total | integer | 总数，默认返回-1（NeedRecordNum=0时） | 100 |
| Limit | integer | 步长 | 100 |
| Offset | integer | 偏移量 | 0 |

### List 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| BillPeriod | string | 账期 |
| ExpenseDate | string | 消费日期 |
| PayerID | string | 付款账号ID |
| PayerUserName | string | 付款账号用户名 |
| PayerCustomerName | string | 付款账号客户名称 |
| SellerID | string | 销售账号ID |
| SellerUserName | string | 销售账号用户名 |
| SellerCustomerName | string | 销售账号客户名称 |
| OwnerID | string | 拥有者账号ID |
| OwnerUserName | string | 拥有者账号用户名 |
| OwnerCustomerName | string | 拥有者账号客户名称 |
| BusinessMode | string | 业务模式 |
| Product | string | 产品代码 |
| ProductZh | string | 产品中文名称 |
| BillingMode | string | 计费模式 |
| ExpenseBeginTime | string | 消费开始时间 |
| ExpenseEndTime | string | 消费结束时间 |
| UseDuration | string | 使用时长 |
| UseDurationUnit | string | 使用时长单位 |
| TradeTime | string | 交易时间 |
| BillID | string | 账单ID |
| BillCategory | string | 账单类型 |
| InstanceNo | string | 实例ID |
| InstanceName | string | 实例名称 |
| ConfigName | string | 配置名称 |
| Element | string | 计费项 |
| Region | string | 地域 |
| Zone | string | 可用区 |
| Factor | string | 因子 |
| ExpandField | string | 扩展字段 |
| Price | string | 价格 |
| PriceUnit | string | 价格单位 |
| Count | string | 数量 |
| Unit | string | 单位 |
| DeductionCount | string | 抵扣数量 |
| OriginalBillAmount | string | 原价金额 |
| PreferentialBillAmount | string | 优惠金额 |
| DiscountBillAmount | string | 折后金额 |
| CouponAmount | string | 代金券金额 |
| PayableAmount | string | 应付金额 |
| PaidAmount | string | 已付金额 |
| UnpaidAmount | string | 未付金额 |
| Currency | string | 币种 |
| SettlementType | string | 结算类型 |
| Project | string | 项目 |
| Tag | string | 标签 |
| SellingMode | string | 销售模式 |
| SolutionZh | string | 解决方案中文名 |
| SubjectName | string | 服务主体名称 |
| RoundAmount | number | 抹零金额 |
| BusiPeriod | string | 业务账期 |
| ReservationInstance | string | 预留实例 |
| BillDetailId | string | 账单明细ID |
| ElementCode | string | 计费项代码 |
| RegionCode | string | 地域代码 |
| ZoneCode | string | 可用区代码 |
| FactorCode | string | 因子代码 |
| ConfigurationCode | string | 配置代码 |
| DeductionUseDuration | string | 抵扣使用时长 |
| CreditCarriedAmount | string | 信控额度抵扣金额 |
| BillingFunction | string | 计费函数 |
| MarketPrice | string | 市场价 |
| DiscountBizBillingFunction | string | 折扣业务计费函数 |
| DiscountBizUnitPrice | string | 折扣业务单价 |
| DiscountBizUnitPriceInterval | string | 折扣业务单价区间 |
| DiscountBizMeasureInterval | string | 折扣业务计量区间 |
| EffectiveFactor | string | 有效因子 |
| PriceInterval | string | 价格区间 |
| MeasureInterval | string | 计量区间 |
| BillingMethodCode | string | 计费方式代码 |
| ProjectDisplayName | string | 项目显示名称 |

## 请求示例

```bash
curl --location 'https://open.volcengineapi.com?Action=ListBillDetail&Version=2022-01-01' \
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
        "RequestId": "202404021750412C935003765CD553A37B",
        "Action": "ListBillDetail",
        "Version": "20220101",
        "Service": "billing"
    },
    "Result": {
        "List": [
            {
                "BillPeriod": "2024-02",
                "ExpenseDate": "2024-02-29",
                "PayerID": "2100153894",
                "PayerUserName": "Doooo",
                "PayerCustomerName": "北京火山引擎科技有限公司",
                "Product": "volume",
                "ProductZh": "弹性块存储",
                "BillingMode": "按量计费",
                "BillCategory": "消费-使用",
                "InstanceNo": "vol-50mgf1r2g7l6hswihmfg",
                "Region": "华北2（北京）",
                "Zone": "可用区B",
                "OriginalBillAmount": "0.042000",
                "DiscountBillAmount": "0.01",
                "PayableAmount": "0.01",
                "Currency": "CNY"
            }
        ],
        "Total": 7721,
        "Limit": 1,
        "Offset": 0
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | RequestInvalid | Request Invalid | 请求参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
