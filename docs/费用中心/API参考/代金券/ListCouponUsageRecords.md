# ListCouponUsageRecords - 查询代金券核销记录

查询代金券核销记录

## 注意事项

- 查询QPS限制为10QPS超过会报错。

## 调试

API Explorer：您可以通过 API Explorer 在线发起调用，无需关注签名生成过程，快速获取调用结果。

[去调试](https://api.volcengine.com/api-explorer/?action=ListCouponUsageRecords&groupName=%E4%BB%A3%E9%87%91%E5%88%B8&serviceCode=billing&version=2022-01-01)

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | ListCouponUsageRecords | 要执行的操作，取值：ListCouponUsageRecords。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| Limit | Integer | 否 | 10 | 每页返回的数据量（默认是10条，最大不超过1000） |
| Offset | Integer | 否 | 0 | 偏移量 |
| CouponID | String | 是 | C1L9L8FMR2HR1O | 代金券ID |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | Array of Object | [] | 代金券核销记录列表 |
| Total | Integer | 10 | 总数 |
| Limit | Integer | 10 | Limit |
| Offset | Integer | 0 | Offset |

### List 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| AccountID | Long | 账号ID |
| ChangeAmount | Number | 变更金额 |
| ChangeType | Integer | 变更类型 |
| CouponID | String | 代金券ID |
| CreatedTime | String | 创建时间 |
| PayType | String | 支付类型 |
| ProductCode | String | 产品代码 |
| ProductName | String | 产品名称 |
| SubBusinessID | String | 子业务ID |
| UserAccountID | Long | 用户账号ID |

## 请求示例

```text
POST /open-apis-v2/coupon?Action=ListCouponUsageRecords&Version=2022-01-01 HTTP/1.1
Host: 
Accept: application/json
Accept-Encoding: gzip
Authorization: ******
Content-Length: 51
Content-Type: application/json; charset=utf-8

{
    "CouponID":"C1L9L8FMR2HR1O",
    "Limit":10,
    "Offset":0
}
```

## 返回示例

```json
{
    "Metadata": {
        "RequestId": "2025081319231647CDA36777B03C062F6F",
        "Action": "ListCouponUsageRecords",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-north-1",
        "HTTPCode": 200,
        "Error": null
    },
    "Limit": 10,
    "List": [
        {
            "AccountID": 2000010593,
            "ChangeAmount": 0.000001,
            "ChangeType": 3,
            "CouponID": "C1L9L8FMR2HR1O",
            "CreatedTime": "2025-08-13T19:09:14+08:00",
            "PayType": "post",
            "ProductCode": "YJQceshi",
            "ProductName": "YJQ测试",
            "SubBusinessID": "zdytest_syncuse_20250812_7",
            "UserAccountID": 2000010593
        }
    ],
    "Offset": 0,
    "Total": 1
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
