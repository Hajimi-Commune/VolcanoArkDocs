# AgentPilot OpenAPI 使用指南
## **环境变量配置**

1. **配置 API_KEY 和 API_URL** ：请正确配置 API_KEY 和 API_URL，确保 API 请求的正常发送与接收。
2. **Workspace ID 获取与配置** ：当前版本暂时支持从环境变量配置 workspace id ，接口暂不支持每次调用单独指定。获取方式为在 PromptPilot 页面切换到某个个人或团队空间后，从顶部 URL 截取 "workspaceId=" 之后的内容，即为当前 workspace_id。后续预计会更新 SDK，支持通过参数传入 workspace id 。

```Bash
export AGENTPILOT_API_URL=https://prompt-pilot.cn-beijing.volces.com
export AGENTPILOT_API_KEY=<YOUR API KEY FROM PROMPTPILOT CONSOLE>
export AGENTPILOT_WORKSPACE_ID=<YOUR WORKSPACE ID FROM PROMPTPILOT CONSOLE>
```

## **Task 和 Prompt 管理**
### **创建任务**
创建任务后，可登录 PromptPilot Console 页面，在任务管理中查看最新创建的任务。
#### **创建文本理解任务** 
所有 model_name 查看[https://www.volcengine.com/docs/82379/1330310](https://www.volcengine.com/docs/82379/1330310) ，成功后将看到创建后的任务信息和 Prompt 版本信息。
```Bash
# Create a new task of text understanding
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=CreateTask"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_category": "DEFAULT",
    "task_name": "test_text_task",
    "messages": [
        {
            "role": "user",
            "content": "这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}"
        }
    ],
    "model_name": "doubao-seed-1.6-250615",
    "criteria": "这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确"
}'
```

如果成功，你将看到创建后的任务信息和Prompt版本信息
```JSON
{"Result":{"success":true,"prompt_version":{"prompt_version_id":"prv-20250905104255-nafnT","task_id":"ta-20250905104255-r8u5s","version":"v1","messages":[{"content":[{"text":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}","type":"text"}],"role":"user"}],"variables_name":["variable_1","variable_2"],"variables_types":null,"model_name":"doubao-seed-1.6-250615","temperature":1.0,"top_p":0.7,"eval_dimension":"这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确","created_at":"2025-08-28T14:58:56.938785","updated_at":"2025-08-28T14:58:56.938811"}},"Error":null}
```

**创建视觉理解任务**
```Bash
# Create a new task of visual understanding
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=CreateTask"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_category": "MULTIMODAL",
    "task_name": "test_visual_task",
    "messages": [
        {
            "role": "user",
            "content": "根据文字 {{text_var}} 和配图 {{image_var}}，写一个小故事。"
        }
    ],
    "variables_types": {
        "text_var": "text",
        "image_var": "image_url"
    }, 
    "model_name": "doubao-seed-1.6-250615",
    "criteria": "这是一个视觉理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确"
}'
```

如果成功，你将看到创建后的任务信息和Prompt版本信息
```JSON
{"Result":{"success":true,"prompt_version":{"prompt_version_id":"prv-20250905104357-mV3yD","task_id":"ta-20250905104357-uDfgL","version":"v1","messages":[{"role":"user","content":[{"type":"text","text":"根据文字 {{text_var}} 和配图 "},{"type":"image_url","image_url":{"url":"{{image_var}}"}},{"type":"text","text":"，写一个小故事。"}]}],"variables_name":["text_var","image_var"],"variables_types":{"text_var":"text","image_var":"image"},"model_name":"doubao-seed-1.6-250615","temperature":1.0,"top_p":0.7,"eval_dimension":"这是一个视觉理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确","created_at":"2025-08-28T15:01:32.633732","updated_at":"2025-08-28T15:01:32.633755"}},"Error":null}
```

**创建多轮对话任务**
```Bash
# Create a new task of dialog
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=CreateTask"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_category": "DIALOG",
    "task_name": "test_dialog_task",
    "task_name": "test_dialog_task",
    "messages": [
        {
            "role": "system",
            "content": "你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。"
        }
    ],
    "model_name": "doubao-seed-1.6-250615",
    "criteria": "这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确"
}'
```

如果成功，你将看到创建后的任务信息和Prompt版本信息
```JSON
{"Result":{"success":true,"prompt_version":{"prompt_version_id":"prv-20250905104417-bsqcQ","task_id":"ta-20250905104417-aaQfG","version":"v1","messages":[{"content":[{"text":"你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。","type":"text"}],"role":"system"}],"variables_name":[],"variables_types":null,"model_name":"doubao-seed-1.6-250615","temperature":1.0,"top_p":0.7,"eval_dimension":"这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确","created_at":"2025-08-28T15:01:28.277163","updated_at":"2025-08-28T15:01:28.277196"}},"Error":null}
```

### **查看某一个 version 的信息**
将示例代码中的 task_id 和 version 分别替换为实际的 task_id 和 version 后运行，成功后将看到该 task_id 下指定 version 的 Prompt 版本信息。
```Bash
# Set task_id and version obtained from above commands or from PromptPilot Console UI
export task_id="ta-20250905101023-qEeWK"
export version="v1"

# Get a prompt version
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=GetPromptTemplate"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_id": "'$task_id'",
    "version": "'$version'"
}'
```

```JSON
{"Result":{"success":true,"prompt_version":{"prompt_version_id":"prv-20250905104255-nafnT","task_id":"ta-20250905104255-r8u5s","version":"v1","messages":[{"content":[{"text":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}","type":"text"}],"role":"user"}],"variables_name":["variable_1","variable_2"],"variables_types":null,"model_name":"doubao-seed-1.6-250615","temperature":1.0,"top_p":0.7,"eval_dimension":"这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确","created_at":"2025-09-05T10:42:55","updated_at":"2025-09-05T10:42:55"}},"Error":null}
```

### **列出一个 task 下的所有版本信息** 
将示例代码中的 task_id 替换为实际的 task_id 后运行，成功后将看到该 task_id 下的所有 Prompt 版本信息。
```Bash
# Set task_id obtained from above commands or from PromptPilot Console UI
export task_id="ta-20250905101023-qEeWK"

# List all prompt versions of a task
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=ListPromptTemplates"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_id": "'$task_id'"
}'
```

```JSON
{"Result":{"success":true,"prompt_versions":[{"prompt_version_id":"prv-20250905104255-nafnT","task_id":"ta-20250905104255-r8u5s","version":"v1","messages":[{"content":[{"text":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}","type":"text"}],"role":"user"}],"variables_name":["variable_1","variable_2"],"variables_types":null,"model_name":"doubao-seed-1.6-250615","temperature":1.0,"top_p":0.7,"eval_dimension":"这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确","created_at":"2025-08-28T15:01:25.470669","updated_at":"2025-08-28T15:01:25.470694"}]},"Error":null}
```

##  **流式生成 Prompt**
###  文本理解
```Bash
# Test text understanding tasks
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=GeneratePromptStream"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_type": "DEFAULT",
    "rule": "从提供的文档中提取总结要点，要点数量不超过10个。",
    "model_name": "doubao-seed-1.6-250615",
    "temperature": 1.0,
    "top_p": 0.7
}'
```

如果成功，你将在Terminal看到流式生成的Prompt结果。
### 视觉理解
```Bash
# Test visual understanding tasks
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=GeneratePromptStream"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_type": "MULTIMODAL",
    "rule": "依据提供给的题目图片，对其中的题目展开解答。",
    "model_name": "doubao-seed-1.6-250615",
    "temperature": 1.0,
    "top_p": 0.7
}'
```

如果成功，你将在Terminal看到流式生成的Prompt结果。
### 多轮对话
```Bash
# Test dialog tasks
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=GeneratePromptStream"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_type": "DIALOG",
    "rule": "你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。",
    "model_name": "doubao-seed-1.6-250615",
    "temperature": 1.0,
    "top_p": 0.7
}'
```

如果成功，你将在Terminal看到流式生成的Prompt结果。
## **回流数据和反馈**
### **数据回流范围** 
可以将变量、图片、对话、模型回答、参照回答、评分及评分原因等各类事件回流到平台。在 PromptPilot Console 页面对应的 task_id 和 version 的批量评测页可查看回流的数据。
### **上传方式**

* **通过完整 curl 命令上传** ：将示例代码中的 task_id 替换为实际的 task_id 后运行，成功后将看到返回后的状态，包括每条数据的上传状态。

```Bash
# Assume you have already known a task_id and one of its version
export task_id="ta-20250904134351-AUtEk"
export version="v1"

# Upload a test dataset with 2 data records
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=TrackingEvent"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "tracking_events": [
        {
            "run_id": "'$(uuidgen)'",
            "run_type": "llm",
            "event_name": "upload a record",
            "task_id": "'$task_id'",
            "prompt_version": "'$version'",
            "variables": {
                "variable_1": "value_1",
                "variable_2": "value_2"
            },
            "output_message": {
                "role": "assistant",
                "content": "这是一个文本理解任务的model answer for value 1 and value 2"
            },
            "reference": {
                "role": "assitant",
                "content": "这是一个文本理解任务的reference answer for value 1 and value 2"
            },
            "feedback": {
                "human_score": 5,
                "human_analysis": "该条数据的人工评分原因",
                "human_confidence": 1.0
            }
        },
        {
            "run_id": "'$(uuidgen)'",
            "run_type": "llm",
            "event_name": "upload a record",
            "task_id": "'$task_id'",
            "prompt_version": "'$version'",
            "variables": {
                "variable_1": "value_3",
                "variable_2": "value_4"
            },
            "output_message": {
                "role": "assistant",
                "content": "这是一个文本理解任务的model answer for value 3 and value 4"
            },
            "reference": {
                "role": "assitant",
                "content": "这是一个文本理解任务的reference answer for value 3 and value 4"
            },
            "feedback": {
                "human_score": 1,
                "human_analysis": "该条数据的人工评分原因",
                "human_confidence": 1.0
            }
        }
    ]
}'
```

```JSON
{"Result":{"success":true,"tracking_events":{"E5C683EB-B0F7-4B2F-BAFC-7AF77AA90666":{"task_id":"ta-20250905104255-r8u5s","prompt_version":"v1","run_id":"E5C683EB-B0F7-4B2F-BAFC-7AF77AA90666","session_id":null,"status":{"success":true,"error":null}},"D005BE7E-7E15-46FF-85D9-DE35A6FB4F4F":{"task_id":"ta-20250905104255-r8u5s","prompt_version":"v1","run_id":"D005BE7E-7E15-46FF-85D9-DE35A6FB4F4F","session_id":null,"status":{"success":true,"error":null}}}},"Error":null}
```

此时，登陆PromptPilot Console页面的批量评测，你将看到回流的数据
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cda8380785f4430c90be916f494da875~tplv-goo7wpa0wc-image.image)

* **通过 JSON 文件上传数据集** ：
   * 下载示例文件[AgentPilot Open API - 发布版](https://bytedance.larkoffice.com/docx/E3cQd122roiXIRxZWzZuVidgs8f?from=lark_search_qa&ccm_open_type=lark_search_qa#HntSd9zv5o4lktxPqvOu1VSPsNh)到某一路径下。
   * 在该文件所在路径运行指定命令，或修改为该文件所在绝对路径。注意需将示例 JSON 文件中的 task_id 替换为自己的 task_id。

```Bash
# Alternatively, you can upload the test dataset in a JSON file.
# You need to set the related fields before submit a request, which may include request_id, task_id, version, run_id, etc.
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=TrackingEvent"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '@./test_dataset.json'
```

## Prompt优化和报告获取
### 提交优化任务
对某个task_id任务下的某个version提交优化job，将task_id和version分别替换为你的task_id和version
```Bash
# Assume you have already known a task_id and one of its version
export task_id="ta-20250904134351-AUtEk"
export version="v1"

# Submit an optimization job for the task_id of the version.
curl --location "$AGENTPILOT_API_URL/agent-pilot-optimize?Version=2024-01-01&Action=OptimizeServiceStartOptimize"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_id": "'$task_id'",
    "version": "'$version'"
}'
```

如果成功，你将得到optimize_job_id，通过该optimize_job_id，可以帮助你

* 查询优化任务的进展，
* 监控中间结果，
* 以及获取优化报告

```JSON
{"Result":{"TaskId":"ta-20250905104255-r8u5s","Version":"v1","OptimizeJobId":"20250905105402_XGtu9_OptimizePrompt"},"Error":null}
```

### 查看任务进展
将下面optimize_job_id替换为你的optimize_job_id后运行
```Bash
export optimize_job_id="20250904152330_zUuSs_OptimizePrompt"

# Check progress during the optimization (State=3) or after the optimization (State=7)
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=OptimizeServiceGetOptimizeProgress"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "optimize_job_id": "'$optimize_job_id'"
}'
```

- State | 解释
- 1 | 优化任务已创建
- 2 | 优化任务运行中
- 3 | 优化任务已完成且优化成功
- 4 | 优化任务已完成且优化失败

比如运行中 (State=2) 如下图
```JSON
{"Result":{"JobInfo":{"JobId":"20250904211525_EvMYQ_OptimizePrompt","Version":"v1","State":2,"OptimizedVersion":null,"CreatedTime":"2025-09-05T13:15:25","UpdatedTime":"2025-09-05T13:15:35"},"Progress":{"ProgressPercent":0.0,"TotalCnt":2,"BetterCnt":0,"WorseCnt":0,"UnchangedCnt":2,"InitFullscoreCnt":1,"FullscoreCntList":[1],"InitAverageScore":3.0,"AverageScoreList":[3.0],"OptimizeTokenConsumption":0,"OptimalPrompt":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}"}},"Error":null}
```

比如优化成功 (State=3) 如下图
```JSON
{"Result":{"JobInfo":{"JobId":"20250904211525_EvMYQ_OptimizePrompt","Version":"v1","State":3,"OptimizedVersion":"v2","CreatedTime":"2025-09-05T13:15:25","UpdatedTime":"2025-09-05T13:16:02"},"Progress":{"ProgressPercent":1.0,"TotalCnt":2,"BetterCnt":0,"WorseCnt":0,"UnchangedCnt":2,"InitFullscoreCnt":1,"FullscoreCntList":[1,1,1],"InitAverageScore":3.0,"AverageScoreList":[3.0,3.0,3.0],"OptimizeTokenConsumption":0,"OptimalPrompt":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}"}},"Error":null}
```

:::tip
你也可以登陆PromptPilot Console页面查看实时进展监控曲线
:::
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e4c2326eaa2b4ada87235ba53e2ce546~tplv-goo7wpa0wc-image.image)

### 获取优化报告
分别将task_id，base_version, ref_version替换为你的优化任务的task_id, base_version，ref_version，你将得到你刚才提交的从base_version优化到ref_version的优化报告。
```Bash
# Set the base and ref versions when the optimization progress is done (State=7).
# The base version is "Version", and the "OptimizedVersion".
export base_version="v1" # Version in previous command
export ref_version="v2" # OptimizedVersion in previous command

# Then, you can get the report.
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=OptimizeServiceGetReport"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "task_id": "'$task_id'",
    "base_version": "'$base_version'",
    "ref_version": "'$ref_version'"
}'
```

如果运行成功，你将看到
```JSON
{"Result":{"base":{"records":[{"record_id":"dr-20250904212324-ymasZ","variables":[{"name":"variable_1","value":"value_1","type":"text"},{"name":"variable_2","value":"value_2","type":"text"}],"model_answer":"这是一个文本理解任务的model answer for value 1 and value 2","ref_answer":"这是一个文本理解任务的reference answer for value 1 and value 2","score":5,"analysis":"该条数据的人工评分原因","confidence":null},{"record_id":"dr-20250904212326-LnpPS","variables":[{"name":"variable_1","value":"value_3","type":"text"},{"name":"variable_2","value":"value_4","type":"text"}],"model_answer":"这是一个文本理解任务的model answer for value 3 and value 4","ref_answer":"这是一个文本理解任务的reference answer for value 3 and value 4","score":1,"analysis":"该条数据的人工评分原因","confidence":null}],"prompt":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}","metric":"","avg_score":3.0},"ref":{"records":[{"record_id":"dr-20250904212605-Stwkk","variables":[{"name":"variable_1","value":"value_1","type":"text"},{"name":"variable_2","value":"value_2","type":"text"}],"model_answer":"这是一个文本理解任务的model answer for value 1 and value 2","ref_answer":"这是一个文本理解任务的reference answer for value 1 and value 2","score":5,"analysis":"该条数据的人工评分原因","confidence":1.0},{"record_id":"dr-20250904212605-49zY5","variables":[{"name":"variable_1","value":"value_3","type":"text"},{"name":"variable_2","value":"value_4","type":"text"}],"model_answer":"这是一个文本理解任务的model answer for value 3 and value 4","ref_answer":"这是一个文本理解任务的reference answer for value 3 and value 4","score":1,"analysis":"该条数据的人工评分原因","confidence":1.0}],"prompt":"这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}","metric":"- 若回答完全未能结合输入的{{variable_1}}和{{variable_2}}，或者存在重大错误、误导性信息，得1分。\n- 若回答有错误、遗漏了输入信息相关的重要内容或表达不清晰，得2分。\n- 若回答基本结合了输入信息，内容正确但不够全面，细节或深度有所欠缺，得3分。\n- 若回答正确且较为完整地结合了输入信息，表达清晰，但还有可提升的空间，得4分。\n- 若回答准确、全面地结合了输入信息，表达清晰流畅，完全满足甚至超出预期要求，得5分。","avg_score":3.0}},"Error":null}
```

## 评估服务
### 提交一个评估请求
#### 文本理解
```Bash
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=EvalServiceInputResponseEvaluate"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "eval_data_example": {
        "example_id": "example_001",
        "messages": [
            {
                "role": "user",
                "content": "What is the capital of France?"
            }
        ],
        "response": [
            {
                "role": "assistant",
                "content": "The capital of France is Paris."
            }
        ],
        "reference": [
            {
                "role": "assistant",
                "content": "Paris is the capital city of France."
            }
        ],
        "score": null,
        "analysis": null,
        "confidence": null
    },
    "metric_prompt": {
        "criteria": "Check if the response correctly identifies Paris as the capital of France."
    }
}'
```

如果成功，你将看到如下格式的输出
```JSON
{"Result":{"success":true,"evaled_data_example":{"example_id":"example_001","messages":[{"role":"user","content":"What is the capital of France?"}],"response":[{"role":"assistant","content":"The capital of France is Paris."}],"reference":[{"role":"assistant","content":"Paris is the capital city of France."}],"score":5,"analysis":"模型回答明确指出法国的首都是巴黎，与理想回答意思一致，正确完成了评估规则的要求，因此给予5分。","confidence":1.0}},"Error":null}
```

#### 视觉理解
```Bash
# Test the evaluation for a multimodal example
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=EvalServiceInputResponseEvaluate"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "eval_data_example": {
        "example_id": "example_001",
        "messages": [
            {
                "role": "user",
                "content": [                    
                    {"text": "How many people are in the image?", "type": "text"},
                    {"image_url": {"url": "https://lf3-static.bytednsdoc.com/obj/eden-cn/lm_sth/ljhwZthlaukjlkulzlp/autope/examples/3.jpeg"}, "type": "image_url"}
                ]
            }
        ],
        "response": [
            {
                "role": "assistant",
                "content": "There are 3 people in the image."
            }
        ],
        "reference": [
            {
                "role": "assistant",
                "content": "There are 5 people in the image."
            }
        ],
        "score": null,
        "analysis": null,
        "confidence": null
    },
    "metric_prompt": {
        "criteria": "Check if the response correctly counts the number of people in the image."
    }
}'
```

如果成功，你将看到如下格式的输出
```JSON
{"Result":{"success":true,"evaled_data_example":{"example_id":"example_001","messages":[{"role":"user","content":[{"type":"text","text":"How many people are in the image?"},{"type":"image_url","image_url":{"url":"https://lf3-static.bytednsdoc.com/obj/eden-cn/lm_sth/ljhwZthlaukjlkulzlp/autope/examples/3.jpeg"}}]}],"response":[{"role":"assistant","content":"There are 3 people in the image."}],"reference":[{"role":"assistant","content":"There are 5 people in the image."}],"score":1,"analysis":"根据评估规则，模型回答需要正确计算图片中的人数。理想回答是5人，而模型回答是3人，明显错误，未达到评估要求。","confidence":1.0}},"Error":null}
```

### 生成一个评估标准
#### 文本理解
```Bash

# Test the criteria generation for a set of examples
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=EvalServiceCriteriaGeneration"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "eval_data_examples": [
        {
            "example_id": "example_001",
            "messages": [
                {
                    "role": "user",
                    "content": "What is the weather like today?"
                }
            ],
            "response": [
                {
                    "role": "assistant",
                    "content": "The weather is pleasant and warm."
                }
            ],
            "reference": [
                {
                    "role": "assistant",
                    "content": "It is sunny with a high of 75 degrees Fahrenheit."
                }
            ],
            "score": 4,
            "analysis": "The response accurately describes the weather but could be more detailed.",
            "confidence": 0.85
        },
        {
            "example_id": "example_002",
            "messages": [
                {
                    "role": "user",
                    "content": "Will it rain tomorrow?"
                }
            ],
            "response": [
                {
                    "role": "assistant",
                    "content": "There is a chance of rain tomorrow."
                }
            ],
            "reference": [
                {
                    "role": "assistant",
                    "content": "According to the forecast, there is a 60% chance of rain tomorrow with possible thunderstorms in the afternoon."
                }
            ],
            "score": 2,
            "analysis": "The response is too vague and lacks the specific details provided in the reference answer.",
            "confidence": 0.9
        }
    ]
}'
```

如果成功，你将看到如下格式的输出
```JSON
{"Result":{"success":true,"generated_criteria":"\n回答与问题无关或完全错误，评为 1 分\n回答过于模糊，缺乏关键细节，评为 2 分\n能准确回答问题，包含一定关键信息，评为 3 分\n回答准确，但详细程度有提升空间，评为 4 分\n回答准确且详细全面，提供丰富相关信息，评为 5 分\n"},"Error":null}
```

#### 视觉理解
```Bash
curl --location "$AGENTPILOT_API_URL/agent-pilot?Version=2024-01-01&Action=EvalServiceCriteriaGeneration"
--header "Authorization: Bearer $AGENTPILOT_API_KEY"
--header "Content-Type: application/json"
--data '{
    "request_id": "'$(uuidgen)'",
    "workspace_id": "'$AGENTPILOT_WORKSPACE_ID'",
    "eval_data_examples": [
        {
            "example_id": "example_001",
            "messages": [
                {
                    "role": "user",
                    "content": [                    
                        {"text": "How many people are in the image?", "type": "text"},
                        {"image_url": {"url": "https://lf3-static.bytednsdoc.com/obj/eden-cn/lm_sth/ljhwZthlaukjlkulzlp/autope/examples/3.jpeg"}, "type": "image_url"}
                    ]
                }
            ],
            "response": [
                {
                    "role": "assistant",
                    "content": "There are 3 people in the image."
                }
            ],
            "reference": [
                {
                    "role": "assistant",
                    "content": "There are 5 people in the image."
                }
            ],
            "score": 1,
            "analysis": "The response incorrectly counts the number of people in the image.",
            "confidence": 1.0
        },
        {
            "example_id": "example_002",
            "messages": [
                {
                    "role": "user",
                    "content": [                    
                        {"text": "How many people are in the image?", "type": "text"},
                        {"image_url": {"url": "https://ark-auto-2100000825-cn-beijing-default.tos-cn-beijing.volces.com/experience_video_demo/experience_videofU0eQ1xX.png"}, "type": "image_url"}
                    ]
                }
            ],
            "response": [
                {
                    "role": "assistant",
                    "content": "There are 1 people in the image."
                }
            ],
            "reference": [
                {
                    "role": "assistant",
                    "content": "There are 2 people in the image."
                }
            ],
            "score": 1,
            "analysis": "The response incorrectly counts the number of people in the image.",
            "confidence": 1.0
        }
    ]
}'
```

如果成功，你将看到如下格式的输出
```JSON
{"Result":{"success":true,"generated_criteria":"\n模型回答的人数与图片实际人数完全一致，得5分。\n模型回答的人数与图片实际人数相差1人，得3分。\n模型回答的人数与图片实际人数相差2人及以上，得1分。\n"},"Error":null}
```
