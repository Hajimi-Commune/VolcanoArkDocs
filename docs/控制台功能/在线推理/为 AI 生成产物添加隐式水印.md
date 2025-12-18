# 为 AI 生成产物添加隐式水印
火山方舟为 API 客户提供隐式水印技术能力，能够针对图片、视频等 AI 生成产物添加文件元数据隐性标识。本文为您介绍隐式水印字段信息、接入方式等内容，帮助API 客户更好地利用火山方舟的隐式水印技术，满足相关法规对 AI 生成合成内容标识的要求。 
:::tip
隐式水印为 Beta 功能，限时免费。
:::
# 背景介绍
随着人工智能技术高速发展，生成合成效果日渐逼真，利用生成合成内容传播虚假新闻、实施诈骗等恶性事件不时爆发。为规避生成合成内容滥用、恶意使用对公共安全及公众权益的负面影响，网信办计划加强"标识"能力建设，2025年3月14日发布了规范性文件《人工智能生成合成内容标识办法》，并委托中国电子技术标准化研究院同步制定、下发强制性国家标准[GB 45438-2025《网络安全技术 人工智能生成合成内容标识方法》](https://www.tc260.org.cn/upload/2025-03-15/1742009439794081593.pdf)，对**人工智能生成合成内容服务提供者、提供网络信息内容传播服务的服务提供者、互联网应用程序分发平台、用户**四类主体提出标识义务、作出细节安排。两份文件都于2025年9月1日正式生效。
# 隐式水印字段
根据法规要求，隐式水印Metadata元数据包含以下字段：

- **字段** | **类型**
- **描述** | **含义**
- **方舟是否帮助用户打标** | **方舟说明**
- Label
- string
- 生成合成标签
- * 属于人工智能合成内容的，取值为 “1”，一般是由AI内容制作方写入
- * 可能为人工智能合成内容的，取值为 “2”，一般是用户声明内容为ai生成写入
- * 疑似为人工智能合成内容的，取值为 “3”，一般是审核识别为疑似AI写入 | 是
- 取值为1
- ContentProducer
- string | 生成合成服务提供者 | 生成合成服务提供者的名称或编码
- 是
- 根据用户在平台输入的信息，帮助用户创建 ContentProducer 参数
- ProduceID | string | 内容制作编号
- 生成合成服务提供者对该内容的唯一编号，为平台方对资源的映射，方舟无法帮客户写入
- 否
- 属于生成方对资源的映射，方舟无法帮客户写入，需要生成方自行将自己资源id写入到Metadata信息中
- ReservedCode1 | string | 预留字段 1
- 用于生成合成服务提供者自主开展安全防护，保护内容、标识完整性的信息 | 否
- 能力开发中
- ContentPropagator | string | 内容传播服务提供者 | 内容传播服务提供者的名称或编码
- 是
- 根据用户在平台输入的信息，帮助用户创建 ContentPropagator参数
- PropagateID
- string | 内容传播编号
- 传播服务提供者对该内容的唯一编号，为平台方对资源的映射，方舟无法帮客户写入
- 否 | 属于传播方对资源的映射，方舟无法帮客户写入，需要传播方自行将自己资源id写入到metadata信息中
- ReservedCode2 | string | 预留字段 2
- 内容传播服务提供者自主开展安全防护，保护内容、标识完整性的信息 | 否
- 能力开发中

# 支持模型
支持对生图、生视频模型的 **MP4 ，JPG、PNG**格式的产物标记隐式水印：

   * [doubao-seedance-1.0-pro](https://www.volcengine.com/docs/82379/1587798)
   * [doubao-seedance-1.0-lite](https://www.volcengine.com/docs/82379/1553576)
   * [wan2.1-14b](https://www.volcengine.com/docs/82379/1556464)
   * [doubao-seedream-4.0](https://www.volcengine.com/docs/82379/1824718)
   * [doubao-seedream-3.0-t2i](https://www.volcengine.com/docs/82379/1555133)
   * [doubao-seededit-3.0-i2i](https://www.volcengine.com/docs/82379/1729477)

# 使用说明

* **接入方式**：支持在方舟控制台**by 推理点**开启隐式水印，多个推理点可输入同主体信息。
* **方舟对哪些字段打标**：Label（生成合成标签）、ContentProducer（内容生成方）、ContentPropagator（内容传播方）

> 注：ProduceID（内容制作编号）、PropagateID（内容传播编号）属于生成/传播方自身对资源id的管理，方舟无法帮客户打标，需要客户自行写入Metadata信息，客户可自定义命名规则。

* **计费**：限时免费。
* **时延影响**：增加时延 4ms 左右。

# 接入方式
## 创建自定义推理点

1. 进入创建自定义推理点，点击“更多”，找到“隐式水印”，查看右侧详细介绍

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/697b203531984b66817dcd4858e4f578~tplv-goo7wpa0wc-image.image)

2. 打开开关，同意弹窗内容，开启隐式水印

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d16cf288b5884dd6afbbbe360b5660b9~tplv-goo7wpa0wc-image.image)

3. 开启需用户勾选生成/传播方，并填写对应**生成/传播方的主体名称和社会统一信用代码**（用于生成标识，为国家标准）

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7556c47f2af64de88c2c4311501063f4~tplv-goo7wpa0wc-image.image)

## 自定义推理点详情页

1. 点击推理点名称，进入详情页

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d893baa62a294401829224786cc69235~tplv-goo7wpa0wc-image.image)

2. 点击隐式水印编辑按钮，可切换水印开关状态

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/95d9601629b24240916f80a9a322668c~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/953fc535f2fb40f2b16d97b48743bdff~tplv-goo7wpa0wc-image.image)

3. 打开开关，同意弹窗内容，开启隐式水印

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cc7723500a4d44d29bc811b148343adb~tplv-goo7wpa0wc-image.image)

4. 开启需用户勾选生成/传播方，并填写对应**生成/传播方的主体名称和社会统一信用代码**（用于生成标识，为国家标准）

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d1a63f890c1c4b29a6c263b460c168f5~tplv-goo7wpa0wc-image.image)

# 验证方式
生成产物添加水印情况可上传到[国家标识检测平台](https://www.gcmark.com/web/index.html#/home)进行验证。

:::tip
生图隐式水印暂时无法通过国家平台验证，后续版本会支持验证，目前可使用[jpdump - JPEG Header Viewer](https://cyber.meme.tips/jpdump/#)查看文件头信息。
:::

# 常见问题
## 如何确认我是不是传播方？
示例参考【**仅供参考，具体情况根据各司法务评估**】：

- 产品情况 | 建议
- 面向个人用户的产品，且产品不带传播属性
- * 需要选择：生成方标识，填入该产品的主体和社会统一信用代码。
- * 不需要选择：传播方标识，传播方标识应为最终传播平台自己添加。
- 面向个人用户的产品，且产品自带传播属性 | * 需要选择：生成方和传播方标识，填入该产品的主体和社会统一信用代码。

## 隐式水印是否会影响视频的生产速度？
目前会增加 4ms 左右延时，后续水印能力增强可能对速度有影响。
