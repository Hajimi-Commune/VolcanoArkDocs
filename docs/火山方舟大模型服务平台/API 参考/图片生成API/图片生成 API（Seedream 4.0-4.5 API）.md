# 图片生成 API（Seedream 4.0-4.5 API）

`POST https://ark.cn-beijing.volces.com/api/v3/images/generations`[运行](https://api.volcengine.com/api-explorer/?action=ImageGenerations&groupName=%E5%9B%BE%E7%89%87%E7%94%9F%E6%88%90API&serviceCode=ark&version=2024-01-01&tab=2#N4IgTgpgzgDg9gOyhA+gMzmAtgQwC4gBcIArmADYgA0IUAlgF4REgBMA0tSAO74TY4wAayJoc5ZDSxwAJhErEZcEgCMccALTIIMyDiwaALBoAMG1gFYTADlbWuMMHCwwCxQPhmgUTTA-l6Ao2MAw-4CLeYB4tkHBgDOJgE2KgF+KgABygGHxgNf6gPSmgN2egCwegHEegCFugLCagCfKgOhKgGbx-oBFRoBjkYCTkZGA34qA2Ur+gKyugI76gOSagOJO-oDU5oCnpoBHphWA+Ib+gBVKI4Cf2oAr1oBOQf5wAMaATHaAy+b+gJKKgP1+gL-xgFRxY4CABoCEVoBTPv6A9maAj7b+gKGxgA3OgHnagNxygJJy-peAuyH+gNyugEbpgFgJgHH4wBjfoBvQOygAY5QAz2tkZoBLfUAQjqAQmtAIoagAIEp6AZXlAHBygC51c7+QAUsUNAPjuD38gHSzQKAOYzADMB52y6xagAlTQA55oBSELR0UA2DaAF7V-IAXU0xgB9FQDuioAvIMA9OaAbz1AM8GI0AHJqAAn1soB-PUAS5GAeASKmz-IAAAPW-kAs8qAEB1-IBA80AL4GMlr+QBc+oBUfUagDwVQA2aiAAL5AA)
本文介绍图片生成模型如 Seedream 4.5 的调用 API ，包括输入输出参数，取值范围，注意事项等信息，供您使用接口时查阅字段含义。
请求参数
请求体
**model**`string`
本次请求使用模型的 [Model ID](https://www.volcengine.com/docs/82379/1513689) 或[推理接入点](https://www.volcengine.com/docs/82379/1099522) (Endpoint ID)。
**prompt **`string`
用于生成图像的提示词，支持中英文。（查看提示词指南：[Seedream 4.0](https://www.volcengine.com/docs/82379/1829186) 、[Seedream 3.0](https://www.volcengine.com/docs/82379/1795150)）
建议不超过300个汉字或600个英文单词。字数过多信息容易分散，模型可能因此忽略细节，只关注重点，造成图片缺失部分元素。
**image**`string/array`
doubao-seededit-3.0-t2i 不支持该参数
输入的图片信息，支持 URL 或 Base64 编码。其中，doubao-seedream-4.5、doubao-seedream-4.0 支持单图或多图输入（[查看多图融合示例](https://www.volcengine.com/docs/82379/1824121#%E5%A4%9A%E5%9B%BE%E8%9E%8D%E5%90%88%EF%BC%88%E5%A4%9A%E5%9B%BE%E8%BE%93%E5%85%A5%E5%8D%95%E5%9B%BE%E8%BE%93%E5%87%BA%EF%BC%89)），doubao-seededit-3.0-i2i 仅支持单图输入。
图片URL：请确保图片URL可被访问。
Base64编码：请遵循此格式`data:image/<图片格式>;base64,<Base64编码>`。注意 `<图片格式>` 需小写，如 `data:image/png;base64,<base64_image>`。
说明
传入图片需要满足以下条件：
图片格式：jpeg、png（doubao-seedream-4.5、doubao-seedream-4.0 模型新增支持 webp、bmp、tiff、gif格式**new**）
宽高比（宽/高）范围：
[1/16, 16] (适用模型：doubao-seedream-4.5、doubao-seedream-4.0）
[1/3, 3] (适用模型：doubao-seededit-3.0-t2i、doubao-seededit-3.0-i2i）
宽高长度（px） > 14
大小：不超过 10MB
总像素：不超过 `6000×6000` px
doubao-seedream-4.5、doubao-seedream-4.0 最多支持传入 14 张参考图。
**size **`string`
**seed**`integer``默认值 -1`
仅 doubao-seedream-3.0-t2i、doubao-seededit-3.0-i2i 支持该参数
随机数种子，用于控制模型生成内容的随机性。取值范围为 [-1, 2147483647]。
注意
相同的请求下，模型收到不同的seed值，如：不指定seed值或令seed取值为-1（会使用随机数替代）、或手动变更seed值，将生成不同的结果。
相同的请求下，模型收到相同的seed值，会生成类似的结果，但不保证完全一致。
**sequential_image_generation**`string``默认值 disabled`
仅 doubao-seedream-4.5、doubao-seedream-4.0 支持该参数 | [查看组图输出示例](https://www.volcengine.com/docs/82379/1824121?lang=zh#%E7%BB%84%E5%9B%BE%E8%BE%93%E5%87%BA%EF%BC%88%E5%A4%9A%E5%9B%BE%E8%BE%93%E5%87%BA%EF%BC%89)
控制是否关闭组图功能。
说明
组图：基于您输入的内容，生成的一组内容关联的图片。
`auto`：自动判断模式，模型会根据用户提供的提示词自主判断是否返回组图以及组图包含的图片数量。
`disabled`：关闭组图功能，模型只会生成一张图。
**sequential_image_generation_options **`object`
仅 doubao-seedream-4.5、doubao-seedream-4.0 支持该参数
组图功能的配置。仅当 **sequential_image_generation **为 `auto` 时生效。
**stream**`Boolean``默认值 false`
仅 doubao-seedream-4.5、doubao-seedream-4.0 支持该参数 | [查看流式输出示例](https://www.volcengine.com/docs/82379/1824121#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
控制是否开启流式输出模式。
`false`：非流式输出模式，等待所有图片全部生成结束后再一次性返回所有信息。
`true`：流式输出模式，即时返回每张图片输出的结果。在生成单图和组图的场景下，流式输出模式均生效。
**guidance_scale **`Float`
doubao-seedream-3.0-t2i 默认值 2.5
doubao-seededit-3.0-i2i 默认值 5.5
doubao-seedream-4.5、doubao-seedream-4.0 不支持
模型输出结果与prompt的一致程度，生成图像的自由度，又称为文本权重；值越大，模型自由度越小，与用户输入的提示词相关性越强。
取值范围：[`1`, `10`] 。
**response_format**`string``默认值 url`
指定生成图像的返回格式。
生成的图片为 jpeg 格式，支持以下两种返回方式：
`url`：返回图片下载链接；**链接在图片生成后24小时内有效，请及时下载图片。**
`b64_json`：以 Base64 编码字符串的 JSON 格式返回图像数据。
**watermark**`Boolean``默认值 true`
是否在生成的图片中添加水印。
`false`：不添加水印。
`true`：在图片右下角添加“AI生成”字样的水印标识。
**optimize_prompt_options****new**`object`
仅 doubao-seedream-4.5（当前仅支持 `standard` 模式）、doubao-seedream-4.0 支持该参数
提示词优化功能的配置。
响应参数
流式响应参数
请参见[文档](https://www.volcengine.com/docs/82379/1824137?lang=zh)。
非流式响应参数
**model**`string`
本次请求使用的模型 ID （`模型名称-版本`）。
**created**`integer`
本次请求创建时间的 Unix 时间戳（秒）。
**data**`array`
输出图像的信息。
说明
doubao-seedream-4.5、doubao-seedream-4.0 模型生成组图场景下，组图生成过程中某张图生成失败时：
若失败原因为审核不通过：仍会继续请求下一个图片生成任务，即不影响同请求内其他图片的生成流程。
若失败原因为内部服务异常（500）：不会继续请求下一个图片生成任务。
**usage**`object`
本次请求的用量信息。
**error**`object`
本次请求，如发生错误，对应的错误信息。

## 示例代码

**Curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
    "size": "2K",
    "watermark": false
}'
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757321139,
    "data": [
        {
            "url": "https://...",
            "size": "3104x1312"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": xxx,
        "total_tokens": xxx
    }
}
```

**Python**

```python
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
    prompt="充满活力的特写编辑肖像，模特眼神犀利，头戴雕塑感帽子，色彩拼接丰富，眼部焦点锐利，景深较浅，具有Vogue杂志封面的美学风格，采用中画幅拍摄，工作室灯光效果强烈。",
    size="2K",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757321139,
    "data": [
        {
            "url": "https://...",
            "size": "3104x1312"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": xxx,
        "total_tokens": xxx
    }
}
```

**Java**

```java
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757321139,
    "data": [
        {
            "url": "https://...",
            "size": "3104x1312"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": xxx,
        "total_tokens": xxx
    }
}
```

**Go**

```go
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
       fmt.Printf("generate images error: %v\n", err)
       return
    }

    fmt.Printf("%s\n", *imagesResponse.Data[0].Url)
}
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757321139,
    "data": [
        {
            "url": "https://...",
            "size": "3104x1312"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": xxx,
        "total_tokens": xxx
    }
}
```

**OpenAI**

```openai
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757321139,
    "data": [
        {
            "url": "https://...",
            "size": "3104x1312"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": xxx,
        "total_tokens": xxx
    }
}
```

**Curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "保持模特姿势和液态服装的流动形状不变。将服装材质从银色金属改为完全透明的清水（或玻璃）。透过液态水流，可以看到模特的皮肤细节。光影从反射变为折射。",
    "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imageToimage.png",
    "size": "2K",
    "watermark": false
}'
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Python**

```python
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Java**

```java
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Go**

```go
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
       fmt.Printf("generate images error: %v\n", err)
       return
    }

    fmt.Printf("%s\n", *imagesResponse.Data[0].Url)
}
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**OpenAI**

```openai
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedream-4-5-251128",
    "prompt": "将图1的服装换为图2的服装",
    "image": ["https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream4_5_imagesToimage_2.png"],
    "sequential_image_generation": "disabled",
    "size": "2K",
    "watermark": false
}'
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Python**

```python
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Java**

```java
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Go**

```go
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
       fmt.Printf("generate images error: %v\n", err)
       return
    }

    fmt.Printf("%s\n", *imagesResponse.Data[0].Url)
}
```

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**OpenAI**

```openai
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

**Response:**

```json
{
    "model": "doubao-seedream-4-5-251128",
    "created": 1757323224,
    "data": [
        {
            "url": "https://...",
            "size": "1760x2368"
        }
    ],
    "usage": {
        "generated_images": 1,
        "output_tokens": 16280,
        "total_tokens": 16280
    }
}
```

**Curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
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

**Response:**

```json
{
  "model": "doubao-seedream-4-5-251128",
  "created": 1757388756,
  "data": [
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    }
  ],
  "usage": {
    "generated_images": 3,
    "output_tokens": 48960,
    "total_tokens": 48960
  }
}
```

**Python**

```python
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

**Response:**

```json
{
  "model": "doubao-seedream-4-5-251128",
  "created": 1757388756,
  "data": [
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    }
  ],
  "usage": {
    "generated_images": 3,
    "output_tokens": 48960,
    "total_tokens": 48960
  }
}
```

**Java**

```java
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

**Response:**

```json
{
  "model": "doubao-seedream-4-5-251128",
  "created": 1757388756,
  "data": [
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    }
  ],
  "usage": {
    "generated_images": 3,
    "output_tokens": 48960,
    "total_tokens": 48960
  }
}
```

**Go**

```go
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
        fmt.Printf("call GenerateImages error: %v\n", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s\n", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:\n", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s\n", i+1, image.Size, url)
    }
}
```

**Response:**

```json
{
  "model": "doubao-seedream-4-5-251128",
  "created": 1757388756,
  "data": [
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    }
  ],
  "usage": {
    "generated_images": 3,
    "output_tokens": 48960,
    "total_tokens": 48960
  }
}
```

**OpenAI**

```openai
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

**Response:**

```json
{
  "model": "doubao-seedream-4-5-251128",
  "created": 1757388756,
  "data": [
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    },
    {
      "url": "https://...",
      "size": "2720x1536"
    }
  ],
  "usage": {
    "generated_images": 3,
    "output_tokens": 48960,
    "total_tokens": 48960
  }
}
```

**Curl**

```bash
curl https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
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

**Response:**

```json
event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396757,
  "image_index": 0,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396785,
  "image_index": 1,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "image_index": 2,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.completed
data: {
  "type": "image_generation.completed",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "usage": {
    "generated_images": 3,
    "output_tokens": 48672,
    "total_tokens": 48672
  }
}

data: [DONE]
```

**Python**

```python
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

**Response:**

```json
event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396757,
  "image_index": 0,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396785,
  "image_index": 1,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "image_index": 2,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.completed
data: {
  "type": "image_generation.completed",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "usage": {
    "generated_images": 3,
    "output_tokens": 48672,
    "total_tokens": 48672
  }
}

data: [DONE]
```

**Java**

```java
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

**Response:**

```json
event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396757,
  "image_index": 0,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396785,
  "image_index": 1,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "image_index": 2,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.completed
data: {
  "type": "image_generation.completed",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "usage": {
    "generated_images": 3,
    "output_tokens": 48672,
    "total_tokens": 48672
  }
}

data: [DONE]
```

**Go**

```go
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
       fmt.Printf("call GenerateImagesStreaming error: %v\n", err)
       return
    }
    defer stream.Close()
    for {
       recv, err := stream.Recv()
       if err == io.EOF {
          break
       }
       if err != nil {
          fmt.Printf("Stream generate images error: %v\n", err)
          break
       }
       if recv.Type == "image_generation.partial_failed" {
          fmt.Printf("Stream generate images error: %v\n", recv.Error)
          if strings.EqualFold(recv.Error.Code, "InternalServiceError") {
             break
          }
       }
       if recv.Type == "image_generation.partial_succeeded" {
          if recv.Error == nil && recv.Url != nil {
             fmt.Printf("recv.Size: %s, recv.Url: %s\n", recv.Size, *recv.Url)
          }
       }
       if recv.Type == "image_generation.completed" {
          if recv.Error == nil {
             fmt.Printf("recv.Usage: %v\n", *recv.Usage)
          }
       }
    }
}
```

**Response:**

```json
event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396757,
  "image_index": 0,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396785,
  "image_index": 1,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "image_index": 2,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.completed
data: {
  "type": "image_generation.completed",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "usage": {
    "generated_images": 3,
    "output_tokens": 48672,
    "total_tokens": 48672
  }
}

data: [DONE]
```

**OpenAI**

```openai
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

**Response:**

```json
event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396757,
  "image_index": 0,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396785,
  "image_index": 1,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.partial_succeeded
data: {
  "type": "image_generation.partial_succeeded",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "image_index": 2,
  "url": "https://...",
  "size": "2496x1664"
}

event: image_generation.completed
data: {
  "type": "image_generation.completed",
  "model": "doubao-seedream-4-5-251128",
  "created": 1757396825,
  "usage": {
    "generated_images": 3,
    "output_tokens": 48672,
    "total_tokens": 48672
  }
}

data: [DONE]
```

**Curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedream-3-0-t2i-250415",
    "prompt": "鱼眼镜头，一只猫咪的头部，画面呈现出猫咪的五官因为拍摄方式扭曲的效果。",
    "response_format": "url",
    "size": "1024x1024",
    "seed": 12,
    "guidance_scale": 2.5,
    "watermark": true
}'
```

**Response:**

```json
{
  "model": "doubao-seedream-3-0-t2i-250415"
  "created": 1589478378,
  "data": [
    {
      "url": "https://..."
    }
  ],
  "usage": {
      "generated_images": 1
      "output_tokens": xx
      "total_tokens": xx 
      
  }
}
```

**Python**

```python
import os
# 通过 pip install 'volcengine-python-sdk[ark]' 安装方舟SDK
from volcenginesdkarkruntime import Ark

# 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
# 初始化Ark客户端，从环境变量中读取您的API Key
client = Ark(
    # 此为默认路径，您可根据业务所在地域进行配置
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    # 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
    api_key=os.environ.get("ARK_API_KEY"),
)

imagesResponse = client.images.generate(
    model="doubao-seedream-3-0-t2i-250415",
    prompt="鱼眼镜头，一只猫咪的头部，画面呈现出猫咪的五官因为拍摄方式扭曲的效果。"
)

print(imagesResponse.data[0].url)
```

**Response:**

```json
{
  "model": "doubao-seedream-3-0-t2i-250415"
  "created": 1589478378,
  "data": [
    {
      "url": "https://..."
    }
  ],
  "usage": {
      "generated_images": 1
      "output_tokens": xx
      "total_tokens": xx 
      
  }
}
```

**Java**

```java
package com.volcengine.ark.runtime;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder().dispatcher(dispatcher).connectionPool(connectionPool).apiKey(apiKey).build();

        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("doubao-seedream-3-0-t2i-250415")
                .prompt("鱼眼镜头，一只猫咪的头部，画面呈现出猫咪的五官因为拍摄方式扭曲的效果。")
                .build();

        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```

**Response:**

```json
{
  "model": "doubao-seedream-3-0-t2i-250415"
  "created": 1589478378,
  "data": [
    {
      "url": "https://..."
    }
  ],
  "usage": {
      "generated_images": 1
      "output_tokens": xx
      "total_tokens": xx 
      
  }
}
```

**Go**

```go
package main

import (
    "context"
    "fmt"
    "os"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
)

func main() {
    client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:  "doubao-seedream-3-0-t2i-250415",
       Prompt: "鱼眼镜头，一只猫咪的头部，画面呈现出猫咪的五官因为拍摄方式扭曲的效果。",
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\n", err)
       return
    }

    fmt.Printf("%s\n", *imagesResponse.Data[0].Url)
}
```

**Response:**

```json
{
  "model": "doubao-seedream-3-0-t2i-250415"
  "created": 1589478378,
  "data": [
    {
      "url": "https://..."
    }
  ],
  "usage": {
      "generated_images": 1
      "output_tokens": xx
      "total_tokens": xx 
      
  }
}
```

**Curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seededit-3-0-i2i-250628",
    "prompt": "改成爱心形状的泡泡",
    "image": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seededit_i2i.jpeg",
    "response_format": "url",
    "size": "adaptive",
    "seed": 21,
    "guidance_scale": 5.5,
    "watermark": true
}'
```

**Response:**

```json
{
    "model": "doubao-seededit-3-0-i2i-250628",    
    "created": 1752479401,                       
    "data": [
        {
            "url": "https://ark-content-generation-v2-cn-beijing.tos-cn-beijing.volces.com/doubao-seededit-3-0-i2i/******"
        }
    ],
    "usage": {
        "generated_images": 1,                   
        "output_tokens": 3772,                    
        "total_tokens": 3772                      
    }
}
```

**Python**

```python
import os
# 通过 pip install 'volcengine-python-sdk[ark]' 安装方舟SDK
# SDK 版本要求≥ 4.0.6，建议使用最新版 SDK，参考文档https://www.volcengine.com/docs/82379/1541595
from volcenginesdkarkruntime import Ark

# 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
# 初始化Ark客户端，从环境变量中读取您的API Key
client = Ark(
    # 此为默认路径，您可根据业务所在地域进行配置
    base_url="https://ark.cn-beijing.volces.com/api/v3",
    # 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
    api_key=os.environ.get("ARK_API_KEY"),
)

imagesResponse = client.images.generate(
    model="doubao-seededit-3-0-i2i-250628",
    prompt="改成爱心形状的泡泡",
    image="https://ark-project.tos-cn-beijing.volces.com/doc_image/seededit_i2i.jpeg",
    seed=123,
    guidance_scale=5.5,
    size="adaptive",
    watermark=True 
)

print(imagesResponse.data[0].url)
```

**Response:**

```json
{
    "model": "doubao-seededit-3-0-i2i-250628",    
    "created": 1752479401,                       
    "data": [
        {
            "url": "https://ark-content-generation-v2-cn-beijing.tos-cn-beijing.volces.com/doubao-seededit-3-0-i2i/******"
        }
    ],
    "usage": {
        "generated_images": 1,                   
        "output_tokens": 3772,                    
        "total_tokens": 3772                      
    }
}
```

**Java**

```java
// SDK 版本要求≥ 0.2.23，建议使用最新版 SDK，参考文档https://www.volcengine.com/docs/82379/1541595

package com.volcengine.ark.runtime;

import com.volcengine.ark.runtime.model.images.generation.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder().dispatcher(dispatcher).connectionPool(connectionPool).apiKey(apiKey).build();

        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("doubao-seededit-3-0-i2i-250628")
                .prompt("改成爱心形状的泡泡")
                .image("https://ark-project.tos-cn-beijing.volces.com/doc_image/seededit_i2i.jpeg")
                .responseFormat(ResponseFormat.Url)
                .seed(123)
                .guidanceScale(5.5)
                .size(Size.Adaptive)
                .watermark(true)
                .build();

        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```

**Response:**

```json
{
    "model": "doubao-seededit-3-0-i2i-250628",    
    "created": 1752479401,                       
    "data": [
        {
            "url": "https://ark-content-generation-v2-cn-beijing.tos-cn-beijing.volces.com/doubao-seededit-3-0-i2i/******"
        }
    ],
    "usage": {
        "generated_images": 1,                   
        "output_tokens": 3772,                    
        "total_tokens": 3772                      
    }
}
```

**Go**

```go
// SDK 版本要求≥ 1.1.23，建议使用最新版 SDK，参考文档https://www.volcengine.com/docs/82379/1541595

package main

import (
    "context"
    "fmt"
    "os"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

func main() {
    client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:          "doubao-seededit-3-0-i2i-250628",
       Prompt:         "改成爱心形状的泡泡",
       Image:          volcengine.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/seededit_i2i.jpeg"),
       ResponseFormat: volcengine.String(model.GenerateImagesResponseFormatURL),
       Seed:           volcengine.Int64(123),
       GuidanceScale:  volcengine.Float64(5.5),
       Size:           volcengine.String(model.GenerateImagesSizeAdaptive),
       Watermark:      volcengine.Bool(true),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\n", err)
       return
    }

    fmt.Printf("%s\n", *imagesResponse.Data[0].Url)
}
```

**Response:**

```json
{
    "model": "doubao-seededit-3-0-i2i-250628",    
    "created": 1752479401,                       
    "data": [
        {
            "url": "https://ark-content-generation-v2-cn-beijing.tos-cn-beijing.volces.com/doubao-seededit-3-0-i2i/******"
        }
    ],
    "usage": {
        "generated_images": 1,                   
        "output_tokens": 3772,                    
        "total_tokens": 3772                      
    }
}
```

属性
data.**error**`object`
错误信息结构体。
data.**url **`string`
图片的 url 信息，当 **response_format **指定为 `url` 时返回。该链接将在生成后 **24 小时内失效**，请务必及时保存图像。
data.**b64_json**`string`
图片的 base64 信息，当 **response_format **指定为 `b64_json` 时返回。
data.**size**`string`
仅 doubao-seedream-4.5、doubao-seedream-4.0 支持该字段。
图像的宽高像素值，格式 `<宽像素>x<高像素>`，如`2048×2048`。
指定生成图像的尺寸信息，支持以下两种方式，不可混用。
方式 1 | 指定生成图像的分辨率，并在prompt中用自然语言描述图片宽高比、图片形状或图片用途，最终由模型判断生成图片的大小。
可选值：`2K`、`4K`
方式 2 | 指定生成图像的宽高像素值：
默认值：`2048x2048`
总像素取值范围：[`2560x1440=3686400`, `4096x4096=16777216`]
宽高比取值范围：[1/16, 16]
采用方式 2 时，需同时满足总像素取值范围和宽高比取值范围。其中，总像素是对宽度和高度的像素乘积限制，而不是对宽度或高度的单独值进行限制。
**有效示例**：`3750x1250`
总像素值 3750x1250=4687500，符合 [3686400, 16777216] 的区间要求；宽高比 3750/1250=3，符合 [1/16, 16] 的区间要求，故该示例值有效。
**无效示例**：`1500x1500`
总像素值 1500x1500=2250000，未达到 3686400 的最低要求；宽高 1500/1500=1，虽符合 [1/16, 16] 的区间要求，但因其未同时满足两项限制，故该示例值无效。
推荐的宽高像素值：
*
data.error.**code**
某张图片生成错误的错误码，请参见[错误码](https://www.volcengine.com/docs/82379/1299023)。
data.error.**message**
某张图片生成错误的提示信息。
指定生成图像的宽高像素值。
默认值：`1024x1024`
总像素取值范围： [`512x512`, `2048x2048`]
error.**code**`string`
请参见[错误码](https://www.volcengine.com/docs/82379/1299023)。
error.**message**`string`
错误提示信息
**
本接口仅支持 API Key 鉴权，请在 [获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 页面，获取长效 API Key。
指定生成图像的宽高像素值。**当前仅支持 adaptive。**
adaptive。将您的输入图片尺寸与下表中的尺寸进行对比，选择最接近的，作为输出图片的尺寸。具体而言，会按顺序从可选比例中，选取与原图宽高比**差值最小**的**第一个**，作为生成图片的比例。
预设的高宽像素
**不同模型支持的****图片****生成能力简介**
**doubao-seedream-4.5****new****、doubao-seedream-4.0**
生成组图（组图：基于您输入的内容，生成的一组内容关联的图片；需配置 **sequential_image_generation **为`auto`**）**
多图生组图，根据您输入的 **多张参考图片（2-14）**+文本提示词 生成一组内容关联的图片（输入的参考图数量+最终生成的图片数量≤15张）。
单图生组图，根据您输入的 单张参考图片+文本提示词 生成一组内容关联的图片（最多生成14张图片）。
文生组图，根据您输入的 文本提示词 生成一组内容关联的图片（最多生成15张图片）。
生成单图（配置 **sequential_image_generation **为`disabled`**）**
多图生图，根据您输入的 **多张参考图片（2-14）**+文本提示词 生成单张图片。
单图生图，根据您输入的 单张参考图片+文本提示词 生成单张图片。
文生图，根据您输入的 文本提示词 生成单张图片。
**doubao-seedream-3.0-t2i**
文生图，根据您输入的 文本提示词 生成单张图片。
**doubao-seededit-****3.0****-i2i**
图生图，根据您输入的 单张参考图片+文本提示词 生成单张图片。
可能类型
图片信息 `object`
生成成功的图片信息。
错误信息 `object`
某张图片生成失败，错误信息。
可选值：`1K`、`2K`、`4K`
总像素取值范围：[`1280x720=921600`, `4096x4096=16777216`]
**有效示例**：`1600x600`
总像素值 1600x600=960000，符合 [921600, 16777216] 的区间要求；宽高比 1600/600=8/3，符合 [1/16, 16] 的区间要求，故该示例值有效。
**无效示例**：`800x800`
总像素值 800x800=640000，未达到 921600 的最低要求；宽高 800/800=1，虽符合 [1/16, 16] 的区间要求，但因其未同时满足两项限制，故该示例值无效。
[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/experience/vision?type=GenImage)[模型列表](https://www.volcengine.com/docs/82379/1330310#%E5%9B%BE%E7%89%87%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1544106#.5Zu-54mH55Sf5oiQ5qih5Z6L)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[调用教程](https://www.volcengine.com/docs/82379/1548482)[接口文档](https://www.volcengine.com/docs/82379/1666945)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
usage.**generated_images **`integer`
模型成功生成的图片张数，不包含生成失败的图片。
仅对成功生成图片按张数进行计费。
usage.**output_tokens**`integer`
模型生成的图片花费的 token 数量。
计算逻辑为：计算 `sum(图片长*图片宽)/256` ，然后取整。
usage.**total_tokens**`integer`
本次请求消耗的总 token 数量。
当前不计算输入 token，故与 **output_tokens** 值一致。
sequential_image_generation_options.**max_images **`integer``默认值 15`
指定本次请求，最多可生成的图片数量。
取值范围： [1, 15]
实际可生成的图片数量，除受到 **max_images **影响外**，**还受到输入的参考图数量影响。**输入的参考图数量+最终生成的图片数量≤15张**。
optimize_prompt_options.**mode **`string``默认值 standard`
设置提示词优化功能使用的模式。
`standard`：标准模式，生成内容的质量更高，耗时较长。
`fast`：快速模式，生成内容的耗时更短，质量一般。
1312
1.95
672
896
0.82
1088
1024
0.94
640
0.47
1376
768
0.6
1280
0.78
1152
21:9
`3024x1296`
512
0.33
1536
1.86
704
832
0.7
1184
928
0.85
1.42
1.1
992
1.67
1:1
`1024x1024`
1.74
736
2:3
`1664x2496`
1216
1.46
960
0.88
1408
2.1
544
0.35
4:3
`1152x864`
1472
2.3
宽高比
宽高像素值
800
0.66
1344
2
576
0.38
`2304x1728`
0.56
`1512x648`
3:2
`2496x1664`
宽
宽/高
高
1.82
0.97
1056
0.42
1.33
864
9:16
`1440x2560`
2.67
3
0.72
2.2
`1248x832`
1248
1.62
1
3:4
`864x1152`
1.29
1.24
1440
2.25
1120
1.17
2.4
16:9
`2560x1440`
1.56
0.51
1504
2.35
1.06
0.75
`720x1280`
`2048x2048`
`832x1248`
2.05
`1280x720`
1.5
608
0.4
2.53
0.91
0.67
0.55
2.82
1.78
`1728x2304`
0.63
****
