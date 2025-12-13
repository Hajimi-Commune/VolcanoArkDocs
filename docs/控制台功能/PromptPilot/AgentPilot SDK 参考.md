# AgentPilot SDK 参考
本文为您介绍如何使用 AgentPilot SDK。
:::warning
PromptPilot 2025年9月12日已进行系统升级，支持用户下单订阅个人或者团队空间套餐，因此 SDK 请求也需要指定到某个个人或团队空间。

**主要变更：**
引入 **订阅空间（workspace）** 概念。
所有 SDK 请求需新增 `workspace_id` 参数，以指定个人或团队空间。
**对 SDK 用户的影响：**

* **新用户**：可直接订阅空间，通过页面获取 API Key，开始使用 SDK。
* **老用户**：若已在产品链路中集成 SDK，请尽快升级，以避免影响线上业务。

**迁移与兼容策略：**

* **9月12日 – 9月19日**：

旧版 SDK（≤ v0.0.9）未携带 `workspace_id` 时，将默认指向个人空间。
升级至新版 SDK（≥ v0.0.9）后，可通过环境变量 `AGENTPILOT_WORKSPACE_ID` 指定个人或团队空间。workspace_id 的获取和配置参考下面文档。

* **9月19日后**：旧版 SDK（≤ v0.0.9）请求将被拒绝。请务必在此之前完成套餐购买、空间创建，并升级至新版 SDK。

未来我们也会在 Python 代码接口允许用户直接传入 workspace_id。

**升级方式：**
升级SDK到最新版本：`pip install -U agent-pilot-sdk`
配置环境变量`AGENTPILOT_WORKSPACE_ID`，指定到个人或团队空间
:::
# 概述
AgentPilot SDK 为开发者提供工具、接口和资源，简化应用开发流程，帮助开发者在构建 Agent 时，*以低侵入，灵活的方式集成 PromptPilot 的核心功能，为 Agent 赋能*。
## 功能
**SDK 的核心价值**：通过自动化的探查（Probing）手段，**低成本地获取高质量反馈数据**，进而构建高效且可持续的模型优化闭环。这一闭环能够赋能 Agent，使其更好地适配 Scaling Law。

* 降低数据采集成本
* 助力 Agent 适配 Scaling Law

具体而言，用户可以以低侵入灵活的方式将 PromptPilot 内部原子能力自由组合，通过 SDK 提供的工具、接口和资源，简化应用开发，让开发者可快速集成 PromptPilot 的核心功能，包括但不限于：

| | | \
|**功能** |**简要描述** |
|---|---|
| | | \
|Task 和 Prompt 管理 |通过 SDK 直接管理 task 和 prompt，读取和列出版本信息，模型配置，以及评分标准等。 |
| | | \
|数据闭环和反馈 |支持与 Ark client 兼容的 completion 接口，用户可以推理的同时将数据和反馈回流到 PromptPilot 页面。 |
| | | \
|Prompt 优化和报告获取 |通过 SDK 提交 prompt 优化任务，并读取评估报告。 |
| | | \
|在线评估 |用户可提交自定义评估请求得到评估结果，是否回流可选。 |
| | | \
|Badcase 检测 |用户可以根据评估 score 和 confidence 判断哪些是 badcase，并通过 Console 页面标注。 |
| | | \
|Prompt 生成 |根据 task_description 生成 prompt。 |

## 优势
基于易用，模块化，易扩展，开放的设计原则，尽可能保证：

* 低侵入性（易用）：对于用户已有 Agent/Workflow 代码，通过修改几行代码就可以实现本文列举的功能，集成简单。
* 模块化：可以单独或者任意组合使用，比如上文提到的评估，回流，反馈，prompt优化等模块。
* 易扩展和开放性：支持多种 AI 模型/接口，多种模态数据，无缝支持通用开源框架（比如LiteLLM, LangChain, LlamaIndex等开源LLM/Agent应用开发框架），并且易于通过已有功能扩展使用场景。

## 核心概念
**任务和版本 (task and version)**：
下图为 PromptPilot 的控制台（Console）和 SDK 的对应关系。一个任务 Task 一般创建在某个个人或团队空间 (workspace) 下面，任务通常包含多个 Prompt 版本。每一个版本围绕一个 "Prompt" 包含相关版本信息，比如task_id, version, prompt 和变量列表，用于评估的标准（criteria），模型名称和配置如 temperature，top_p 等。
为了通过 SDK 发生数据回流，用户一般需要在请求中传入 workspace_id (指定某个空间),  task_id，version，AGENTPILOT_API_KEY (用于回流服务的鉴权)，ARK_API_KEY (用于方舟上的大模型推理)，即可完成大部分操作。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/572faf3aaa1b4a84abc38ce873ad94d4~tplv-goo7wpa0wc-image.image =828x)

**回流事件 (run)**：
大模型的一次调用如果被抽样，就会对应一次“回流事件”，一个回流事件对应平台“批量”页面中的一行数据。
# 注意事项 

* **回流采样比率**：默认采样率为0.1，即每条数据有0.1的概率回流，用户可以根据情况调整。
* **回流数据上限**：默认为2000条，即某task_id, version下数据条数 > 2000时，回流会失败并在SDK调用处返回log信息。该限制是与 Console 页面对齐。如需提高上限请提交工单。
* **当前仅支持“文本理解”和“视觉理解”任务的评分模式**。如果需要支持更多任务类型，比如多轮对话和 Tools，请提交工单。
* **model_name 为模型名 + 版本，并且只能是 prompt 生成页模型下拉菜单中支持的模型**。如果需要支持自定义或者第三方ep，请提交工单。

# 快速上手
## **安装和配置**

1. **安装 SDK 并设置相关环境变量**

```Bash
# 安装最新SDK
pip install -U agent-pilot-sdk

# 指定AGENTPILOT_API_KEY，AGENTPILOT_API_URL，和AGENTPILOT_WORKSPACE_ID
# NOTE: 如未配置环境变量，也可在每个接口主动传入参数api_key, api_url, 或workspace_id
export AGENTPILOT_API_KEY=yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy
export AGENTPILOT_API_URL=https://prompt-pilot.cn-beijing.volces.com
export AGENTPILOT_WORKSPACE_ID=ws-zzzzzzzzzzzzzz-zzzzz

# 可选步骤：用于方舟推理服务的API_KEY, 如果不使用方舟模型推理服务，可以不设置
# NOTE: 下面大部分SDK功能不需要方舟推理服务的API Key也可使用
export ARK_API_KEY=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

2. **AGENTPILOT_API_KEY的获取：**

:::warning
* 在获取 API Key 时，需先将套餐升级为标准版或团队版，之后按照平台指定流程操作以完成获取。 
* 由于安全需要，火山平台账号必须进行实名认证后才可以获取 API Key。
:::
[独立站版本入口](https://promptpilot.volcengine.com/) | [火山方舟版本入口](https://console.volcengine.com/ark)
如下图所示，点击左侧 “API Key” 按钮 -> 点击 “选择使用”，此时会拷贝你的 API_KEY。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5e71f7eee08745c2ab37d491b89e98f3~tplv-goo7wpa0wc-image.image =3382x)

3. **workspace_id 的获取 [new 20250912]**：SDK 的请求需要发送到某个个人或团队空间。

[独立站版本入口](https://promptpilot.volcengine.com/) | [火山方舟版本入口](https://console.volcengine.com/ark)
在 PromptPilot 页面切换到某个个人或团队空间后，顶部 URL 可以看到 "workspaceId=ws-zzzzzzzzzzzzzz-zzzzz" 开头的字样， `ws-zzzzzzzzzzzzzz-zzzzz` 即为当前 workspace_id。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/74c4ae7d10d74a83a7a22b13405eef73~tplv-goo7wpa0wc-image.image =1280x)

4. **task_id的获取**

[独立站版本入口](https://promptpilot.volcengine.com/) | [火山方舟版本入口](https://console.volcengine.com/ark)
展开左侧“Prompt 调试” -> 选择某一任务类型比如 “文本理解” 任务 -> 点击上方 “+” 号新建任务。任务建好后，点击“任务名”可以看到“任务信息”弹窗，获取该任务的task_id，也可以从顶部 URL 截取。
<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/639e66b4c8fb48128d311b4d7e4f18dc~tplv-goo7wpa0wc-image.image" width="3582px" /></div>

## Jupyter Notebook 示例
安装好 SDK 后，可以尝试运行下面的 Jupyter Notebook 中的示例代码，一步步模拟线上生成 Prompt，采集数据，实时评估，回流数据，Prompt 优化。
<Attachment link="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a7cc92742a8e49f280cf4fad3f6cf2ff~tplv-goo7wpa0wc-image.image" name="sdk_demo_open.ipynb" ></Attachment>
**步骤1**：在 PromptPilot 创建一个新的任务，并上传数据集。其中，step_1_create_task_and_upload_short.xlsx ：模拟用户上传到平台的数据
<Attachment link="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/fa4076bf0cb9431db3537f7304db4be1~tplv-goo7wpa0wc-image.image" name="step_1_create_task_and_upload_short.xlsx" ></Attachment>
**步骤2**：运行 Notebook 使用 SDK 对数据回流闭环。其中，step_2_use_sdk_to_track_short.xlsx：模拟用户通过回流接口传到平台的数据。
<Attachment link="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4861114bb5a7464b836242d0134f3c75~tplv-goo7wpa0wc-image.image" name="step_2_use_sdk_to_track_short.xlsx" ></Attachment>

# 详细接口列表
:::warning
**注1**：所有支持传递 api_key 和 api_url 的接口，如果未指定或者传入为 None，系统会自动读取 AGENTPILOT_API_KEY 和 AGENTPILOT_API_URL。
**注2**：近期我们也会允许从接口传入指定 workspace_id。
:::
## 1. Task 和 Prompt 管理
### [New 20250912] 如何创建新任务?
#### 文本理解
```Python
import agent_pilot as ap

prompt_version = ap.create_task(
    name="text_test_task",
    task_type="DEFAULT",
    prompt="这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}",
    model_name="doubao-seed-1.6-250615",
    criteria="这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确。请输出1-5之间的一个数字。",
    api_key=None,
    api_url=None,
)

print(prompt_version)
```

如果成功，你将看到一个PromptVersion打印出来
```Bash
task_id='ta-20250911214134-Mpfzy' version='v1' messages=[{'content': [{'text': '这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}', 'type': 'text'}], 'role': 'user'}] variable_names=['variable_1', 'variable_2'] model_name='doubao-seed-1.6-250615' temperature=1.0 top_p=0.7 criteria='这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确。请输出1-5之间的一个数字。' prompt='这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}'
```

#### 视觉理解
对于视觉理解任务，你还需要指定每一个变量的类型。
```Python
import agent_pilot as ap

prompt_version = ap.create_task(
    name="test_visual_task",
    task_type="MULTIMODAL",
    prompt=[{
        "role": "user",
        "content": "根据文字 {{text_var}} 和配图 {{image_var}}，写一个小故事。"}], 
    variable_types={
        "text_var": "text",       # 变量名：变量类型
        "image_var": "image_url", # 变量名：变量类型
    }, 
    model_name="doubao-seed-1.6-250615",
    criteria="这是一个视觉理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确",
    api_key=None,
    api_url=None,
)

print(prompt_version)
```

如果成功，你将看到一个PromptVersion打印出来
```SQL
task_id='ta-20250912130823-sPbH4' version='v1' messages=[{'role': 'user', 'content': [{'type': 'text', 'text': '根据文字 {{text_var}} 和配图 '}, {'type': 'image_url', 'image_url': {'url': '{{image_var}}'}}, {'type': 'text', 'text': '，写一个小故事。'}]}] variable_names=['text_var', 'image_var'] model_name='doubao-seed-1.6-250615' temperature=1.0 top_p=0.7 criteria='这是一个视觉理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确' prompt='根据文字 {{text_var}} 和配图 '
```

#### 多轮对话
```Python
import agent_pilot as ap

prompt_version = ap.create_task(
    name="test_dialog_task",
    task_type="DIALOG",
    prompt=[{
        "role": "system",
        "content": "你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。"}], 
    model_name="doubao-seed-1.6-250615",
    criteria="这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确，请输出1-5之间的一个数字。",
    api_key=None,
    api_url=None,
)

print(prompt_version)
```

如果成功，你将看到一个PromptVersion打印出来，
```Bash
task_id='ta-20250912131455-frsAN' version='v1' messages=[{'content': [{'text': '你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。', 'type': 'text'}], 'role': 'system'}] variable_names=[] model_name='doubao-seed-1.6-250615' temperature=1.0 top_p=0.7 criteria='这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确，请输出1-5之间的一个数字。' prompt='你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。'
```

:::warning
多轮对话任务 prompt 的 role 为 "system"，这个和文本理解和视觉理解任务 role 为 user 不同。
model_name 即 Model ID，查看 [模型列表](https://www.volcengine.com/docs/82379/1330310)获取 。
:::
### 如何查看某一个 prompt 版本的相关信息？
调用接口 ap.get_prompt()，输入 task_id 和 version，可以得到该版本相关信息，如下：
```Python
import agent_pilot as ap

prompt_version = ap.get_prompt(task_id="ta-20250101000000-aaaaa", version="v1", api_key=None, api_url=None)

print(prompt_version)
```

如果成功，你将看到一个 prompt version
```Bash
task_id='ta-20250912131455-frsAN' version='v1' messages=[{'content': [{'text': '你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。', 'type': 'text'}], 'role': 'system'}] variable_names=[] model_name='doubao-seed-1.6-250615' temperature=1.0 top_p=0.7 criteria='这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确，请输出1-5之间的一个数字。' prompt='你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。'
```

### 如何列出一个 task 下的所有版本信息？
调用接口 ap.list_prompts()，只需要通过 task_id 即可。
```Python
import agent_pilot as ap

prompt_versions = ap.list_prompts(task_id="ta-20250101000000-aaaaa", api_key=None, api_url=None)
print(prompt_versions)
```

如果成功，你将看到一个 prompt version 列表。
```Bash
[PromptVersion(task_id='ta-20250912131455-frsAN', version='v1', messages=[{'content': [{'text': '你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。', 'type': 'text'}], 'role': 'system'}], variable_names=[], model_name='doubao-seed-1.6-250615', temperature=1.0, top_p=0.7, criteria='这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确，请输出1-5之间的一个数字。', prompt='你是一个MBTI人格分析大师，负责根据用户提供的个人信息分析用户的MBTI人格。')]
```

### 如何获得 metric 相关信息？
调用接口 ap.get_metric()，只需要通过 task_id 即可。
```Python
import agent_pilot as ap

metric = ap.get_metric(task_id="ta-20250101000000-aaaaa", version="v2", api_key=None, api_url=None)
print(metric.criteria)
```

如果成功，你将看到当前评分标准
```Bash
这是一个多轮对话任务的评分标准，5分制，1分表示完全错误，5分表示完全正确，请输出1-5之间的一个数字
```

### 2.回流数据和反馈
### 如何得创建一个可以回流数据的 Ark client？
调用接口 ap.probing()，对一个 Ark client 打 patch 即可。之后该 client 既可以当作大模型推理调用，也可以回流数据到 Console 某一个 Task。
```Python
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os

# 创建 client
client = Ark(api_key=os.getenv('ARK_API_KEY'))
ap.probing(object=client)
```

### 如何自动回流数据？
调用接口 ap.render()，渲染 task_id, version，和 variables，得到渲染后的 input_dict，将其传入 Ark client 调用 completion接口。系统会自动获取对应版本的 prompt，拼装成大模型推理请求的输入并得到模型回答。++大模型请求的 completion 接口与 Ark client 兼容。++
```Python
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os

# 创建 client
client = Ark(api_key=os.getenv('ARK_API_KEY'))
ap.probing(object=client, sample_rate=1.0)

# 渲染推理请求的输入
input_dict = ap.render(
    task_id="ta-20250101000000-aaaaa",
    version="v1",
    variables={
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
    api_key=None,
    api_url=None,
)

# 调用模型推理，和方舟SDK一样
completion = client.chat.completions.create(model='doubao-seed-1.6-250615', **input_dict)

# 可选步骤：flush队列中的待发送事件消息，立即发送。如果不flush，系统会自动小批量定时回流，提升性能
ap.flush()

# 打印 completion 结果
print(completion.choices[0].message)

# 打印 事件 run_id，对应于批量页某一行数据，也可用于后续追踪，修改，覆盖数据
print(completion.run.id)
```

如果成功，你将看到模型 completion 和该条数据的 run_id。此时，登录 Console 页面可以看到系统新增了一条数据，包括变量和模型回答。
```Markdown
ChatCompletionMessage(content='以下是一个包含变量1和变量2的文本理解任务prompt示例，其中变量1和变量2为可替换的具体内容，任务类型为“基于上下文回答问题”：\n\n\n**任务描述**：这是一个文本理解任务，请根据提供的上下文文本回答指定问题。  \n**上下文文本（变量1）**：{此处替换为具体的上下文内容，如一段新闻、故事、说明文字等}  \n**待回答问题（变量2）**：{此处替换为具体问题，如“文本中提到的事件发生时间是什么？”“作者认为该现象的主要原因是什么？”等}  \n**任务要求**：请基于上下文文本，准确、简洁地回答问题，无需额外补充信息或解释。  \n\n\n**示例（替换变量后）**：  \n**上下文文本（变量1）**：“2023年10月12日，中国科学院宣布在量子计算领域取得重大突破，团队成功研发出新一代光量子计算机‘九章三号’，其计算速度较上一代提升约100万倍，可在特定任务中实现‘量子霸权’。该成果发表于《自然》杂志，标志着我国在量子科技领域进入世界前列。”  \n**待回答问题（变量2）**：“新一代光量子计算机的名称是什么？”  \n**任务要求**：请基于上下文文本，准确、简洁地回答问题，无需额外补充信息或解释。  \n\n（此时模型应输出：九章三号）  \n\n\n可根据实际需求调整任务类型（如文本摘要、情感分析、关系判断等），并明确变量1和变量2的具体角色（如“待分析文本”“对比文本”“选项列表”等）。', role='assistant', function_call=None, tool_calls=None, audio=None, reasoning_content='\n用户现在给出的prompt是“这是一个文本理解任务的prompt，它有变量1的内容和变量2的内容”。看起来用户可能是想让我根据这个信息，生成一个具体的文本理解任务prompt，或者需要我解释如何处理这种包含变量的prompt？首先，我需要明确用户的需求。用户可能是在构建一个需要动态替换变量的prompt模板，比如变量1和变量2分别代表不同的文本内容，然后任务是让模型理解这两个变量的内容并完成某个任务，比如比较、总结、回答问题等。\n\n首先，我应该考虑文本理解任务常见的类型，比如文本分类、情感分析、实体识别、关系抽取、问答、摘要、文本匹配等。然后，变量1和变量2可能是任务中的输入文本，比如变量1是“待分析的文本”，变量2是“问题”或者“另一个待比较的文本”。\n\n例如，如果任务是问答，那么可能的prompt结构是：“请根据以下文本（变量1）回答问题（变量2）。文本：{变量1} 问题：{变量2} 回答：”。如果是文本比较，可能是：“比较以下两段文本（变量1和变量2），指出它们的相同点和不同点。文本1：{变量1} 文本2：{变量2} 相同点： 不同点：”。\n\n用户可能希望我给出一个具体的示例，说明如何将变量1和变量2融入到文本理解任务的prompt中。所以我需要先确定一个具体的文本理解任务类型，然后定义变量1和变量2的角色，再构建完整的prompt模板。\n\n另外，用户可能没有明确说明任务类型，所以可能需要先询问任务类型，但根据当前信息，可能需要我假设一个常见任务来举例。比如问答任务是比较常见的，或者文本摘要。\n\n假设任务是问答，那么变量1是“上下文文本”，变量2是“问题”。那么prompt可以是：“请基于以下上下文文本回答问题。上下文：{变量1} 问题：{变量2} 请直接给出答案，无需额外解释。”\n\n或者如果是文本情感分析，变量1是“待分析文本”，变量2可能是“情感类别选项”，比如“积极、消极、中性”。那么prompt：“分析以下文本的情感倾向，从{变量2}中选择一个。文本：{变量1} 情感倾向：”\n\n不过用户只说“变量1的内容和变量2的内容”，没有说明变量的作用，所以可能需要更通用的结构，明确变量的位置。比如：“这是一个文本理解任务，请根据提供的信息完成任务。信息1：{变量1} 信息2：{变量2} 任务要求：[具体任务，如：总结信息1和信息2的主要内容/判断信息1和信息2是否表达相同观点/根据信息1回答信息2中的问题]”\n\n可能用户需要的是一个模板，其中变量1和变量2是占位符，用户可以替换为具体内容，然后加上任务要求。所以更清晰的方式是先定义任务类型，再指出变量的位置。比如假设任务是“文本匹配”，判断两段文本是否相关，那么变量1和变量2是两段文本，prompt：“判断以下两段文本是否相关（相关/不相关）。文本A：{变量1} 文本B：{变量2} 是否相关：”\n\n总结下来，用户可能需要的是一个包含变量1和变量2的文本理解任务prompt示例，明确变量的位置和任务要求。因此，我应该提供一个具体的例子，说明变量如何融入，以及任务是什么，这样用户可以根据自己的实际任务调整。')
2c5a0c61-2e50-4e5c-90fb-b112f805aebc
```

注意：对于视觉理解任务，支持 prompt 文本内容混合编排，变量可以是 text 或 image_url 类型。在render该变量的时候，传入对应的文本内容和image_url 即可。
```Python
import agent_pilot as ap

input_dict = ap.render(
    task_id = "ta-20250101000000-aaaaa",
    version = "v2",
    variables = {
        "text_var": "文字内容",
        "image_var": "https://www.image.com/1.jpg",
    },
    api_key=None,
    api_url=None,
)
```

### 如何手动回流数据？
除了上述通过completion接口自动回流数据，SDK还提供手动回流数据的能力，也可以在自动回流之后，手动覆盖数据，比如添加理想回答(reference)，或者更新变量。
```Python
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os
import time
import uuid

# 创建一个 run_id 或者从上面 completion 接口的返回值获得某个回流事件 的 run_id
run_id = str(uuid.uuid4())
task_id = "ta-20250101000000-aaaaa"
version = "v1"

# 回流/覆盖某个run_id对应的一行数据
ap.track_event(
    run_type="llm",
    event_name="reference",  # 回流事件名称，任何名字都可以
    run_id=run_id,
    task_id=task_id,
    version=version,
    variables={
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
    input_messages=[{"role": "user", "content": "这是一个模型输入"}],
    output_message={"role": "user", "content": "这是一个模型输出"},
    reference="这是一个参考答案",
    api_key=None,
    api_url=None,
)
ap.flush()
```

### 如何人工反馈评分？
打 patch 后的 Ark client 与标准 Ark completion 接口几乎相同，除了推理返回值 completion 带有一个回流事件 (run)，作为一条数据的后续追踪 (track) 的唯一标识 (run.id)，用户只要拥有这个标识，即可追加后续操作，比如添加人工评分和AI评分等。
承接上面代码，调用接口 ap.track_feedback()，传入 feedback，其格式是一个 dict，通过字段 human_score, human_analysis, human_confidence即可传入人工评分。回流后，登录 Console 页面可以到上面那条数据，并且添加了评分和评分原因。++该功能与 Console 页面手动评分功能对齐。++
```Python
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os

# 创建 client
client = Ark(api_key=os.getenv('ARK_API_KEY'))
ap.probing(object=client, sample_rate=1.0)

# 渲染推理请求的输入
input_dict = ap.render(
    task_id="ta-20250101000000-aaaaa",
    version="v1",
    variables={
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
    api_key=None,
    api_url=None,
)

# 调用模型推理，和方舟SDK一样
completion = client.chat.completions.create(model='doubao-seed-1.6-250615', **input_dict)

# 可选步骤：flush队列中的待发送事件消息，立即发送。如果不flush，系统会自动小批量定时回流，提升性能
ap.flush()

# 打印 completion 结果
print(completion.choices[0].message)

# 打印 事件 run_id，对应于批量页某一行数据，也可用于后续追踪，修改，覆盖数据
print(completion.run.id)

# 得到上述回流事件 run 的唯一标识 run.id
run = completion.run
if run is not None:
    print(run.task_id)
    print(run.version)
    print(run.id)

# 追踪并添加人工反馈分数
ap.track_feedback(
    task_id=run.task_id,
    version=run.version,
    run_id=run.id,
    feedback={
        "human_score": 5,
        "human_analysis": "该条数据的人工评分原因",
        "human_confidence": 1.0,
    },
)

ap.flush()
```

如果运行成功，登陆 PromptPilot 该任务下的批量页，你将看到，回流数据已添加了评分和评分原因。
<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3e0872eb97ca4a53b6900a0c4beb11fc~tplv-goo7wpa0wc-image.image" width="2472px" /></div>

## 3.在线评估
### 如何提交在线评估请求？
调用 ap.eval.evaluate() 接口，用户需要输入一个 example 和一个 metric，均为 dict 类型，提供相关字段即可。
```Python
import agent_pilot as ap

# 待评估样本
example = {
    "prompt": "这是一个prompt模板，它有{{variable_1}}和{{variable_2}}",
    "variables": {
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
    "response": "模型回答的内容",
}

# 评分标准
metric = {
    "criteria": "文本表达流畅、逻辑清晰。回答内容充分，无明显遗漏得5分，输出数字5。否则输出数字1。",
}

# 调用评估
result = ap.eval.evaluate(
    example=example,
    metric=metric,
    api_key=None,
    api_url=None,
)
print(result)
```

如果成功，你将看到
```Bash
example_id=None reference=None score=1 analysis='任务问题未明确具体需求，模型回答未体现出对变量1和变量2内容的处理，回答内容不充分，存在明显遗漏，未满足评估规则中回答内容充分、无明显遗漏的要求，因此评分为1分。' confidence=0.9
```

特别地，我们提供如下可选参数

* example_id：用户可以通过指定该 id ，将待评估样本和评估结果一一对应起来，方便用户查找。
* reference：如果用户评分标准 criteria 需要将模型回答和参照回答进行比较，可以通过该字段传递该条样本的参照回答，比如 criteria 为 "模型回答与参照回答内容一致。"

```Python
import agent_pilot as ap

# 待评估样本
example = {
    "example_id": "ex_id_1",                  # 可选填
    "prompt": "这是一个prompt模板，它有{{variable_1}}和{{variable_2}}",
    "variables": {
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
    "response": "模型回答的内容",
    "reference": "参照回答的内容",               # 可选填
}

# 评分标准
metric = {
    "criteria": "模型回答与参照回答内容一致，得5分，输出数字5。否则输出数字1。",
}

# 调用评估
result = ap.eval.evaluate(
    example=example,
    metric=metric,
    api_key=None,
    api_url=None,
)
print(result)
```

如果成功，你将看到
```Bash
example_id='ex_id_1' reference='参照回答的内容' score=1 analysis='模型回答的内容与参照回答的内容不一致，根据评估规则，应得1分。' confidence=1.0
```

此外，为了方便用户，我们还支持以下两种方式构建样本

* 以 messages 字段输入，对齐 Ark client 的 messages 字段接口。
* 以 input 字段输入，直接对大模型提问。

```Python
import agent_pilot as ap

# 评分标准
metric = {
    "criteria": "文本表达流畅、逻辑清晰。回答内容充分，无明显遗漏得5分，输出数字5。否则输出数字1。",
}

# 以 messages 字段直接输入，对齐 Ark client 的 messages 字段接口
example = {
    "messages": [
        {"role": "system", "content": "你是一个客气礼貌的点餐机器人，帮助用户点餐。"},
        {"role": "user", "content": "你们家的水煮鱼辣不辣？"},
        {"role": "assistant", "content": "你好，可以选择微辣，中辣，重辣。"},
        {"role": "user", "content": "帮我来一份微辣。"},
    ],
    "response": "好的，一份水煮鱼微辣，为您点单，请稍后。",
}

# 调用评估
result = ap.eval.evaluate(
    example=example,
    metric=metric,
    api_key=None,
    api_url=None,
)
print(result)

# 以 input 字段输入，直接对大模型提问
example = {
    "input": "这是一个prompt模板，它有变量1的内容和变量2的内容",
    "response": "模型回答的内容",
}

# 调用评估
result = ap.eval.evaluate(
    example=example,
    metric=metric,
    api_key=None,
    api_url=None
)
print(result)
```

如果成功，可看到对上面两种 example 的评估结果
```Bash
example_id=None reference=None score=1 analysis='模型回答虽然文本表达流畅、逻辑清晰，但作为一个客气礼貌的点餐机器人，回答内容不够充分，未体现出客气礼貌的态度，存在明显遗漏，因此根据评估规则，给予1分。' confidence=1.0
example_id=None reference=None score=1 analysis='任务问题未明确具体需求，模型回答‘模型回答的内容’未体现对变量1和变量2内容的相关阐述，回答内容不充分，存在明显遗漏，因此给予1分。' confidence=0.9
```

### 如何回流AI评分？
跟回流人工评分一样，通过回流事件 (run) 的唯一标识 (run.id) 追踪 (track) 到该事件，并调用接口 ap.track_feedback()，传入 feedback 添加AI评分作为反馈。
与人工评分不同的是，AI评分需要传入 human_score, human_analysis, human_confidence 字段作为 feedback。回流后登陆 Console 页面可以看到 run.id 所对应的样本更新了评分和评分原因，++旁边并显示“AI 评分”字样++。++该功能与 Console 页面智能评分功能对齐。++
```Python
import agent_pilot as ap
from volcenginesdkarkruntime import Ark
import os
import time

# 创建 client
client = Ark(api_key=os.getenv('ARK_API_KEY'))
ap.probing(object=client, sample_rate=1.0)

# 渲染推理请求的输入
input_dict = ap.render(
    task_id="ta-20250101000000-aaaaa",
    version="v1",
    variables={
        "variable_1": "变量1的内容",
        "variable_2": "变量2的内容",
    },
)

# 调用模型推理，和方舟SDK一样
completion = client.chat.completions.create(model='doubao-1.5-pro-32k-250115', **input_dict)
print(completion.choices[0].message)

run = completion.run

if run is not None:
    result = ap.eval.evaluate(
        example={
            "example_id": "example_id_1",
            "input": input_dict["messages"][0]["content"][0]["text"],
            "response": completion.choices[0].message.content,
        },
        metric={
            "criteria": "文本表达流畅、逻辑清晰。回答内容充分，无明显遗漏得5分，输出数字5。否则输出数字1。",
        },
        api_key=None,
        api_url=None,
    )
    print(result)

    ap.track_feedback(
        task_id=run.task_id,
        version=run.version,
        run_id=run.id,
        feedback={
            "metric_score": result.score,
            "metric_analysis": result.analysis,
            "metric_confidence": result.confidence,
        },
    )
    
    ap.flush()
```

如果运行成功，登陆 PromptPilot 该任务下的批量页，你将看到，回流数据已添加了评分和评分原因，并且评分标记为“AI评分”。
<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9007bdef39c848b2b71abc5eb18f7466~tplv-goo7wpa0wc-image.image" width="2442px" /></div>

### 如何生成评分标准？
调用接口 ap.eval.generate_criteria()，通过传入一个样本列表 examples 生成评分标准，其中 examples 每一个元素是一个 example dict。++该功能++++对齐++++智能评分的生成评分标准功能。++
特别的，样本列表中每一个样本可以包含评分 score 和评分原因 analysis。生成评分标准时，系统不区分是人工评分还是AI评分。
```Python
import agent_pilot as ap

# 样本列表
examples = [
    {
        "prompt": "这是一个prompt模板，它有{{variable_1}}和{{variable_2}}",
        "variables": {
            "variable_1": "样本1中变量1的内容",
            "variable_2": "样本1中变量2的内容",
        },
        "response": "模型回答1",
        "reference": "参照回答1",            # 可选填
        "score": 5,                         # 可选填
        "analysis": "评分原因1",             # 可选填
    },
    {
        "prompt": "这是一个prompt模板，它有{{变量1}}和{{变量2}}",
        "variables": {
            "variable_1": "样本2中变量1的内容",
            "variable_2": "样本2中变量2的内容",
        },
        "response": "模型回答2",
        "reference": "参照回答2",            # 可选填
        "score": 1,                         # 可选填
        "analysis": "评分原因2",             # 可选填
    },
]

criteria = ap.eval.generate_criteria(
    examples=examples,
    api_key=None,
    api_url=None,
)
print(criteria)
```

如果成功，你将得到如下评分标准。
```Bash
模型回答内容与用户输入要求完全不符，存在严重错误，得1分
模型回答内容部分关键要求未满足，或存在较多小问题，得2分
模型回答内容基本满足用户输入要求，但有一些小瑕疵，格式基本符合要求，得3分
模型回答内容准确完整，格式规范，且在某些方面超出基本要求，得4分
模型回答内容和格式完美符合用户输入要求，且有额外亮点，得5分
```

## 4.Prompt 优化和报告获取
### 如何提交 prompt 优化任务？
调用接口 ap.optimize.create()，传入 task_id 并指定 base_version，系统会创建一个优化 job 并绑定一个 OptimizeJob 对象 opt_job。该对象包含 job_id 信息，作为该优化 job 的唯一标识。
```Python
import agent_pilot as ap
import time

# 调用接口 ap.optimize.create() 创建优化 job
opt_job = ap.optimize.create(task_id="ta-20250101000000-aaaaa", base_version="v1", api_key=None, api_url=None)
print(opt_job)
```

如果成功，你将看到如下优化任务信息
```SQL
task_id='ta-20250912125011-WDAtF' base_version='v1' job_id='20250912145806_gqynL_OptimizePrompt' api_key='XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX' api_url='https://prompt-pilot.cn-beijing.volces.com' local_debug=False optimized_version=None
```

### 如何查看优化状态和进度？
通过 opt_job 对象，调用接口 opt_job.get_job_info()，可以查看当前优化 job 信息，包括 state 和 progress。

* 优化结束后的 state 只能为 {ap.OptimizeState.SUCCESS, ap.OptimizeState.FAILED} 中的一个，因此如果不是这两个 state，可以等待一定时间后继续查询。
* 优化运行中的 state 只能为 {ap.OptimizeState.CREATED, ap.OptimizeState.RUNNING} 中的一个。

承接上面代码，每5秒查询一次优化优化 job 信息：
```Python
import agent_pilot as ap
import time

# 调用接口 ap.optimize.create() 创建优化 job
opt_job = ap.optimize.create(task_id="ta-20250101000000-aaaaa", base_version="v1", api_key=None, api_url=None)
print(opt_job)

# 通过 opt_job 对象查询 JobInfo
job_info = opt_job.get_job_info()
while job_info.state not in {ap.OptimizeState.SUCCESS, ap.OptimizeState.FAILED}:
    # 查看当前 state
    print(job_info.state)
    # 查看当前 progress
    if job_info.progress is not None:
        print(job_info.progress.percent)         # 当前优化 job 完成百分比
        print(job_info.progress.optimal_prompt)  # 当前最优的 prompt
        print(job_info.optimized_version)

    # 5秒后重新获取 JobInfo
    time.sleep(5)
    job_info = opt_job.get_job_info()

# 优化 job 结束后，状态为 SUCCESS 或 FAILED
print(job_info.state in {ap.OptimizeState.SUCCESS, ap.OptimizeState.FAILED})
```

如果成功，你将看到每5秒刷新的 log
```YAML
INFO:agent_pilot.optimize.optimize_job:Starting optimization for task_id: ta-20250912125011-WDAtF, version: v1
INFO:agent_pilot.http_client:[post] request_id: 3c6716e4-0dac-4775-b686-fcecc5f08674, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceStartOptimize
INFO:agent_pilot.optimize.optimize_job:Optimization started successfully. OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
task_id='ta-20250912125011-WDAtF' base_version='v1' job_id='20250912150037_P3Cnj_OptimizePrompt' api_key='XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX' api_url='https://prompt-pilot.cn-beijing.volces.com' local_debug=False optimized_version=None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 39f08182-5b67-4462-b7dd-ffac8feddd2f, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.CREATED
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 1e06ab8e-9c24-4714-bfc0-841ee964833b, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.CREATED
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: d36375f4-00c8-4969-b137-62e3ae74e349, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.CREATED
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: afe476e3-44db-445d-a069-bbb2d0451047, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.CREATED
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 7ed5babe-6f67-4ab2-a4de-1fe354f3847f, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.RUNNING
0.0
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 18525cba-c1de-4aba-b533-7c4c067d36b8, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.RUNNING
0.1
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: fa042c06-77ad-4c6d-a83b-c8f744107926, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.RUNNING
0.1
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 1409e82c-695e-41d6-b9dd-03c9c9bdc8a4, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.RUNNING
0.1
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: c0799d2c-630e-46cd-8c8b-8c02cb511083, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
OptimizeState.RUNNING
0.1
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
None
INFO:agent_pilot.optimize.optimize_job:Getting optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
INFO:agent_pilot.http_client:[post] request_id: 05054e6b-6ec0-405a-ae25-693cb70bf646, api_url: https://prompt-pilot.cn-beijing.volces.com, action: OptimizeServiceGetOptimizeProgress
INFO:agent_pilot.optimize.optimize_job:Successfully fetched optimization progress for OptimizeJobId: 20250912150037_P3Cnj_OptimizePrompt
True
```

注意：任务优化中，也可以通过 PromptPilot 的 Console 界面查看优化进度如下
<div style="text-align: center"></div>

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/05733b103e7f443dad57ed0cc385d29b~tplv-goo7wpa0wc-image.image =1280x)

### 如何获取优化报告？
如果优化成功，调用接口 opt_job.get_report()，可以获得优化报告。
```Python
import agent_pilot as ap
import time

# 调用接口 ap.optimize.create() 创建优化 job
opt_job = ap.optimize.create(task_id="ta-20250912125011-WDAtF", base_version="v1", api_key=None, api_url=None)
print(opt_job)

# 通过 opt_job 对象查询 JobInfo
job_info = opt_job.get_job_info()
while job_info.state not in {ap.OptimizeState.SUCCESS, ap.OptimizeState.FAILED}:
    # 查看当前 state
    print(job_info.state)
    # 查看当前 progress
    if job_info.progress is not None:
        print(job_info.progress.percent)         # 当前优化 job 完成百分比
        print(job_info.progress.optimal_prompt)  # 当前最优的 prompt
        print(job_info.optimized_version)

    # 5秒后重新获取 JobInfo
    time.sleep(5)
    job_info = opt_job.get_job_info()

# 优化 job 结束后，状态为 SUCCESS 或 FAILED
print(job_info.state in {ap.OptimizeState.SUCCESS, ap.OptimizeState.FAILED})

if job_info.state == ap.OptimizeState.SUCCESS:
    report = opt_job.get_report()
    opt, base = report.opt, report.base

    # 比较优化前后prompt
    print(opt.prompt, base.prompt)

    # 比较优化前后metric
    print(opt.metric, base.metric)

    # 比较优化前后平均分
    print(opt.avg_score, base.avg_score)

    # 列出优化前后data records
    print(opt.records, base.records)
```

如果成功，你将看到最终报告内容如下，你也可以登录 PromptPilot Console 页面查看报告。
```Python
这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}} 这是一个文本理解任务的prompt，它有{{variable_1}}和{{variable_2}}
这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确。请输出1-5之间的一个数字。 这是一个文本理解任务的评分标准，5分制，1分表示完全错误，5分表示完全正确。请输出1-5之间的一个数字。
5.0 5.0
[OptimizeDataRecord(record_id='dr-20250912150518-Z8ua8', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='要构建一个包含“变量1”和“变量2”的文本理解任务prompt，关键是明确任务目标、变量角色及输出要求。以下是具体步骤和示例，帮助你将变量融入prompt中：\n\n\n### **1. 明确任务类型与变量角色**\n首先确定文本理解任务的具体类型（如问答、摘要、情感分析、关系抽取等），并定义变量1和变量2的功能（例如：输入文本、问题、参考文本、关键词等）。\n\n\n### **2. 构造通用prompt框架**\n核心结构：**任务说明 + 变量内容引用 + 输出格式要求**。  \n确保清晰告知模型“要做什么”“基于哪些信息”“输出什么形式”。\n\n\n### **3. 示例：不同任务场景下的prompt设计**\n\n#### **场景1：问答任务（变量1=文章，变量2=问题）**  \n**Prompt：**  \n“请仔细阅读以下文章（变量1）：  \n[在此插入变量1的具体内容，例如：一篇关于人工智能发展的科技文章]  \n\n然后回答问题（变量2）：  \n[在此插入变量2的具体内容，例如：“人工智能的核心技术包括哪些？”]  \n\n要求：直接给出答案，无需额外解释。”  \n\n\n#### **场景2：文本摘要（变量1=长文本，变量2=摘要长度要求）**  \n**Prompt：**  \n“为以下长文本（变量1）生成摘要：  \n[在此插入变量1的具体内容，例如：一篇500字的研究报告]  \n\n摘要要求（变量2）：  \n[在此插入变量2的具体内容，例如：“不超过100字，涵盖研究目的、方法和核心结论”]  \n\n请输出符合要求的摘要。”  \n\n\n#### **场景3：文本比较（变量1=文本A，变量2=文本B，任务=分析异同）**  \n**Prompt：**  \n“比较以下两段文本，分析它们在主题、观点上的相同点和不同点。  \n\n文本A（变量1）：  \n[在此插入变量1的具体内容，例如：“环保政策应侧重限制工业排放”]  \n\n文本B（变量2）：  \n[在此插入变量2的具体内容，例如：“环保政策应优先推动可再生能源发展”]  \n\n输出格式：  \n相同点：[用1-2句话总结]  \n不同点：[用1-2句话总结]”  \n\n\n### **关键注意事项**  \n- **变量定位清晰**：明确标注“变量1”“变量2”的角色（如“输入文本”“问题”），避免模型混淆。  \n- **任务指令简洁**：用短句说明“做什么”，避免冗余（例如：“回答问题”而非“请你基于提供的信息对后续问题进行解答”）。  \n- **输出格式明确**：指定输出形式（如“直接给答案”“分点列出”），降低模型理解成本。  \n\n\n如果需要针对具体任务（如NER、情感分析）或变量内容（如对话历史、多轮问题）设计prompt，可以提供更多细节，我会进一步优化！', ref_answer=None, score=5, analysis='该条数据的人工评分原因', confidence=1.0), OptimizeDataRecord(record_id='dr-20250912150518-CQxt6', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='请你提供变量1和变量2的具体内容，以及该文本理解任务prompt的完整表述，这样我就能更好地为你完成相关任务了。', ref_answer=None, score=5, analysis='模型回答文本表达流畅、逻辑清晰。由于任务问题仅告知存在文本理解任务prompt及有变量1和变量2的内容，未给出具体内容，模型回答合理地向用户索要完成任务所需的关键信息，不存在内容遗漏的问题，符合评估标准。', confidence=1.0), OptimizeDataRecord(record_id='dr-20250912150518-Z8ua8', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='要构建一个包含“变量1”和“变量2”的文本理解任务prompt，关键是明确任务目标、变量角色及输出要求。以下是具体步骤和示例，帮助你将变量融入prompt中：\n\n\n### **1. 明确任务类型与变量角色**\n首先确定文本理解任务的具体类型（如问答、摘要、情感分析、关系抽取等），并定义变量1和变量2的功能（例如：输入文本、问题、参考文本、关键词等）。\n\n\n### **2. 构造通用prompt框架**\n核心结构：**任务说明 + 变量内容引用 + 输出格式要求**。  \n确保清晰告知模型“要做什么”“基于哪些信息”“输出什么形式”。\n\n\n### **3. 示例：不同任务场景下的prompt设计**\n\n#### **场景1：问答任务（变量1=文章，变量2=问题）**  \n**Prompt：**  \n“请仔细阅读以下文章（变量1）：  \n[在此插入变量1的具体内容，例如：一篇关于人工智能发展的科技文章]  \n\n然后回答问题（变量2）：  \n[在此插入变量2的具体内容，例如：“人工智能的核心技术包括哪些？”]  \n\n要求：直接给出答案，无需额外解释。”  \n\n\n#### **场景2：文本摘要（变量1=长文本，变量2=摘要长度要求）**  \n**Prompt：**  \n“为以下长文本（变量1）生成摘要：  \n[在此插入变量1的具体内容，例如：一篇500字的研究报告]  \n\n摘要要求（变量2）：  \n[在此插入变量2的具体内容，例如：“不超过100字，涵盖研究目的、方法和核心结论”]  \n\n请输出符合要求的摘要。”  \n\n\n#### **场景3：文本比较（变量1=文本A，变量2=文本B，任务=分析异同）**  \n**Prompt：**  \n“比较以下两段文本，分析它们在主题、观点上的相同点和不同点。  \n\n文本A（变量1）：  \n[在此插入变量1的具体内容，例如：“环保政策应侧重限制工业排放”]  \n\n文本B（变量2）：  \n[在此插入变量2的具体内容，例如：“环保政策应优先推动可再生能源发展”]  \n\n输出格式：  \n相同点：[用1-2句话总结]  \n不同点：[用1-2句话总结]”  \n\n\n### **关键注意事项**  \n- **变量定位清晰**：明确标注“变量1”“变量2”的角色（如“输入文本”“问题”），避免模型混淆。  \n- **任务指令简洁**：用短句说明“做什么”，避免冗余（例如：“回答问题”而非“请你基于提供的信息对后续问题进行解答”）。  \n- **输出格式明确**：指定输出形式（如“直接给答案”“分点列出”），降低模型理解成本。  \n\n\n如果需要针对具体任务（如NER、情感分析）或变量内容（如对话历史、多轮问题）设计prompt，可以提供更多细节，我会进一步优化！', ref_answer=None, score=5, analysis='该条数据的人工评分原因', confidence=1.0), OptimizeDataRecord(record_id='dr-20250912150518-CQxt6', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='请你提供变量1和变量2的具体内容，以及该文本理解任务prompt的完整表述，这样我就能更好地为你完成相关任务了。', ref_answer=None, score=5, analysis='模型回答文本表达流畅、逻辑清晰。由于任务问题仅告知存在文本理解任务prompt及有变量1和变量2的内容，未给出具体内容，模型回答合理地向用户索要完成任务所需的关键信息，不存在内容遗漏的问题，符合评估标准。', confidence=1.0)] [OptimizeDataRecord(record_id='dr-20250912142603-sGip3', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='要构建一个包含“变量1”和“变量2”的文本理解任务prompt，关键是明确任务目标、变量角色及输出要求。以下是具体步骤和示例，帮助你将变量融入prompt中：\n\n\n### **1. 明确任务类型与变量角色**\n首先确定文本理解任务的具体类型（如问答、摘要、情感分析、关系抽取等），并定义变量1和变量2的功能（例如：输入文本、问题、参考文本、关键词等）。\n\n\n### **2. 构造通用prompt框架**\n核心结构：**任务说明 + 变量内容引用 + 输出格式要求**。  \n确保清晰告知模型“要做什么”“基于哪些信息”“输出什么形式”。\n\n\n### **3. 示例：不同任务场景下的prompt设计**\n\n#### **场景1：问答任务（变量1=文章，变量2=问题）**  \n**Prompt：**  \n“请仔细阅读以下文章（变量1）：  \n[在此插入变量1的具体内容，例如：一篇关于人工智能发展的科技文章]  \n\n然后回答问题（变量2）：  \n[在此插入变量2的具体内容，例如：“人工智能的核心技术包括哪些？”]  \n\n要求：直接给出答案，无需额外解释。”  \n\n\n#### **场景2：文本摘要（变量1=长文本，变量2=摘要长度要求）**  \n**Prompt：**  \n“为以下长文本（变量1）生成摘要：  \n[在此插入变量1的具体内容，例如：一篇500字的研究报告]  \n\n摘要要求（变量2）：  \n[在此插入变量2的具体内容，例如：“不超过100字，涵盖研究目的、方法和核心结论”]  \n\n请输出符合要求的摘要。”  \n\n\n#### **场景3：文本比较（变量1=文本A，变量2=文本B，任务=分析异同）**  \n**Prompt：**  \n“比较以下两段文本，分析它们在主题、观点上的相同点和不同点。  \n\n文本A（变量1）：  \n[在此插入变量1的具体内容，例如：“环保政策应侧重限制工业排放”]  \n\n文本B（变量2）：  \n[在此插入变量2的具体内容，例如：“环保政策应优先推动可再生能源发展”]  \n\n输出格式：  \n相同点：[用1-2句话总结]  \n不同点：[用1-2句话总结]”  \n\n\n### **关键注意事项**  \n- **变量定位清晰**：明确标注“变量1”“变量2”的角色（如“输入文本”“问题”），避免模型混淆。  \n- **任务指令简洁**：用短句说明“做什么”，避免冗余（例如：“回答问题”而非“请你基于提供的信息对后续问题进行解答”）。  \n- **输出格式明确**：指定输出形式（如“直接给答案”“分点列出”），降低模型理解成本。  \n\n\n如果需要针对具体任务（如NER、情感分析）或变量内容（如对话历史、多轮问题）设计prompt，可以提供更多细节，我会进一步优化！', ref_answer=None, score=5, analysis='该条数据的人工评分原因', confidence=None), OptimizeDataRecord(record_id='dr-20250912142603-sGip3', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='要构建一个包含“变量1”和“变量2”的文本理解任务prompt，关键是明确任务目标、变量角色及输出要求。以下是具体步骤和示例，帮助你将变量融入prompt中：\n\n\n### **1. 明确任务类型与变量角色**\n首先确定文本理解任务的具体类型（如问答、摘要、情感分析、关系抽取等），并定义变量1和变量2的功能（例如：输入文本、问题、参考文本、关键词等）。\n\n\n### **2. 构造通用prompt框架**\n核心结构：**任务说明 + 变量内容引用 + 输出格式要求**。  \n确保清晰告知模型“要做什么”“基于哪些信息”“输出什么形式”。\n\n\n### **3. 示例：不同任务场景下的prompt设计**\n\n#### **场景1：问答任务（变量1=文章，变量2=问题）**  \n**Prompt：**  \n“请仔细阅读以下文章（变量1）：  \n[在此插入变量1的具体内容，例如：一篇关于人工智能发展的科技文章]  \n\n然后回答问题（变量2）：  \n[在此插入变量2的具体内容，例如：“人工智能的核心技术包括哪些？”]  \n\n要求：直接给出答案，无需额外解释。”  \n\n\n#### **场景2：文本摘要（变量1=长文本，变量2=摘要长度要求）**  \n**Prompt：**  \n“为以下长文本（变量1）生成摘要：  \n[在此插入变量1的具体内容，例如：一篇500字的研究报告]  \n\n摘要要求（变量2）：  \n[在此插入变量2的具体内容，例如：“不超过100字，涵盖研究目的、方法和核心结论”]  \n\n请输出符合要求的摘要。”  \n\n\n#### **场景3：文本比较（变量1=文本A，变量2=文本B，任务=分析异同）**  \n**Prompt：**  \n“比较以下两段文本，分析它们在主题、观点上的相同点和不同点。  \n\n文本A（变量1）：  \n[在此插入变量1的具体内容，例如：“环保政策应侧重限制工业排放”]  \n\n文本B（变量2）：  \n[在此插入变量2的具体内容，例如：“环保政策应优先推动可再生能源发展”]  \n\n输出格式：  \n相同点：[用1-2句话总结]  \n不同点：[用1-2句话总结]”  \n\n\n### **关键注意事项**  \n- **变量定位清晰**：明确标注“变量1”“变量2”的角色（如“输入文本”“问题”），避免模型混淆。  \n- **任务指令简洁**：用短句说明“做什么”，避免冗余（例如：“回答问题”而非“请你基于提供的信息对后续问题进行解答”）。  \n- **输出格式明确**：指定输出形式（如“直接给答案”“分点列出”），降低模型理解成本。  \n\n\n如果需要针对具体任务（如NER、情感分析）或变量内容（如对话历史、多轮问题）设计prompt，可以提供更多细节，我会进一步优化！', ref_answer=None, score=5, analysis='该条数据的人工评分原因', confidence=None), OptimizeDataRecord(record_id='dr-20250912144851-RDTiL', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='请你提供变量1和变量2的具体内容，以及该文本理解任务prompt的完整表述，这样我就能更好地为你完成相关任务了。', ref_answer=None, score=5, analysis='模型回答文本表达流畅、逻辑清晰。由于任务问题仅告知存在文本理解任务prompt及有变量1和变量2的内容，未给出具体内容，模型回答合理地向用户索要完成任务所需的关键信息，不存在内容遗漏的问题，符合评估标准。', confidence=1.0), OptimizeDataRecord(record_id='dr-20250912144851-RDTiL', variables=[Variable(name='variable_1', value='变量1的内容', type='text'), Variable(name='variable_2', value='变量2的内容', type='text')], model_answer='请你提供变量1和变量2的具体内容，以及该文本理解任务prompt的完整表述，这样我就能更好地为你完成相关任务了。', ref_answer=None, score=5, analysis='模型回答文本表达流畅、逻辑清晰。由于任务问题仅告知存在文本理解任务prompt及有变量1和变量2的内容，未给出具体内容，模型回答合理地向用户索要完成任务所需的关键信息，不存在内容遗漏的问题，符合评估标准。', confidence=1.0)]
```

## 5.Prompt 生成
调用接口 ap.generate_prompt_stream()，传入 task_description 和模型配置，流式生成 prompt。++该功能与 Console 的 Prompt 生成页功能对齐。++
下例中生成一个多轮对话任务的 prompt。如果需要生成文本理解或者视觉理解的 prompt，task_type 分别改为 "DEFAULT" 或 "MULTIMODAL"。
```Python
import agent_pilot as ap

generated_prompt = ""
usage = None
print("Streaming response:")
for chunk in ap.generate_prompt_stream(
    task_description="我需要一个最强带货，输入是用户提问，输出是产品介绍",
    task_type="DIALOG",
    model_name="doubao-seed-1.6-250615",
    temperature=1.0,
    top_p=0.7,
    api_key=None,
    api_url=None,
):
    # 处理流式输出
    generated_prompt += chunk.data.content
    print(chunk.data.content, end="", flush=True)
    if chunk.event == "usage":
        usage = chunk.data.usage

print(f"Generated prompt: {str(generated_prompt)}")
print(f"Usage: {usage}")
```

如果成功，你将看到
```Markdown
# 角色定位：最强带货专家  
你是一位极具感染力和说服力的带货专家，擅长通过精准洞察用户需求，用生动、诱人的语言激发购买欲望，让用户在听完你的介绍后产生强烈的拥有欲。你的核心任务是：无论用户提出何种与产品相关的问题（如功能、价格、优势、适用场景等），都能迅速转化为针对性的产品介绍，实现“提问即种草”。

# 带货核心策略  
## 1. 需求锚定法  
- 首先快速捕捉用户提问中的潜在需求（如用户问“夏天用什么护肤品不油腻？”，需求是“清爽、控油”），并在回答开头直接点明：“你是不是在找一款涂上去像水一样，夏天出油再多也能保持皮肤哑光的护肤品？”  
- 用“你是不是”“我懂你”“很多人都遇到过”等话术建立共鸣，让用户觉得“你完全懂我”。  

## 2. 产品亮点爆破  
- 针对用户需求，提炼产品3个核心亮点，用“3秒见效”“用完惊为天人”“再也离不开”等夸张但真实的效果描述强化记忆点。  
- 每个亮点搭配具体场景：“比如早上赶时间，涂它2分钟就能出门，一整天脸都不会泛油光，同事还以为你偷偷补了妆！”  

## 3. 紧迫感与稀缺性营造  
- 主动透露“隐藏福利”：“现在下单不仅立减50元，前100名还送同款小样，库存只剩最后30组了哦！”  
- 用“错过今天再等一年”“这个颜色线下专柜早就卖断货了，线上只剩最后50件”等话术刺激决策。  

## 4. 对比碾压术  
- 若用户提及竞品或犹豫，直接用“碾压式对比”突出优势：“你说的那款我知道，它的XX功能需要单独付费开通，而我们这款直接免费送，而且效果比它强3倍！”  
- 强调“性价比之王”：“同样的价格，别人只能买基础款，我们这款买一送三，等于白赚两个单品！”  

# 对话流程与禁忌  
## 必做流程  
1. **接住问题→锚定需求→爆破亮点→营造稀缺→催促下单**，全程不超过3句话，节奏紧凑，不给用户犹豫时间。  
2. 每轮回答结尾必须包含明确的行动指令：“点击下方链接直接抢”“现在拍还能发顺丰明天到”“赶紧去看，手慢无！”  

## 绝对禁忌  
- 禁止使用“可能”“也许”“大概”等模糊词汇，必须用“100%有效”“绝对好用”“谁用谁知道”等肯定语气。  
- 禁止冗长介绍，用户问“多少钱”，直接答：“原价299，今天活动价199，再送价值99元赠品，等于100块买3样，现在下单吗？”  

# 示例参考  
- 用户问：“这个吹风机吹头发快吗？我早上赶时间。”  
- 回答：“你是不是每天早上吹头发要10分钟，还总担心伤头发？这款吹风机3分钟就能吹干及腰长发，而且带负离子护发，吹完头发像做了护理一样顺滑！现在下单送同款风嘴，库存只剩28个，手慢无，赶紧点链接抢！”  

记住：你的每一句话都要像钩子，勾住用户的耳朵，更勾住他们的钱包！Generated prompt: # 角色定位：最强带货专家  
你是一位极具感染力和说服力的带货专家，擅长通过精准洞察用户需求，用生动、诱人的语言激发购买欲望，让用户在听完你的介绍后产生强烈的拥有欲。你的核心任务是：无论用户提出何种与产品相关的问题（如功能、价格、优势、适用场景等），都能迅速转化为针对性的产品介绍，实现“提问即种草”。

# 带货核心策略  
## 1. 需求锚定法  
- 首先快速捕捉用户提问中的潜在需求（如用户问“夏天用什么护肤品不油腻？”，需求是“清爽、控油”），并在回答开头直接点明：“你是不是在找一款涂上去像水一样，夏天出油再多也能保持皮肤哑光的护肤品？”  
- 用“你是不是”“我懂你”“很多人都遇到过”等话术建立共鸣，让用户觉得“你完全懂我”。  

## 2. 产品亮点爆破  
- 针对用户需求，提炼产品3个核心亮点，用“3秒见效”“用完惊为天人”“再也离不开”等夸张但真实的效果描述强化记忆点。  
- 每个亮点搭配具体场景：“比如早上赶时间，涂它2分钟就能出门，一整天脸都不会泛油光，同事还以为你偷偷补了妆！”  

## 3. 紧迫感与稀缺性营造  
- 主动透露“隐藏福利”：“现在下单不仅立减50元，前100名还送同款小样，库存只剩最后30组了哦！”  
- 用“错过今天再等一年”“这个颜色线下专柜早就卖断货了，线上只剩最后50件”等话术刺激决策。  

## 4. 对比碾压术  
- 若用户提及竞品或犹豫，直接用“碾压式对比”突出优势：“你说的那款我知道，它的XX功能需要单独付费开通，而我们这款直接免费送，而且效果比它强3倍！”  
- 强调“性价比之王”：“同样的价格，别人只能买基础款，我们这款买一送三，等于白赚两个单品！”  

# 对话流程与禁忌  
## 必做流程  
1. **接住问题→锚定需求→爆破亮点→营造稀缺→催促下单**，全程不超过3句话，节奏紧凑，不给用户犹豫时间。  
2. 每轮回答结尾必须包含明确的行动指令：“点击下方链接直接抢”“现在拍还能发顺丰明天到”“赶紧去看，手慢无！”  

## 绝对禁忌  
- 禁止使用“可能”“也许”“大概”等模糊词汇，必须用“100%有效”“绝对好用”“谁用谁知道”等肯定语气。  
- 禁止冗长介绍，用户问“多少钱”，直接答：“原价299，今天活动价199，再送价值99元赠品，等于100块买3样，现在下单吗？”  

# 示例参考  
- 用户问：“这个吹风机吹头发快吗？我早上赶时间。”  
- 回答：“你是不是每天早上吹头发要10分钟，还总担心伤头发？这款吹风机3分钟就能吹干及腰长发，而且带负离子护发，吹完头发像做了护理一样顺滑！现在下单送同款风嘴，库存只剩28个，手慢无，赶紧点链接抢！”  

记住：你的每一句话都要像钩子，勾住用户的耳朵，更勾住他们的钱包！
Usage: {'total_tokens': 3706}
```