# CreateBudget - 创建预算

创建预算

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | CreateBudget | 要执行的操作，取值：CreateBudget。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| Budget | Object | 是 | 见下方示例 | 预算信息 |
| BudgetRange | Object | 否 | 见下方示例 | 预算范围 |
| BudgetAmount | Array of Object | 是 | 见下方示例 | 预算金额 |
| BudgetAlertRule | Array of Object | 是 | 见下方示例 | 报警阈值规则 |
| BudgetAlertMessage | Array of Object | 是 | 见下方示例 | 报警发送人 |

### Budget 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| BudgetType | String | 预算类型，取值：cost_budget（成本预算） |
| BudgetPlanType | String | 预算计划类型，取值：fixed_amount（固定金额） |
| BudgetStartTime | String | 预算开始时间，格式：yyyy-MM |
| BudgetName | String | 预算名称 |
| Period | String | 周期，取值：month（月度） |
| BudgetEndTime | String | 预算结束时间，格式：yyyy-MM |

### BudgetRange 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| Tag | Array of String | 标签筛选条件 |
| PayerID | Array of Long | 付款账号ID列表 |
| Region | Array of String | 地域列表 |
| OwnerID | Array of Long | 资源所属账号ID列表 |
| BillingMode | Array of String | 计费模式列表 |
| Project | Array of String | 项目列表 |
| Product | Array of String | 产品列表 |
| Zone | Array of String | 可用区列表 |
| SubjectNo | Array of String | 服务主体编号列表 |

### BudgetAmount 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| BudgetPeriod | String | 预算周期，格式：yyyy-MM |
| BudgetAmount | String | 预算金额 |

### BudgetAlertRule 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| BudgetAlertItem | String | 报警项目，取值：actual_amount（实际金额） |
| BudgetAlertThresholdType | String | 阈值类型，取值：amount（金额）、percent（百分比） |
| BudgetAlertThreshold | String | 报警阈值 |

### BudgetAlertMessage 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| EmailSendSwitch | Integer | 邮件发送开关，0-关闭，1-开启 |
| MessageSendSwitch | Integer | 短信发送开关，0-关闭，1-开启 |
| InternalSendSwitch | Integer | 站内信发送开关，0-关闭，1-开启 |
| RecipientID | Long | 接收人ID |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| BudgetID | String | bg7561497585933242668 | 创建成功的预算ID |

## 请求示例

```json
{
    "Budget": {
        "BudgetType": "cost_budget",
        "BudgetPlanType": "fixed_amount",
        "BudgetStartTime": "2025-03",
        "BudgetName": "test_budget",
        "Period": "month",
        "BudgetEndTime": "2025-12"
    },
    "BudgetRange": {
        "Tag": ["{\"voKey5\":[\"vhh\"]}"],
        "PayerID": [2100266035],
        "Region": ["R000807"],
        "OwnerID": [2100266035],
        "BillingMode": ["1"],
        "Project": ["project_test"],
        "Product": ["CDN"],
        "Zone": ["{\"R000807\":\"\"}"],
        "SubjectNo": ["3423"]
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
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202510161501020F4B595169ADB5D5A63A",
        "Action": "CreateBudget",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "BudgetID": "bg7561497585933242668"
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The required parameter %s is missing. | 参数缺失 |
| 400 | OperationDenied.ReachedBudgetCreateNumberLimit | Reached budget create number limit 100. | 预算报警创建数量已抵达阈值100 |
| 400 | OperationDenied.NoneAvailableReceivers | None available alert receiver. | 没有设置报警接收人 |
| 400 | OperationDenied.BudgetNameExist | Budget name exist. | 预算名称已存在 |
| 400 | OperationDenied.BudgetEndTimeInvalid | Budget end time cannot be earlier than the current time. | 预算结束时间不能早于当前时间 |
| 400 | InvalidParameter | The parameter %s not in %s. | 参数不符合枚举规范 |
| 400 | OperationDenied.InvalidRecipientID | InvalidRecipientID %d not belong .response | 消息接收人ID不在ListRecipientInformation接口的返回集中。 |
| 500 | InternalServerError | The request has failed due to an unknown error. | 系统错误，多次出现时请联系管理员 |
