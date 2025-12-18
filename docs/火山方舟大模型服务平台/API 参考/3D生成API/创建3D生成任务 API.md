# 创建3D生成任务 API

`POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks`
本文介绍创建3D生成任务 API 的输入输出参数，供您使用接口时查阅字段含义。模型会依据传入的图片信息生成3D，待生成完成后，您可以按条件查询任务并获取生成的3D文件。
请求参数
跳转 [响应参数](#L9tzcCyD)
请求体
**model**`string`
您需要调用的模型的 ID （Model ID），[开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)，并[查询 Model ID](https://www.volcengine.com/docs/82379/1330310) 。
您也可通过 Endpoint ID 来调用模型，获得限流、计费类型（前付费/后付费）、运行状态查询、监控、安全等高级能力，可参考[获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522)。
**content**`object[]`
输入给模型，用于生成3D文件的信息。
模型文本命令(选填)
控制3D文件输出的规格。
**subdivisionlevel **`string``默认值 medium``简写 sl`
3D文件中多边形面的数量。
枚举值：
`high`：200000面**new**
`medium`：100000面
`low`：30000面**new**
**fileformat **`string``默认值 glb``简写 ff`
生成的3D文件格式。
枚举值：
`glb` 、`obj`、`usd`、`usdz`
响应参数
跳转 [请求参数](#RxN8G2nH)
**id **`string`
3D生成任务 ID 。创建3D生成任务为异步接口，获取 ID 后，需要通过 [查询3D生成任务 API](https://www.volcengine.com/docs/82379/1521309) 来查询3D生成任务的状态。任务成功后，会输出生成3D的`file_url`。

## 示例代码

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seed3d-1-0-250928",
    "content": [
        {
            "type": "text",
            "text": "--subdivisionlevel medium --fileformat glb"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seed3d_imageTo3d.png"
            }
        }
    ]
}'
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

**python**

```python
import os
# Install SDK:  pip install 'volcengine-python-sdk[ark]' .
from volcenginesdkarkruntime import Ark 

# 初始化Ark客户端
client = Ark(
    # The base URL for model invocation .
    base_url="https://ark.cn-beijing.volces.com/api/v3", 
    # Get API Key：https://console.volcengine.com/ark/region:ark+cn-beijing/apikey
    api_key=os.getenv('ARK_API_KEY'), 
)

print("----- create request -----")
# 创建3D生成任务
create_result = client.content_generation.tasks.create(
    # Replace with Model ID .
    model="doubao-seed3d-1-0-250928", 
    content=[
        {
            # 参数
            "type": "text",
            "text": "--subdivisionlevel medium --fileformat glb"
        },
        {
            # 图片URL
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seed3d_imageTo3d.png"
            }
        }
    ]
)
print(create_result)
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

**java**

```java
package com.volcengine.ark.runtime;

import com.volcengine.ark.runtime.model.content.generation.DeleteContentGenerationTaskResponse;
import com.volcengine.ark.runtime.model.content.generation.*;
import com.volcengine.ark.runtime.model.content.generation.CreateContentGenerationTaskRequest.Content;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class ContentGenerationTaskExample {
    // 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
    // 初始化Ark客户端，从环境变量中读取您的API Key
    static String apiKey = System.getenv("ARK_API_KEY");
    static ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
    static Dispatcher dispatcher = new Dispatcher();
    static ArkService service = ArkService.builder()
           .dispatcher(dispatcher)
           .connectionPool(connectionPool)
           .apiKey(apiKey)
           .baseUrl("https://ark.cn-beijing.volces.com/api/v3") // The base URL for model invocation .
           .build();
    public static void main(String[] args) {
        //Replace with Model ID .
        String model = "doubao-seed3d-1-0-250928"; 
        
        System.out.println("----- CREATE Task Request -----");
        List<Content> contents = new ArrayList<>();

        // 参数
        contents.add(Content.builder()
                .type("text")
                .text("--subdivisionlevel medium --fileformat glb")
                .build());

        // 图片URL
        contents.add(Content.builder()
                .type("image_url")
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder()
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seed3d_imageTo3d.png")
                        .build())
                .build());
        
        // 创建3D生成任务
        CreateContentGenerationTaskRequest createRequest = CreateContentGenerationTaskRequest.builder()
                .model(model)
                .content(contents)
                .build();
 
        CreateContentGenerationTaskResult createResult = service.createContentGenerationTask(createRequest);
        System.out.println(createResult);
        
        service.shutdownExecutor();
    }
}
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

**go**

```go
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
        // 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
        // 初始化Ark客户端，从环境变量中读取您的API Key
        client := arkruntime.NewClientWithApiKey(
                // 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
                os.Getenv("ARK_API_KEY"),
                // The base URL for model invocation .
                arkruntime.WithBaseUrl("https://ark.cn-beijing.volces.com/api/v3"),
        )
        ctx := context.Background()
        // Replace with Model ID .
        modelEp := "doubao-seed3d-1-0-250928" 

        fmt.Println("----- create content generation task -----")
        // 创建3D生成任务
        createReq := model.CreateContentGenerationTaskRequest{
                Model: modelEp,
                Content: []*model.CreateContentGenerationContentItem{
                        {
                                // 参数
                                Type: model.ContentGenerationContentItemTypeText,
                                Text: volcengine.String("--subdivisionlevel medium --fileformat glb"),
                        },
                        {
                                // 图片URL
                                Type: model.ContentGenerationContentItemTypeImage,
                                ImageURL: &model.ImageURL{
                                        URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seed3d_imageTo3d.png", 
                                },
                        },
                },
        }

        createResponse, err := client.CreateContentGenerationTask(ctx, createReq)
        if err != nil {
                fmt.Printf("create content generation error: %v", err)
                return
        }
        fmt.Printf("Task Created with ID: %s", createResponse.ID)
}
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

属性
content.**type **`string`
输入内容的类型，此处应为 `text`。
content.**text **`string`
输入给模型的文本内容，包括：
**参数（选填）**：通过`--[parameters]`的方式，控制3D文件输出的规格，详情见 **模型文本命令(选填****）**。
信息类型
**图片信息**`object`
输入给模型的图片内容，模型会根据输入的2D图像生成完整的3D文件。
**文本信息**`object`
输入给模型的文本内容，当前仅支持参数。
***
输入内容的类型，此处应为 `image_url`。
content.**image_url **`object`
输入给模型的图片对象。
示例
content.image_url.**url **`string`
图片信息，可以是图片URL或图片 Base64 编码。
图片URL：请确保图片URL可被访问。
Base64编码：请遵循此格式`data:image/<图片格式>;base64,<Base64编码>`，注意 `<图片格式>` 需小写，如 `data:image/png;base64,{base64_image}`。
说明
传入图片需要满足以下条件：
总像素：小于 4096×4096 px
大小：小于等于10MB
格式支持：jpg、jpeg、png、webp、bmp
[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/experience/vision)[模型列表](https://www.volcengine.com/docs/82379/1330310#_3d%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1544106?lang=zh#_3d%E7%94%9F%E6%88%90%E6%A8%A1%E5%9E%8B)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[调用教程](https://www.volcengine.com/docs/82379/1874993)[接口文档](https://www.volcengine.com/docs/82379/1856293)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
本接口仅支持 API Key 鉴权，请在 [获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 页面，获取长效 API Key。
**模型支持的3D生成能力简介**
**doubao-seed3d**
doubao-seed3d-1-0-250928：图生3D，根据您输入图片（1张）+参数（可选）生成一个带纹理和 PBR 材质的3D文件。
// 指定生成的3D文件的多边形面的数量为100000面，生成的有纹理的模型文件格式为obj
"content":[
{
"type":"text",
"text":"--subdivisionlevel medium --fileformat obj"
}
]
