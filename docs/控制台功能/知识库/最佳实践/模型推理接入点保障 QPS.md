# 模型推理接入点保障 QPS
:::tip
知识库服务提供了默认公共推理接入点，方便用户快速开启知识问答试用及调试。但在有较高生产级 QPS （Query Per Second，每秒查询并发量）需求，建议使用自建的推理接入点。
:::
# 一、推理接入点创建
## 1、推理接入点定义
在使用大语言模型时，往往需要将模型部署成在线服务，并生成唯一服务访问入口，即为推理接入点
## 2、推理接入点创建
进入【火山方舟】产品控制台 / 在线推理 模块，点击【[创建推理接入点](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint/create?customModelId=)】
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/769b2a9a9eb842519053f90ab45ba263~tplv-goo7wpa0wc-image.image)
在创建接入点时，可以按照业务实际需求，填写业务名称及描述信息。重点关注以下参数配置：

* **接入模型**：请参考以下两节 [三、大语言生成模型](/docs/84313/1321481#37933fe6)，按照不同场景选择模型
* **购买方式**：
   * **按 Token 计费**：按照实际消耗Token量后付费，更加灵活，但可达到的并发上限较低。可通过产品页面，查看不同模型对应的访问限制详情
   * **按模型单元计费**：按照模型可支持的 TPM（Token Per Minute) 付费，可支持更高并发。预付费模式下保障资源确定性，适合对SLA要求更高或流量更平滑的业务。注意：购买模型单元前，需联系火山客服协调资源加白，再进行线上购买

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/00270f8e8af14a84a41f1ee08b420d34~tplv-goo7wpa0wc-image.image)

# 二、大语言生成模型选择
## 1、推理接入点创建
在创建推理接入点，可以选择使用【模型广场】下的官方 Doubao 模型进行构建，也可以按需切换到【模型仓库】使用精调后的模型进行构建。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/393dc8409e1d4e69a9fc496efedc5db1~tplv-goo7wpa0wc-image.image)

## 2、检索生成选择
在进行检索测试时，开启【大模型回答】，先选择想要使用的模型，再选择模型对应私有接入点即可

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/68626c6ca0614f85954e793b14c42056~tplv-goo7wpa0wc-image.image)

此外，通过 API 调用私有接入点可参考 [search_knowledge（新）](/docs/84313/1350012)
