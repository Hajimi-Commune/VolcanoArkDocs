# OpenAPI 调用说明

> **注意**：本文档不再维护且将与近期下线，查看最新账单OpenAPI调用说明文档请点击跳转至[费用中心-API调用说明](../../API参考/API调用说明.md)。

本文介绍了调用火山引擎费用中心 - 账单管理 OpenAPI 的调用方法，对于具体 OpenAPI 接口的介绍，请参考[OpenAPI 接口列表](OpenAPI接口列表.md)。

## 一、前提条件

当前我们提供了通过 http 请求直接调用和通过火山引擎 SDK 调用两种方式来使用我们提供的服务，这两种方式都需要事先进行以下操作：

- 通过控制台注册账号，获取对应的 AccessKey ID 和 AccessKey Secret（AK/SK），用于 API 请求鉴权。可通过对应环境的 **控制台 > 账号头像 > 密钥管理** 页面获取。
- 开通火山引擎服务，并确保使用的账号具有对应资源的访问权限。

## 二、通过 SDK 调用 OpenAPI 接口

当前已支持 Golang、Java、Python、PHP 和 NodeJS 这五种编程语言的 SDK。这些 SDK 封装了 API 请求时的鉴权过程，可以节省开发者自行编写鉴权代码的时间和精力，也是我们所推荐使用的。五种语言 SDK 的链接如下：

- [火山引擎 Golang SDK](https://github.com/volcengine/volc-sdk-golang/tree/main/example/billing)
- [火山引擎 Java SDK](https://github.com/volcengine/volc-sdk-java/tree/main/example/src/main/java/com/volcengine/example/billing)
- [火山引擎 Python SDK](https://github.com/volcengine/volc-sdk-python/tree/main/volcengine/example/billing)
- [火山引擎 PHP SDK](https://github.com/volcengine/volc-sdk-php/tree/main/examples/Billing)
- [火山引擎 NodeJS SDK](https://github.com/volcengine/volc-sdk-nodejs/tree/main/src/services/billing)

火山引擎账单管理 OpenAPI 的服务名为 **billing**，相关 SDK 代码主要由两部分组成，分别是业务实现代码（一般位于 Service 目录下）和调用样例代码（一般位于 Example 目录下）。用户可以首先根据 README.md 文件中的介绍下载安装我们提供的 SDK，然后仿照我们提供的调用样例代码编写代码，调用火山引擎费用中心 - 账单管理 OpenAPI 接口。

## 三、OpenAPI 调用介绍

当我们没有提供用户所用开发语言对应的 SDK，或者用户有定制化的需求时，也可以通过 http 请求的方式直接调用火山引擎费用中心 - 账单管理 OpenAPI。具体介绍如下。

### 1. 请求结构

调用火山引擎费用中心 - 账单管理 OpenAPI 接口时，是通过向指定服务地址发送请求，并需满足签名信息和具体接口的业务信息来完成的。API 的请求需要包含服务地址、请求方式、请求参数等信息。接口调用成功后，对返回结果进行解析。

以创建空间接口为例，一条未编码的URL请求示例如下：

```
https://billing.volcengineapi.com?Action=ListBill &Version=2022-01-01 &<公共请求参数> &<请求体>
```

其中：

- `https` 指定了请求通信协议。
- `billing.volcengineapi.com` 指定了服务接入地址（Endpoint）。
- `Action=ListBill` 指定了要调用的 API。
- `Version=2022-01-01` 指定了 API 接口版本。
- `<公共请求参数>` 是系统规定的公共参数。
- `<请求体>` 是调用 API 接口需要的业务参数。

发送的 API 请求需要服务地址、请求方式、请求参数等信息。调用成功后，对返回结果进行解析。

#### (1) 服务地址

火山引擎费用中心 - 账单管理 OpenAPI 的服务接入地址如下：

| 服务地域 | 域名 | 备注 |
| --- | --- | --- |
| 国内 | https://billing.volcengineapi.com | 火山引擎 |

#### (2) 通信协议

火山引擎费用中心 - 账单管理提供的所有接口均通过 HTTPS 进行通信，提供高安全性的通信通道。

#### (3) 请求方式

根据各个接口的具体需求，选择 GET 或 POST 方式发起请求。

#### (4) 请求参数

在发起请求时，请求体中可能会包含两类参数：公共请求参数和接口特有的业务参数。

- 公共请求参数是每一个接口需要包含的，具体可参见公共请求参数小节。
- 接口特有的业务参数是各个接口特有的，详见各个接口的描述。

#### (5) 字符编码

请求及返回结果使用 UTF-8 的字符集进行编码。

### 2. 公共请求参数

公共请求参数是每个接口都需要使用的请求参数，在发送请求时都需要携带这些公共请求参数，否则会导致请求失败。公共请求参数首字母均为大写，以此区分接口特有的请求参数。

调用火山引擎费用中心 - 账单管理服务 OpenAPI 需要的公共请求参数如下：

| 名称 | 位置 | 类型 | 是否必填 | 描述 | 示例值 |
| --- | --- | --- | --- | --- | --- |
| Action | Query | String | 是 | 接口名称，与具体的接口相关，表示要执行的操作 | ListBill |
| Version | Query | String | 是 | 接口版本 | 2022-01-01 |
| X-Date | Header | String | 是 | 发送请求的时间戳 | 20201103T104027Z，使用UTC时间，精确到秒 |
| Authorization | Header | String | 是 | 发送的请求中包含的签名 | HMAC-SHA256 Credential={AccessKey}/{ShortDate}/{Region}/{Service}/request, SignedHeaders={SignedHeaders}, Signature={Signature}。关于签名参数的生成方法，参考火山引擎 OpenAPI 公共参数的详细说明 |
| Content-Type | Header | String | 否 | 资源的 MIME 类型 | application/x-www-form-urlencoded; charset=utf-8 |

有关火山引擎 OpenAPI 公共参数的详细说明，请参考[公共参数](https://www.volcengine.com/docs/6369/67268/)。

### 3. 签名机制

对于每一次 HTTPS 协议请求，会根据访问中的签名信息验证访问请求者的身份。具体由用户的火山引擎账号对应的 AccessKey ID 和 AccessKey Secret（AK/SK）对称加密验证实现。

具体的签名机制和方法，参考火山引擎 OpenAPI [签名机制](https://www.volcengine.com/docs/6369/67265)。

### 4. 返回结果

#### (1) 返回结果结构体

API 请求可返回以下结果：

| 字段 | 类型 | 是否一定返回 | 说明 |
| --- | --- | --- | --- |
| ResponseMetadata | Array | 是 | 响应元数据；需通过解析该结构体，判断响应结果，参考以下 ResponseMetadata 结构体说明 |
| Result | Any | 否 | 响应数据；调用成功时返回 |

**ResponseMetadata 结构体说明**

| 字段 | 类型 | 是否一定返回 | 说明 |
| --- | --- | --- | --- |
| RequestId | String | 是 | 请求 ID |
| Action | String | 是 | OpenAPI 接口名称 |
| Version | String | 是 | OpenAPI 接口版本 |
| Service | String | 是 | OpenAPI 服务名称 |
| Region | String | 是 | 服务所在地域信息 |
| Error | Array | 否 | 错误信息；调用失败时返回，参考以下 Error 结构体说明 |

**Error 结构体说明**

| 字段 | 类型 | 是否一定返回 | 说明 |
| --- | --- | --- | --- |
| CodeN | Int64 | 是 | 状态码 |
| Code | String | 是 | 状态消息 |
| Message | String | 是 | 具体的错误描述信息 |

#### (2) 返回结果示例

**调用成功**

```json
{
  "ResponseMetadata":{
    "RequestId":"202208121125500102251460630633D4C2",
    "Action":"ListBill",
    "Version":"20220101",
    "Service":"billing",
    "Region":"cn-north-1"
  },
  "Result":{
    "List":[
      {
        "BillPeriod":"2022-03",
        "PayerID":"2100052604",
        "PayerUserName":"lmh_test12",
        "PayerCustomerName":"payerCustomerName",
        "OwnerID":"2100052604",
        "OwnerUserName":"lmh_test12",
        "OwnerCustomerName":"ownerCustomerName",
        "Product":"auto_test_product",
        "ProductZh":"自动化测试用产品",
        "BusinessMode":"普通业务",
        "BillingMode":"1",
        "ExpenseBeginTime":"2022-03-01 20:11:07",
        "ExpenseEndTime":"",
        "TradeTime":"2022-03-01 20:11:07",
        "BillID":"Order7070081209812701484",
        "BillCategoryParent":"消费",
        "OriginalBillAmount":"100.000000",
        "PreferentialBillAmount":"0.000000",
        "RoundBillAmount":"0.000000",
        "DiscountBillAmount":"100.00",
        "CouponAmount":"0.00",
        "PayableAmount":"100.00",
        "PaidAmount":"100.00",
        "UnpaidAmount":"0.00",
        "Currency":"CNY",
        "PayStatus":"已结清"
      }
    ],
    "Total":70,
    "Limit":10,
    "Offset":0
  }
}
```

**调用失败**

```json
{
  "ResponseMetadata": {
    "RequestId": "20220812112024010133031077101D7CBB",
    "Action": "ListBill",
    "Version": "2022-01-01",
    "Service":"billing",
    "Region":"cn-north-1",
    "Error": {
      "CodeN": 100024,
      "Code": "InvalidAuthorization",
      "Message": "Invalid 'Authorization' header, Pls check your authorization header."
    }
  }
}
```
