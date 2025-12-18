# CleanUpFinancialRelation - 删除企业财务关联记录

删除企业财务关联记录

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | CleanUpFinancialRelation | 要执行的操作，取值：CleanUpFinancialRelation。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| RelationID | String | 是 | rel7083374554428477740 | 关系ID |
| SubAccountID | Long | 是 | 2100000253 | 子账号 |
| Relation | Integer | 是 | 1 | 关系类型 |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| IsIdempotent | Boolean | true | 是否为幂等请求 |
| IsSuccess | Boolean | true | 是否成功 |

## 请求示例

```text
{
    "RelationID": "rel7473054407572082950",
    "SubAccountID": 2100098001,
    "Relation": 1
}
```

## 返回示例

```text
{
    "ResponseMetadata": {
        "RequestId": "20250219190507117250153250D23682",
        "Action": "CleanUpFinancialRelation",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "IsSuccess": true
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
