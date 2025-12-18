# VideoPilot 功能介绍
## **概述**
VideoPilot 是一款专注于将 AI 生成视频提升至可商用级别的工具，当前核心推出了 **视频再创** 模块，旨在为用户提供基于参考视频的高可控性、高一致性与高审美性的二次创作服务。
### **核心能力**

* 基于现有参考视频进行精准二次创作，保留原始视频核心逻辑与节奏的同时实现创意升级。
* 支持最长 1 分钟的视频制作，覆盖多场景商用需求。
* 提供从需求定义到视频导出的全流程创作工具链，包含关键帧编辑、片段精调等专业功能。

### **适用场景**

* **广告制作**：快速迭代广告片创意，替换产品主体或调整视觉风格。
* **电商推广**：生成多版本商品展示视频，适配不同平台传播需求。
* **影视制作**：实现剧情续写、风格迁移或镜头语言优化等前期创意验证。

## 功能计费

* 生成、调试视频每次任务耗费积分会在积分池里扣除，11月前登录平台会赠送试用积分，购买套餐会额外再赠送积分。订阅套餐请参见[PromptPilot 订阅管理](/docs/82379/1827371)。
* 视频的长度和分辨率和模型的选择会影响积分的消耗额，积分数和实际调用token消耗数正相关。

## 注意事项

* 当图像涉及政治、法律敏感内容时，会产生上传/生成不成功或超时报错等情况。
* 使用素材请注意原始素材拥有者的版权问题。

## **功能演示**
VideoPilot 支持多种维度的视频再创能力，以下为核心功能效果展示：
<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9d655b29fe6240689048505936785bce~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9d655b29fe6240689048505936785bce~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>

- **原视频** | <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a89c75f535034903a6046cf1297e519e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a89c75f535034903a6046cf1297e519e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- **故事续写** | 基于原视频逻辑延伸剧情，如"走到悬崖边眺望夕阳下海岸线"
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42c8f1327f5c405abaf0aaf3bb15e25b~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42c8f1327f5c405abaf0aaf3bb15e25b~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- **多主体替换** | 替换画面核心主体，如"换成红裙子手里拿本书"
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/431f68fc53774dcca98705d0864e9867~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/431f68fc53774dcca98705d0864e9867~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- **风格变化** | 转换整体视觉风格，如"换成宫崎骏漫画风"
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e39a97d0bd804c149e089d03711ce69b~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e39a97d0bd804c149e089d03711ce69b~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- **镜头运动** | 调整镜头运动轨迹，如"快速拉远至航拍视角"
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/830509155fdb48b79d18c6e5d091f313~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/830509155fdb48b79d18c6e5d091f313~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>

## **创作流程**
### **步骤1：描述视频生成需求**
访问 [PromptPilot控制台-VideoPilot](https://promptpilot.volcengine.com/super-agent/video-pilot/reproduction)，输入素材与生成目标，界面示意如下。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d1ec0a8230e24207830ba0fa9658dd3d~tplv-goo7wpa0wc-image.image)
#### **素材上传**

* 参考视频：上传需进行二次创作的原始视频文件，作为AI生成的基础逻辑依据。
* 参考图：按需上传用于主体替换、风格参考等的辅助图片，提升生成可控性。

#### **需求输入**
以自然语言清晰描述创作需求，示例：

* "将参考视频中的广告商品替换为参考图中的饮料，保持原视频镜头运动不变"
* "参考视频风格转换为复古胶片风，人物服饰颜色调整为深蓝色"

#### **参数设置**

* **视频比例与分辨率**：选择目标视频的宽高比与分辨率，分辨率直接影响生成所需积分消耗，高分辨率对应更高的画质与积分成本。
* **生成模式选择**：
   * **一键成片**：适用于8秒以内的短视频，生成速度快，拆帧精细度中等，适合快速迭代创意。
   * **预调试**：适用于复杂长视频，拆帧精细度高，生成时间相对较长，适合对细节要求严苛的商用场景。

### **步骤2：调整关键帧**
平台会自动解析参考视频为连续的关键帧片段，用户可通过精细化编辑确保生成效果符合预期。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d23722beabd646688f38df9f4055be91~tplv-goo7wpa0wc-image.image)
#### **添加关键帧**
用户可在视频时间轴任意位置选取画面并插入关键帧，用于强化特定画面的细节控制，操作示意如下。建议在剧情转折、主体变化等关键节点添加自定义关键帧。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/64bff0a2ae184f5184144fbe6fe762f6~tplv-goo7wpa0wc-image.image)
#### **编辑关键帧**
支持对现有关键帧进行多维度修改。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7f09fcdacd80471f92106a1ffd3f3012~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8817e6caee264a668ad779ed840bd1b0~tplv-goo7wpa0wc-image.image)

* **局部重绘**：点击"局部重绘"按钮，框选需修改的画面区域，输入重绘需求（如"消除画面中的电线杆"），可搭配参考图提升精准度，操作示意如下。支持局部消除、元素添加、细节优化等操作。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2d487cf22d724fdea7246d2a2bcc35c9~tplv-goo7wpa0wc-image.image)

* **修改镜头描述**：每个关键帧对应自动生成的镜头描述文本（包含画面主体、构图、光影等信息），修改描述后点击"重绘关键帧"，AI将依据新描述更新画面。该描述同时影响当前关键帧及后续过渡画面的生成逻辑，操作示意如下。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dbfd34908b6c438d87373510b43dc32c~tplv-goo7wpa0wc-image.image)

#### **生成视频**
完成关键帧调整后，点击"生成视频"按钮，平台将基于关键帧逻辑与需求描述合成完整视频。
### **步骤3：精调视频**
视频生成后可进行片段级优化，确保最终效果符合商用标准。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b077bd537bff4ecbbb7c1cc0c77f1e09~tplv-goo7wpa0wc-image.image)
#### **重新生成片段**
选中不满意的视频片段，补充或修改描述细节（如"增强画面对比度，人物面部更清晰"），点击"重新生成片段"即可局部优化，无需重新生成完整视频。
#### **分割视频**
当片段中仅部分内容需调整时，使用"分割"工具将视频切分为多个子片段，单独对目标子片段进行编辑或重新生成。
#### **调整速度**
通过设置速度系数控制片段播放速度：

* 速度系数＞1：加速播放（如系数为2时，6秒视频压缩为3秒）
* 速度系数＜1：减速播放（如系数为0.5时，3秒视频延长为6秒）

速度调整不改变画面内容，仅影响播放节奏。
### **步骤4：下载视频**
视频调整至满意状态后，点击界面右上角"下载视频"按钮，即可导出生成的视频文件。
### **步骤5：历史任务管理**
所有创作任务自动保存至"创作历史"模块（首页下方）。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c832ddef7e05470c9fec7bd91f483711~tplv-goo7wpa0wc-image.image)

* 任务排序：默认按创建时间倒序排列，最新任务显示在最上方。
* 团队筛选：团队空间用户可通过右上角筛选工具，按创作者维度查询特定成员的历史任务。
* 二次编辑：支持从历史任务中调取已生成的视频，可以进入精调环节进行二次创作。
