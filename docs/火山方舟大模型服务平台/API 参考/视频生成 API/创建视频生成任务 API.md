# 创建视频生成任务 API

`POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks`[运行](https://api.volcengine.com/api-explorer/?action=CreateContentsGenerationsTasks&data=%7B%7D&groupName=%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90API&query=%7B%7D&serviceCode=ark&version=2024-01-01)
本文介绍创建视频生成任务 API 的输入输出参数，供您使用接口时查阅字段含义。模型会依据传入的图片及文本信息生成视频，待生成完成后，您可以按条件查询任务并获取生成的视频。
请求参数
跳转 [响应参数](#L9tzcCyD)
请求体
**model**`string`
您需要调用的模型的 ID （Model ID），[开通模型服务](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)，并[查询 Model ID](https://www.volcengine.com/docs/82379/1330310) 。
您也可通过 Endpoint ID 来调用模型，获得限流、计费类型（前付费/后付费）、运行状态查询、监控、安全等高级能力，可参考[获取 Endpoint ID](https://www.volcengine.com/docs/82379/1099522)。
**content**`object[]`
输入给模型，生成视频的信息，支持文本信息和图片信息。
**callback_url**`string`
填写本次生成任务结果的回调通知地址。当视频生成任务有状态变化时，方舟将向此地址推送 POST 请求。
回调请求内容结构与[查询任务API](https://www.volcengine.com/docs/82379/1521309)的返回体一致。
回调返回的 status 包括以下状态：
queued：排队中。
running：任务运行中。
succeeded： 任务成功。（如发送失败，即5秒内没有接收到成功发送的信息，回调三次）
failed：任务失败。（如发送失败，即5秒内没有接收到成功发送的信息，回调三次）
expired：任务超时，即任务处于**运行中或排队中**状态超过过期时间。可通过 **execution_expires_after **字段设置过期时间。
**return_last_frame**`boolean``默认值 false`
true：返回生成视频的尾帧图像。设置为 `true` 后，可通过 [查询视频生成任务接口](https://www.volcengine.com/docs/82379/1521309) 获取视频的尾帧图像。尾帧图像的格式为 png，宽高像素值与生成的视频保持一致，无水印。
使用该参数可实现生成多个连续视频：以上一个生成视频的尾帧作为下一个视频任务的首帧，快速生成多个连续视频，调用示例详见 [教程](https://www.volcengine.com/docs/82379/1366799?lang=zh#%E7%94%9F%E6%88%90%E5%A4%9A%E4%B8%AA%E8%BF%9E%E7%BB%AD%E8%A7%86%E9%A2%91)。
false：不返回生成视频的尾帧图像。
**service_tier****new**`string``默认值 default`
不支持修改已提交任务的服务等级
指定处理本次请求的服务等级类型，枚举值：
default：在线推理模式，RPM 和并发数配额较低（详见 [模型列表](https://www.volcengine.com/docs/82379/1330310?lang=zh#%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)），适合对推理时效性要求较高的场景。
flex：离线推理模式，TPD 配额更高（详见 [模型列表](https://www.volcengine.com/docs/82379/1330310?lang=zh#%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)），价格为在线推理的 50%， 适合对推理时延要求不高的场景。
**execution_expires_after****new**`integer``默认值 172800`
任务超时阈值。指定任务提交后的过期时间（单位：秒），从 **created at** 时间戳开始计算。默认值 172800 秒，即 48 小时。取值范围：[3600，259200]。
不论使用哪种 **service_tier**，都建议根据业务场景设置合适的超时时间。超过该时间后任务会被自动终止，并标记为`expired`状态。
模型文本命令(选填)
**在文本提示词后追加 --[parameters] **，控制视频输出的规格，包括宽高比、帧率、分辨率等。
不同模型，可能对应支持不同的参数与取值，详见 [输出视频格式](https://www.volcengine.com/docs/82379/1366799?lang=zh#%E8%AE%BE%E7%BD%AE%E8%BE%93%E5%87%BA%E8%A7%86%E9%A2%91%E6%A0%BC%E5%BC%8F) 。当输入的参数或取值不符合所选的模型时，内容会被忽略或报错。
**resolution **`string``简写 rs`
doubao-seedance-1-0-lite 默认值：`720p`
doubao-seedance-1-0-pro&pro-fast 默认值：`1080p`
视频分辨率，枚举值：
480p
720p
1080p：参考图场景不支持
**ratio **`string``简写 rt`
文生视频默认值是`16:9`
图生视频默认值一般是`adaptive`。特别注意，参考图生视频的默认值是`16:9`
生成视频的宽高比例。不同宽高比对应的宽高像素值见下方表格。
16:9
4:3
1:1
3:4
9:16
21:9
adaptive：仅图生视频支持。根据所上传图片的比例，自动选择最合适的宽高比。
**duration**`integer``默认值 5``简写 dur`
duration 和 frames 二选一即可，frames 的优先级高于 duration。如果您希望生成整数秒的视频，建议指定 duration。
生成视频时长，单位：秒。支持 2~12 秒**new**。
**frames****new**`integer``简写 frames`
duration 和 frames 二选一即可，frames 的优先级高于 duration。如果您希望生成小数秒的视频，建议指定 frames。
生成视频的帧数。通过指定帧数，可以灵活控制生成视频的长度，生成小数秒的视频。
由于 frames 的取值限制，仅能支持有限小数秒，您需要根据公式推算最接近的帧数。
计算公式：帧数 = 时长 × 帧率（24）。
取值范围：支持 [29, 289] 区间内所有满足 `25 + 4n` 格式的整数值，其中 n 为正整数。
例如：假设需要生成 2.4 秒的视频，帧数=2.4×24=57.6。由于 frames 不支持 57.6，此时您只能选择一个最接近的值。根据 25+4n 计算出最接近的帧数为 57，实际生成的视频为 57/24=2.375 秒。
**framespersecond**`integer``默认值 24``简写 fps`
帧率，即一秒时间内视频画面数量。仅支持 24
**seed**`integer``默认值 -1``简写 seed`
种子整数，用于控制生成内容的随机性。
取值范围：[-1, 2^32-1]之间的整数。
注意
相同的请求下，模型收到不同的seed值，如：不指定seed值或令seed取值为-1（会使用随机数替代）、或手动变更seed值，将生成不同的结果。
相同的请求下，模型收到相同的seed值，会生成类似的结果，但不保证完全一致。
**camerafixed**`boolean``默认值 false``简写 cf`
参考图场景不支持
是否固定摄像头。枚举值：
true：固定摄像头。平台会在用户提示词中追加固定摄像头，实际效果不保证。
false：不固定摄像头。
**watermark**`boolean``默认值 false``简写 wm`
生成视频是否包含水印。枚举值：
false：不含水印。
true：含有水印。
响应参数
跳转 [请求参数](#RxN8G2nH)
**id **`string`
视频生成任务 ID 。创建视频生成任务为异步接口，获取 ID 后，需要通过 [查询视频生成任务 API](https://www.volcengine.com/docs/82379/1521309) 来查询视频生成任务的状态。任务成功后，会输出生成视频的`video_url`。

## 示例代码

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-pro-250528",
    "content": [
        {
            "type": "text",
            "text": "多个镜头。一名侦探进入一间光线昏暗的房间。他检查桌上的线索，手里拿起桌上的某个物品。镜头转向他正在思索。 --ratio 16:9"
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

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    print("----- create request -----")
    resp = client.content_generation.tasks.create(
        model="doubao-seedance-1-0-pro-250528",
        content=[{"text":"多个镜头。一名侦探进入一间光线昏暗的房间。他检查桌上的线索，手里拿起桌上的某个物品。镜头转向他正在思索。 --ratio 16:9","type":"text"}]
    )
    print(resp)
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
        client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
        ctx := context.Background()

        req := model.CreateContentGenerationTaskRequest{
                Model: "doubao-seedance-1-0-pro-250528",
                Content: []*model.CreateContentGenerationContentItem{
                        &model.CreateContentGenerationContentItem{
                                Type: "text",
                                Text: volcengine.String("多个镜头。一名侦探进入一间光线昏暗的房间。他检查桌上的线索，手里拿起桌上的某个物品。镜头转向他正在思索。 --ratio 16:9"),
                        },
                },
        }

        resp, err := client.CreateContentGenerationTask(ctx, req)
        if err != nil {
                fmt.Printf("create content generation error: %v\n", err)
                return
        }
        fmt.Printf("Task Created with ID: %s\n", resp.ID)
}
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.content.generation.CreateContentGenerationTaskRequest;
import com.volcengine.ark.runtime.model.content.generation.CreateContentGenerationTaskRequest.*;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

/*
# pom.xml
<dependency>
  <groupId>com.volcengine</groupId>
  <artifactId>volcengine-java-sdk-ark-runtime</artifactId>
  // 替换正式版本号
  <version>LATEST</version>
</dependency>
*/

public class Sample {

    static String apiKey = System.getenv("ARK_API_KEY");

    static ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
    static Dispatcher dispatcher = new Dispatcher();
    static ArkService service =
            ArkService.builder()
                    .dispatcher(dispatcher)
                    .connectionPool(connectionPool)
                    .apiKey(apiKey)
                    .build();

    public static void main(String[] args) throws JsonProcessingException {

        List<Content> contentForReqList = new ArrayList<>();
        Content elementForContentForReqList0 = new Content();
        elementForContentForReqList0.setType("text");
        elementForContentForReqList0.setText(
                "多个镜头。一名侦探进入一间光线昏暗的房间。他检查桌上的线索，手里拿起桌上的某个物品。镜头转向他正在思索。 --ratio 16:9");
        contentForReqList.add(elementForContentForReqList0);

        CreateContentGenerationTaskRequest req =
                CreateContentGenerationTaskRequest.builder()
                        .model("doubao-seedance-1-0-pro-250528")
                        .content(contentForReqList)
                        .build();

        service.createContentGenerationTask(req).toString();

        // shutdown service after all requests is finished
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

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-pro-fast-251015",
    "content": [
        {
            "type": "text",
            "text": "女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --ratio adaptive  --dur 5"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png"
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

from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    print("----- create request -----")
    resp = client.content_generation.tasks.create(
        model="doubao-seedance-1-0-pro-fast-251015",
        content=[{"text":"女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --ratio adaptive  --dur 5","type":"text"},{"image_url":{"url":"https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png"},"type":"image_url"}]
    )
    print(resp)

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
        client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
        ctx := context.Background()

        req := model.CreateContentGenerationTaskRequest{
                Model: "doubao-seedance-1-0-pro-fast-251015",
                Content: []*model.CreateContentGenerationContentItem{
                        &model.CreateContentGenerationContentItem{
                                Type: "text",
                                Text: volcengine.String("女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --ratio adaptive  --dur 5"),
                        },
                        &model.CreateContentGenerationContentItem{
                                Type: "image_url",
                                ImageURL: &model.ImageURL{
                                        URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png",
                                },
                        },
                },
        }

        resp, err := client.CreateContentGenerationTask(ctx, req)
        if err != nil {
                fmt.Printf("create content generation error: %v\n", err)
                return
        }
        fmt.Printf("Task Created with ID: %s\n", resp.ID)
}
```

**Response:**

```json
{
  "id": "cgt-2025******-****"
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.content.generation.CreateContentGenerationTaskRequest;
import com.volcengine.ark.runtime.model.content.generation.CreateContentGenerationTaskRequest.*;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

/*
# pom.xml
<dependency>
  <groupId>com.volcengine</groupId>
  <artifactId>volcengine-java-sdk-ark-runtime</artifactId>
  // 替换正式版本号
  <version>LATEST</version>
</dependency>
*/

public class Sample {

    static String apiKey = System.getenv("ARK_API_KEY");

    static ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
    static Dispatcher dispatcher = new Dispatcher();
    static ArkService service =
            ArkService.builder()
                    .dispatcher(dispatcher)
                    .connectionPool(connectionPool)
                    .apiKey(apiKey)
                    .build();

    public static void main(String[] args) throws JsonProcessingException {

        List<Content> contentForReqList = new ArrayList<>();
        Content elementForContentForReqList0 = new Content();
        elementForContentForReqList0.setType("text");
        elementForContentForReqList0.setText("女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --ratio adaptive  --dur 5");

        ImageUrl imageUrlForElementForContentForReqList1 = new ImageUrl();
        imageUrlForElementForContentForReqList1.setUrl(
                "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png");
        Content elementForContentForReqList1 = new Content();
        elementForContentForReqList1.setType("image_url");
        elementForContentForReqList1.setImageUrl(imageUrlForElementForContentForReqList1);
        contentForReqList.add(elementForContentForReqList0);
        contentForReqList.add(elementForContentForReqList1);

        CreateContentGenerationTaskRequest req =
                CreateContentGenerationTaskRequest.builder()
                        .model("doubao-seedance-1-0-pro-fast-251015")
                        .content(contentForReqList)
                        .build();

        service.createContentGenerationTask(req).toString();

        // shutdown service after all requests is finished
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

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-pro-250528",
    "content": [
         {
            "type": "text",
            "text": "360度环绕运镜"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_first_frame.jpeg"
            },
            "role": "first_frame"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_last_frame.jpeg"
            },
            "role": "last_frame"
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
from volcenginesdkarkruntime import Ark

# 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
# 初始化Ark客户端，从环境变量中读取您的API Key

client = Ark(
    # 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
    api_key=os.environ.get("ARK_API_KEY"),
)

print("----- create request -----")
# 创建视频生成任务
create_result = client.content_generation.tasks.create(
    # 设置模型ID
    model="doubao-seedance-1-0-pro-250528", 
    content=[
        {
            # 文本提示词与参数组合
            "type": "text",
            "text": "360度环绕运镜"
        },
        {
            # 首帧图片URL
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_first_frame.jpeg"
            },
            "role": "first_frame"
        },
        {
            # 尾帧图片URL
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_last_frame.jpeg"
            },
            "role": "last_frame"
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
           .build();
    public static void main(String[] args) {
        //替换为您的 Model ID
        String model = "doubao-seedance-1-0-pro-250528"; 
        
        System.out.println("----- CREATE Task Request -----");
        List<Content> contents = new ArrayList<>();

        // 添加文本提示词与参数组合
        contents.add(Content.builder()
                .type("text")
                .text("360度环绕运镜")
                .build());

        // 添加首帧图片URL
        contents.add(Content.builder()
                .type("image_url")
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder()
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_first_frame.jpeg")
                        .build())
                .role("first_frame")
                .build());

        // 添加尾帧图片URL
        contents.add(Content.builder()
                .type("image_url")
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder()
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_last_frame.jpeg")
                        .build())
                .role("last_frame")
                .build());
        
        // 创建视频生成任务
        CreateContentGenerationTaskRequest createRequest = CreateContentGenerationTaskRequest.builder()
                .model(model)
                .content(contents)
                .build();
 
        CreateContentGenerationTaskResult createResult = service.createContentGenerationTask(createRequest);
        System.out.println(createResult);
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
        "github.com/volcengine/volcengine-go-sdk/volcengine"
        "os"

        "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
        "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
)

func main() {
        // 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
        // 初始化Ark客户端，从环境变量中读取您的API Key
        client := arkruntime.NewClientWithApiKey(
                // 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
                os.Getenv("ARK_API_KEY"),
        )
        ctx := context.Background()      
        // 设置模型ID
        modelEp := "doubao-seedance-1-0-pro-250528" 

        fmt.Println("----- create content generation task -----")
        // 创建视频生成任务
        createReq := model.CreateContentGenerationTaskRequest{
                Model: modelEp,
                Content: []*model.CreateContentGenerationContentItem{
                        {
                                // 文本提示词与参数组合
                                Type: model.ContentGenerationContentItemTypeText,
                                Text: volcengine.String("360度环绕运镜"),
                        },
                        {
                                // 首帧图片URL
                                Type: model.ContentGenerationContentItemTypeImageURL,
                                ImageURL: &model.ImageURL{
                                        URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_first_frame.jpeg",
                                },
                                Role: volcengine.String("first_frame"),
                        },
                        {
                                // 尾帧图片URL
                                Type: model.ContentGenerationContentItemTypeImageURL,
                                ImageURL: &model.ImageURL{
                                        URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seepro_last_frame.jpeg",
                                },
                                Role: volcengine.String("last_frame"),
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

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-lite-i2v-250428",
    "content": [
         {
            "type": "text",
            "text": "[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_1.png"
            },
            "role": "reference_image"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_2.png"
            },
            "role": "reference_image"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_3.png"
            },
            "role": "reference_image"
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
from volcenginesdkarkruntime import Ark 
 
# 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中 
# 初始化Ark客户端，从环境变量中读取您的API Key 
 
client = Ark( 
    # 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改 
    api_key=os.environ.get("ARK_API_KEY"), 
 
print("----- create request -----") 
# 创建视频生成任务 
create_result = client.content_generation.tasks.create( 
    # 替换 <Model> 为模型的Model ID
    model="doubao-seedance-1-0-lite-i2v-250428",  
    content=[ 
        { 
            # 文本提示词与参数组合 
            "type": "text", 
            "text": "[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格" 
        }, 
        { 
            # 第一张参考图片URL 
            # 参考图需要传入1～4张
            "type": "image_url", 
            "image_url": { 
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_1.png" 
            }, 
            "role": "reference_image" 
        }, 
        { 
            # 第二张参考图片URL 
            "type": "image_url", 
            "image_url": { 
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_2.png" 
            }, 
            "role": "reference_image" 
        }, 
        { 
            # 第三张参考图片URL
            "type": "image_url", 
            "image_url": { 
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_3.png" 
            }, 
            "role": "reference_image" 
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
           .build(); 
    public static void main(String[] args) { 
        //替换为您的 Model ID
        String model = "doubao-seedance-1-0-lite-i2v-250428";  
         
        System.out.println("----- CREATE Task Request -----"); 
        List<Content> contents = new ArrayList<>(); 
 
        // 添加文本提示词与参数组合 
        contents.add(Content.builder() 
                .type("text") 
                .text("[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格") 
                .build()); 
 
        // 第一张参考图片URL 
        // 参考图需要传入1～4张
        contents.add(Content.builder() 
                .type("image_url") 
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder() 
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_1.png") 
                        .build()) 
                .role("reference_image") 
                .build()); 
 
        // 第二张参考图片URL  
        contents.add(Content.builder() 
                .type("image_url") 
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder() 
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_2.png") 
                        .build()) 
                .role("reference_image") 
                .build()); 
        
        // 第三张参考图片URL  
        contents.add(Content.builder() 
                .type("image_url") 
                .imageUrl(CreateContentGenerationTaskRequest.ImageUrl.builder() 
                        .url("https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_3.png") 
                        .build()) 
                .role("reference_image") 
                .build()); 
         
        // 创建视频生成任务 
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

    "github.com/volcengine/volcengine-go-sdk/volcengine"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
)

func main() {
    // 请确保您已将 API Key 存储在环境变量 ARK_API_KEY 中
    // 初始化Ark客户端，从环境变量中读取您的API Key
    client := arkruntime.NewClientWithApiKey(
        // 从环境变量中获取您的 API Key。此为默认方式，您可根据需要进行修改
        os.Getenv("ARK_API_KEY"),
    )
    ctx := context.Background()
    //替换为您的 Model ID
    modelEp := "doubao-seedance-1-0-lite-i2v-250428"

    fmt.Println("----- create content generation task -----")
    // 创建视频生成任务
    createReq := model.CreateContentGenerationTaskRequest{
        Model: modelEp,
        Content: []*model.CreateContentGenerationContentItem{
            {
                // 文本提示词与参数组合
                Type: model.ContentGenerationContentItemTypeText,
                Text: volcengine.String("[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格"),
            },
            {
                // 第一张参考图片URL
                // 参考图需要传入1～4张
                Type: model.ContentGenerationContentItemTypeImage,
                ImageURL: &model.ImageURL{
                    URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_1.png",
                },
                Role: volcengine.String("reference_image"),
            },
            {
                // 第二张参考图片URL
                Type: model.ContentGenerationContentItemTypeImage,
                ImageURL: &model.ImageURL{
                    URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_2.png",
                },
                Role: volcengine.String("reference_image"),
            },
            {
                // 第三张参考图片URL
                Type: model.ContentGenerationContentItemTypeImage,
                ImageURL: &model.ImageURL{
                    URL: "https://ark-project.tos-cn-beijing.volces.com/doc_image/seelite_ref_3.png",
                },
                Role: volcengine.String("reference_image"),
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

**curl**

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY" \
  -d '{
    "model": "doubao-seedance-1-0-lite-i2v-250428",
    "content": [
        {
            "type": "text",
            "text": "女孩抱着狐狸，女孩睁开眼，温柔地看向镜头，狐狸友善地抱着，镜头缓缓拉出，女孩的头发被风吹动  --ratio adaptive  --dur 5"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "data:image/png;base64,aHR0******cG5n"
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

**json**

```json
// 指定生成视频的宽高比为16:9，时长为 5 秒，帧率为 24 fps，分辨率为720p，包含水印，种子整数为11，不固定摄像头

//参数使用简写
"content": [
        {
            "type": "text",
            "text": "女孩抱着狐狸 --rs 720p --rt 16:9 --dur 5 --fps 24 --wm true --seed 11 --cf false"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png"
            }
        }
    ]
    
//参数使用全称
"content": [
        {
            "type": "text",
            "text": "女孩抱着狐狸 --resolution 720p --ratio 16:9 --duration 5 --framespersecond 24 --watermark true --seed 11 --camerafixed false"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/i2v_foxrgirl.png"
            }
        }
    ]
```

**Response:**

```json
无
```

属性
content.**type **`string`
输入内容的类型，此处应为 `text`。
content.**text **`string`
输入给模型的文本内容，描述期望生成的视频，包括：
**文本提示词（必填）**：支持中英文。建议不超过500字。字数过多信息容易分散，模型可能因此忽略细节，只关注重点，造成视频缺失部分元素。提示词的更多使用技巧请参见 [Seedance 提示词指南](https://www.volcengine.com/docs/82379/1587797)。
**参数（选填）**：在文本提示词后追加--[parameters]，控制视频输出的规格，详情见 **模型文本命令(选填****）**。
信息类型
**文本信息**`object`
输入给模型生成视频的内容，文本内容部分。
**图片信息**`object`
输入给模型生成视频的内容，图片信息部分。
***
**不同宽高比对应的宽高像素值**
Note：图生视频，选择的宽高比与您上传的图片宽高比不一致时，方舟会对您的图片进行裁剪，裁剪时会居中裁剪，详细规则见 [图片裁剪规则](https://www.volcengine.com/docs/82379/1366799#%E5%9B%BE%E7%89%87%E8%A3%81%E5%89%AA%E8%A7%84%E5%88%99)。
*
示例
输入内容的类型，此处应为 `image_url`。支持图片URL或图片 Base64 编码。
content.**image_url **`object`
输入给模型的图片对象。
content.**role **`string``条件必填`
图片的位置或用途。
首帧图生视频、首尾帧图生视频、参考图生视频为 3 种互斥的场景，不支持混用。
图生视频-首尾帧
**支持模型：**doubao-seedance-pro、doubao-seedance-lite-i2v
**字段role取值：**需要传入2个image_url对象，且字段role必填。
首帧图片对应的字段role为：first_frame
尾帧图片对应的字段role为：last_frame
说明
传入的首尾帧图片可相同。首尾帧图片的宽高比不一致时，以首帧图片为主，尾帧图片会自动裁剪适配。
[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/experience/vision)[模型列表](https://www.volcengine.com/docs/82379/1330310#%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1544106?redirect=1&lang=zh#02affcb8)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[调用教程](https://www.volcengine.com/docs/82379/1366799)[接口文档](https://www.volcengine.com/docs/82379/1520758)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
content.image_url.**url **`string`
图片信息，可以是图片URL或图片 Base64 编码。
图片URL：请确保图片URL可被访问。
Base64编码：请遵循此格式`data:image/<图片格式>;base64,<Base64编码>`，注意 `<图片格式>` 需小写，如 `data:image/png;base64,{base64_image}`。
传入图片需要满足以下条件：
图片格式：jpeg、png、webp、bmp、tiff、gif。
宽高比（宽/高）： (0.4, 2.5)
宽高长度（px）：(300, 6000)
大小：小于 30 MB
图生视频-参考图
**支持模型：**doubao-seedance-1-0-lite-i2v
**字段role取值：**需要传入1～4个image_url对象，且字段role必填。
每张参考图片对应的字段role均为：reference_image
参考图生视频功能的文本提示词，可以用自然语言指定多张图片的组合。但若想有更好的指令遵循效果，**推荐使用“[图1]xxx，[图2]xxx”的方式来指定图片**。
示例1：戴着眼镜穿着蓝色T恤的男生和柯基小狗，坐在草坪上，3D卡通风格
示例2：[图1]戴着眼镜穿着蓝色T恤的男生和[图2]的柯基小狗，坐在[图3]的草坪上，3D卡通风格
图生视频-首帧
**支持模型：**doubao-seedance-pro、doubao-seedance-pro-fast、doubao-seedance-lite-i2v
**字段role取值：**需要传入1个image_url对象，且字段role可不填，或字段role为：first_frame
本接口仅支持 API Key 鉴权，请在 [获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 页面，获取长效 API Key。
**不同模型支持的视频生成能力简介**
**doubao-seedance-pro**
图生视频-首尾帧，根据您输入的首帧图片+尾帧图片+文本提示词（可选）+参数（可选）生成目标视频。
图生视频-首帧，根据您输入的首帧图片+文本提示词（可选）+参数（可选）生成目标视频。
文生视频，根据您输入的文本提示词+参数（可选）生成目标视频。
**doubao-seedance-pro-fast****new**
图生视频-首帧，根据您输入的首帧图片+文本提示词（可选）+参数（可选）生成目标视频。
文生视频，根据您输入的文本提示词+参数（可选）生成目标视频。
**doubao-seedance-lite**
**doubao-seedance-1-0-lite-t2v：**文生视频，根据您输入的文本提示词+参数（可选）生成目标视频。
**doubao-seedance-1-0-lite-i2v：**
图生视频-参考图，根据您输入的**参考图片（1-4张）**+文本提示词（可选）+ 参数（可选）生成目标视频。
图生视频-首尾帧，根据您输入的首帧图片+尾帧图片+文本提示词（可选）+参数（可选）生成目标视频。
图生视频-首帧，根据您输入的首帧图片+文本提示词（可选）+参数（可选）生成目标视频。
// 指定生成视频的宽高比为16:9，时长为 5 秒，帧率为 24 fps，分辨率为720p，包含水印，种子整数为11，不固定摄像头
"content":[
{
"type":"text",
"text":"小猫对着镜头打哈欠。 --rs 720p --rt 16:9 --dur 5 --fps 24 --wm true --seed 11 --cf false"
}
]
2176×928
宽高像素值
宽高比
分辨率
1088×1920
1664×1248
640×640
704×1248
864×480
1248×704
544×736
960×960
1920×1088
1080p`参考图场景不支持`
736×544
1120×832
1440×1440
832×1120
1504×640
960×416
480×864
1248×1664
Tips：一键展开折叠，快速检索内容
打开页面右上角开关，**ctrl **+ **f** 可检索页面内所有内容。
