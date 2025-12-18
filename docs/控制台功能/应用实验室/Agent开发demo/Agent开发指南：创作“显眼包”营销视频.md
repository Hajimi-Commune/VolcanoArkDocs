# Agent开发指南：创作“显眼包”营销视频
本文以 “多巴胺” 热点为核心，构建了完整且智能的创作闭环。首先，通过人机交互与数据挖掘锁定营销热点，生成精准的视频创作提示词；其次，运用 Seedance-1.0-lite-i2v 和 Seedance-1.0-pro 两款专业视频生成模型，打造出三段风格鲜明的显眼包商品多巴胺风格视频；最后，借助 vevod mcp 强大的视频处理能力，完成三段视频的拼接与转场特效添加，最终输出兼具创意与传播力的优质营销视频作品，实现从热点洞察到成品落地的高效转化。
# 任务目标

* **场景设定**

用户与 “显眼包” 智能交互体沟通，捕捉到当下热门的 “多巴胺” 主题，并将其作为核心创意元素。通过接入全网内容数据与企业专属知识库的系统持续挖掘契合市场需求的营销视频热点，为后续创作提供坚实的数据支撑。

* **功能介绍**

- MCP / 服务 | 功能介绍
- DeepSearch | 一款专为处理复杂问题而精心设计的高效工具，集成了联网搜索、知识库、网页解析、Python 代码执行器等丰富的 MCP 服务
- 联网搜索 | 联网搜索工具，用于实时搜索互联网公开域内容
- 知识库 | 在知识库内检索内容的工具
- Vevod | 通过对话交互的方式，实现多视频时域拼接、长视频分段截取与拼接、添加转场动画及字幕等剪辑操作
- Seedance-1.0-pro 图生视频模型服务 | 通过指定提示词和首帧图的方式，生成视频
- Seedance-1.0-lite-i2v 首尾帧生视频模型服务 | 通过指定提示词、首帧图和尾帧图的方式，生成视频

* **演示视频**：

<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8d32f2e788f34a57a30f1acab9be47b5~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8d32f2e788f34a57a30f1acab9be47b5~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
# 前置准备

* **注册火山引擎账号**：如您已有火山引擎账号，可跳过该步骤。如您为未注册过火山引擎账号的企业或个人，首先请 [注册火山引擎账号](https://console.volcengine.com/auth/signup) 并完成相关认证，详见 [账号注册流程](https://www.volcengine.com/docs/6261/64925)。
* **下载Trae IDE客户端**：如果您已有Trae IDE客户端，可通过该步骤。如您未下载过Trae IDE客户端，首先请前往[Trae官方网站](https://www.trae.com.cn/?utm_source=volcengine&utm_medium=mcp-marketplace)，下载客户端并完成账号注册。

# 详细操作指南
## 获取关键词
### 配置DeepSearch

1. 进入[火山方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/application)，点击火山方舟控制台右侧导航栏【应用广场】，双击打开“DeepSearch”应用，在应用界面内选择【体验应用】。
   点击直达：[“DeepSearch”应用](https://console.volcengine.com/ark/region:ark+cn-beijing/application/detail?id=bot-20250414112950-6x4km-procode-preset&prev=application)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29eb60d4696e42b08769c2b0f030a6f3~tplv-goo7wpa0wc-image.image)

2. 用户可在页面右侧自主配置“DeepSearch”，“DeepSearch”支持自定义设置问题拆解层数与MCP服务。MCP服务默认全部开启，建议只保留上面三个MCP服务，即联网搜索、知识库、网页解析。 

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6de063d0949547069e2802f7135338ae~tplv-goo7wpa0wc-image.image)
### 应用DeepSearch
#### 输入问题
在问题输入框输入以下内容：*“我想制作一个营销视频，帮我看看网上各类营销视频热点关键词吧，尤其是各类AI玩具类的营销视频热点词。”*
#### **输出结果**
DeepSearch应用返回结果：AI玩偶热点词为多巴胺。注意此处需自行部署DeepSearch，并完成知识库MCP的配置 。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/65cbbaffdfd34103a24b800cad2eaff5~tplv-goo7wpa0wc-image.image)

## 获取Prompt提示词
### 前置条件

* Python 3.13+
* 火山引擎账号及AccessKey/SecretKey

### 配置 Vevod MCP 

1. 进入Trae应用，点击右上角的设置按钮打开MCP服务。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/163fe7729e984ae5b28eba9fdcdbb2e1~tplv-goo7wpa0wc-image.image)

2. 点击添加新的mcp服务，在搜索栏搜索“Vevod mcp”，点击【+】添加Vevod MCP服务。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b33369dea65641b0ab2fbd7bbf119614~tplv-goo7wpa0wc-image.image)

3. 点击【获取】“VOLCENGINE_ACCESS_KEY”和“VOLCENGINE_SECRET_KEY”按钮，跳转到“[API访问密钥](https://console.volcengine.com/iam/keymanage/)”界面，获取“Access Key ID”和“Secret Access Key”后分别填入“VOLCENGINE_ACCESS_KEY”和“VOLCENGINE_SECRET_KEY”。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/94fc56828ad244c39038e9f080f3ff6c~tplv-goo7wpa0wc-image.image)
### **配置智能体**

1. 点击右上角的设置，进入智能体界面

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0218f98a8a1e4ed694b978787f422db4~tplv-goo7wpa0wc-image.image)

2. 创建智能体

分别按下述信息填写相对应的信息，构建爆款智造官：
名称：爆款视频智造官 
提示词按照如下填写：
```Markdown
#角色
结合获取到的热点词,生成具备传播效果的高质量视频内容
#任务
获取到热点词,生成多条高质量的视频提示词
```

工具需要选择刚才配置好的VeVOD MCP
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e199305857074b66bc4859726f81606a~tplv-goo7wpa0wc-image.image)
### 获取提示词
在对话框下方智能体中选择“爆款视频智造官”，并在问题输入框输入以下内容：*“请结合当前获取到的热点关键词"多巴胺",以显眼包AI玩偶为主角,生成3个视频生成所需要的提示词”。*得到智能体的回答。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b46a0c969c484a6a857383c9d0cfd63d~tplv-goo7wpa0wc-image.image)
## 获取视频
### **前置条件**

1. 在使用 Seedance 视频生成模型前，需在火山方舟大模型服务平台的[开通管理](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false&tab=ComputerVision)页面，选择「视觉大模型」下 Seedance 1.0 lite 和 pro 的开通服务，以完成相应模型的开通操作。详细说明请参考[模型开通管理](https://www.volcengine.com/docs/82379/1159200)文档。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ec4c44870fe74855b1df6207bfb8d7ea~tplv-goo7wpa0wc-image.image)

2. 完成视频生成模型开通后，进入火山方舟大模型服务平台的[API Key 管理](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)页面，在此页面创建并获取 API Key，该 API Key 将用于后续的视频生成调用。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3a9cf38c1c7c47b2970d95bb5eac9acc~tplv-goo7wpa0wc-image.image)
### 生成视频

1. **基于Seedance 1.0 lite i2v 模型首尾帧功能**

打开Trae IDE客户端，选择此前创建好的“**爆款视频智造官**”智能体，将内置模型选定为“**Doubao-1.5-thinking-pro**”。随后，在对话框中输入并发送以下命令以进行视频生成操作。在此过程中，需将`Bearer`之后的API Key替换为上一步所获取的API Key 。该命令运用的是**Seedance 1.0 lite首尾帧生视频**功能，用户还可依据自身实际需求，对`text`后面的提示词以及`url`后面的图片地址进行替换。 如需了解更多关于视频生成API的使用说明，请查阅[视频生成文档](https://www.volcengine.com/docs/82379/1520757)。 
Trae对话框的具体命令如下：
```Bash
帮我用下述命令，生成第1段视频，并打印接口真实响应信息。
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks
  -H "Content-Type: application/json"
  -H "Authorization: Bearer 16397bb4-c488-4107-b2e6-*********"
  -d '{
    "model": "doubao-seedance-1-0-lite-i2v-250428",
    "content": [
        {
            "type": "text",
            "text": "图1（首帧图）玩偶在空中缓缓下落，落下的瞬间极速变化成图2（尾帧图）中玩偶的样子，落地后出现图2画面，画面中的五彩棉花糖云彩大幅度晃动，保持玩偶形象始终稳定，保持玩偶大小稳定，保持画风始终一致"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://video-image-test2025space.tos-cn-beijing.volces.com/%E5%9C%BA%E6%99%AF1-%E9%A6%96%E5%B8%A7.png"
            },
            "role": "first_frame"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://video-image-test2025space.tos-cn-beijing.volces.com/%E5%9C%BA%E6%99%AF1-%E5%B0%BE%E5%B8%A7.png"
            },
            "role": "last_frame"
        }
    ]
}'
```

操作展示图如下：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1455b400427744ffb8e7208db32ecb88~tplv-goo7wpa0wc-image.image)

2. **基于Seedance 1.0 pro 模型图生视频功能**

同样地，将内置模型选定为“**Doubao-1.5-thinking-pro**”，在对话框中输入并发送以下命令以进行视频生成操作。同样需将`Bearer`之后的API Key替换为前面所获取的API Key 。该命令运用的是**Seedance 1.0 pro图生视频**功能，用户还可依据自身实际需求，对`text`后面的提示词以及`url`后面的图片地址进行替换。 
Trae对话框的具体命令如下：
```Bash
帮我用下述命令，分别生成第2段视频和第3段视频，并打印接口真实响应信息。
注意将命令中的{prompt}分别替换成下述两组prompt，并将命令中的 {image_url} 替换为下述两组图片地址
第2段视频对应的prompt：
画面中云彩晃动起来，城堡的门打开，图中穿着粉色衣服戴着粉色墨镜的蓝色玩偶走出来，一直走到热气球旁边。首帧开始，保持玩偶形象始终稳定，保持玩偶大小稳定，超高清，4k画质，细节丰富
第2段视频对应的图片地址 https://video-image-test2025space.tos-cn-beijing.volces.com/%E5%9C%BA%E6%99%AF2-%E5%9B%BE%E7%89%87new.png
第3段视频对应的prompt：
图中坐着热气球的玩偶向上坐着热气球飞出图画，热气球中的黄色火焰自然燃烧，画面中的五彩棉花糖云彩大幅度晃动，保持玩偶形象始终稳定，保持玩偶大小稳定
第3段视频对应的图片地址 https://video-image-test2025space.tos-cn-beijing.volces.com/%E5%9C%BA%E6%99%AF3-%E5%9B%BE%E7%89%87.png
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks
  -H "Content-Type: application/json"
  -H "Authorization: Bearer 16397bb4-c488-4107-b2e6-*********"
  -d '{
    "model": "doubao-seedance-1-0-pro-250528",
    "content": [
        {
            "type": "text",
            "text": "{prompt}"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "{image_url}"
            }
        }
    ]
}'
```

操作图如下：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c4e818ffe21b4da1b1a2668ba2880fb2~tplv-goo7wpa0wc-image.image)
### **查询进度**
完成视频生成任务创建后，将内置模型选定为“**Doubao-1.5-thinking-pro**”，在对话框中输入并发送以下命令以进行视频生成结果的查询操作。在此过程中，同样要将`Bearer`之后的API Key替换为之前获取到的API Key。 
Trae对话框的具体命令如下：

```Bash
帮我根据上述3组响应中的id字段，分别用下述命令查询视频生成的进度，并打印真实响应信息
curl -X GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/{id}
  -H "Content-Type: application/json"
  -H "Authorization: Bearer 16397bb4-c488-4107-b2e6-*********"
```

效果展示图如下：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f5dbfffbfa084a97a0c1791c46dfb9d1~tplv-goo7wpa0wc-image.image)
### 视频拼接
以下将运用 VeVOD MCP 对上述视频进行拼接，并添加视频转场效果，将内置模型选定为“**Doubao-1.5-thinking-pro**”，在对话框中输入并发送以下命令以进行视频拼接和添加转场操作。在具体操作中，选用的是泛开的转场效果。若需了解和选用更多转场效果，可查阅[转场效果](https://www.volcengine.com/docs/4/102412#%E8%BD%AC%E5%9C%BA-id)说明文档。 
Trae对话框的具体命令如下：
> 将上述三个视频下载地址对应的5s视频，依次拼接在一起合成一个新的15s视频，每两段视频之间添加泛开的转场效果，可以用mcp-test-space空间

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5f219a67ee23428db1414d342beffc5d~tplv-goo7wpa0wc-image.image)
### 视频查看&下载
完成上述操作后，前往火山引擎-[视频点播界面](https://console.volcengine.com/vod/region:vod+cn-north-1/overview/)，点击然后进入mcp-test-space空间。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d8a9eaa5190a456195170cc7458b175e~tplv-goo7wpa0wc-image.image)

在此查看并可以下载最终制作完成的视频，如下图所示。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2bea790bc214c1a99a4780086314b59~tplv-goo7wpa0wc-image.image)

# 附录：开源应用介绍

* 该案例中用到的 [DeepSearch](https://console.volcengine.com/ark/region:ark+cn-beijing/application/detail?id=bot-20250414112950-6x4km-procode-preset&prev=application) 为火山方舟应用实验室 的 开源原型应用，您可在[应用广场](https://console.volcengine.com/ark/region:ark+cn-beijing/application)免费体验。DeepSearch 专为应对复杂问题而设计的高效工具，集成了浏览器使用、联网搜索、知识库、网页解析等丰富的 MCP 服务。无论是学术研究、企业决策，还是产品调研场景，它都能助力用户深入挖掘信息，提出切实可行的解决策略。
* 此外，应用实验室汇集了更多高难度、高价值的解决方案案例，并开放源代码，助力企业快速构建大模型应用。
* **开源信息**：
   * DeepSearch开源地址：https://github.com/volcengine/ai-app-lab/tree/main/demohouse/deep_search_mcp/backend
   * 应用实验室开源地址：https://github.com/volcengine/ai-app-lab
   * 开源协议：Apache License 2.0 https://github.com/volcengine/ai-app-lab/blob/main/APACHE_LICENSE
