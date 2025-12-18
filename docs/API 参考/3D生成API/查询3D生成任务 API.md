# 查询3D生成任务 API

`GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/{id}`
查询3D生成任务的状态。
说明
仅支持查询最近 7 天的历史数据。时间计算统一采用UTC时间戳，返回的7天历史数据范围以用户实际发起查询请求的时刻为基准（精确到秒），时间戳区间为 [T-7天, T)。
请求参数
跳转 [响应参数](#7mi8G8RI)
**id**`string`
您需要查询的3D生成任务的 ID 。
说明
上面参数为Query String Parameters，在URL String中传入。
响应参数
跳转 [请求参数](#RxN8G2nH)
**id **`string`
3D生成任务 ID 。
**model**`string`
任务实际使用的模型名称和版本，`模型名称-版本`。
**status**`string`
任务状态，以及相关的信息：
`queued`：排队中。
`running`：任务运行中。
`cancelled`：取消任务，取消状态24h自动删除（只支持`queued`状态的任务被取消）。
`succeeded`： 任务成功。
`failed`：任务失败。
**error**`object`
错误提示信息。
任务失败时返回错误数据，具体参见 [错误处理](https://www.volcengine.com/docs/82379/1299023#方舟错误码)。
**created_at**`integer`
任务创建时间的 Unix 时间戳（秒）。
**updated_at**`integer`
任务当前状态更新时间的 Unix 时间戳（秒）。
**content**`object`
3D生成任务的输出内容。
**subdivisionlevel**`string`
3D文件中多边形面的数量。
**fileformat**`string`
生成的3D文件格式。
**usage**`object`
本次请求的 token 用量。

## 示例代码

**curl**

```bash
curl -X GET https://ark.cn-beijing.volces.com/api/v3/contents/generations/tasks/cgt-2025**** \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ARK_API_KEY"
```

**Response:**

```json
{
    "id"："cgt-2025******-****",
    "model":"doubao-seed3d-1-0-250928",
    "status":"succeeded",
    "created_at":"1718049470",
    "updated_at":"1718049470",
    "content":{
        "file_url":"https://xxx"，
     }，
    "subdivisionlevel": "medium",
    "fileformat": "glb",
    "usage":{
        "total_tokens":30000，
        "completion_tokens":30000，
    }
}
```

**python**

```python
from volcenginesdkarkruntime import Ark

client = Ark(api_key=os.environ.get("ARK_API_KEY"))

if __name__ == "__main__":
    resp = client.content_generation.tasks.get(
        task_id="cgt-2025****",
    )
    print(resp)
```

**Response:**

```json
{
    "id"："cgt-2025******-****",
    "model":"doubao-seed3d-1-0-250928",
    "status":"succeeded",
    "created_at":"1718049470",
    "updated_at":"1718049470",
    "content":{
        "file_url":"https://xxx"，
     }，
    "subdivisionlevel": "medium",
    "fileformat": "glb",
    "usage":{
        "total_tokens":30000，
        "completion_tokens":30000，
    }
}
```

**java**

```java
package com.volcengine.sample;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.volcengine.ark.runtime.model.content.generation.GetContentGenerationTaskRequest;
import com.volcengine.ark.runtime.service.ArkService;
import java.util.concurrent.TimeUnit;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

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

        GetContentGenerationTaskRequest req = GetContentGenerationTaskRequest.builder().build();

        service.getContentGenerationTask(req).toString();
        System.out.println(service.getContentGenerationTask(req));

        service.shutdownExecutor();
    }
}
```

**Response:**

```json
{
    "id"："cgt-2025******-****",
    "model":"doubao-seed3d-1-0-250928",
    "status":"succeeded",
    "created_at":"1718049470",
    "updated_at":"1718049470",
    "content":{
        "file_url":"https://xxx"，
     }，
    "subdivisionlevel": "medium",
    "fileformat": "glb",
    "usage":{
        "total_tokens":30000，
        "completion_tokens":30000，
    }
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
)

func main() {
        client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
        ctx := context.Background()

        req := model.GetContentGenerationTaskRequest{}
        resp, err := client.GetContentGenerationTask(ctx, req)
        if err != nil {
                fmt.Printf("get content generation task error: %v
", err)
                return
        }
        fmt.Printf(resp)
}
```

**Response:**

```json
{
    "id"："cgt-2025******-****",
    "model":"doubao-seed3d-1-0-250928",
    "status":"succeeded",
    "created_at":"1718049470",
    "updated_at":"1718049470",
    "content":{
        "file_url":"https://xxx"，
     }，
    "subdivisionlevel": "medium",
    "fileformat": "glb",
    "usage":{
        "total_tokens":30000，
        "completion_tokens":30000，
    }
}
```

本接口支持 API Key 鉴权，详见[鉴权认证方式](https://www.volcengine.com/docs/82379/1298459)。
属性
error.**code**`string`
错误码。
error.**message**`string`
usage.**completion_tokens**`integer`
模型输出3D文件花费的 token 数量。
usage.**total_tokens**`integer`
本次请求消耗的总 token 数量。3D生成模型不统计输入 token，输入 token 为 0，故 **total_tokens**=**completion_tokens**。
content.f**ile_url**`string`
生成的3D文件的url。为保障信息安全，生成的3D文件会在24小时后被清理，请及时转存。
模型会自动将生成的3D文件打包为.zip格式以供下载，压缩包内文件的格式与您创建任务时所指定的格式一致。
[体验中心](https://console.volcengine.com/ark/region:ark+cn-beijing/experience/vision)[模型列表](https://www.volcengine.com/docs/82379/1330310#_3d%E7%94%9F%E6%88%90%E8%83%BD%E5%8A%9B)[模型计费](https://www.volcengine.com/docs/82379/1544106?lang=zh#_3d%E7%94%9F%E6%88%90%E6%A8%A1%E5%9E%8B)[API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)
[调用教程](https://www.volcengine.com/docs/82379/1874993)[接口文档](https://www.volcengine.com/docs/82379/1860231)[常见问题](https://www.volcengine.com/docs/82379/1359411)[开通模型](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement?LLM=%7B%7D&OpenTokenDrawer=false)
**
