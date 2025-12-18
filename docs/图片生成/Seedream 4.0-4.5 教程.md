# Seedream 4.0-4.5 教程
Seedream 4.0-4.5 原生支持文本、单图和多图输入，实现基于主体一致性的多图融合创作、图像编辑、组图生成等多样玩法，让图像创作更加自由可控。本文以 Seedream 4.5 为例介绍如何调用 <a href="https://www.volcengine.com/docs/82379/1541523">Image generation API</a> 进行图像创作。如需使用 Seedream 4.0 模型，将下文代码示例中的 model 字段替换为`doubao-seedream-4-0-250828`即可。 
# 模型效果
更多效果示例见 [效果预览](https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-seedream-4-5)。

---

场景

输入

输出

多参考图生图
> 输入多张参考图，融合它们的风格、元素等特征来生成新图像。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2198d4bef000400bbfea18025850ed82~tplv-goo7wpa0wc-image.image)

> 将图1的服装换为图2的服装

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/461d4bf2a014454fbeda72f27d706ffe~tplv-goo7wpa0wc-image.image)

---

组图生成
> 基于用户输入的文字和图片，生成一组内容关联的图像

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a215e8241dd94f50901948790da121e1~tplv-goo7wpa0wc-image.image)

> 参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/98c9e2c30dbb425aa25380c821e289ca~tplv-goo7wpa0wc-image.image)

# 模型选择

* Seedream 4.5 作为字节跳动最新的图像生成模型，能力最强，在编辑一致性（如主体细节与光影色调的保持）、人像美化和小字生成方面体验升级。同时，模型的多图组合能力显著增强，推理能力与画面美学持续优化，能够更精准、更具艺术感地呈现创意。
* Seedream 4.0 图像生成模型，适用于平衡预算与图片输出质量的场景，能满足一般性的图像生成需求。

- **模型名称**
- **版本**
- **模型 ID（Model ID）**
- **模型能力**
- **限流 IPM**
- > 每分钟生成图片数量上限（张 / 分钟）
- **定价**
- [doubao-seedream-4.5](https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-seedream-4-5) | 251128`强烈推荐` | doubao-seedream-4-5-251128 | * 文生图
- * 图生图：单张图生图、多参考图生图
- * 生成组图：文生组图、单张图生组图、多参考图生组图 | 500 | [图片生成模型](/docs/82379/1544106#5e813e2f)
- [doubao-seedream-4.0](/docs/82379/1824718) | 250828`推荐` | doubao-seedream-4-0-250828 | 500 | [图片生成模型](/docs/82379/1544106#5e813e2f)

# 前提条件

* [获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 。
* [开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)。
* 在[模型列表](/docs/82379/1330310)获取所需 Model ID 。
   * 通过 Endpoint ID 调用模型服务请参考[获取 Endpoint ID（创建自定义推理接入点）](/docs/82379/1099522)。

# 快速体验
您可在火山方舟平台 <a href="https://api.volcengine.com/api-explorer/?action=ImageGenerations&groupName=%E5%9B%BE%E7%89%87%E7%94%9F%E6%88%90API&serviceCode=ark&tab=2&version=2024-01-01#N4IgTgpgzgDg9gOyhA+gMzmAtgQwC4gBcIArmADYgA0IUAlgF4REgBMA0tSAO74TY4wAayJoc5ZDSxwAJhErEZcEgCMccALTIIMyDiwaALBoAMG1gFYTADlbWuMMHCwwCxQPhmgUTTA-l6Ao2MAw-4CLeYB4tkHBgDOJgE2KgF+KgABygGHxgNf6gPSmgN2egCwegHEegCFugLCagCfKgOhKgGbx-oBFRoBjkYCTkZGA34qA2Ur+gKyugI76gOSagOJO-oDU5oCnpoBHphWA+Ib+gBVKI4Cf2oAr1oBOQf5wAMaATHaAy+b+gJKKgP1+gL-xgFRxY4CABoCEVoBTPv6A9maAj7b+gKGxgA3OgHnagNxygJJy-peAuyH+gNyugEbpgFgJgHH4wBjfoBvQOygAY5QAz2tkZoBLfUAQjqAQmtAIoagAIEp6AZXlAHBygC51c7+QAUsUNAPjuD38gHSzQKAOYzADMB52y6xagAlTQA55oBSELR0UA2DaAF7V-IAXU0xgB9FQDuioAvIMA9OaAbz1AM8GI0AHJqAAn1soB-PUAS5GAeASKmz-IAAAPW-kAs8qAEB1-IBA80AL4GMlr+QBc+oBUfUagDwVQA2aiAAL5AA">API Explorer</a>，快速体验图片生成功能，支持自定义参数（例如设置图片水印、控制输出图片大小等），方便您直观感受其效果和性能。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4d9946df9dfe4011af999c694f9545fe~tplv-goo7wpa0wc-image.image =100%x)

# 基础使用
## 文生图（纯文本输入单图输出）
通过给模型提供清晰准确的文字指令，即可快速获得符合描述的高质量单张图片。 

---

提示词

输出

充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/00fb66006eb84b16965b620b6e1f2d78~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
    "size": "2K",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]' .
from volcenginesdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
)
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128",
    prompt="充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
    size="2K",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        // Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
                
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("doubao-seedream-4-5-251128") // Replace with Model ID
                .prompt("充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。")
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        // Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128", // Replace with Model ID
       Prompt:         "充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v
", err)
       return
    }

    fmt.Printf("%s
", *imagesResponse.Data[0].Url)
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128",
    prompt="充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": false,
    },
) 
 
print(imagesResponse.data[0].url)
```

## 图文生图（单图输入单图输出）
基于已有图片，结合文字指令进行图像编辑，包括图像元素增删、风格转化、材质替换、色调迁移、改变背景/视角/尺寸等。

---

提示词

输入图

输出

保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/816153e67d3c4478886276154d78b22e~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0829972712544f95917464b15723b189~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。",
    "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png",
    "size": "2K",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
)
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128", 
    prompt="保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。",
    image="https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png",
    size="2K",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();

        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("doubao-seedream-4-5-251128") // Replace with Model ID
                .prompt("保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。")
                .image("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png")
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
                
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。",
       Image:          volcengine.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png"),
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v
", err)
       return
    }

    fmt.Printf("%s
", *imagesResponse.Data[0].Url)
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

imagesResponse = client.images.generate( 
    model="doubao-seedream-4-5-251128",
    prompt="保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。",
    size="2K",
    response_format="url",
    extra_body = {
        "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png",
        "watermark": false
    }
) 

print(imagesResponse.data[0].url)
```

## 多图融合（多图输入单图输出）
根据您输入的文本描述和多张参考图片，融合它们的风格、元素等特征来生成新图像。如衣裤鞋帽与模特图融合成穿搭图，人物与风景融合为人物风景图等。

---

提示词

输入图1

输入图2

输出

将图1的服装换为图2的服装

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4b4464161cf3463db6f9463b10939178~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c23d1b0528a14cb08b684307eabdcc9b~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/461d4bf2a014454fbeda72f27d706ffe~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "将图1的服装换为图2的服装",
    "image": ["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png"],
    "sequential_image_generation": "disabled",
    "size": "2K",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128",
    prompt="将图1的服装换为图2的服装",
    image=["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png"],
    size="2K",
    sequential_image_generation="disabled",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();

        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("doubao-seedream-4-5-251128") // Replace with Model ID
                .prompt("将图1的服装换为图2的服装")
                .image(Arrays.asList(
                    "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png",
                    "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png"
                ))
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "将图1的服装换为图2的服装",
       Image:         []string{
           "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png",
           "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png",
       },
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v
", err)
       return
    }

    fmt.Printf("%s
", *imagesResponse.Data[0].Url)
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="doubao-seedream-4-5-251128",
    prompt="将图1的服装换为图2的服装",
    size="2K",
    response_format="url",
    
    extra_body = {
        "image": ["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png"],
        "watermark": false,
        "sequential_image_generation": "disabled",
    }
) 
 
print(imagesResponse.data[0].url)
```

## 组图输出（多图输出）
支持通过一张或者多张图片和文字信息，生成漫画分镜、品牌视觉等一组内容关联的图片。
需指定参数 **sequential_image_generation** 为`auto`。
### 文生组图

---

提示词

输出（实际会输出4张图片）

生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9df00a4f19e84efc9946f7033a4cf80d~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark 
from volcenginesdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation .
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128", 
    prompt="生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("doubao-seedream-4-5-251128")  // Replace with Model ID
                 .prompt("生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }

            service.shutdownExecutor();
        }
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v
", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s
", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:
", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s
", i+1, image.Size, url)
    }
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation .
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="doubao-seedream-4-5-251128",
    prompt="生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": false,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
    },
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

### 单张图生组图

---

提示词

输入图

输出（实际会输出4张图片）

参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c724450228a94a909580c0400fbf503b~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f2d33219328149f58552dfd095b5f502~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格",
    "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages.png",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]' .
from volcenginesdkarkruntime import Ark 
from volcenginesdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation .
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID .
    model="doubao-seedream-4-5-251128",
    prompt="参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格",
    image="https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages.png",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("doubao-seedream-4-5-251128") // Replace with Model ID
                 .prompt("参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格")
                 .image("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages.png")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }

            service.shutdownExecutor();
        }
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格",
       Image:          volcengine.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages.png"),
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v
", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s
", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:
", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s
", i+1, image.Size, url)
    }
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="doubao-seedream-4-5-251128", 
    prompt="参考这个LOGO，做一套户外运动品牌视觉设计，品牌名称为“GREEN"，包括包装袋、帽子、卡片、挂绳等。绿色视觉主色调，趣味、简约现代风格", 
    size="2K",
    response_format="url",
    extra_body={
        "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages.png",
        "watermark": false,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
    }   
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

### 多参考图生组图

---

提示词

输入图1

输入图2

输出（实际会输出3张图片）

生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/77024d8e03f24862b066bfc385301120~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2cbc5cf5a68d44899fc52f177fb9cf51~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6971832f48384aea82b6009006cd3e56~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上",
    "image": ["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_2.png"],
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 3
    },
    "size": "2K",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]' .
from volcenginesdkarkruntime import Ark 
from volcenginesdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-5-251128",
    prompt="生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上",
    image=["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_2.png"],
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=3),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();

        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(3);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("doubao-seedream-4-5-251128") // Replace with Model ID
                 .prompt("生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上")
                 .image(Arrays.asList(
                     "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_1.png",
                     "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_2.png"
                 ))
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);

        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }

            service.shutdownExecutor();
        }
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 5
    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上",
       Image:         []string{
           "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_1.png",
           "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_2.png",
       },

       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v
", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s
", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:
", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s
", i+1, image.Size, url)
    }
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="doubao-seedream-4-5-251128", 
    prompt="生成3张女孩和奶牛玩偶在游乐园开心地坐过山车的图片，涵盖早晨、中午、晚上",
    size="2K",
    response_format="url",
    extra_body={
        "image": ["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimages_2.png"],
        "watermark": false,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 3
        },
    }   
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

## **提示词建议**

* 建议用**简洁连贯**的自然语言写明 **主体 + 行为 + 环境**，若对画面美学有要求，可用自然语言或短语补充 **风格**、**色彩**、**光影**、**构图** 等美学元素。详情可参见 [Seedream 4.0-4.5 提示词指南](/docs/82379/1829186)。
* 文本提示词（prompt）建议不超过300个汉字或600个英文单词。字数过多信息容易分散，模型可能因此忽略细节，只关注重点，造成图片缺失部分元素。

# 进阶使用
## 流式输出
Seedream 4.5、Seedream 4.0 模型支持流式图像生成，模型生成完任一图片即返回结果，让您能更快浏览到生成的图像，改善等待体验。
通过设置 **stream** 参数为`true`，即可开启流式输出模式。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/643230864ffc43a8a37ef775cd51ac30~tplv-goo7wpa0wc-image.image)

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖",
    "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages_1.png",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "size": "2K",
    "stream": true,
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark 
from volcenginesdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

if __name__ == "__main__":
    stream = client.images.generate(
        # Replace with Model ID
        model="doubao-seedream-4-5-251128",
        prompt="参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖",
        image="https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages_1.png",
        size="2K",
        sequential_image_generation="auto",
        sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
        response_format="url",
        stream=True,
        watermark=False
    )
    for event in stream:
        if event is None:
            continue
        if event.type == "image_generation.partial_failed":
            print(f"Stream generate images error: {event.error}")
            if event.error is not None and event.error.code.equal("InternalServiceError"):
                break
        elif event.type == "image_generation.partial_succeeded":
            if event.error is None and event.url:
                print(f"recv.Size: {event.size}, recv.Url: {event.url}")
        elif event.type == "image_generation.completed":
            if event.error is None:
                print("Final completed event:")
                print("recv.Usage:", event.usage)
        elif event.type == "image_generation.partial_image":
            print(f"Partial image index={event.partial_image_index}, size={len(event.b64_json)}")
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("doubao-seedream-4-5-251128") //Replace with Model ID .
                 .prompt("参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖")
                 .image("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages_1.png")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(true)
                 .watermark(false)
                 .build();
        System.out.println(generateRequest.toString());
        
        service.streamGenerateImages(generateRequest)
                .doOnError(Throwable::printStackTrace)
                .blockingForEach(
                        choice -> {
                            if (choice == null) return;
                            if ("image_generation.partial_failed".equals(choice.getType())) {
                                if (choice.getError() != null) {
                                    System.err.println("Stream generate images error: " + choice.getError());
                                    if (choice.getError().getCode() != null && choice.getError().getCode().equals("InternalServiceError")) {
                                        throw new RuntimeException("Server error, terminating stream.");
                                    }
                                }
                            }
                            else if ("image_generation.partial_succeeded".equals(choice.getType())) {
                                if (choice.getError() == null && choice.getUrl() != null && !choice.getUrl().isEmpty()) {
                                    System.out.printf("recv.Size: %s, recv.Url: %s%n", choice.getSize(), choice.getUrl());
                                }
                            }
                            else if ("image_generation.completed".equals(choice.getType())) {
                                if (choice.getError() == null && choice.getUsage() != null) {
                                    System.out.println("recv.Usage: " + choice.getUsage().toString());
                                }
                            }
                        }
                );
        service.shutdownExecutor();
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "io"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-5-251128",
       Prompt:         "参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖",
       Image:          volcengine.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages_1.png"),
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }
    
    stream, err := client.GenerateImagesStreaming(ctx, generateReq)
    if err != nil {
       fmt.Printf("call GenerateImagesStreaming error: %v
", err)
       return
    }
    defer stream.Close()
    for {
       recv, err := stream.Recv()
       if err == io.EOF {
          break
       }
       if err != nil {
          fmt.Printf("Stream generate images error: %v
", err)
          break
       }
       if recv.Type == "image_generation.partial_failed" {
          fmt.Printf("Stream generate images error: %v
", recv.Error)
          if strings.EqualFold(recv.Error.Code, "InternalServiceError") {
             break
          }
       }
       if recv.Type == "image_generation.partial_succeeded" {
          if recv.Error == nil && recv.Url != nil {
             fmt.Printf("recv.Size: %s, recv.Url: %s
", recv.Size, *recv.Url)
          }
       }
       if recv.Type == "image_generation.completed" {
          if recv.Error == nil {
             fmt.Printf("recv.Usage: %v
", *recv.Usage)
          }
       }
    }
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation .
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

if __name__ == "__main__":
    stream = client.images.generate(
        model="doubao-seedream-4-5-251128",
        prompt="参考图1，生成四图片，图中人物分别带着墨镜，骑着摩托，带着帽子，拿着棒棒糖",
        size="2K",
        response_format="b64_json",
        stream=True,
        extra_body={
            "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imageToimages_1.png",
            "watermark": false,
            "sequential_image_generation": "auto",
            "sequential_image_generation_options": {
                "max_images": 4
            },
        },
    )
    for event in stream:
        if event is None:
            continue
        elif event.type == "image_generation.partial_succeeded":
            if event.b64_json is not None:
                print(f"size={len(event.b64_json)}, base_64={event.b64_json}")
        elif event.type == "image_generation.completed":
            if event.usage is not None:
                print("Final completed event:")
                print("recv.Usage:", event.usage)
```

## 提示词优化控制
通过设置 **optimize_prompt_options.mode** 参数，您可以在 `standard` 模式和 `fast` 模式之间进行选择，以根据自身对图片质量和生成速度的不同需求来优化提示词。 

* 为平衡生成速度与图像质量，Seedream 4.0 支持将 **optimize_prompt_options.mode** 设置为 `fast` 模式以显著提升生成速度，但会在一定程度上牺牲图片质量。
* Seedream 4.5 专注于高质量图片输出，仅支持 `standard` 模式。

**Curl**

```Plain
Text
curl https://ark.cn-beijing.volces.com/api/v3/images/generations
  -H "Content-Type: application/json"
  -H "Authorization: Bearer $ARK_API_KEY"
  -d '{
    "model": "doubao-seedream-4-0-250828",
    "prompt": "生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "optimize_prompt_options": {
        "mode": "fast"
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
```

**Python**

```Python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]'
from volcenginesdkarkruntime import Ark 
from volcenginesdkarkruntime.types.images.images import SequentialImageGenerationOptions
from volcenginesdkarkruntime.types.images.images import OptimizePromptOptions

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="doubao-seedream-4-0-250828", 
    prompt="生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    optimize_prompt_options=OptimizePromptOptions(mode="fast"),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

**Java**

```Java
package com.ark.sample;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest.OptimizePromptOptions optimizePromptOptions = new GenerateImagesRequest.OptimizePromptOptions();
        optimizePromptOptions.setMode("fast");
        
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("doubao-seedream-4-0-250828")  //Replace with Model ID
                 .prompt("生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .optimizePromptOptions(optimizePromptOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }

            service.shutdownExecutor();
        }
    }
}
```

**Go**

```Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation .
        arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
    )    
    ctx := context.Background()
    
    var (
    sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages = 4
    mode model.OptimizePromptMode = model.OptimizePromptModeFast
    )
    
    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seedream-4-0-250828",
       Prompt:         "生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
       Size:           volcengine.String("2K"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Watermark:      volcengine.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
       OptimizePromptOptions: &model.OptimizePromptOptions{
       Mode: &mode,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v
", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s
", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:
", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s
", i+1, image.Size, url)
    }
}
```

**OpenAI**

```Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="doubao-seedream-4-0-250828",
    prompt="生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": false,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
        "optimize_prompt_options": {"mode": "fast"}
    },
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
```

## 自定义图片输出规格
您可以配置以下参数来控制图片输出规格：

* **size** ：指定输出图片的尺寸大小。
* **response_format** ：指定生成图像的返回格式。
* **watermark** ：指定是否为输出图片添加水印。

### 图片输出尺寸
支持两种尺寸设置方式，不可混用。

* 方式 1 ：指定生成图像的分辨率，并在 prompt 中用自然语言描述图片宽高比、图片形状或图片用途，最终由模型判断生成图片的大小。
   * 可选值：`1K`(Seedream 4.5 不支持)、`2K`、`4K`
* 方式2 ：指定生成图像的宽高像素值。
   * 默认值：`2048x2048`
   * 总像素取值范围：
      * seedream 4.5：[`2560x1440=3686400`, `4096x4096=16777216`] 
      * seedream 4.0：[`1280x720=921600`, `4096x4096=16777216`] 
   * 宽高比取值范围：[1/16, 16]

---

方式1

方式2

```JSON
{
    "prompt": "生成一组共4张海报，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围", // prompt 中用自然语言描述图片宽高比、图片形状或图片用途
    "size": "2K"  // 通过参数 size 指定生成图像的分辨率
}
```

```JSON
{
    "prompt": "生成一组共4张连贯插画，核心为同一庭院一角的四季变迁，以统一风格展现四季独特色彩、元素与氛围", 
    "size": "2048x2048"  // 通过参数 size 指定生成图像的宽高像素值
}
```

### 图片输出方式
图像 API 返回的图片格式为 jpeg 。通过设置 **response_format** 参数，可以指定生成图像的返回方式：

* `url`：返回图片下载链接。
* `b64_json`：以 Base64 编码字符串的 JSON 格式返回图像数据。

```JSON
{
    "response_format": "url"
}
```

### 图片中添加水印
通过设置 **watermark** 参数，来控制是否在生成的图片中添加水印。

* `false`：不添加水印。
* `true`：在图片右下角添加“AI生成”字样的水印标识。

```JSON
{
    "watermark": true
}
```

# 使用限制
**SDK 版本升级**
为保证模型功能的正常使用，请务必升级至最新 SDK 版本。相关步骤可参考 [安装及升级 SDK](/docs/82379/1541595)。

**图片传入限制**

* 图片格式：jpeg、png、webp、bmp、tiff、gif
* 宽高比（宽/高）范围：[1/16, 16]
* 宽高长度（px） > 14
* 大小：不超过 10 MB
* 总像素：不超过 6000×6000 px
* 最多支持传入 14 张参考图。

**保存时间**
任务数据（如任务状态、图片URL等）仅保留24小时，超时后会被自动清除。请您务必及时保存生成的图片。 

**限流说明**

* RPM 限流：账号下同模型（区分模型版本）每分钟生成图片数量上限。若超过该限制，生成图片时会报错。
* 不同模型的限制值不同，详见 [图片生成能力](/docs/82379/1330310#d3e5e0eb)。

# 附：故事书/连环画制作
[火山方舟大模型体验中心](https://www.volcengine.com/experience/ark?mode=vision&model=doubao-seedream-4-0-250828) 提供了故事书和连环画功能，该功能结合了 doubao-seed-1.6 模型和 doubao-seedream-4.0 模型，可实现一句话生成动漫、连环画、故事书，满足用户多样化的创作需求。 
连环画的实现过程与故事书类似，本文以故事书为例，为您介绍生成故事书的工作流和技术实现步骤，方便您在本地快速复现。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d590e440ff7447feaed8fa8f4d91e746~tplv-goo7wpa0wc-image.image)

## 工作流
故事书生成的工作流如下：

<img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSIxMDgycHgiIGhlaWdodD0iMzA5cHgiIHZpZXdCb3g9Ii0wLjUgLTAuNSAxMDgyIDMwOSI+PGRlZnMvPjxnPjxyZWN0IHg9IjIiIHk9IjIiIHdpZHRoPSIxMDc3IiBoZWlnaHQ9IjMwNCIgZmlsbD0iI2ZmZmZmZiIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iODEwIiB5PSI0MyIgd2lkdGg9IjIyNSIgaGVpZ2h0PSIyMTciIHJ4PSIyMS43IiByeT0iMjEuNyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1kYXNoYXJyYXk9IjE2IDYgMiA2IiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iNDIiIHk9IjQzIiB3aWR0aD0iNzQwIiBoZWlnaHQ9IjIxNyIgcng9IjIxLjciIHJ5PSIyMS43IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWRhc2hhcnJheT0iMTYgNiAyIDYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSI1MiIgeT0iMTE5IiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiNmZmU1OTkiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDE0OXB4OyBtYXJnaW4tbGVmdDogNTNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZToxNHB4Ij7mlYXkuovliJvkvZw8L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cmVjdCB4PSIyNTIiIHk9IjExOSIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjZmZlNTk5IiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxNDlweDsgbWFyZ2luLWxlZnQ6IDI1M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj48Zm9udCBzdHlsZT0iZm9udC1zaXplOjE0cHgiPuaVheS6i+WIhumVnOaLhuinozwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gMTcyIDE0OSBMIDI0NS42MyAxNDkiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSAyNTAuODggMTQ5IEwgMjQzLjg4IDE1Mi41IEwgMjQ1LjYzIDE0OSBMIDI0My44OCAxNDUuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjQ1MiIgeT0iMTE5IiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiNmZmU1OTkiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDE0OXB4OyBtYXJnaW4tbGVmdDogNDUzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxmb250IHN0eWxlPSJmb250LXNpemU6MTRweCI+5pWF5LqL5qCH6aKY44CB566A5LuL44CBPGJyIC8+5paH5qGI55Sf5oiQPC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHBhdGggZD0iTSAzNzIgMTQ5IEwgNDQ1LjYzIDE0OSIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ1MC44OCAxNDkgTCA0NDMuODggMTUyLjUgTCA0NDUuNjMgMTQ5IEwgNDQzLjg4IDE0NS41IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iNjUyIiB5PSIxMTkiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIHJ4PSI5IiByeT0iOSIgZmlsbD0iI2ZmZTU5OSIgc3Ryb2tlPSIjMDAwMDAwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTQ5cHg7IG1hcmdpbi1sZWZ0OiA2NTNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZToxNHB4Ij7mlYXkuovphY3lm748YnIgLz5wcm9tcHTnlJ/miJA8L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDU3MiAxNDkgTCA2NDUuNjMgMTQ5IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNjUwLjg4IDE0OSBMIDY0My44OCAxNTIuNSBMIDY0NS42MyAxNDkgTCA2NDMuODggMTQ1LjUgWiIgZmlsbD0iIzAwMDAwMCIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSI4NTIiIHk9IjExOSIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjY2NlNWZmIiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxNDlweDsgbWFyZ2luLWxlZnQ6IDg1M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj48Zm9udCBzdHlsZT0iZm9udC1zaXplOjE0cHgiPuaVheS6i+mFjeWbvueUn+aIkDwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gNzcyIDE0OSBMIDg0NS42MyAxNDkiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA4NTAuODggMTQ5IEwgODQzLjg4IDE1Mi41IEwgODQ1LjYzIDE0OSBMIDg0My44OCAxNDUuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMyNy41IiB5PSIyMDgiIHdpZHRoPSIxNjkiIGhlaWdodD0iMjkiIGZpbGw9Im5vbmUiIHN0cm9rZT0ibm9uZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDFweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyMjNweDsgbWFyZ2luLWxlZnQ6IDQxMnB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDIxcHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjRkZEOTY2OyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm93cmFwOyAiPjxmb250IHN0eWxlPSJmb250LXNpemU6MThweCI+ZG91YmFvLXNlZWQtMS42PC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHJlY3QgeD0iODk3IiB5PSIyMDgiIHdpZHRoPSI0OSIgaGVpZ2h0PSIyOSIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMXB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDIyM3B4OyBtYXJnaW4tbGVmdDogOTIycHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMjFweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICM3RUE2RTA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3dyYXA7ICI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZToxOHB4Ij5kb3ViYW8tc2VlZHJlYW0tNC4wPC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PC9nPjwvc3ZnPg==" from="flow-chart" payload="{"data":{"mxGraphModel":{"dx":"1186","dy":"791","grid":"0","gridSize":"10","guides":"1","tooltips":"1","connect":"1","arrows":"1","fold":"1","page":"0","pageScale":"1","pageWidth":"827","pageHeight":"1169"},"mxCellMap":{"1hY8BOpv":{"id":"1hY8BOpv"},"tk8feSlP":{"id":"tk8feSlP","style":"","parent":"1hY8BOpv"},"uJn7bRCH":{"id":"uJn7bRCH","value":"","style":"rounded=0;whiteSpace=wrap;html=1;strokeColor=none;","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","parent":"tk8feSlP","-0-mxGeometry":{"x":"302","y":"361","width":"1077","height":"304","as":"geometry"}},"dfnqfF1m":{"id":"dfnqfF1m","value":"","style":"group","vertex":"1","connectable":"0","parent":"tk8feSlP","-0-mxGeometry":{"x":"342","y":"402","width":"993","height":"217","as":"geometry"}},"i0DlOEcd":{"id":"i0DlOEcd","value":"","style":"group","parent":"dfnqfF1m","vertex":"1","connectable":"0","-0-mxGeometry":{"width":"993","height":"217","as":"geometry"}},"mOzlLPF8":{"id":"mOzlLPF8","value":"","style":"rounded=1;arcSize=10;dashed=1;strokeColor=#000000;fillColor=none;gradientColor=none;dashPattern=8 3 1 3;strokeWidth=2;","parent":"i0DlOEcd","vertex":"1","diagramName":"Group","diagramCategory":"BPMN general","-0-mxGeometry":{"x":"768","width":"225","height":"217","as":"geometry"}},"9X8NsMiZ":{"id":"9X8NsMiZ","value":"","style":"rounded=1;arcSize=10;dashed=1;strokeColor=#000000;fillColor=none;gradientColor=none;dashPattern=8 3 1 3;strokeWidth=2;","parent":"i0DlOEcd","vertex":"1","diagramName":"Group","diagramCategory":"BPMN general","-0-mxGeometry":{"width":"740","height":"217","as":"geometry"}},"Nl41c3Oz":{"id":"Nl41c3Oz","value":"<font style=\"font-size:14px\">故事创作</font>","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=#FFE599;","parent":"i0DlOEcd","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"10","y":"76","width":"120","height":"60","as":"geometry"}},"XIeCrqcA":{"id":"XIeCrqcA","value":"<font style=\"font-size:14px\">故事分镜拆解</font>","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=#FFE599;","parent":"i0DlOEcd","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"210","y":"76","width":"120","height":"60","as":"geometry"}},"28J6bNlJ":{"id":"28J6bNlJ","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"i0DlOEcd","source":"Nl41c3Oz","target":"XIeCrqcA","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"NP8Df4r3":{"id":"NP8Df4r3","value":"<font style=\"font-size:14px\">故事标题、简介、<br />文案生成</font>","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=#FFE599;","parent":"i0DlOEcd","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"410","y":"76","width":"120","height":"60","as":"geometry"}},"c0AVaicU":{"id":"c0AVaicU","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"i0DlOEcd","source":"XIeCrqcA","target":"NP8Df4r3","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"NyyhQrJD":{"id":"NyyhQrJD","value":"<font style=\"font-size:14px\">故事配图<br />prompt生成</font>","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=#FFE599;","parent":"i0DlOEcd","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"610","y":"76","width":"120","height":"60","as":"geometry"}},"31AeePhF":{"id":"31AeePhF","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"i0DlOEcd","source":"NP8Df4r3","target":"NyyhQrJD","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"vWTz9Pn5":{"id":"vWTz9Pn5","value":"<font style=\"font-size:14px\">故事配图生成</font>","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=#CCE5FF;","parent":"i0DlOEcd","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"810","y":"76","width":"120","height":"60","as":"geometry"}},"9Xr6RWXQ":{"id":"9Xr6RWXQ","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"i0DlOEcd","source":"NyyhQrJD","target":"vWTz9Pn5","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"J1jOoH0n":{"id":"J1jOoH0n","value":"<font style=\"font-size:18px\">doubao-seed-1.6</font>","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;fontSize=21;fontColor=#FFD966;","parent":"i0DlOEcd","vertex":"1","-0-mxGeometry":{"x":"285.5","y":"165","width":"169","height":"29","as":"geometry"}},"tJTCip0t":{"id":"tJTCip0t","value":"<font style=\"font-size:18px\">doubao-seedream-4.0</font>","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;fontSize=21;fontColor=#7EA6E0;","parent":"i0DlOEcd","vertex":"1","-0-mxGeometry":{"x":"855","y":"165","width":"49","height":"29","as":"geometry"}}},"mxCellList":["1hY8BOpv","tk8feSlP","uJn7bRCH","dfnqfF1m","i0DlOEcd","mOzlLPF8","9X8NsMiZ","Nl41c3Oz","XIeCrqcA","28J6bNlJ","NP8Df4r3","c0AVaicU","NyyhQrJD","31AeePhF","vWTz9Pn5","9Xr6RWXQ","J1jOoH0n","tJTCip0t"]},"lastEditTime":0,"snapshot":""}" />

## 技术实现

1. 根据用户提供的提示词和参考图，调用 doubao-seed-1.6 模型，进行故事创作 > 故事分镜拆解 > 生成分镜的文案和画面描述 > 生成书名 > 生成故事总结，并汇总成 JSON 格式输出。

System Prompt 如下：
```Plain Text
# 角色

你是一位**绘本创作大师**。

## 任务

贴合用户指定的**读者群（儿童/青少年/成人/全年龄）**，创作**情节线性连贯的、生动有趣的、充满情绪价值和温度的、有情感共鸣的、分镜-文案-画面严格顺序对应的绘本内容**：
- 核心约束：**分镜拆分→文案（scenes）→画面描述（scenes_detail）必须1:1顺序绑定**，从故事开头到结尾，像「放电影」一样按时间线推进，绝无错位。

## 工作流程

1.  充分理解用户诉求。 优先按照用户的创作细节要求执行（如果有）
2.  **故事构思:** 创作一个能够精准回应用户诉求、提供情感慰藉的故事脉络。整个故事必须围绕“共情”和“情绪价值”展开。
3.  **分镜结构与数量:**
    * 将故事浓缩成 **5~10** 个关键分镜，最多10个（不能超过10个）。
    * 必须遵循清晰的叙事弧线：开端 → 发展 → 高潮 → 结局。
4.  **文案与画面 (一一对应):**
    * **文案 ("scenes"字段):** 为每个分镜创作具备情感穿透力的文案。文案必须与画面描述紧密贴合，共同服务于情绪的传递。**禁止在文案中使用任何英文引号 ("")**。不能超过10个。
    * **画面 ("scenes_detail"字段):** 为每个分镜构思详细的画面。画风必须贴合用户诉求和故事氛围。描述需包含构图、光影、色彩、角色神态等关键视觉要素，达到可直接用于图片生成的标准。
5.  **书名 ("title"字段):**
    * 构思一个简洁、好记、有创意的书名。
    * 书名必须能巧妙地概括故事精髓，并能瞬间“戳中”目标用户的情绪共鸣点。
6.  **故事总结 ("summary"字段):**
    * 创作一句**不超过30个汉字**的总结。
    * 总结需高度凝练故事的核心思想与情感价值。
7. 整合输出：将所有内容按指定 JSON 格式整理输出。

## 安全限制
生成的内容必须严格遵守以下规定：
1.  **禁止暴力与血腥:** 不得包含任何详细的暴力、伤害、血腥或令人不适的画面描述。
2.  **禁止色情内容:** 不得包含任何色情、性暗示或不适宜的裸露内容。
3.  **禁止仇恨与歧视:** 不得包含针对任何群体（基于种族、宗教、性别、性取向等）的仇恨、歧视或攻击性言论。
4.  **禁止违法与危险行为:** 不得描绘或鼓励任何非法活动、自残或危险行为。
5.  **确保普遍适宜性:** 整体内容应保持在社会普遍接受的艺术创作范围内，避免极端争议性话题。

## 输出格式要求
整理成以下JSON格式，scenes 和 scenes_detail 要与分镜保持顺序一致，一一对应，最多10个（不能超过10个）：
{  
  "title": "书名",
  "summary": "30字内的总结",
  "scenes": [
    "分镜1的文案，用50字篇幅传递情绪和情感，引发读者共鸣，语言风格需符合设定。",
    "分镜2的文案"
  ],
  "scenes_detail": [
    "图片1：这是第一页的画面描述。必须以'图片'+序号开头。要有强烈的视觉感，详细描述构图（如特写、远景）、光影、色彩、角色表情、动作和环境细节，符合生图提示词的要求。",
    "图片2："
  ]
}
```

2. 提取返回结果 JSON 中的 scenes_detail 字段，作为图片生成的 Prompt 。
3. 处理图片生成的 Prompt:
   1. 将数组转化成字符串
   2. 在 prompt 末尾补充"最后，为故事书创作一个封面。 再检查所有图片，去除图片中的文字"。
   3. 在 prompt 开头添加用户输入的提示词。
4. 根据图片生成的 Prompt 和用户提供的参考图，调用 doubao-seedream-4.0 模型的生成组图能力，为故事的所有分镜文案生成配图。
5. 按照顺序拼装图片和文字即可得到故事书内容 ，用户按需进行展示即可。
