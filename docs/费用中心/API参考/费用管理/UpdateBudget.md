# UpdateBudget - 更新预算

更新预算

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | UpdateBudget | 要执行的操作，取值：UpdateBudget。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| BudgetID | String | 是 | bg7561497585933242668 | 预算ID |
| Budget | Object | 否 | 见下方示例 | 预算信息（需要更新的字段） |
| BudgetRange | Object | 否 | 见下方示例 | 预算范围（需要更新的字段） |
| BudgetAmount | Array of Object | 否 | 见下方示例 | 预算金额（需要更新的字段） |
| BudgetAlertRule | Array of Object | 否 | 见下方示例 | 报警阈值规则（需要更新的字段） |
| BudgetAlertMessage | Array of Object | 否 | 见下方示例 | 报警发送人（需要更新的字段） |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| Success | Boolean | true | 是否成功 |

## 请求示例

```json
{
    "BudgetID": "bg7561497585933242668",
    "Budget": {
        "BudgetName": "updated_budget_name",
        "BudgetEndTime": "2026-12"
    },
    "BudgetAmount": [
        {
            "BudgetPeriod": "2025-10",
            "BudgetAmount": "200"
        }
    ]
}
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202510161520020F4B595169ADB5D5A63E",
        "Action": "UpdateBudget",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "Success": true
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The required parameter %s is missing. | 参数缺失 |
| 400 | InvalidParameter.BudgetNotExist | Budget not exist. | 预算不存在 |
| 400 | OperationDenied.BudgetNameExist | Budget name exist. | 预算名称已存在 |
| 400 | OperationDenied.BudgetEndTimeInvalid | Budget end time cannot be earlier than the current time. | 预算结束时间不能早于当前时间 |
| 500 | InternalServerError | The request has failed due to an unknown error. | 系统错误，多次出现时请联系管理员 |
