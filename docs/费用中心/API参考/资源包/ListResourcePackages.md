# ListResourcePackages - 查询资源包列表

查询资源包、预留实例劵、预留存储容量包的信息。

## 注意事项

- 最多支持查询失效时间在18个月内的数据，超过不会返回数据。
- MaxResults 数据查询每页不超过 20。
- 查询QPS限制为10QPS，超过会报错。

## 请求参数

| 参数名称 | 类型 | 是否必选 | 描述 | 示例值 |
| --- | --- | --- | --- | --- |
| Action | string | 是 | 要执行的操作，取值：ListResourcePackages。 | ListResourcePackages |
| Version | string | 是 | API的版本，取值：2022-01-01。 | 2022-01-01 |
| ResourceType | string | 是 | 查询的资源实例类型。枚举字段：Package: 资源包；RI：预留实例劵；RSC：预留存储容量包； | Package |
| MaxResults | string | 是 | 每次查询返回最大条数，上限20。 | 20 |
| NextToken | string | 否 | 下一页开始ID，首次查询填空""，后续翻页返回中会携带下一次起始ID，当返回为空时，表示后续无数据。 | 1000 |
| Product | string | 否 | 资源实例对应商品编码 | CDN |
| EffectiveTimeBegin | string | 否 | 资源实例生效开始时间。日期格式按照 ISO8601 标准表示，并需要使用 UTC 时间。格式为：yyyy-MM-ddTHH:mm:ssZ。 | 2022-02-02T12:00:00Z |
| EffectiveTimeEnd | string | 否 | 资源实例生效结束时间。日期格式按照 ISO8601 标准表示，并需要使用 UTC 时间。格式为：yyyy-MM-ddTHH:mm:ssZ。 | 2025-02-02T12:00:00Z |
| Status | string | 否 | 资源实例状态：NotEffective：未生效；Effective：生效中；FailedToCreate：创建失败；UsedUp：已用完；Expired：已到期；Refunded：已退款； | Effective |

## 响应参数

| 参数名称 | 类型 | 描述 | 示例值 |
| --- | --- | --- | --- |
| List | object[] | 资源实例信息列表 | - |
| NextToken | string | 下一页起始ID | 100 |

### List 对象字段说明

| 字段名称 | 类型 | 描述 |
| --- | --- | --- |
| OwnerID | string | 拥有者账号ID |
| UserName | string | 用户名 |
| PackageType | string | 资源包类型 |
| InstanceNo | string | 实例编号 |
| InstanceName | string | 实例名称 |
| Product | string | 产品代码 |
| ProductName | string | 产品名称 |
| TotalAmount | string | 总量 |
| AvailableAmount | string | 可用量 |
| Unit | string | 单位 |
| EffectiveTime | string | 生效时间 |
| ExpiryTime | string | 到期时间 |
| BillTime | string | 计费时间 |
| Status | string | 状态 |
| ConfigurationCode | string | 配置代码 |
| ConfigurationName | string | 配置名称 |
| Specification | string | 规格 |
| SpecificationUnit | string | 规格单位 |
| RegionCode | string | 地域代码 |
| ZoneCode | string | 可用区代码 |
| SpecCalculateFactor | string | 规格计算因子 |
| ResetPeriod | string | 重置周期 |
| ResetByNaturalMonth | string | 是否按自然月重置 |
| SubjectNo | string | 服务主体名称 |

## 请求示例

```http
POST /?Action=ListResourcePackages&Version=2022-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240820T092339Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240820/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "ResourceType": "Package",
    "MaxResults": "20",
    "NextToken": ""
}
```

## 响应示例

```json
{
    "ResponseMetadata": {
        "RequestId": "20240823183418140163168173B94778",
        "Action": "ListResourcePackages",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "List": [
            {
                "OwnerID": "2100000001",
                "UserName": "user",
                "PackageType": "Periodic",
                "InstanceNo": "Instance7365828935959969836",
                "InstanceName": "资源包实例1",
                "Product": "CDN",
                "ProductName": "内容分发网络",
                "TotalAmount": "500",
                "AvailableAmount": "499",
                "Unit": "GB",
                "EffectiveTime": "2022-02-02T12:00:00Z",
                "ExpiryTime": "2025-02-02T12:00:00Z",
                "BillTime": "2022-02-02T12:00:00Z",
                "Status": "Effective",
                "ConfigurationCode": "CDN_mainland_500GB_1m_package",
                "ConfigurationName": "国内CDN-500GB-1月有效期",
                "Specification": "500",
                "SpecificationUnit": "GB",
                "RegionCode": "cn-beijing",
                "ZoneCode": "cn-beijing-a",
                "SpecCalculateFactor": "1",
                "ResetPeriod": "monthly",
                "ResetByNaturalMonth": "true",
                "SubjectNo": "北京火山引擎科技有限公司"
            }
        ],
        "NextToken": "100"
    }
}
```

## 错误码

| HttpCode | 错误码 | 错误信息 | 错误描述 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
