# QueryBudgetDetail - 查询预算详情

查询预算详情

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | QueryBudgetDetail | 要执行的操作，取值：QueryBudgetDetail。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| BudgetID | String | 是 | bg7561497585933242668 | 预算ID |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| Budget | Object | {} | 预算信息 |
| BudgetRange | Object | {} | 预算范围 |
| BudgetAmount | Array of Object | [] | 预算金额列表 |
| BudgetAlertRule | Array of Object | [] | 报警阈值规则列表 |
| BudgetAlertMessage | Array of Object | [] | 报警发送人列表 |

## 请求示例

```json
{
    "BudgetID": "bg7561497585933242668"
}
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202510161515020F4B595169ADB5D5A63D",
        "Action": "QueryBudgetDetail",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "Budget": {
            "BudgetID": "bg7561497585933242668",
            "BudgetName": "test_budget",
            "BudgetType": "cost_budget",
            "BudgetPlanType": "fixed_amount",
            "Period": "month",
            "BudgetStartTime": "2025-03",
            "BudgetEndTime": "2025-12",
            "Status": "active"
        },
        "BudgetRange": {
            "Tag": [],
            "PayerID": [2100266035],
            "Region": ["R000807"],
            "OwnerID": [2100266035],
            "BillingMode": ["1"],
            "Project": ["project_test"],
            "Product": ["CDN"],
            "Zone": [],
            "SubjectNo": []
        },
        "BudgetAmount": [
            {
                "BudgetPeriod": "2025-10",
                "BudgetAmount": "100"
            }
        ],
        "BudgetAlertRule": [
            {
                "BudgetAlertItem": "actual_amount",
                "BudgetAlertThresholdType": "amount",
                "BudgetAlertThreshold": "200"
            }
        ],
        "BudgetAlertMessage": [
            {
                "EmailSendSwitch": 0,
                "MessageSendSwitch": 0,
                "InternalSendSwitch": 0,
                "RecipientID": 123456
            }
        ]
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The required parameter %s is missing. | 参数缺失 |
| 400 | InvalidParameter.BudgetNotExist | Budget not exist. | 预算不存在 |
| 500 | InternalServerError | The request has failed due to an unknown error. | 系统错误，多次出现时请联系管理员 |
