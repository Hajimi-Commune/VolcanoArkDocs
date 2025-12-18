# ListPackageUsageDetails - 查询资源包抵扣明细列表

查询资源包、预留实例劵、预留存储容量包的抵扣明细

## 注意事项

- 最多支持查询抵扣时间在18个月内的数据，超过会报错。
- MaxResults 数据查询每页不超过 50。
- 查询QPS限制 为10 QPS 超过会报错。

## 请求说明

- 数据按照抵扣时间降序排序。
- NextToken 首次查询传 "" , 后续如果仍有数据将返回下一页起始ID 用于下次查询。
- 后续列表无数据返回时，表示本次请求结束。

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

| 参数 | 类型 | 是否必填 | 示例值 | 描述 |
| --- | --- | --- | --- | --- |
| Action | String | 是 | ListPackageUsageDetails | 要执行的操作，取值：ListPackageUsageDetails。 |
| Version | String | 是 | 2022-01-01 | API的版本，取值：2022-01-01。 |
| ResourceType | String | 是 | Package | 查询抵扣的资源实例类型。枚举字段：Package: 资源包；RI：预留实例劵；RSC：预留存储容量包； |
| DeductBeginTime | String | 是 | 2022-02-02T12:00:00Z | 抵扣开始时间(左闭右开)。日期格式按照 ISO8601 标准表示，并需要使用 UTC 时间。格式为：yyyy-MM-ddTHH:mm:ssZ。 |
| DeductEndTime | String | 是 | 2025-02-02T12:00:00Z | 抵扣结束时间（左闭右开）。日期格式按照 ISO8601 标准表示，并需要使用 UTC 时间。格式为：yyyy-MM-ddTHH:mm:ssZ。 |
| MaxResults | String | 是 | 50 | 每次查询返回最大条数，上限50。 |
| NextToken | String | 否 | "1000" | 下一页开始ID,首次查询填空""，后续翻页返回中会携带下一次起始ID，当返回为空时，表示后续无数据。 |
| InstanceNo | String | 否 | PackageProduct7372449512270004268 | 抵扣的资源实例ID。 |

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

| 参数 | 类型 | 示例值 | 描述 |
| --- | --- | --- | --- |
| List | Array of Object | [] | 抵扣明细列表 |
| NextToken | String | 1200 | 下一页起始ID |

### List 对象结构

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| OwnerID | String | 资源所属账号ID |
| UserName | String | 用户名 |
| Product | String | 产品代码 |
| ProductName | String | 产品名称 |
| ConfigurationName | String | 配置名称 |
| ConfigurationCode | String | 配置代码 |
| InstanceNo | String | 资源包实例号 |
| InstanceName | String | 资源包实例名称 |
| Specification | String | 规格 |
| SpecificationUnit | String | 规格单位 |
| RegionCode | String | 地域代码 |
| ZoneCode | String | 可用区代码 |
| PackageType | String | 资源包类型 |
| BeforeAmount | String | 抵扣前余量 |
| DeductionAmount | String | 抵扣数量 |
| AfterAmount | String | 抵扣后余量 |
| Unit | String | 单位 |
| BeginTime | String | 开始时间 |
| EndTime | String | 结束时间 |
| DeductionTime | String | 抵扣时间 |
| DeductionInstanceNo | String | 被抵扣实例号 |
| DeductionUseAmount | String | 抵扣使用量 |
| DeductionInstanceUnit | String | 抵扣实例单位 |
| DeductionSpecification | String | 抵扣规格 |
| DeductionSpecificationUnit | String | 抵扣规格单位 |
| DeductionRatio | String | 抵扣比例 |
| DeductionProduct | String | 被抵扣产品 |
| DeductionAccountID | String | 被抵扣账号ID |
| DeductionUserName | String | 被抵扣用户名 |
| DeductionCalculateFactor | String | 抵扣计算因子 |
| DeductBillingFactor | String | 抵扣计费因子 |
| DeductionElementCode | String | 抵扣元素代码 |
| DeductionRegionCode | String | 抵扣地域代码 |
| SubjectNo | String | 服务主体 |

## 请求示例

```text
POST /?Action=ListPackageUsageDetails&Version=2022-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240820T093909Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240820/cn-beijing/billing/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
    "ResourceType": "Package",
    "DeductBeginTime": "2022-02-02T12:00:00Z",
    "DeductEndTime": "2025-02-02T12:00:00Z",
    "MaxResults": "50",
    "NextToken": ""
}
```

## 返回示例

```json
{
    "ResponseMetadata": {
        "RequestId": "2024082318562600110203118586F1BE",
        "Action": "ListPackageUsageDetails",
        "Version": "2022-01-01",
        "Service": "billing",
        "Region": "cn-beijing"
    },
    "Result": {
        "List": [
            {
                "OwnerID": "2100000001",
                "UserName": "user",
                "Product": "dcdn",
                "ProductName": "全站加速",
                "ConfigurationName": "流量包-1TB",
                "ConfigurationCode": "traffic_package-1TB",
                "InstanceNo": "Package7388090431495520300",
                "InstanceName": "资源包实例1",
                "Specification": "5",
                "SpecificationUnit": "GB",
                "RegionCode": "cn-beijing",
                "ZoneCode": "cn-beijing-a",
                "PackageType": "Periodic",
                "BeforeAmount": "5",
                "DeductionAmount": "1",
                "AfterAmount": "4",
                "Unit": "GB",
                "BeginTime": "2024-07-05T19:00:00Z",
                "EndTime": "2024-07-05T20:00:00Z",
                "DeductionTime": "2024-07-05T18:21:15Z",
                "DeductionInstanceNo": "Instance7388090304050184236",
                "DeductionUseAmount": "1",
                "DeductionInstanceUnit": "GB",
                "DeductionSpecification": "100",
                "DeductionSpecificationUnit": "GB",
                "DeductionRatio": "1",
                "DeductionProduct": "CDN",
                "DeductionAccountID": "2100000001",
                "DeductionUserName": "user",
                "DeductionCalculateFactor": "2",
                "DeductBillingFactor": "类型-ESSD_PL0",
                "DeductionElementCode": "流量",
                "DeductionRegionCode": "cn-beijing",
                "SubjectNo": "北京火山引擎科技有限公司"
            }
        ],
        "NextToken": "1200"
    }
}
```

## 错误码

下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/6369/68677)文档。

| 状态码 | 错误码 | 错误信息 | 说明 |
| --- | --- | --- | --- |
| 400 | MissingParameter | The request is missing %s parameter. | 参数缺失 |
| 400 | InvalidParam | The parameter %s is invalid. | 参数无效 |
| 429 | FrequentRequest | Frequent operations, please try again later. | 操作频繁，请稍后尝试 |
| 500 | InternalError | Service has some internal Error. Pls Contact With Admin. | 服务内部异常 |
