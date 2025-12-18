# ListBudget - 查询预算列表

查询预算列表

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | ListBudget | 要执行的操作，取值：ListBudget。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| Limit | Integer | 否 | 10 | 每页返回的数据量（默认是10条，最大不超过100） |
| Offset | Integer | 否 | 0 | 偏移量 |
| BudgetName | String | 否 | test_budget | 预算名称，支持模糊搜索 |
| BudgetID | String | 否 | bg7561497585933242668 | 预算ID |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | Array of Object | [] | 预算列表 |
| Total | Integer | 10 | 总数 |
| Limit | Integer | 10 | Limit |
| Offset | Integer | 0 | Offset |

### List 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| BudgetID | String | 预算ID |
| BudgetName | String | 预算名称 |
| BudgetType | String | 预算类型 |
| BudgetPlanType | String | 预算计划类型 |
| Period | String | 周期 |
| BudgetStartTime | String | 预算开始时间 |
| BudgetEndTime | String | 预算结束时间 |
| Status | String | 预算状态 |
| CreatedTime | String | 创建时间 |
| UpdatedTime | String | 更新时间 |

## 请求示例

```json
{
    "Limit": 10,
    "Offset": 0,
    "BudgetName": "test"
}
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202510161505020F4B595169ADB5D5A63B",
        "Action": "ListBudget",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "List": [
            {
                "BudgetID": "bg7561497585933242668",
                "BudgetName": "test_budget",
                "BudgetType": "cost_budget",
                "BudgetPlanType": "fixed_amount",
                "Period": "month",
                "BudgetStartTime": "2025-03",
                "BudgetEndTime": "2025-12",
                "Status": "active",
                "CreatedTime": "2025-10-16T15:01:02+08:00",
                "UpdatedTime": "2025-10-16T15:01:02+08:00"
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
| 400 | InvalidParameter | The parameter %s is invalid. | 参数无效 |
| 500 | InternalServerError | The request has failed due to an unknown error. | 系统错误，多次出现时请联系管理员 |
