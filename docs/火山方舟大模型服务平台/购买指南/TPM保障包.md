# TPM保障包
TPM 保障包是针对某个特定模型以及版本保障请求并发达到一定 TPM（ Tokens per Minute）的计费模式。本文为您介绍TPM 保障包的主要优势、支持模型、购买说明等内容。
# 主要优势

* 更低的延迟：相比按Token计费，TPM 保障包的延迟更低，通常比直接按Token计费低 1/3 - 1/2，如 Doubao-1.5-pro 模型的TPM保障包，TPOT（Time per Output Token）可低至 20 ms。
* 更高的并发：支持超过默认限流额度的并发。
* 更强稳定性：提供高资源确定性保障，在保障范畴内不会命中异常流量熔断、限速策略，持续保障业务服务可用性。
* 支持按小时和按天付费：您可以在业务高峰时叠加按小时计费的保障包和按天的保障包，贴合流量波峰波谷，避免资源浪费。
* 支持弹性伸缩：后付费TPM保障包，支持动态调整当前接入点的 TPM保障包购买量，帮助在业务高峰期扩容，在业务低谷期释放资源，提升资源利用率并降低成本。
* 可叠加按Token计费使用：优先消耗保障额度内流量且不受模型默认限流影响，超出部分自动降级为按 Token 付费（超出部分流量**计算在默认限流额度中**），既保障可预估的流量，又对临时流量有一定缓冲能力。

# 适用场景

* 对高流量业务提供资源保障，适合大流量、可预估流量大小，生产级高 SLA 要求的场景。
* 希望请求延时更低的线上业务。

# 支持模型
> 支持基础模型的在线推理场景

当前支持模型参见[TPM 保障包](/docs/82379/1544106#95750410)，具体支持的模型以控制台显示为准。
如需支持更多模型版本，可提交[工单](https://console.volcengine.com/workorder/create?step=2&SubProductID=P00001166)申请。
# 使用限制

* 不支持 Responses API。
* 不支持上下文缓存 Context API。
* 不支持结构化输出功能（ **response_format.type** 无法设置为 `json_object`、`json_schema` ）。
* 当接入点请求超过保障包额度时，自动切换为普通在线推理模式，以 **按Token付费** 模式收费。
* 模型请求的输入范围符合下方条件。如不符合，无法使用 TPM 保障包，请求返回的 **service_tier** 字段为`default`，即普通在线推理模式，不扣减保障包额度。
   * Doubao 模型，输入长度 (0, 128k]。
   * DeepSeek 模型，输入长度(0, 64k]。

# 计费说明
TPM 保障包支持按小时后付费和按天预付费，两种方式可叠加购买，单价请参见 [TPM 保障包](/docs/82379/1544106#95750410)。
## 计费方式对比

- 计费类型 | 后付费 | 预付费
- 计费特点 | 按实际使用时长计费，精确到秒 | 按天计费，价格更优惠
- 弹性配置 | 支持 | -
- 适合场景 | 短期、弹性或者服务请求波动较大的场景 | 中长期稳定或服务请求相对稳定的场景

## 后付费（按小时）

* **计费特点**：按照实际购买时长收费，计费粒度精确到秒。购买后持续计费，如需停止计费可在接入点详情页进行退订。
   > 举例：假设您在16:00下单成功，在18:20:31退订成功。则计费时长为 2 小时 20 分钟 31 秒，计费单价会换算成每秒钟单价进行计算。
* **计费粒度**：秒。不足一秒按一秒计算。
* **出账周期**：按小时结算，账单出账时间通常在当前计费周期结束后1-2小时左右，具体以系统实际出账时间为准。例如：16:00-17:00 的账单约在 18:00-19:00 出账。
* **欠费说明**：欠费后，资源会继续保留，依然会产生费用。欠费24小时后，将回收资源停止计费。请及时续费或销毁资源。

## 预付费（按天）

* **生效时间**：实时生效
* **到期时间**：第 N 天的 23:59 到期。其中，N 为您购买的 TPM 保障包的天数，购买当天为第 1 天。
   以购买了 1 天期限的 TPM 保障包举例：如果早上 9:00下单，则当天晚上 23:59 到期；如果晚上 21:00 下单，仍旧是当天晚上 23:59 到期。
* **提前退订**：退订退款金额=实付金额-已使用金额×惩罚系数。其中，已使用金额和使用时长相关，使用时长按自然日首尾取整（用了一分钟、还剩一分钟都算一天）。
* **续费说明**：建议开启自动续费能力，您可以在官网费用中心查看当前自动续费起始时间。

# 购买流程
TPM保障包支持叠加购买。您可以在创建推理接入点时购买；也可以在接入点详情页进行购买、续费、退订等操作。
> 如果业务对于延时有需求，请通过[工单](https://console.volcengine.com/workorder/create?step=2&SubProductID=P00001166)提需求。

## 创建推理接入点时购买

1. 访问[方舟控制台-在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)，切换到 **自定义推理接入点** 页签，单击 **创建推理接入点**。
2. 在打开的页面中填写接入点名称，选择模型类型，并选择接入模式为 **TPM保障包**。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/07a0dbcd51194863bfcb9f3233b1f4ca~tplv-goo7wpa0wc-image.image)
3. 使用 [TPM计算器](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint/create) 预估需要购买的输入和输出额度，并 [申请配额](https://console.volcengine.com/ark/region:ark+cn-beijing/quota?quota=%7B%22business%22%3A%22Endpoint%22%2C%22quota%22%3A%22ModelTPM%22%2C%22table%22%3A%7B%7D%7D)。配置项参数说明详见[配置参数](/docs/82379/1510762#ea03124a)。
4. 勾选协议，并单击 **创建并接入**，完成下单。

## 推理接入点详情页购买

1. 访问[方舟控制台-在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)，切换到 **自定义推理接入点** 页签。
2. 单击目标接入点名称，进入接入点概览页。在算力保障区域，根据不同的付费类型，选择购买TPM保障包。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cb577d5a33e74969a4434896999a3c84~tplv-goo7wpa0wc-image.image)

# **配置参数**
:::tip
建议您打开 **创建说明** 开关，帮助您了解每个配置项的使用场景和含义，轻松完成TPM 保障包的下单。
:::

- 配置名称 | 配置说明
- 接入模式 | 必填，本场景选择 **TPM保障包**。
- 计费类型 | 选择TPM保障包的计费类型：
- * **后付费**：按量计费，使用灵活，适合短期或者服务请求波动较大的场景。
- * **预付费**：提前购买，价格较为优惠，适合长期或者服务请求相对稳定的场景。
- * **组合使用**：预付费TPM保障包和后付费TPM保障包支持叠加使用。创建推理接入点时只能选择 1 种计费类型，操作叠加购买多种TPM保障包，需要创建完成推理接入点后，在推理接入点详情页进行配置。
- 购买额度 | 根据您的业务需求，为模型输入和模型输出分别灵活购买所需的TPM保障包额度。
- :::warning
- 对于**doubao-seed-1.6 系列模型，不同长度请求抵扣 TPM 速度不同，您需要按抵扣系数计算购买的TPM。**每个模型的抵扣系数不同，您可通过 [TPM 计算器](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint/create)**（登录后在下单页面使用）** 查看相应的抵扣系数，并估算实际需要购买的**可抵扣TPM**。
- 弹性伸缩 | **后付费（按小时）** 购买TPM保障包的配置项，选填。
- 弹性伸缩是一种按需自动调整资源量的机制，可根据指定的弹性规则，动态调整当前接入点的 TPM保障包购买量，帮助在业务高峰期扩容，在业务低谷期释放资源，提升资源利用率并降低成本。
- 支持以下两种弹性规则，多个弹性规则可叠加，灵活覆盖全业务周期。详细配置要求和注意事项请参见控制台 **创建说明**。
- * **定时弹性**：您可以设置在某一时间节点（可精确到分钟）后将TPM保障包的输入TPM和输出TPM调整到您所需的新的值。
- * **周期性弹性**：您可以「按天重复」、「按周重复」或「Cron表达式」来设置时间规则，TPM保障包的输入TPM和输出TPM将会在您所设置的时间节点后调整到您所需的新的值。
- 购买时长 | **预付费（按天）**购买TPM保障包的配置项，必填。
- TPM保障包的购买时长。
- 自动续费 | **预付费（按天）**购买TPM保障包的配置项，选填。
- 推荐您进行选择，保障服务持续可用。
- * 单次自动续费时长：选择每一次执行续费操作的续费时长。
- * 自动续费次数：默认为永久生效，您可以根据业务填写自定义次数。

# 调整数量/续费/退订

1. 访问[方舟控制台-在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)，切换到**自定义推理接入点**页签。
2. 单击目标接入点名称，进入接入点概览页。在算力保障区域，根据需要对TPM保障包进行调整数量、续费或退订。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d15b16374d7b47f6b4ed316e798688c2~tplv-goo7wpa0wc-image.image)
:::warning
未到期的TPM保障包退订会产生惩罚系数，无法 100%退费。
:::

# 修改弹性规则
对于按小时后付费的TPM保障包，支持在接入点详情页修改弹性规则。

1. 访问[方舟控制台-在线推理](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint?config=%7B%7D)，切换到**自定义推理接入点**页签。
2. 单击目标接入点名称，进入接入点概览页。在算力保障区域，单击 **弹性详情**，可查看当前的弹性规则。
3. 单击 **调整弹性**，可对弹性规则进行修改。

# 订阅通知
您可以使用火山引擎消息通知服务（后简称 SNS） 来感知TPM保障包信息通知。
## 订阅流程

1. 申请 “SNS开白” ，使用请提交[工单](https://console.volcengine.com/workorder/create?step=2&SubProductID=P90000689)申请，并同步申请对应的消息事件：
   * ModelTPMNewFailed：TPM保障包新购失败告警
   * ModelTPMScaleUpFailed：TPM保障包扩容失败告警
2. 在 **主题** 页面创建主题。
   * 发布者选项指定账号：`2100444922`
   * 服务选择：`ark`

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/437917a96bb84c02b1a5fafe5a3f2bd1~tplv-goo7wpa0wc-image.image)

3. 在 **云服务事件订阅** 页面创建事件订阅。Topic TRN选择刚刚创建的主题，事件选择`ModelTPMNewFailed`、`ModelTPMScaleUpFailed`。
4. 在**订阅**页面，订阅前面创建的主题，并配置可接收端地址

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/76693dd573b34176b8ca85cce0c23e30~tplv-goo7wpa0wc-image.image)

* 配置完订阅后，SNS 会向接收端发送对应的确认链接，需确认该链接来完成订阅，确认链接demo如下。需要回调下文中SubscribeURL

```JSON
{
  "Type": "SubscriptionConfirmation",
  "MessageId": "f11b9a8f-****",
  "TopicTrn": "trn:sns:cn-beijing:2100000825:topic/test",
  "Message": "You have chosen to subscribe to the topic trn:sns:cn-beijing:2100000825:topic/wyy_test. To confirm the subscription, visit the SubscribeURL included in this message.",
  "Timestamp": "2025-01-14T07:18:59Z",
  "SignatureVersion": "1",
  "Signature": "MEUCIB3NsKw***=",
  "SigningCertURL": "https://sns-public-cn-beijing.tos-cn-beijing.volces.com/certificates/cn-beijing-a31d91fc-0683-****.pem",
  "SubscribeToken": "eyJhbGciOiJIUzI1Ni****",
  "SubscribeURL": "https://sns.cn-beijing.volcengineapi.com?Action=ConfirmSubscription&Version=2023-01-01&Token=eyJhbGc***"
}
```

* 回调成功后可在 **订阅** 页面看到对应的订阅状态为：已确认，表示订阅已完成。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/301d7375496f4ac6863e38b360cdb1e0~tplv-goo7wpa0wc-image.image)
## 订阅信息内容格式
TPM保障包订阅失败时通知的内容。
### ModelTPMNewFailed

* TPM保障包新购失败告警

```JSON
{
        "EventID": "tpmro-202502111****-****",
        "EventName": "ModelTPMNewFailed",
        "EventTime": "2025-02-11T19:41:36+08:00",
        "AccountID": 2100000825,
        "ModelTPMInfo": {
                "EndpointID": "ep-2025021******-cn***",
                "FoundationModelName": "doubao-lite-4k"
        }
}
```

### ModelTPMScaleUpFailed

* TPM保障包扩容失败告警

```JSON
{
        "EventID": "tpmro-202502111****-****",
        "EventName": "ModelTPMScaleUpFailed",
        "EventTime": "2025-02-11T19:41:36+08:00",
        "AccountID": 2100000825,
        "ModelTPMInfo": {
                "EndpointID": "ep-20250******-cn***",
                "FoundationModelName": "doubao-lite-4k"
        }
}
```

# 常见问题
请参见[TPM保障包](/docs/82379/1359411#5e8ec656)。
# 附：定时弹性规则示例
假设下午6点到晚上9点期间，某推理接入点需要大约 10000 TPM 的保障包。则可以设置两条周期性弹性规则：

* 规则一：在下午6点将保障包的值设置为输入 10k TPM， 输出 1 k TPM。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b48c41af19cb4c918d90b5963df5d8a4~tplv-goo7wpa0wc-image.image)

规则二：在晚上9点 将保障包的输入和输出值都设置为 0
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/fdd85a3ea2b24f058204c7c32427c680~tplv-goo7wpa0wc-image.image)
