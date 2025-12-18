# QueryBalanceAcct - 查询用户账户余额信息

查询账户余额信息。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：QueryBalanceAcct。 | QueryBalanceAcct |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| AccountID | integer | 账号ID | 21000000X |
| ArrearsBalance | string | 欠费金额 | 1.01 |
| AvailableBalance | string | 可用余额 | 1.01 |
| CashBalance | string | 现金余额 | 1.01 |
| CreditLimit | string | 信控额度 | 1.01 |
| FreezeAmount | string | 冻结金额 | 1.01 |

## 请求示例

```http
GET /?Action=QueryBalanceAcct&Version=2022-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Authorization: Basic QUtUQWIwQTRhxxxxxxxx
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202308231151163C400BE8545DED89B87D",
        "Action": "QueryBalanceAcct",
        "Version": "2022-01-01",
        "Service": "billing"
    },
    "Result": {
        "AccountID": 210xxxxxxx,
        "ArrearsBalance": "1.01",
        "AvailableBalance": "77.01",
        "CashBalance": "83.01",
        "CreditLimit": "0.01",
        "FreezeAmount": "5.01"
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 500 | InternalError | The request has failed due to an unknown error | 系统未知异常，请重试 |
| 400 | RecordNoFound | The Record No Found. | 查询无记录 |
| 400 | InvalidParam | The parameter is invalid | 请求参数非法 |
