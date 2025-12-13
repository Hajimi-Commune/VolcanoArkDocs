# Seedance-1.0-lite 提示词指南
本文介绍 Seedance-1.0-lite 文生视频和图生视频的提示词（prompt）使用技巧，帮助您快速上手视频创作，将创意转化为视频内容。
## 模型简介
**Seedance 1.0** 是字节跳动豆包大模型团队最新推出的视频生成基础模型系列。**Seedance 1.0 lite** 作为该模型系列的小参数量版本，在取得出色的视频生成质量的同时，显著提升了生成速度，是兼顾效果与效率的性价比之选。
## 提示词参数
[创建视频生成任务 API ](https://www.volcengine.com/docs/82379/1520757)中，跟提示词有关的参数如下：
content.**text**：输入给模型的文本内容，描述期望生成的视频，包括：

   * **提示词（必填）**：支持中英文。
   * [参数（选填）](https://www.volcengine.com/docs/82379/1366799#%E6%A8%A1%E5%9E%8B%E6%96%87%E6%9C%AC%E5%91%BD%E4%BB%A4%E6%AF%94%E8%BE%83)：在文本提示词后追加--[parameters]，控制视频输出的规格。本文主要用到的有：
      * resolution `简写 rs`：分辨率
      * duration `简写 dur`：生成视频时长（秒）
      * camerafixed `简写 cf`：是否固定摄像头

```Plain Text
{
    "model": "doubao-seedance-1-0-lite-i2v-250428",
    "content": [
        {
            "type": "text",
            "text": "女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --rs 720p --dur 5 --cf false"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png"
            }
        }
    ]
}
```

## 图生视频提示词技巧
### 基础提示词
:::tip
**提示词 = 主体 + 运动， 背景 + 运动，镜头 + 运动 ...**
:::

1. **基础结构**：图生视频已经有了场景，因此**尽量减少（甚至避免）对静止/无变化部分的描述**，在**明确指出运动对象**的情况下，**多描述运动的部分**，包括主体的运动、背景的运动/变化、以及镜头的运动。
2. **简单直接**：**尽量使用简单词语和句子结构**，模型会根据我们的表达与对图像画面的理解进行提示词扩写,生成符合预期的视频。
3. **特征描述**：当主体具有一些突出特征时，可以加上突出特征来更好定位主体，比如老人， 戴墨镜的女人等。描述运动时，**关键的程度副词一定要明确**，比如快速、幅度大。
4. **遵从图片**：需要基于输入的图片内容来写，需要明确写出主体以及想做的动作或者运镜， **需注意提示词不要与图片内容/基础参数存在事实矛盾**。比如图片中是一个男人，提示词写“一个女人在跳舞”；比如背景是草原，提示词写“男人在咖啡厅里唱歌”；比如手上没有饰品，提示词写“带饰品的那只手”；比如基础参数选择了固定镜头，却在提示词里写了镜头环绕。
5. **负向提示词不生效** ：模型不响应负向提示词。

| | | | \
|**输入参数** |中间结果（不显示） |**生成视频（单主体+单动作）** |
|---|---|---|
| | | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d347d7d7c6914bf1b33bbed2541b5401~tplv-goo7wpa0wc-image.image =1280x) |\
|**prompt**：老人戴上眼镜 |\
|**基础参数**：固定镜头，720p，10s |> 根据输入图片，模型会获取到场景信息：*冷色调，特写镜头拍摄一位满脸胡须的中老年白人男子，他眉头紧皱，眼睛看向画面右侧，眼神凶狠，表情严肃。他留着灰白的络腮胡，脸上有皱纹，他的眼睛大而深邃，鼻梁高挺，鼻孔突出，眼周皱纹和法令纹明显，身着浅色上衣。背景是模糊的。* |\
| | |\
| | |\
| |> 根据prompt，模型会进行合理扩写：*在室内环境中，一个老年男人脸部的特写。他皱着眉看向画面右侧，然后抬起双手将一副眼镜架在鼻梁上。* | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dede8464735547f1ad744be6c6d580dd~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dede8464735547f1ad744be6c6d580dd~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |

### 多个连续动作提示词
模型对多拍动作质量有着强响应，支持时序性的多个连续动作，以及多个主体的不同动作。可以尝试写:
:::tip
**提示词 = 主体1 + 运动1  + 运动2**
**提示词 = 主体1 + 运动1  + 主体2 + 运动2 ...**
:::
依次列举即可，模型会根据我们的表达与对图像画面的理解进行提示词扩写，生成符合预期的视频。

| | | \
|**输入参数** |**生成视频** |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e0a95adb321f4d57ac2a4ee35dfcff5c~tplv-goo7wpa0wc-image.image =864x) |\
|**prompt**：打太极，镜头环绕，聚焦脸部 |\
|**基础参数**：不固定镜头，720p，5s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/62d356da4bb84776a4b7266fb6f82b4a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/62d356da4bb84776a4b7266fb6f82b4a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/746eece3d1b6427e8f1961c87ea2cfa1~tplv-goo7wpa0wc-image.image =832x) |\
|**prompt**：转过脸对着镜头向前走，然后停下，脸上露出愤怒的表情，然后叉腰 |\
|**基础参数**：不固定镜头，720p，10s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5d3be8c30c7345e3b408050ad8c97c09~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5d3be8c30c7345e3b408050ad8c97c09~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0b4d75b28081422c8d1ef6e89ffc7ce6~tplv-goo7wpa0wc-image.image =832x) |\
|**prompt**：镜头聚焦于背景的老师，前景的女孩变得模糊，老师非常生气的骂人 |\
|**基础参数**：不固定镜头，720p，5s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/49907383a22a4d4987f78e4b2b6247ea~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/49907383a22a4d4987f78e4b2b6247ea~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a42b96b1fda9482b814bf86261348247~tplv-goo7wpa0wc-image.image =1120x) |\
|**prompt**：**镜头下摇**，从女人脸上拉到她的正面全身全景，她在对着镜头打了个招呼后，走出画面 |\
|**基础参数**：不固定镜头，720p，10s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5e092452059d4f0692f2ed72fe4f7ed0~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5e092452059d4f0692f2ed72fe4f7ed0~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c48023ed49754cd384a725356b44cca4~tplv-goo7wpa0wc-image.image =1120x) |\
|**prompt**：女人一边哭泣一边喝酒，一个男人走进来安慰她 |\
|**基础参数**：不固定镜头，720p，5s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b606fba018094b028062f2c842bfec58~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b606fba018094b028062f2c842bfec58~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |

### 运镜提示词
> 包括镜头切换

可以在提示词中使用自然语言描述你想要的镜头变化，支持环绕、航拍、变焦、平移、跟随、手持等运镜，以及镜头切换。镜头语言响应是seedance 1.0的强项。

1. 在写一致性多镜头的提示词时，需要**写出镜头之间内在的联系**。
2. 镜头的变化，**通过“镜头切换”这一明确提示词来进行连接**。
3. 切镜后如果场景发生了变化，需要**对新的场景进行描述**。
4. 有运镜提示词时，基础参数要选择“不固定镜头”。
5. 运镜提示词在文生视频场景也同样适用。

| | | | | \
|运镜 |提示词 |**输入参数** |**生成视频** |
|---|---|---|---|
| | | | | \
|**镜头切换** |**镜头切换** |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3537d1f1a0884de3a2eec797168507b3~tplv-goo7wpa0wc-image.image =1280x) |\
| | |**prompt**：小猫和小狗吃猫粮，**镜头切换**到特写猫粮颗颗分明。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/89adb3cb681f41ec9bf66bf5a8e93b39~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/89adb3cb681f41ec9bf66bf5a8e93b39~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
|^^|^^| | | \
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/137fdf84b0c14f0eacac391d77454b2d~tplv-goo7wpa0wc-image.image =2048x) |\
| | |**prompt**：手表指针匀速转动，**镜头切换**，男人扬起手扶了一下自己的金丝眼镜，柔和的光线打在表盘上 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/27f1a7d848664696b245af1a2445a82a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/27f1a7d848664696b245af1a2445a82a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
|^^|^^| | | \
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6358ef56130c4e1c963957e60b8449ad~tplv-goo7wpa0wc-image.image =956x) |\
| | |**prompt**：全景拍摄，模特微笑着走近镜头。**镜头切换**，近景拍摄模特下半身，裤子的直筒设计和布料垂坠感在行走中得到突出。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/928b3c2a8f4d4a5595a5fbeff20d7260~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/928b3c2a8f4d4a5595a5fbeff20d7260~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
| | | | | \
|平移 |**镜头向上/下/左/右移动** |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b7edf59eaa39402b97a984130be06c35~tplv-goo7wpa0wc-image.image =1456x) |\
| | |**prompt**：**镜头缓慢向下移动**，到街道，路灯。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/505c57942101480d8350e1564bec3e79~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/505c57942101480d8350e1564bec3e79~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
|^^|^^| | | \
| | |**prompt**：神庙的深处，一个背着背包的男人找到了一位古代智者的雕像。**镜头向左移动**，雕像手中握着一本古老的书籍，似乎在守护着某种重要的知识。 |\
| | |**基础参数**：不固定镜头，视频比例16:9 ，720p，5s |\
| | | | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4c21f626f9cf40bb928e815c711cfdac~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4c21f626f9cf40bb928e815c711cfdac~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |\
| | | | |\
| | | | |
| | | | | \
|变焦 |**镜头拉远** |\
| |**镜头推近** |\
| |**镜头聚焦** |\
| |**特写**  |\
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b474ffe9a4644f94b13b53e8b95c3881~tplv-goo7wpa0wc-image.image =612x) |\
| | |**prompt**：一家人在露营，欢快的氛围，镜头**特写**到帐篷，360度展示帐篷全景。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cb63b41665724f5aafd575577ef02622~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cb63b41665724f5aafd575577ef02622~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |\
| | | | |
| |^^| | | \
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/88667374a218458db53cc77f4e6d8589~tplv-goo7wpa0wc-image.image =2048x) |\
| | |**prompt**：这是一个雕塑家的双手在一块大理石上凿刻时颤抖的**特写**镜头，然后**镜头拉远**，看到是一个小孩雕塑家，他脸上带着自信的微笑。 |\
| | |**基础参数**：不固定镜头，720p，5s |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0c001ee5e8c04dad80e40676b3022123~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0c001ee5e8c04dad80e40676b3022123~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |
| | | | | \
|环绕 |**镜头环绕360度展示** |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/702690afd0d14775bd3a4af990551162~tplv-goo7wpa0wc-image.image =900x) |\
| | |**prompt**：**镜头环绕运镜** |\
| | |**基础参数**：不固定镜头，720p，5s |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfc2680c6cc041f7accc7e42914931c6~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfc2680c6cc041f7accc7e42914931c6~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |
|^^|^^| | | \
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/343fb36770334fef821fee54922d8c9a~tplv-goo7wpa0wc-image.image =1192x) |\
| | |**prompt**：**环绕镜头**，缓慢推进至人物的脸部特写 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/84ec1ae27ac1455f88ced0b3192ea955~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/84ec1ae27ac1455f88ced0b3192ea955~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
| | | | | \
|航拍 |**航拍** |\
| |**广角** |\
| | |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/52deab7feaa7483482578354e0734c45~tplv-goo7wpa0wc-image.image =2973x) |\
| | |**prompt**：男人张开双臂，**航拍**环绕镜头。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/41c176e1a610498ba2a25e9f1675d053~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/41c176e1a610498ba2a25e9f1675d053~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
| | | | | \
|**手持** |**手持镜头** |\
| |**微微抖动** |\
| | |**prompt**：**手持镜头**，画面**微微抖动**体现手持感，跟随在一只在玫瑰花园中散步的猫身侧 |\
| | |**基础参数**：不固定镜头，视频比例16:9 ，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d19667f7160b47b2a99847f06f0ce418~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d19667f7160b47b2a99847f06f0ce418~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |
| | | | | \
|跟随 |**镜头跟随** |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bf9543b8d3bc498da14cfdb982a83560~tplv-goo7wpa0wc-image.image =1248x) |\
| | |**prompt**：**镜头跟随**一位驾驶哈雷摩托的男性，特写镜头，骑手露出狂野的笑容，镜头突然向上摇，有一只秃鹫在上空盘旋。 |\
| | |**基础参数**：不固定镜头，720p，5s | |\
| | | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/da32742b34d647fba6347e34065dab48~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/da32742b34d647fba6347e34065dab48~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | | |\
| | | | |

### 程度副词
如果想要在视频中突出动作频率与强度，或者主体的特征，合理使用程度副词。

1. **明确**：模型无法从输入的参考图上获取到运动的程度，所以一定要在prompt中明确，否则模型会根据它自己的理解去补充，可能跟用户的意图背离。如“汽车驶过”改为“汽车快速驶过”。
2. 可适当**夸大**程度，增强视频的表现力：如“男子咆哮”改为“男子疯狂咆哮”，“翅膀扇动”，改为“翅膀大幅度扇动”，会更容易接近想要的效果。

:::tip
**程度提示词：快速 剧烈 大幅度 高频率 强力 疯狂 ...**
:::

| | | \
|**输入参数** |**生成视频** |
|---|---|
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5606a0387ce94961a06d017c46011ab2~tplv-goo7wpa0wc-image.image =2912x) |\
|车辆**快速**往前开，穿过时空通道，光影流动，镜头**快速**跟随 |\
|**基础参数**：不固定镜头，720p，5s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/70bd4af9bb0b4b3fab464d0299e53313~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/70bd4af9bb0b4b3fab464d0299e53313~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |
| | | \
|![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a5cfeb0bb0554fbe81a9694f541761f4~tplv-goo7wpa0wc-image.image =1024x) |\
|一个身着专业运动服的运动员。双腿**快速**交替，双臂**有力**摆动，在赛场上全力冲刺，在他冲过终点线后，观众欢呼。 画面采用跟拍视角，充分展示运动员冲过终点线的细节。 |\
|**基础参数**：不固定镜头，720p，5s | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6477e2d611f44940a1606711e8a1323e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6477e2d611f44940a1606711e8a1323e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |\
| | |

## 文生视频提示词技巧
### 基础提示词
:::tip
**提示词 = 主体 + 运动 + 场景 + 镜头、风格...**
:::

1. 主体+运动+场景是最核心和基本的要素，模型会进行提示词扩写,生成符合预期的视频。
2. 参考图生视频，**连续动作、运镜、程度副词**等的提示词指南同样对文生视频有效果，负向提示词不响应。
3. 如何更好的描述你需要的内容：
   1. **详细的人物描写**：注重人物的外表、着装、姿势。
   2. **环境细节的呈现**：自然环境（如山脉、沙漠、瀑布等）或者建筑环境（如工作室、浴室等）的详细描述，能帮助你强调场景的视觉和感觉体验。
   3. **情绪与动态的结合**：通过描绘人物的情绪状态和环境动态，创造了层次丰富的叙事。
   4. **氛围的渲染**：通常我们在视觉呈现上有一些渲染氛围的技巧。例如可以运用对光线的描述：黄昏、清晨、昏暗、温暖的光等

| ||| \
|T2V | | |
|---|---|---|
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5f6a8c08eb434e4d9fbb200d86f5d389~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5f6a8c08eb434e4d9fbb200d86f5d389~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|**Prompt**：具有设计感的人像摄影，迷幻清冷的淡蓝色调，蝴蝶光，近景拍摄一位年轻的白人女性。她有着高层次的黑色短发，右边的眉毛上挑，睫毛浓密，鼻梁高挺，咬着红唇，表情不屑地瞪着镜头。镜头后拉，前景是破碎的玻璃在空中，它挡住了女子的部分面部。 |\
|**基础参数**：不固定镜头，720p，比例 16:9，5s |\
| | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d72b9dcec4774d778ada9f3abaad2e51~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d72b9dcec4774d778ada9f3abaad2e51~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |**Prompt**：中近景拍摄一位留着凌乱的黑色短发的年轻男子在夜晚吃鸡腿。他看起来有些狼狈，脸脏脏的，肿眼泡，下颌圆润，鼻子上有几颗黑痣，有些许胡渣，牙齿有些泛黄，眼睛看向画面左侧，有些失神，身着蓝灰色的破旧风衣，袖口和衣服上沾着许多脏污。男子拿着鸡腿靠近嘴边，咬了一口鸡腿，随后直勾勾的看着前方，露出猥琐的笑容。手指和手心都有污渍。背景是虚化的城市夜晚景象，有黄蓝色的灯光 |\
| |**基础参数**：固定镜头，720p，比例 4:3，5s | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bd7c449401f74e2887d466fa489ae5c4~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bd7c449401f74e2887d466fa489ae5c4~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |清冷的色调，雪花飘落的冬天山谷中，近景拍摄一位明艳动人的年轻白人美女侧身扭头看向镜头。她有着黑色的波浪卷长发，下巴尖尖的，眉毛浓密上挑，眼窝深邃，眼瞳是红色的，化着深色眼影和上挑的眼线，鼻梁挺直，嘴唇偏厚，涂着非常鲜艳的红唇，下颌线清晰，指甲非常长，做着红色美甲。女子身着一件黑袍，戴着帽子将眉毛遮住，领口微敞着锁骨清晰，眼睛盯着镜头，眼神十分勾人。背景是覆盖着厚雪的绿色植被，雪花在空中飘落。镜头向左微微环绕女人拍摄，女人抬起右手放在下巴上，看着镜头露出妩媚的笑容。 |\
| | |**基础参数：​**不固定镜头，720p，比例 9:16，5s |

### 风格响应词

| ||| \
|视频生成示例 | | |
|---|---|---|
| | | | \
|国漫 |\
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29ec1410a1094583baf6bc81a8a72e4a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29ec1410a1094583baf6bc81a8a72e4a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |水墨 |\
| | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/828f619a8f674cfca17229d0fed392b4~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/828f619a8f674cfca17229d0fed392b4~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |水彩 |\
| | | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2c3b87f10b64bbba10b4d70e1b72d44~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a2c3b87f10b64bbba10b4d70e1b72d44~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |
| | | | \
|日漫 |\
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42329c407ba747159456a4403fe3c87e~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/42329c407ba747159456a4403fe3c87e~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |美漫 |\
| | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d6da76517e6a46bf922add01ebe4d7bd~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d6da76517e6a46bf922add01ebe4d7bd~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |剪纸 |\
| | | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/db3bf020854b47a484a00ec04a24f668~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/db3bf020854b47a484a00ec04a24f668~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |
| | | | \
|体素 |\
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfb6dd80e6034445bac0d70991199bde~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bfb6dd80e6034445bac0d70991199bde~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |毛毡 |\
| | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/52e14d86d02244099a2e2d5b43629672~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/52e14d86d02244099a2e2d5b43629672~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |线稿 |\
| | | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/723ebbb346524b259f5e20bfcd2555f3~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/723ebbb346524b259f5e20bfcd2555f3~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |

## 常见问题案例
### 问题1：视频肢体崩坏如何解决
模型有一定肢体崩坏的概率，可以多试几次**抽卡**选择效果好的视频，或者通过prompt避开展示手/脚部的镜头。
### 问题2：图生视频的结果没有遵循重要的程度副词

* **问题案例**
   
   | | | \
   |输入图片 |原始 prompt 与视频效果 |\
   | | |
   |---|---|
   | | | \
   |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/500905e504134eb4bc9572bd38d018e7~tplv-goo7wpa0wc-image.image =2048x) |\
   | |大幅度运动，仰拍，镜头缓慢跟随，旋转绕行，在暴风雪笼罩的冰川上空，一只巨大的冰霜丧尸骸骨龙在风雪中展翅飞翔，展开巨大的腐烂的骸骨双翼，身体散发出幽蓝的光芒，蓝色的能量从它的身体和翅膀上流动，风雪在它周围狂卷，压迫感，震撼，恢弘大气，氛围光照 |\
   | | |\
   | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4a3472df26824a61a7f607980caa4075~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4a3472df26824a61a7f607980caa4075~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
   | | |

* **问题分析**
   prompt中太多描述是参考图已有的信息，模糊了重点。需要删除不必要的内容，并强调重要的程度副词。
* **优化后效果**
   
   | | | \
   |输入图片 |优化后 prompt 与视频效果 |
   |---|---|
   | | | \
   |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/500905e504134eb4bc9572bd38d018e7~tplv-goo7wpa0wc-image.image =2048x) |\
   | |一只龙**快速**的拍动翅膀，翅膀运动的**幅度很大**。仰拍，镜头旋转绕行。压迫感 |\
   | | |\
   | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5adfccebb46142b287fa919b0083f771~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5adfccebb46142b287fa919b0083f771~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
   | | |

### 问题3：图生视频的结果中有些地方不符合要求

* **问题案例**
   
   | | | \
   |输入图片 |原始 prompt 与视频效果 |\
   | | |
   |---|---|
   | | | \
   |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0040732412cb49f987482701429750bb~tplv-goo7wpa0wc-image.image =1024x) |\
   | |画面中的男孩带着耳机陶醉地欣赏音乐，头随着音乐微微摆动，镜头拉近画面中模特带着耳机的耳朵，展示耳机的舒适。 |\
   | | |\
   | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1061b00f356d410eb8c93ad16af874a5~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1061b00f356d410eb8c93ad16af874a5~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
   | | |

* **问题分析**
   prompt看起来没有问题，但眼神有点怪。需在prompt中重新强调眼神。
* **优化后效果**
   
   | | | \
   |输入图片 |优化后 prompt 与视频效果 |
   |---|---|
   | | | \
   |![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f663f0747ee747b6833a0caf8c9c6a72~tplv-goo7wpa0wc-image.image =1024x) |\
   | |画面中的男孩带着耳机陶醉地欣赏音乐，**他眼神自然**，头随着音乐微微摆动，镜头拉近画面中模特带着耳机的耳朵，展示耳机的舒适 |\
   | | |\
   | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/50b07785bb964c97911c3133e33ba5b7~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/50b07785bb964c97911c3133e33ba5b7~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
   | | |