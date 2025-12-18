# doubao-seedance-1.0-lite

<code>模型效果</code>

★★★★

<code>速度</code>

★★★★★

<code>价格（元/百万token）</code>

10 

[输出]

<code>输入</code>

Text, 

Image , <del>Video</del>, <del>Audio</del>

图片、文本

<code>输出</code>

<del>Text</del>, 

<del>Image</del>, <del>Audio</del>, Video

视频

---

seedance 1.0 是字节跳动豆包大模型团队最新推出的视频生成基础模型。seedance 1.0 lite 模型作为该模型系列的小参数量版本，在取得出色的视频生成质量的同时，显著提升了生成速度，是兼顾效果与效率的性价比之选。

分辨率：720p，480p，1080p`参考图场景不支持`
帧率：24 fps
时长：2~12 秒
视频格式：mp4

---

## 模型优势

* ### **深度语义理解与准确指令遵循** 可精细控制人物外貌气质、衣着风格、表情动作，在多主体动作解析、嵌入式文本响应、程度副词和镜头切换响应方面，也展现出了绝对优势。

- 视频生成示例
- **图生视频 i2v**
- 精细控制人物表情与动作变化
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/deafa27ec78d4c6ba9005744c5cc70b2~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/deafa27ec78d4c6ba9005744c5cc70b2~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 转过脸对着镜头向前走，然后停下，她一脸生气，然后叉腰 | 精准生成多主体复杂动作
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/038faf57f37145479db23b6095cad7f0~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/038faf57f37145479db23b6095cad7f0~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 几个人拥抱起来，作出拥抱的动作 | 对程度副词、镜头切换响应准确
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/518cba15a0224449bfe082df8967a703~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/518cba15a0224449bfe082df8967a703~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 美丽的女人在车里，风景在背景中移动。女人转脸看向镜头，满脸兴奋。镜头切换，一个男人正在开车，同样面带笑容
- **文生视频 t2v**
- 精细控制人物外貌气质与衣着风格
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/10207b332fda4779a5bd497f380a2b48~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/10207b332fda4779a5bd497f380a2b48~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 清冷的色调，雪花飘落的冬天山谷中，近景拍摄一位明艳动人的年轻白人美女侧身扭头看向镜头。
- 她有着黑色的波浪卷长发，下巴尖尖的，眉毛浓密上挑，眼窝深邃，眼瞳是红色的，化着深色眼影和上挑的眼线，鼻梁挺直，嘴唇偏厚，涂着非常鲜艳的红唇，下颌线清晰，指甲非常长，做着红色美甲。
- 女子身着一件黑袍，戴着帽子，领口微敞着锁骨清晰，眼睛盯着镜头，眼神十分勾人。
- 背景是覆盖着厚雪的绿色植被，雪花在空中飘落。镜头向左微微环绕女人拍摄，女人抬起右手放在下巴上，看着镜头露出妩媚的笑容。 | 精准生成多主体复杂动作
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/34b112e6df6f4406bae3251d7e37d0f5~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/34b112e6df6f4406bae3251d7e37d0f5~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 旋转镜头，三只长得一模一样的猿猴围成一个圈，一个用手捂住眼睛，一个用手捂住耳朵，一个用手捂住嘴巴。 | 文本嵌入响应准确
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5dab81615f9f49e39b7496df4039b81e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5dab81615f9f49e39b7496df4039b81e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 一个由火焰组成的“FIRE”字母在空中悬浮着，一阵风吹过，火焰烧地更加猛烈最终失去控制充满整个画面

* ### 丝滑连贯的首尾帧过渡效果 只需指定视频的起始和结束图片，模型即可生成流畅衔接首、尾帧的视频，实现画面间自然、连贯的过渡效果，满足用户对视频精准控制的需求。

- 视频效果 | 输入
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c0a0c8926df34980b5145716e483658a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c0a0c8926df34980b5145716e483658a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 首帧图片
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5f8310c5d7af43dfaf552b82ce7d90c7~tplv-goo7wpa0wc-image.image)
- 尾帧图片
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/05bca24abe1549328b4ab3941ddc9514~tplv-goo7wpa0wc-image.image)
- 【prompt】
- 一只蓝绿精卫鸟变成人形

* ### **专业的影视级运镜控制** 运镜能力强，支持环绕、航拍、变焦、平移、跟随、手持等多种镜头语言。

- 视频生成示例
- 环绕
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7a76162d61a348049104b8418343c5b4~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7a76162d61a348049104b8418343c5b4~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 一个挂满灯笼的中式庭院中，站着一个长着泪痣、穿白色婚纱的亚洲女孩。镜头环绕拍摄，最后对准女孩面部，她突然抬起头，嘴角露出神秘的微笑。 | 变焦
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d1d23b1bf98a4f18838f3fe3b79bd6f1~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d1d23b1bf98a4f18838f3fe3b79bd6f1~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 写实风格，晴朗的蓝天之下，一大片白色的雏菊花田，镜头逐渐拉近，最终定格在一朵雏菊花的特写上，花瓣上有几颗晶莹的露珠
- 平移
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d8408508faef49fcbd93b4e631c5f3c0~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d8408508faef49fcbd93b4e631c5f3c0~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 神庙的深处，一个背着背包的男人找到了一位古代智者的雕像。镜头向左移动，雕像手中握着一本古老的书籍，似乎在守护着某种重要的知识。 | 跟随
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7e17534f05164697a337c19f4f046411~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7e17534f05164697a337c19f4f046411~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 荒凉的戈壁环境，镜头跟随一位驾驶哈雷摩托的男性，特写镜头，骑手的额头绑着土黄色的头巾，身着蓝色和银色条状装饰的皮质骑手服，露出狂野的笑容后，镜头突然向上摇，有一只秃鹫在上空盘旋。
- 航拍
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/72561a1ef4cf42aa83e5a884eaf0ff14~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/72561a1ef4cf42aa83e5a884eaf0ff14~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 慢镜头拍摄，广角镜头缓慢穿越亚马逊河流的上空，河流两侧的雨林清晰可见 | 手持
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/91ce7eab09e0441692078a75f409a706~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/91ce7eab09e0441692078a75f409a706~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 手持镜头，画面微微抖动体现手持感，跟随在一只在玫瑰花园中散步的猫身侧

* ### **丰富、自然的风格** 文生视频可直出各种风格，图生视频可丝滑兼容各种风格的首图。

- 视频生成示例
- 国漫
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29ec1410a1094583baf6bc81a8a72e4a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29ec1410a1094583baf6bc81a8a72e4a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 水墨
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/828f619a8f674cfca17229d0fed392b4~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/828f619a8f674cfca17229d0fed392b4~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 水彩
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2c3b87f10b64bbba10b4d70e1b72d44~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2c3b87f10b64bbba10b4d70e1b72d44~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 日漫
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42329c407ba747159456a4403fe3c87e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42329c407ba747159456a4403fe3c87e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 美漫
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d6da76517e6a46bf922add01ebe4d7bd~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d6da76517e6a46bf922add01ebe4d7bd~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 剪纸
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/db3bf020854b47a484a00ec04a24f668~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/db3bf020854b47a484a00ec04a24f668~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 体素
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfb6dd80e6034445bac0d70991199bde~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfb6dd80e6034445bac0d70991199bde~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 毛毡
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/52e14d86d02244099a2e2d5b43629672~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/52e14d86d02244099a2e2d5b43629672~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 线稿
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/723ebbb346524b259f5e20bfcd2555f3~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/723ebbb346524b259f5e20bfcd2555f3~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>

* ### **影调细腻的超清画质** 提供 480p、720p、1080p 三种分辨率，质感细腻，影调丰富，可以适配大屏幕。

- 视频生成示例
- 480p
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bd3030024ac84ffbb1f5c50b1362dc7e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bd3030024ac84ffbb1f5c50b1362dc7e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 720p
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f0ef5383e646477a8f1ad872eb2911f2~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f0ef5383e646477a8f1ad872eb2911f2~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 1080p
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7c8a839d102b44829ad040f4c003ef2a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7c8a839d102b44829ad040f4c003ef2a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 1080p
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3927d606c2dc4d1caac355683fee5640~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3927d606c2dc4d1caac355683fee5640~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>

* ### 多参考图像视频生成 支持输入1-4张图像作为参考，模型能精准捕捉并提取不同对象（如：人物、虚拟形象、动物、商品、服饰、环境等）的核心特征，将其自然、协调地融入生成的视频

- 视频生成示例
- 【prompt】
- [图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2637ac87f1e64bd897bfc651fe7d0386~tplv-goo7wpa0wc-image.image)
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9450c9444b574112a9f228db9e81cdf4~tplv-goo7wpa0wc-image.image)
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/574b8785f4b740ddaff791655e8633ba~tplv-goo7wpa0wc-image.image)
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f697bb45f2fd4c6f84a7f0c5c2f9e703~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f697bb45f2fd4c6f84a7f0c5c2f9e703~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 蓝发少年高冷的走在前面，粉发女孩在后面紧紧跟着
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/516709990e3a4189807f35b3f6c43c3b~tplv-goo7wpa0wc-image.image)
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bf556c3c33ef400fb361b5882d466fa5~tplv-goo7wpa0wc-image.image)
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ff2a400535c04c939b583dd28dfa03e1~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ff2a400535c04c939b583dd28dfa03e1~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 年轻骑士牵着马，向前走来
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a0b01a7b05464ec2b055ef4488579d50~tplv-goo7wpa0wc-image.image)
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4b0713c683244fd2b38782d0198e8930~tplv-goo7wpa0wc-image.image)
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0ca888b004f24f1f902beb6d183b4e73~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0ca888b004f24f1f902beb6d183b4e73~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
- 【prompt】
- 女生身着薄荷绿长裙，面带微笑向镜头打招呼，展示新装的魅力
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/22a172a88aaa4fd58f84f827e9b70062~tplv-goo7wpa0wc-image.image)
- ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/59f54d90366342df9b94876fb7b52066~tplv-goo7wpa0wc-image.image)
- <BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2b2562ab19944337afcc30074e2c90d1~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2b2562ab19944337afcc30074e2c90d1~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>

## 模型价格

文生视频与图生视频同价，10 元/百万 token。支持多种视频规格，不同规格视频的单价详见 [视频生成模型](/docs/82379/1544106#02affcb8)。
## 模型版本
doubao-seedance-lite

* doubao-seedance-1-0-lite-t2v-250428：根据您输入的文本提示词 + 参数（可选）生成目标视频。
* doubao-seedance-1-0-lite-i2v-250428：
   * 根据您输入的首帧图片 + `new`尾帧图片（可选）+文本提示词（可选）+ 参数（可选）生成目标视频。
   * 根据您输入的`new`参考图片（1-4张）+文本提示词（可选）+ 参数（可选）生成目标视频。

## 模型限流

* RPM 限流：300

> 账号下同模型（区分模型版本）每分钟允许创建的任务数量上限。若超过该限制，创建视频生成任务时会报错。

* 并发数限制：5

> 账号下同模型（区分模型版本）同一时刻在处理中的任务数量上限，超过此限制的任务将进入队列等待处理。

## 使用文档
> 视频生成为异步接口，您需要先创建视频生成任务，再通过视频生成任务的 ID 去查询视频生成结果。

<a href="https://www.volcengine.com/docs/82379/1366799">视频生成</a>

模型调用教程

供您了解如何调用该模型，包括参数如何配置以及一些典型使用示例代码，您可以基于此进行扩展。

<a href="https://www.volcengine.com/docs/82379/1520758">视频生成 API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

<a href="https://www.volcengine.com/docs/82379/1587797">Seedance 提示词指南</a>

文生视频和图生视频的提示词（prompt）使用技巧，帮助您快速上手视频创作，将创意转化为视频内容。
