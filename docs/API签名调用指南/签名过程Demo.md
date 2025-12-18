# 签名过程Demo

由于API签名细节比较复杂，自行写代码实现需要3-5个工作日，若您在使用过程中遇到问题，可随时通过[提交工单](https://console.volcengine.com/workorder/create?step=3&SubProductID=P90000691&FlowKey=hnobouUXRZsoWwBPZztd)或点击页面右下方的"在线客服"与我们的技术工程师沟通，由专家协助您解决问题。

## 签名自动生成

推荐使用[API Explorer](https://api.volcengine.com/api-explorer)的签名自动生成功能，只需输入ak、sk、服务地址等信息，将会自动生成请求签名和可执行的curl命令，分步骤展示签名的生成过程。

> **说明**
> - 签名串生成工具不会对输入AK、SK、host做准确性校验，请务必确保信息准确，否则生成的签名串将无法正常使用。
> - 密钥为敏感信息，请勿泄露相关数据。

## 签名Demo说明

**以下提供了签名过程的伪代码示例，方便您了解签名的计算过程。我们推荐您使用火山引擎[SDK](SDK概览.md)、[API Explorer](https://api.volcengine.com/api-explorer)来进行签名及API调用。若您仍需自行编码计算签名，我们还提供了具体语言的[签名代码](签名源码示例.md)供您参考。**

> 示例中使用的Access Key如下:
> **Access Key ID:** AKLTYWViMTVmZGYzM2E0NDI5Mzk2MDZjNjFmMjc2MjRjMzg
> **Secret Access Key:** WkRZeE1EQmxPVGhsWWpWak5HVmtNbUUxTXpZeU9UVXlOMlE1TmpZeVlqTQ==

原始请求为：

```
GET https://iam.volcengineapi.com/?Action=ListUsers&Version=2018-01-01&Limit=10&Offset=0 HTTP/1.1
Host: iam.volcengineapi.com
X-Date:20240619T071306Z
```

签名过程如下：

### 步骤1：创建规范请求

规范请求如下

```
CanonicalRequest =
  HTTPRequestMethod + '\n' +
  CanonicalURI + '\n' +
  CanonicalQueryString + '\n' +
  CanonicalHeaders + '\n' +
  SignedHeaders + '\n' +
  HexEncode(Hash(RequestPayload))
```

**HTTPRequestMethod**

```
GET
```

**CanonicalURI**

```
/
```

**CanonicalQueryString**

```
Action=ListUsers&Limit=10&Offset=0&Version=2018-01-01
```

**CanonicalHeaders**

将需要参与签名的header的key全部转成小写，然后以ASCII排序后以key-value的方式组合后换行构建。**注意需要在结尾增加一个回车换行`\n`。**

```
host:iam.volcengineapi.com
x-date:20240619T071306Z
```

**SignedHeaders**

```
host;x-date
```

**HexEncode(Hash(RequestPayload))**

无论是GET请求还是POST请求都有**RequestPayload**，其中本次GET请求中的RequestPayload是空字符串。

这里的hash算法代指：**sha256 []byte**

```
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

**最终CanonicalRequest**

```
GET
/
Action=ListUsers&Limit=10&Offset=0&Version=2018-01-01
host:iam.volcengineapi.com
x-date:20240619T071306Z

host;x-date
e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
```

### 步骤2：创建待签字符串

```
StringToSign =
  Algorithm + '\n' +
  RequestDate + '\n' +
  CredentialScope + '\n' +
  HexEncode(Hash(CanonicalRequest))
```

**Algorithm**

目前是一个固定的字符串

```
HMAC-SHA256
```

**RequestDate**

请求发起的时间，与X-Date相同。

```
20240619T071306Z
```

**CredentialScope**

指代信任状，格式为：`"${YYYYMMDD}/${region}/${service}/request"`，其中`"${YYYYMMDD}"`取X-Date中的日期，`"${region}"`和`"${service}"`参考公共参数的Region及Service取值，`request`为固定值。

此请求信息如下：

```
20240619/cn-beijing/iam/request
```

**HexEncode(Hash(CanonicalRequest))**

```
5ed5bca3905e1fcbf789abb56a17c2d819674a3bcfa468ae476bd1ea80d135cb
```

**最终StringToSign**

```
HMAC-SHA256
20240619T071306Z
20240619/cn-beijing/iam/request
5ed5bca3905e1fcbf789abb56a17c2d819674a3bcfa468ae476bd1ea80d135cb
```

### 步骤3：构建签名

> HMAC这里代指HMAC-SHA256

**Signingkey示例**

```
HMAC(HMAC(HMAC(HMAC(kSecret,"20240619"),"cn-beijing"),"iam"),"request")
```

以下示例显示了此 HMAC 哈希操作序列生成的派生签名密钥。Signingkey实际值为二进制数据，以下十六进制的字符串仅用于展示。

```
abee62e533a58934c49954459a3c3237d2fccea517c9a7c8a2651d8ea7779826
```

**Signature示例**

```
signature = HexEncode(HMAC(Signingkey, StringToSign))
```

最终的结果如下：

```
e31c4558bcfe08a286001f59cedbf0791ffd0b2362f10e55ee2627467bcdde93
```

### 步骤4：将签名添加到请求当中

在请求中增加Authorization的header如下：

```
Authorization: HMAC-SHA256 Credential={AccessKeyId}/{CredentialScope}, SignedHeaders={SignedHeaders}, Signature={Signature}
```

完整结果如下：

```
HMAC-SHA256 Credential=AKLTYWViMTVmZGYzM2E0NDI5Mzk2MDZjNjFmMjc2MjRjMzg/20240619/cn-beijing/iam/request, SignedHeaders=host;x-date, Signature=e31c4558bcfe08a286001f59cedbf0791ffd0b2362f10e55ee2627467bcdde93
```
