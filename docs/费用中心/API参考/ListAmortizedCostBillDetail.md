# ListAmortizedCostBillDetail - 分页查询成本账单明细

分页查询成本账单明细

## 调试

API Explorer

您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=ListAmortizedCostBillDetail&groupName=%E8%B4%A6%E5%8D%95%E4%B8%AD%E5%BF%83&serviceCode=billing&version=2022-01-01)

## 请求参数

下方仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数名称 | 类型 | 是否必选 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | string | 必选 | ListAmortizedCostBillDetail | 要执行的操作，取值：ListAmortizedCostBillDetail。 |
| Version | string | 必选 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| AmortizedMonth | string | 必选 | 2023-08 | 成本账期：格式为YYYY-MM；仅支持单月查询 |
| Limit | integer | 必选 | 10 | 数量：[1-300] |
| Offset | integer | 可选 | 0 | 偏移量 |
| OwnerID | long[] | 可选 | [2100057673] | Owner账号ID |
| PayerID | long[] | 可选 | [2100057673] | Payer账号ID |
| Product | string[] | 可选 | [ECS] | 产品名称，默认不选为全部 |
| InstanceNo | string | 可选 | - | 实例号 |
| AmortizedType | string | 可选 | - | 分摊类型 |
| NeedRecordNum | integer | 可选 | 1 | 是否需要访问列表的总记录数：1：表示需要；0：表示不需要 |

## 返回参数

下方仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数名称 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | object[] | - | 成本账单明细列表 |
| List.AmortizedMonth | string | 2023-08 | 成本账期 |
| List.PayerID | string | 2100153894 | 支付账号ID |
| List.OwnerID | string | 2100153894 | Owner账号ID |
| List.Product | string | ECS | 产品英文名称 |
| List.ProductZh | string | 云服务器 | 产品中文名称 |
| List.InstanceNo | string | i-xxxxx | 实例号 |
| List.InstanceName | string | test-instance | 实例名称 |
| List.AmortizedType | string | - | 分摊类型 |
| List.BillPeriod | string | 2023-08 | 账期 |
| List.OriginalAmortizedAmount | string | 168.740000 | 分摊原价 |
| List.DiscountAmortizedAmount | string | 19.68 | 分摊折后价 |
| List.CouponAmortizedAmount | string | 0.00 | 分摊代金券 |
| List.PayableAmortizedAmount | string | 19.68 | 分摊应付金额 |
| Total | integer | 100 | 总数 |
| Limit | integer | 100 | 步长 |
| Offset | integer | 0 | 偏移量 |

## 请求示例

```text
curl --location 'https://open.volcengineapi.com?Action=ListAmortizedCostBillDetail&Version=2022-01-01' \
--header 'AccessKey: AK***mE' \
--header 'SecretKey: T0***RQ==' \
--header 'Region: cn-north-1' \
--header 'ServiceName: billing' \
--header 'Content-Type: application/json' \
--header 'User-Agent: volcengine-go-sdk' \
--data '{
    "AmortizedMonth": "2024-01",
    "Limit": 10,
    "Offset": 0,
    "NeedRecordNum": 1
}'
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202302151539373422AB4D3113FA66F8E3",
        "Action": "ListAmortizedCostBillDetail",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1"
    },
    "Result": {
        "List": [
            {
                "AmortizedMonth": "2022-10",
                "PayerID": "2000010593",
                "OwnerID": "2000010593",
                "Product": "ECS",
                "ProductZh": "云服务器",
                "InstanceNo": "i-xxxxx",
                "InstanceName": "test-instance",
                "AmortizedType": "按天分摊",
                "BillPeriod": "2022-10",
                "OriginalAmortizedAmount": "168.740000",
                "DiscountAmortizedAmount": "158449.68",
                "CouponAmortizedAmount": "0.00",
                "PayableAmortizedAmount": "158449.68"
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
