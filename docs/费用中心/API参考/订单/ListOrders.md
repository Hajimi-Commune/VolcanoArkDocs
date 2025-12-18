# ListOrders - 批量查询订单信息

批量查询订单信息。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListOrders。 | ListOrders |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| NextToken | string | 否 | 指定下一次查询的起始位，用于翻页 | "MjAwMDAwMDAyNTk3MDkzMDg0OA==" |
| MaxResults | integer | 否 | 单次查询的页大小，缺省值为10，范围支持[0,100] | 10 |
| CreateTimeStart | string | 否 | 创建时间段起（缺省查询当前时间最近 1 小时内订单），格式：YYYY-MM-DD'T'HH:MM:SS'Z' | 2024-06-01T14:00:00+08:00 |
| CreateTimeEnd | string | 否 | 创建时间段止（缺省查询当前时间最近 1 小时内订单），格式：YYYY-MM-DD'T'HH:MM:SS'Z'，最大支持时间间隔31天，开始&结束时间必须同时有值或无值 | 2024-06-01T15:00:00+08:00 |
| OrderType | string | 否 | 订单类型：新购：Purchase；试用：Trial；更配：Modify；续费：Renew；转正：Formalize；退订：Unsubscribed；预留实例调整：RIAdjustment；临时升配：TempUpgrade；费用调整：CostAdjustment | Purchase |
| Status | string | 否 | 订单状态：待支付：UnPaid；已支付：Paid；已取消：Closed；支付中：Paying；已退款：Refunded；退款中：Refunding；退款失败：RefundFail；部分退款：PartialRefunded | UnPaid |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| OrderInfos | object[] | 订单信息集合 | - |
| NextToken | string | 下一次查询的起始位 | "MjAwMDAwMDAyNTk3MDkzMDg0OA==" |
| MaxResults | integer | 查询的页大小 | 10 |

### OrderInfos 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| OrderID | string | 订单ID |
| Status | string | 订单状态 |
| CreateTime | string | 创建时间 |
| OrderType | string | 订单类型 |
| SubjectNo | string | 服务主体编号 |
| SellerID | integer | 卖方ID |
| SellerCustomerName | string | 卖方客户名称 |
| PayerID | integer | 付款方ID |
| PayerCustomerName | string | 付款方客户名称 |
| BuyerID | integer | 买方ID |
| BuyerCustomerName | string | 买方客户名称 |
| OriginalAmount | string | 原价金额 |
| DiscountAmount | string | 折扣金额 |
| CouponAmount | string | 代金券金额 |
| PayableAmount | string | 应付金额 |
| PaidAmount | string | 已付金额 |

## 请求示例

```http
POST /?Action=ListOrders&Version=2022-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240701T032229Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240701/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "NextToken": "",
    "MaxResults": 10,
    "CreateTimeStart": "2024-06-01T14:00:00+08:00",
    "CreateTimeEnd": "2024-06-01T15:00:00+08:00",
    "OrderType": "Purchase",
    "Status": "UnPaid"
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "20240618204053152208253056FCA4F5",
        "Action": "ListOrders",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "OrderInfos": [
            {
                "OrderID": "Order123456",
                "Status": "Paid",
                "CreateTime": "2024-06-01T12:00:00+08:00",
                "OrderType": "Purchase",
                "SubjectNo": "3423",
                "SellerID": 3423,
                "SellerCustomerName": "北京火山引擎科技有限公司",
                "PayerID": 123456789,
                "PayerCustomerName": "测试账号",
                "BuyerID": 123456789,
                "BuyerCustomerName": "测试账号",
                "OriginalAmount": "10000.00",
                "DiscountAmount": "8000.00",
                "CouponAmount": "600.00",
                "PayableAmount": "1400.00",
                "PaidAmount": "1400.00"
            }
        ],
        "NextToken": "10000",
        "MaxResults": 10
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
