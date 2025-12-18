# Agent开发指南：生成“显眼包”网站
:::tip
**嗨，小伙伴们！豆包大模型一键生成的「显眼包」网页来啦！**

* 访问地址：[http://sd13cp6meq1emkiunq5hg.apigateway-cn-beijing.volceapi.com](http://sd13cp6meq1emkiunq5hg.apigateway-cn-beijing.volceapi.com/) 

   （本网页由AI生成）
:::
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/149cef8c59b846dd9fcab6e14afb558e~tplv-goo7wpa0wc-image.image)

本文通过 [火山方舟大模型体验中心](https://www.volcengine.com/experience/ark) 调用了 Doubao、DeepSeek 等先进模型，参考用户手绘设计图与自然语言指令快速生成 HTML 代码，并借助火山引擎函数服务（veFaaS）MCP，**实现了从设计稿解析、代码生成到公网可访问网页的全流程自动化部署**。
## **任务目标**

* **场景设定**：基于用户设定的需求，依据手绘设计图生成风格可爱、以蓝色为主色调的火山引擎 AI 玩偶 “显眼包” 产品网页，并将网页部署到 veFaaS，获取公网可访问链接，用于产品推广。
* **能力介绍**：
   * **模型能力**：借助 Doubao、DeepSeek 等模型，解析手绘设计图视觉元素与自然语言指令，生成符合要求的 HTML 代码。
   * **Canvas 功能**：实时预览生成的 HTML 代码效果，支持多轮对话调整网页风格、配色等细节 。
   * **函数服务（veFaaS）**：事件驱动的无服务器函数托管计算平台，实现代码上传、函数创建与运行环境配置，助力网页部署。
   * **API 网关（APIG）**：为部署的网页分配公网域名，实现安全、高效的流量转发，使网页可通过公网访问。
* **演示视频**：以下视频展示了生成“显眼包”网站 demo 的能力。

<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/abadacbcc31e474a973983b39453c668~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/abadacbcc31e474a973983b39453c668~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
## 前提条件

* **账号注册**：[注册火山引擎账号](https://console.volcengine.com/auth/signup) 并完成个人或企业认证，详见 [账号注册流程](https://www.volcengine.com/docs/6261/64925)。
* **服务开通**：
   * **开通API网关**：通过函数服务部署 Web 应用前，需先在 API 网关侧创建网关实例。开通参见 [使用流程](https://www.volcengine.com/docs/6569/82995)，操作详见 [创建实例](https://www.volcengine.com/docs/6569/85693)。
   * **对象存储（可选）**：您可体验中心内一键开通。具体使用教程见[对象存储控制台快速入门](https://www.volcengine.com/docs/6349/74830)。
   * **函数服务**：您也可在体验中心内一键开通，具体参见下文使用步骤。

> 注意：**API网关、对象存储、函数服务的使用均会产生相关计费**，请您按需开通。
> 详见 [API网关计费概述](https://www.volcengine.com/docs/6569/185249)、[对象存储计费概述](https://www.volcengine.com/docs/6349/78455)、[函数服务计费概述](https://www.volcengine.com/docs/6662/107454)。

## 生成网站代码
### 操作步骤

1. 访问 [火山方舟大模型体验中心](https://www.volcengine.com/experience/ark?model=doubao-1-5-thinking-vision-pro-250428)，登录火山引擎账号。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/fa03e34fcb8d40df96ae082dbd6a8eb6~tplv-goo7wpa0wc-image.image)
2. 点击模型信息旁的切换按钮，将模型切换为 Doubao-Seed-1.6模型，250615版本。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/306c99d30b6148e2b8c7a925d345776b~tplv-goo7wpa0wc-image.image)
3. 点击 **Canvas** 按钮，打开预览 html 代码功能。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/40c62aa0546544aea84d25b7f246db8c~tplv-goo7wpa0wc-image.image)
4. 点击 **+** 符号打开附件上传界面，选择**图片上传**。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bc8dff24286346e7b78beb6816ed0b52~tplv-goo7wpa0wc-image.image)
5. 选择一个本地手绘稿设计图上传，我们以下面的图片为例。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ab683c7139e64d629288a12aa3a429b0~tplv-goo7wpa0wc-image.image)
6. 在输入框中输入想要生成网页的具体要求（可包括网页主题、风格、配色、是否需要加入额外的产品参考素材等）。例如：
   ```Plain Text
   参考给出的手绘设计图，生成一个火山引擎AI玩偶“显眼包”产品的网页，显眼包是火山引擎送给伙伴们的礼物，网页风格可爱，主题颜色为蓝色，产品素材如下：
   1、主图：
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/显眼包全家福.jpg
   2、细节图：
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3034.jpg
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3053.jpg
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3056.jpg
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3073.jpg
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3075.jpg
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/3081.jpg
   3、产品说明书：
   https://force-video-data.tos-cn-beijing.volces.com/显眼包AI玩偶/显眼包说明书.pdf
   ```

7. 点击 **发送按钮** 开始执行任务。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7f9314432fbb48e28ed98bd749f99c43~tplv-goo7wpa0wc-image.image)
8. 等待模型完成推理。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/18d923ed83ad416fafc5e9d040b27862~tplv-goo7wpa0wc-image.image)

### 效果展示
模型推理完成后，将在右侧屏幕通过Canvas快速预览生成的html网页。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/43dc65fa38bb478386d9dfc808be0445~tplv-goo7wpa0wc-image.image)
您也可以通过顶端的 **代码/预览** 切换按钮切换显示方式。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/87e560653ed84f3fa8ebf9bbae7b016b~tplv-goo7wpa0wc-image.image)
### 多轮对话优化（可选）
您可以继续通过对话让模型调整生成网页的效果。
例如：把网页主题颜色改为淡粉色，重新生成。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aaa926ad179c495db30627b8c1e1c243~tplv-goo7wpa0wc-image.image)
生成效果对比如下。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/43dc65fa38bb478386d9dfc808be0445~tplv-goo7wpa0wc-image.image)
优化前

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/23932def2d624179aa8bd61a8c96e02e~tplv-goo7wpa0wc-image.image)
优化后

## 部署网页代码
### 操作步骤
#### 开启函数服务 MCP

1. 点击 **MCP** 按钮，打开MCP服务器管理面板。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5633dd92cbad4d1fbbfa4f00b41c680e~tplv-goo7wpa0wc-image.image)
2. 开通 **函数服务（veFaaS）**。在MCP服务器管理面板中，找到 **veFaaS** 功能，点击右侧开启按钮。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d45d696517d94627be5af01d183cbcd6~tplv-goo7wpa0wc-image.image)
   1. 若未开通函数服务，系统可能会提示一键授权开通，请点击 **确定开通与授权** 按钮。
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/56e8919df905480e8448fc911d8cc011~tplv-goo7wpa0wc-image.image)
   2. 关闭 create_zip_base64 功能，其余均保持开启。
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/aa8195511a2e46b191405a6af3b0a78d~tplv-goo7wpa0wc-image.image)
3. 配置成功后，鼠标指向 MCP 功能，会显示已启用 veFaaS 函数服务。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0c36eed889d94ae9b5b8944f2b098772~tplv-goo7wpa0wc-image.image)

#### 执行任务
在文本框中输入部署要求，点击发送按钮。 示例输入：
```Plain Text
把生成的网页部署到veFaaS，记得上传生成的代码文件，最后给我返回一个公网可以访问的地址
```

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/48684cbe8d374b1693681eb01dd6758c~tplv-goo7wpa0wc-image.image)
模型开始推理。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/45ddec7debaf46229384e4c0fa7d6c2e~tplv-goo7wpa0wc-image.image)
### 效果展示
模型推理完成后，大模型会返回部署好的网页链接信息。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/56e3e17e0c1a47539067b7534928d731~tplv-goo7wpa0wc-image.image)
点击链接（[http://sd13cp6meq1emkiunq5hg.apigateway-cn-beijing.volceapi.com](http://sd13cp6meq1emkiunq5hg.apigateway-cn-beijing.volceapi.com/) ），即可预览网页部署效果。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b3dad5e93e9a41349d134db982cbed19~tplv-goo7wpa0wc-image.image)
