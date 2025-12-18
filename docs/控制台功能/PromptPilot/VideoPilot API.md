# VideoPilot API
VideoPilot API 套件使用户能够通过提示将参考视频转化为重新创作的作品。同时，用户可以对重新创作视频的关键参数施加约束或引导，这些参数包括场景、角色、动作、风格和相机设置。此外，该 API 套件允许通过提示和反馈来提升视频质量，它会根据用户反馈重新生成特定的视频片段，以提高质量或使视频更符合用户意图。
### Endpoints
#### **ImitateAndGenerateVideo**
根据参考视频和可选的参考图像，按照用户的编辑指令生成新视频。
##### **Parameters**

- **名称** | **类型** | **是否必填** | **描述**
- RefVideoUrl | string | ✅ | 参考视频的 URL，用于定义待模仿的运动、节奏或风格
- UserMessage | string | ✅ | 描述期望转换效果或创意概念的用户提示词（例如 “呈现霓虹都市夜景的视觉效果”）
- RefImages | list<string> | 可选（≤1 个） | 用于指导外观、色调或主体特征的参考图片 URL（即将扩展至 2 个）
- Model | string | 可选 | 生成模型选择，目前支持 doubao-seedance-1-0-pro-250528 和 doubao-seedance-1-0-lite-i2v-250428
- TimeBudget | integer | 可选（取值 1、2 或 3） | 分配的算力或质量等级，数值越高，可生成更长时长或更高质量的结果
- VideoRatio | string | 可选 | 视频宽高比（例如 “1:1”“16:9”）
- VideoResolution | string | 可选 | 视频分辨率（例如 “1080p”“720p”）
- ImitationSetting | string | 可选 | 模仿策略，例如'imitative' 或 'creative'

##### **Response**

- **字段** | **类型** | **描述**
- TaskId | string | 异步视频生成任务的唯一标识

Example API Usage
```C++
curl --location "https://prompt-pilot.cn-beijing.volces.com/video-pilot?Version=1.0&Action=ImitateAndGenerateVideo"
--header "Authorization: Bearer {VIDEO_PILOT_API_KEY}"
--header "Content-Type: application/json"
--data '{
    "RequestId": "'$(uuidgen)'",
    "WorkspaceId": "{VIDEO_PILOT_WORKSPACE_ID}",
    "RefVideoUrl": "https://link/to/your/video/ or data:video/mp4;base64,...",
    "UserMessage": "Anime Girl Instead",
    "RefImages": ["data:image/png;base64,..."],
    "Model": "doubao-seedance-1-0-pro-250528",
    "TimeBudget": 2,
    "VideoRatio": "16:9",
    "VideoResolution": "1080p"
}'
```

Example Return
```Plain Text
{"Result":{"TaskId":"vgt-2025103112362-ASDFG"},"Error":null}
```

#### **GetTaskResult**
检索已完成任务的生成结果。
##### **Parameters**

- **名称** | **类型** | **是否必填** | **描述**
- TaskId | string | ✅ | 用于查询任务状态的标识 ID（即 TaskId）。

##### **Response**
TaskObject:

- **字段** | **类型** | **描述**
- TaskId | str | 查询任务 ID
- VideoSegments | list<VideoSegmentObject> | 生成的视频片段列表：所有生成的视频分段集合。
- FullVideo | str | 完整视频的 URL：包含完整视频的网络访问链接。
- TaskStatus | str | 任务状态：当前视频生成任务的进展情况

**VideoSegmentObject**

- **字段** | **类型** | **描述**
- SegmentId | str | 唯一标识：用于区分该片段的专属 ID。
- IndexInVideo | integer | 该分段在完整视频时序中的排序位置。
- VideoURL | string | 生成的视频片段 URL
- KeyframeURL | string | 该片段的代表性关键帧图片 URL
- ScenePrompt | string | 自动生成或优化后的场景描述提示词
- VersionNum | int | 当前片段的版本号

Example API Usage
```C++
curl --location "https://prompt-pilot.cn-beijing.volces.com/video-pilot?Version=1.0&Action=GetTaskResult"
--header "Authorization: Bearer {VIDEO_PILOT_API_KEY}"
--header "Content-Type: application/json"
--data '{
    "RequestId": "'$(uuidgen)'",
    "WorkspaceId": "{VIDEO_PILOT_WORKSPACE_ID}",
    "TaskId": "Your Task ID",
}'
```

Example Return - Pending
```Plain Text
{"Result":{"TaskId":"Your Task ID","FullVideo":"","TaskStatus":"pending","VideoSegments":[]},"Error":null}
```

Example Return - Finished
```SQL
{
  "Result": {
    "TaskId": "vgt-2025103112362-ASDFG",
    "FullVideo": "Video URL",
    "TaskStatus": "completed",
    "VideoSegments": [
      {
        "SegmentId": "vpv-2025104536724-ABCDE",
        "IndexInVideo": 0,
        "VideoURL": "Video URL",
        "KeyframeURL": "Keyframe URL",
        "ScenePrompt": "Your scene script",
        "VersionNum": 1
      },
      {
        "SegmentId": "vpv-20251031150729-3Au32",
        "IndexInVideo": 1,
        "VideoURL": "Video URL",
        "KeyframeURL": "Keyframe URL",
        "ScenePrompt": "Your scene script",
        "VersionNum": 1
      }
    ]
  },
  "Error": null
}
```

#### **ListSegmentVersions**
检索特定视频片段的所有历史版本或替代版本，例如通过多次迭代或重新生成所产生的版本。
##### **Parameters**

- 名称 | 类型 | 是否必填 | 描述
- TaskId | string | ✅ | 与视频关联的原始任务 ID
- SegmentIndex | integer | ✅ | 用于查询版本化片段的索引

##### **Response**

- 字段 | 类型 | 描述
- VideoSegments | list<video_segment> | 根据索引返回的视频片段列表

Example API Usage
```C++
curl --location "https://prompt-pilot.cn-beijing.volces.com/video-pilot?Version=1.0&Action=ListSegmentVersions"
--header "Authorization: Bearer {VIDEO_PILOT_API_KEY}"
--header "Content-Type: application/json"
--data '{
    "RequestId": "'$(uuidgen)'",
    "WorkspaceId": "{VIDEO_PILOT_WORKSPACE_ID}",
    "TaskId": "Your TaskId",
    "SegmentIndex": 0,
}'
```

Example Return
```Plain Text
{
  "Result": {
    "VideoSegments": [
      {
        "SegmentId": "vpv-2025104536724-ABCDE",
        "IndexInVideo": 0,
        "VideoURL": "Video URL",
        "KeyframeURL": "Keyframe URL",
        "ScenePrompt": "Your scene script",
        "VersionNum": 1
      },
      {
        "SegmentId": "vpv-20251031150729-3Au32",
        "IndexInVideo": 0,
        "VideoURL": "Video URL",
        "KeyframeURL": "Keyframe URL",
        "ScenePrompt": "Your scene script",
        "VersionNum": 2
      }
    ]
  },
  "Error": null
}
```

#### **ListTask**
列出当前用户/会话的所有现有任务 ID。
##### **Parameters**
None.
##### **Response**

- 字段 | 类型 | 描述
- Tasks | list<TaskObject> | 该用户名下所有的任务标识集合

Example API Usage
```C++
curl --location "https://prompt-pilot.cn-beijing.volces.com/video-pilot?Version=1.0&Action=ListTask"
--header "Authorization: Bearer {VIDEO_PILOT_API_KEY}"
--header "Content-Type: application/json"
--data '{
    "RequestId": "'$(uuidgen)'",
    "WorkspaceId": "{VIDEO_PILOT_WORKSPACE_ID}"
}'
```

Example Return
```Plain Text
{
  "Result": {
    "Tasks": [
      {
        "TaskId": "vgt-20251120191558-ASDFG",
        "FullVideo": "Full Video URL",
        "TaskStatus": "completed",
        "VideoSegments": null
      },
      {
        "TaskId": "vgt-20251120191600-ASDFG",
        "FullVideo": "Full Video URL",
        "TaskStatus": "completed",
        "VideoSegments": null
      },
      {
        "TaskId": "vgt-20251120191615-8yM8X",
        "FullVideo": "",
        "TaskStatus": "failed",
        "VideoSegments": null
      }
    ]
  },
  "Error": null
}
```

#### **RegenerateVideoSegmentFromFeedback**
根据用户反馈重新生成特定视频片段，提高质量或使其更符合预期。
##### **Parameters**

- 名称 | 类型 | 是否必填 | 描述
- TaskId | string | ✅ | 与视频关联的原始任务 ID
- SegmentId | integer | ✅ | 作为重新生成基准版本的片段 ID
- FeedbackMessage | string | ✅ | 描述需修改内容的用户反馈（例如 “调柔光线并添加雾气”）

##### **Response**

- 字段 | 类型 | 描述
- TaskId | string | 重新生成任务对应的新任务 ID

Example API Usage
```C++
curl --location "https://prompt-pilot.cn-beijing.volces.com/video-pilot?Version=1.0&Action=RegenerateVideoSegmentFromFeedback"
--header "Authorization: Bearer {VIDEO_PILOT_API_KEY}"
--header "Content-Type: application/json"
--data '{
    "RequestId": "'$(uuidgen)'",
    "WorkspaceId": "{VIDEO_PILOT_WORKSPACE_ID}",
    "TaskId": "Your TaskId",
    "FeedbackMessage": "Add floating cats",
    "SegmentId": "Get it from get task result",
}'
```

Example Return
```Plain Text
{"Result":{"TaskId":"Your Task ID"},"Error":null}
```

## ExtractKeyFramesAndPlot
该 API 旨在通过提取视频关键帧和场景描述来源，构建热门视频的提示词库。
### Parameters

* `VideoURL`: 可下载的链接（如来自 tos 的链接）或 base64 格式内容

### Response

- 字段名 | 类型 | 是否必填 | 描述
- TaskId | string | ✅ | 用于查询任务状态的任务 ID

## GetExtractKeyFramesAndPlotResult
### Parameters

- 字段名 | 类型 | 是否必填 | 描述
- TaskId | string | ✅ | 用于查询任务状态的任务 ID

### Response
响应将包含视频各片段的首帧、末帧及对应的场景提示词，结构如下：
响应将包含视频各片段的首帧、末帧及对应的场景提示词，结构如下：

- 字段名 | 类型 | 描述
- VideoSegments | list<RefVideoSegmentObject> | 按索引排列的视频片段列表
- TaskStatus | string | 任务状态

RefVideoSegmentObject

- 字段名 | 类型 | 描述
- SegmentId | string | 片段唯一标识
- IndexInVideo | integer | 片段在整个视频中的位置
- VideoURL | string | 生成的视频片段链接
- KeyframeURL | string | 该片段的代表性关键帧图片链接
- ScenePrompt | string | 自动生成或优化的场景描述提示词
- LastframeURL | string | 该片段的末帧图片链接

### 典型用法

1. 若需通过批量模仿生成视频，可使用`imitate_and_generate_video`接口。

该 API 会还原参考视频的细节，同时应用你提供的指令创建新视频。
为获得最佳效果，建议提供一张参考图片，用于指导视觉风格或主体特征。
任务完成后，可通过`get_task_result`接口获取生成的视频输出。

2. 若需微调或优化生成视频的特定部分，可使用`regenerate_video_segment_from_feedback`接口。

该接口接收用户反馈，仅重新生成选中的片段，并根据你的输入进行优化。
对于需要多次尝试才能达到理想效果的复杂视频片段，该接口尤为实用。

## AgenticShortVideoGeneration
智能短视频生成功能可自动处理多张参考图片，用于视频生成。
### Parameters

- 参数名 | 类型 | 是否必填 | 描述
- UserMessage | string | ✅ | 描述期望的转化效果或核心概念的用户提示词（例如：“将其制作成霓虹城市的夜景风格”）
- RefImages | list<string> | 可选（最多 2 张） | 最多 2 张参考图片链接，用于指导视频的外观、基调或主体特征（后续将支持更多参考图片）
- Model | string | 可选 | 指定用于生成的模型，目前支持 doubao-seedance-1-0-pro-250528 和 doubao-seedance-1-0-lite-i2v-250428
- VideoRatio | string | 可选 | 视频宽高比（例如：“1:1”、“16:9”）
- VideoResolution | string | 可选 | 视频分辨率（例如：“1080p”、“720p”）
- Duration | int | 可选（默认 5 秒） | 视频时长（单位：秒）

### Response
响应将包含异步视频生成任务的唯一标识，结构如下：

- 字段名 | 类型 | 描述
- TaskId | string | 异步视频生成任务的唯一标识
