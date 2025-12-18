# ListRecipientInformation - 查询报警接收人信息

查询报警接收人信息

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | ListRecipientInformation | 要执行的操作，取值：ListRecipientInformation。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | Array of Object | [] | 报警接收人列表 |

### List 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| RecipientID | Long | 接收人ID |
| RecipientName | String | 接收人姓名 |
| RecipientEmail | String | 接收人邮箱 |
| RecipientPhone | String | 接收人手机号 |
| RecipientType | String | 接收人类型 |

## 请求示例

```text
POST /?Action=ListRecipientInformation&Version=2022-01-01 HTTP/1.1
Host: billing.volcengineapi.com
Content-Type: application/json
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "202510161525020F4B595169ADB5D5A63F",
        "Action": "ListRecipientInformation",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "List": [
            {
                "RecipientID": 123456,
                "RecipientName": "张三",
                "RecipientEmail": "zhangsan@example.com",
                "RecipientPhone": "13800138000",
                "RecipientType": "admin"
            }
        ]
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 500 | InternalServerError | The request has failed due to an unknown error. | 系统错误，多次出现时请联系管理员 |
