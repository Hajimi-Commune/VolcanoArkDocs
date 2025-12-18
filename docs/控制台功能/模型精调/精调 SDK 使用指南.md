# 精调 SDK 使用指南
# 概述
火山方舟精调 SDK 是火山方舟为开发者提供的 **通过编程方式创建和管理大模型精调任务的工具包** ，旨在为需要定制化、自动化精调流程的用户提供灵活、高效的操作入口。区别于控制台的图形化操作，开发者可通过 SDK 将精调任务创建和管理流程集成至本地或第三方系统，实现精调任务的代码化管理，尤其适用于需要结合自定义函数（如奖励函数、Rollout函数）的强化学习场景。
# 前提条件

* 您已注册火山引擎账号并完成实名认证，具体步骤参见[账号注册](https://www.volcengine.com/docs/6261/64925)及[实名认证](https://www.volcengine.com/docs/6261/64935)。
* 您已在[方舟控制台](https://console.volcengine.com/ark/region:ark+cn-beijing)开通目标模型服务、精调算力计费项及依赖的云产品（TOS-数据存储、KMS-数据及模型加密、TLS-日志及轨迹存储、veFaaS-运行plugin函数）。

<strong>开通模型服务、精调算力计费项</strong>

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/293c40d0f0034556b14b0c58ac0e52c9~tplv-goo7wpa0wc-image.image)

<strong>开通云产品</strong>

* 您可以在火山引擎控制台开通对应的云产品。
* 如果您是首次使用精调功能，建议参考[创建并查看模型精调任务](/docs/82379/1099460) 在控制台创建一个RL精调任务，并在创建页中完成相关云产品的开通。

> 云产品统一开通入口在排期开发中；后续可以一键开通。

* 您已准备好精调数据集，具体格式请参考[模型精调数据集格式说明](/docs/82379/1099461)。

# SDK 安装
  您可以通过以下 pip 命令安装精调 SDK。
> 运行环境：python>=3.10

```Python
#火山版本
pip install https://ark-public-example-cn-beijing.tos-cn-beijing.volces.com/ark-sdk/ark_sdk-0.2.8.tar.gz
```

# 使用指南
## 配置授权信息
您需要授权将SDK终端关联到指定的账号和项目，具体操作如下：

1. 在终端工具中，使用`ark login`命令开启授权过程。
2. 请按提示依次输入以下信息，以回车结束。
   * AK/SK：密钥包括 Access Key ID（简称为 AK） 和 Access Key Secret（简称为 SK），详情请参考 [Access Key（密钥）管理](https://www.volcengine.com/docs/6291/65568)。
   * Region：输入您需要访问的可用区，默认为`cn-beijing`。
   * Project：选择您的项目，默认为`default`。

## 创建精调任务
### 初始化项目
您可通过`ark init workspace <文件夹名> --template <模版名>`命令，使用指定的template模板初始化一个具备基础结构和配置、可立即使用的精调项目，例如：
```Python
 ark init workspace ark_rl_project --template rl_demo
#工作区内结构如下
#<ark_rl_project>
#├── data
#│   └── mcj_rollout_test_dataset.jsonl
#├── plugins
#│   ├── random_reward.py
#│   └── raw_rollout.py
#│   └── weather_rollout.py
#│   └── async_weather_rollout.py
#│   └── test_utils.py
#├── job.py
#├── job.yaml
#├── README.md
#├── arkworkspace.toml
#└── requirements.txt
#└── test_faas.py
```

成功执行后将显示：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/57bdbe63a62c4798951fa5c87cbfc0a3~tplv-goo7wpa0wc-image.image)

目前支持的模板如下：

- **template 模版名** | **简介**
- rl_demo
- 该模版具体为通过强化学习，模型能够更清楚地理解何时以及如何使用自定义函数调用天气工具，从而实现更精准、更流畅的 天气问答 功能。可以根据需要扩展为通过强化学习微调大型语言模型(LLM)，使其能够更智能地 通过对话(Chat)API结合自定义工具实现特定功能。
- rl_search_mcp_demo | 该模版具体为利用 Arkitect框架 与 MCP，或直接调用外部工具 API ，通过 强化学习微调大型语言模型（LLM） ，使其在深度搜索（Deep Search）场景下表现卓越。

### 配置精调参数
目前支持的模型及训练方式如下，后续更新将同步至本文档：

- foundation_model | customization_type
- name
- > 模型名 | model_version
- > 模型版本 | FinetuneSft
- > 全量-SFT | FinetuneLoRA
- > LoRA-SFT | GRPO
- > 全量-GRPO | GRPOLoRA
- > LoRA-GRPO | DPO
- > 全量-DPO | DPOLoRA
- > LoRA-DPO | PPO
- > 全量-PPO
- doubao-seed-1-6 | 250615【推荐】 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅
- doubao-seed-1-6-flash | 250615 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅
- doubao-seed-1-6-flash | 250828【推荐】 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅

初始化项目后，通过`cd ark_rl_project`命令进入项目。
您可以通过 Python 对象 或 yaml文件 来配置精调项目相关参数，包括：

* **必选参数**：
   * customization_type：指定训练方式，例如 FinetuneSft、FinetuneLoRA 或 GRPO。
   * model_reference：配置基础模型及其版本信息。您可以通过以下两种方式之一指定：
      * foundation_model：基于模型广场中的模型进行训练。
         * name：模型名称。
         * model_version：模型版本。
      * custom_model_id：指定模型仓库中某个模型的 ID。此参数与 foundation_model 互斥，请二选一。
   * data：配置训练所用的数据。
      * training_set：配置训练集。您可以通过以下三种方式之一提供：
         1. local_files：提供一组本地文件。单个文件大小不超过 2GB，最多可上传 20 个文件。
         2. tos_bucket 和 tos_paths：指定 TOS 存储桶名称及其中的对象路径列表。
         3. datasets：使用已创建的数据集。
            * dataset_id：数据集 ID。
            * dataset_version_id：数据集版本 ID。
            * multiplier：数据混入倍率。
            * sample_count：数据混入条数，与 multiplier 参数互斥。
         * preset_dataset (可选)：配置混入的预置数据集。
            * dataset_version_id：预置数据集 ID。
            * inject_multiplier：混入倍率，与 inject_sample_count 互斥。
            * inject_sample_count：混入样本条数。
      * validation_set (可选)：配置验证集。其格式与 training_set 相同，支持本地文件、TOS 或数据集三种方式。
      * validation_percentage (可选)：从训练集中划分一定比例的数据作为验证集。此参数与 validation_set 互斥。**可选参数**：
   * **`custom_rl_pipeline`：强化学习流程配置，当训练方式为 GRPO或 PPO 时必填，详见**[强化学习配置](/docs/82379/1852871#757db87e)**。**
   * **`enable_trajectory`: 是否开启记录轨迹分析功能（仅支持对 RL 训练开启）。**开启此功能后，系统自动采集训练过程各样本的数据输入输出结果，记录强化学习精调轨迹，并展示于方舟控制台轨迹分析功能下。此记录有助于模型效果分析与问题排查，**对强化学习至关重要，建议训练前开启**。如需提前查看该功能效果，详见[轨迹分析](https://bytedance.larkoffice.com/docx/JXSnd9NLboSqyUxDQ8mcrSfHnYg#TBj3dIwXsoljYkxbdIRcRchwnRb)文档。（该功能依赖TLS日志服务开通，需先联系管理员，前往[TLS控制台](https://console.volcengine.com/tls/region:tls+cn-beijing/v2?)开通日志服务配置）
   * `name`：任务名称
   * `project`：任务所属的项目
   * `hyperparameters`：超参配置，超参信息查询方法见下
   * `save_model_limit`：保存训练产物数量上限

**获取训练可用超参信息**
训练时不同模型版本对应不同超参数，若要获取可用的超参数信息，可通过`ark get foundation-model` 命令获取。使用该命令时需要指定必填参数 `--model`（模型名）、`--version`（模型版本）、 `--fields hyperparameters` （表明查询超参）。
示例命令：
```Bash
ark get foundation-model --model doubao-seed-1-6 --version 250615 --fields hyperparameters
```

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/83c68fcafc9f4e5ea4ee34859ea695c7~tplv-goo7wpa0wc-image.image)

#### 1.通过python对象配置元数据
在初始化项目中，您可以打开初始化项目中的`job.py`文件，通过修改代码中的参数部分信息来配置元数据。具体如下：
```Python
if __name__ == "__main__":
    # 创建模型定制任务实例
    mcj = ModelCustomizationJob(
        name="sdk-job",  # 任务名称，用于唯一标识当前精调任务
        tags=[Tag(key="foo", value="bar")],  # 任务标签，用于分类管理（键值对形式）
        model_reference=ModelReference(  # 基础模型引用配置
            foundation_model=FoundationModelReference(
                name="doubao-seed-1-6-flash",  # 基础模型名称
                model_version="250615"  # 基础模型版本号
            )
        ),
        hyperparameters={  # 训练超参数配置（控制模型训练过程的关键参数）
            "batch_size": "32",  # 每次训练迭代的样本批量大小
            "clip_ratio_high": "0.2",  # 策略更新时的剪辑比率上限（控制更新幅度）
            "clip_ratio_low": "0.2",  # 策略更新时的剪辑比率下限
            "kl_coefficient": "0.001",  # KL散度系数（平衡新旧策略差异，防止更新过快）
            "loss_agg_mode": "seq-mean-token-mean",  # 损失函数聚合方式（序列平均+token平均）
            "lr": "0.000001",  # 学习率（控制参数更新速度）
            "lr_warmup_steps": "5",  # 学习率预热步数（逐步提升学习率，稳定训练）
            "max_new_tokens": "1024",  # 模型生成文本的最大token数量
            "num_generations": "8",  # 每次迭代生成的样本数量（用于强化学习的数据增强）
            "num_iterations_per_batch": "2",  # 每批数据的迭代训练次数
            "save_every_n_steps": "10",  # 每训练10步保存一次模型 checkpoint
            "temperature": "1.0",  # 生成文本的温度参数（1.0表示随机性中等）
            "test_every_n_steps": "5",  # 每训练5步执行一次测试验证
            "test_num_generations": "1",  # 测试时生成的样本数量
            "test_top_p": "1",  # 测试时的top_p参数（1.0表示不限制生成多样性）
            "top_p": "1",  # 训练时的top_p参数（控制生成文本的概率分布范围）
            "num_steps": "10",  # 总训练步数（整个任务的训练迭代次数）
        },
        data=Data(  # 训练数据配置
            training_set=TrainingDataset(  # 训练数据集设置
                local_files=[
                    "./data/mcj_rollout_test_dataset.jsonl",  # 本地训练数据文件路径
                    # tos_bucket: foo_bucket  # （可选）云存储桶名称
                    # tos_paths: ["training_data.jsonl"]  # （可选）云存储中的数据文件路径
                ]
            )
        ),
        custom_rl_pipeline=GRPOPipeline(  # 自定义强化学习（RL）流程配置
            graders=[  # 评分器列表（用于计算奖励值的组件）
                PipelinePluginWrapper(
                    plugin=random_reward_fn,  # 奖励函数插件（定义如何计算奖励分）
                    envs={"foo": "bar"},  # 传递给奖励函数的环境变量
                    weight=0.5  # 该评分器在总奖励中的权重占比
                ),
            ],
            rollout=PipelinePluginWrapper(  # 采样器配置（生成训练样本的组件）
                plugin=demo_rollout,  # 采样函数插件（定义如何生成模型输出样本）
                envs={"foo": "bar"}  # 传递给采样函数的环境变量
            ),
        ),
    )

    # 提交任务到平台
    mcj.submit()
    # 打印任务提交结果及查看地址
    print(f"Job submitted. view job at {mcj.url}")
    
```

#### 2.通过yaml文件配置元数据
您也可在 `job.yaml` 文件中配置元数据，具体如下：
```YAML
name: sdk-job
customization_type: GRPO
model_reference:
  foundation_model:
    name: doubao-seed-1-6-flash
    model_version: '250615'
hyperparameters:
  batch_size: '128'
  clip_ratio_high: '0.2'
  clip_ratio_low: '0.2'
  kl_coefficient: '0.001'
  loss_agg_mode: seq-mean-token-mean
  lr: '0.000001'
  lr_warmup_steps: '5'
  max_new_tokens: '1024'
  num_generations: '8'
  num_iterations_per_batch: '2'
  save_every_n_steps: '10'
  temperature: '1.0'
  test_every_n_steps: '5'
  test_num_generations: '1'
  test_top_p: '1'
  top_p: '1'
  num_steps: '20'
custom_rl_pipeline:
  graders:
  - plugin:
      name: random_reward
      python_func: plugins.random_reward:random_reward_fn
      envs:
        foo: bar
    weight: 0.5
  rollout:
    plugin:
      name: demo_rollout
      python_func: plugins.weather_rollout:demo_rollout
      envs:
        foo: bar
data: # 数据配置
  training_set:
    local_files:
    - ./data/mcj_rollout_test_dataset.jsonl # 训练集本地文件路径
    # tos_bucket: foo_bucket
    # tos_paths: ["training_data.jsonl"]
  validation_set:
     local_files:
    - ./data/mcj_rollout_test_dataset.jsonl # 验证集本地文件路径
save_model_limit: 1 # 保存模型的数量限制
```

### 提交精调任务
通过python对象完成上述配置后，可通过下面的命令提交精调任务。
```Python
 python job.py
```

通过yaml文件完成上述配置后，可通过下面的命令提交精调任务。
```Python
ark create mcj -f job.yaml
```

## 强化学习配置
### 选择强化学习流程
创建强化学习任务时，需要配置强化学习流程`custom_rl_pipeline`，需要选择pipeline类型，选择pipeline以后需要配置所需的plugin函数，配置plugin 函数也有明确要求，具体如下：

- 支持的 pipeline 类型 | `GRPOPipeline `和`PPOPipeline`
- rollout plugin 要求 | 仅支持填入 1 个自定义 rollout plugin，不填默认使用单轮模型推理 rollout 逻辑
- grader plugin 要求 | 至少需要填入一个 grader plugin，支持填写多个并分别配置权重

### 创建强化学习 Plugin 函数
在强化学习任务中，您需要创建并使用以下两种自定义 Plugin 函数：

* **Rollout Plugin 函数**：使用 @rollout 装饰器进行标记。它用于实现自定义的 Rollout 逻辑，例如在特定业务场景中执行多轮推理，或实现多次调用工具的 Agent。
* **Grader Plugin 函数**：用于计算奖励（Reward）分数。它支持以下两种评分方式：
   * **单样本评分器**：使用 @single_grader 装饰器。此函数用于对单个样本进行评分。当您输入多个样本时，平台会自动将其拆分，逐一评分后汇总结果。
   * **多样本评分器**：使用 @group_grader 装饰器。此函数用于一次性对一组样本进行评分，并返回该组所有样本的得分。

您可以在工作区内的任意位置创建这些函数，但必须使用 SDK 提供的相应装饰器（如 @rollout）来声明其 Plugin 类型、元信息及运行时配置。**如果函数未使用装饰器标记，系统将无法将其识别为可用的 Plugin。**
:::tip
##### **装饰器**
用于标记 plugin 函数类型，声明 plugin 相关元信息与运行时配置。提交任务时将根据plugin类型校验是否满足函数签名和强化学习pipeline是否完备。

* **类型**：支持`@rollout`/`@single_grader`/ `@group_grader`三种类型的装饰器
* **参数**：
   * **name**：可选，为 plugin 指定名字，不提供时默认使用函数名
   * **description**：可选，为 plugin 附加描述
   * **runtime.instance**：可选，指定 plugin 运行实例规格，当过载时可适当增加。默认值 CPU1MEM2（对应cpu1核内存2gb）、最大值CPU16MEM128，cpu核数：内存gb数=1:2、1:4、1:8
   * **runtime.timeout**：可选，plugin 函数执行的超时秒数，默认值取决于服务端逻辑，最大值900。plugin耗时过高会导致训练资源闲置增加费用，建议尽可能优化减少耗时
   * **runtime.max_concurrency**：可选，单 plugin 函数实例最大请求并发数，超该值触发扩容，可根据负载情况调整。默认值10，最大100，最小1
:::
我们提供了以下rollout plugin函数模版和Grader plugin函数模版，您可参考签名要求和函数模版实现所需函数。
#### **Rollout函数**
**函数模版**

* **例1:通过对话(Chat) API+自定义工具实现天气问答**
   * 在初始化项目`rl_demo`中`weather_rollout.py`模版： 利用精调SDK提供的封装，通过 Ark() 获取客户端，训练逻辑（包括状态更新和完成处理）由SDK自动管理。以下是示例代码：
   ```Python
   @rollout(
       name="demo_rollout",
       runtime=Runtime(
           instance=PluginInstance.CPU1MEM2,
           max_concurrency=100,
           min_replicas=1,
           max_replicas=10,
           timeout=900,
       ),
   )
   @coze_monitor
   def demo_rollout(  # sync的函数，会由@rollout转换成线程并发的async函数来提升运行效率
       context: PluginContext,
       proxy: RolloutInferenceProxy,
       sample: ChatCompletionSample,
   ) -> Optional[RolloutResult]:
       """
       训练时，每个样本会调用 n_sample 次rollout函数，每次调用rollout函数当前仅支持返回一条轨迹（wrap_client_inplace的client最后一次调用上下文）
       """
       # 使用训练感知客户端 - 仅此一行不同！
       client = Ark(base_url=proxy.url, api_key=proxy.jwt_token)
       # 可以使用openai的client
       # from openai import OpenAI
       # from ark_sdk.core.plugin.rollout.proxy import wrap_client_inplace
       # client = OpenAI(base_url=proxy.url, api_key=proxy.jwt_token)
       wrap_inplace_trace(client)
       wrap_client_inplace(client, proxy=proxy)
       req = sample.model_dump()
       messages = req.pop("messages")
       tools = (req.pop("tools") or []) + rollout_tools
       step = 0
   
       while True:
           # 步骤2: 发起模型请求，由于模型在收到工具执行结果后仍然可能有工具调用意愿，因此需要多次请求
           completion = client.chat.completions.create(
               # model 字段仅在本地测试时生效
               model=LOCAL_TEST_MODEL,
               messages=messages,
               tools=tools,
           )
           # 注意：训练逻辑已经自动处理，无需手动判断模式或调用process_completion！
           messages.append(completion.choices[0].message.model_dump())
           step += 1
           logger.info(f"completion: {completion.choices[0].message.model_dump()}")
   
           if step >= 30:
               break
   
           if completion.choices[0].finish_reason != "tool_calls":
               # 模型最终总结，没有调用工具意愿
               break
           tool_calls = completion.choices[0].message.tool_calls
           for tool_call in tool_calls or []:
               tool_name = tool_call.function.name
               if tool_name == "get_current_weather":
                   # 步骤 3：调用外部工具
                   try:
                       args = json.loads(tool_call.function.arguments)
                       tool_result = get_current_weather(**args)
                   except Exception as e:
                       logger.error(f"get_current_weather error: {e}")
   
                       # 重试，rollout请求，超过重试次数会DISCARD掉本条数据（包裹所有n smaple）
                       # return RolloutResult(status=PluginStatus.RETRY, error=str(e))
   
                       # 丢弃，丢弃本样本（包裹所有n smaple）
                       # return RolloutResult(status=PluginStatus.DISCARD, error="")
   
                       # 失败，返回FAILURE重试本样本，超过重试次数会拉挂任务。如果不处理exception，默认会返回FAILURE
                       # return RolloutResult(status=PluginStatus.FAILURE, error=str(e))
   
                       # extra字段用于传递额外的信息给reward函数，此处通知reward需要惩罚为-1分
                       return RolloutResult(
                           status=PluginStatus.SUCCESS,
                           extra={
                               "reward": -1,
                           },
                       )
   
                   # 步骤 4：回填工具结果，并获取模型总结回复
                   messages.append(
                       {
                           "role": "tool",
                           "content": tool_result,
                           "tool_call_id": tool_call.id,
                       }
                   )
   
       # 失败，丢弃本样本（包裹所有n smaple）
       # return RolloutResult(status=PluginStatus.DISCARD, error="")
       # 默认return None则视为rollout成功
       return None
   ```

   * 在初始化项目rl_demo中raw_rollout模版：通过手动初始化 Ark 客户端，并需要显式调用 proxy.update_state_from_messages 和 proxy.process_completion 来处理训练过程中的状态更新和完成回调，其实现是同步的，这赋予了开发者更底层的控制能力。以下是示例代码：
   ```Python
   @rollout(
       runtime=Runtime(
           instance=PluginInstance.CPU1MEM2,
           max_concurrency=100,
           min_replicas=1,
           max_replicas=10,
       ),
   )
   @coze_monitor
   def demo_rollout(
       context: PluginContext,
       proxy: RolloutInferenceProxy,
       sample: ChatCompletionSample,
   ) -> Optional[RolloutResult]:
       client = Ark(base_url=proxy.url, api_key=proxy.jwt_token)
       # 可选：添加cozeloop trace，用于debug监控模型调用输入输出
       wrap_inplace_trace(client)
       req = sample.model_dump()
       messages = req.pop("messages")
       tools = (req.pop("tools") or []) + rollout_tools
       while True:
           # NOTE:强化学习场景特殊逻辑
           # 请求前发给 proxy 检查历史信息、更新内部状态（当 message 更改历史时，proxy 会自动截断内部维护的 token ids）。
           proxy.update_state_from_messages([ChatCompletionMessage(**msg) for msg in messages])
           # 步骤2: 发起模型请求，由于模型在收到工具执行结果后仍然可能有工具调用意愿，因此需要多次请求
           completion = client.chat.completions.create(
               # model 字段仅在本地测试时生效
               model=LOCAL_TEST_MODEL,
               messages=messages,
               tools=tools,
               # NOTE:强化学习场景特殊逻辑
               extra_body=proxy.get_extra_body(),
               extra_headers=proxy.headers,
           )
           assert isinstance(completion, ChatCompletion)
           # NOTE:强化学习场景特殊逻辑
           proxy.process_completion(ChatCompletionResponse(**completion.model_dump()))
           logger.info(f"completion: {completion.choices[0].message.model_dump()}")
           messages.append(completion.choices[0].message.model_dump())
           if completion.choices[0].finish_reason != "tool_calls":
               # 模型最终总结，没有调用工具意愿
               break
           tool_calls = completion.choices[0].message.tool_calls
           for tool_call in tool_calls or []:
               tool_name = tool_call.function.name
               if tool_name == "get_current_weather":
                   # 步骤 3：调用外部工具
                   args = json.loads(tool_call.function.arguments)
                   tool_result = get_current_weather(**args)
                   # 步骤 4：回填工具结果，并获取模型总结回复
                   messages.append(
                       {
                           "role": "tool",
                           "content": tool_result,
                           "tool_call_id": tool_call.id,
                       }
                   )
       return
   ```

* **例2:通过**[Arkitect](https://github.com/volcengine/ai-app-lab/tree/main/arkitect)**框架+MCP实现deep search**
   * 在初始化项目`rl_search_mcp_demo`中`draft_rollout_arkitect.py`模版：它利用Arkitect框架和MCP（Multi-Client Proxy）机制，旨在实现基于强化学习的大模型微调，特别是在融合信息搜索场景下。该函数通过 build_mcp_clients_from_config 从配置文件加载多个MCP客户端作为工具，并使用 wrap_async_client_inplace 将Arkitect的上下文（ ctx ）与Rollout代理（ proxy ）进行绑定，从而实现训练感知。以下是示例代码：
   ```Python
   @rollout(
       name="code_sandbox_grader_single_grader",
       description="code_sandbox_grader",
       runtime=Runtime(
           instance=PluginInstance.CPU1MEM2,
           max_concurrency=100,
           min_replicas=1,
           max_replicas=10,
       ),
   )
   async def demo_rollout(
       context: PluginContext,
       proxy: RolloutInferenceProxy,
       sample: ChatCompletionSample,
   ) -> Optional[RolloutResult]:
       mcp_clients, cleanup = build_mcp_clients_from_config(CONFIG_FILE_PATH)
       req = sample.model_dump()
       messages = req.pop("messages")
       ctx = Context(
           model="doubao-seed-1-6-flash-250615",
           tools=[
               *mcp_clients.values(),
               # get_current_weather,
           ],  # 直接在这个list里传入你的所有python 方法作为tool，tool的描述会自动带给模型推理，tool的执行在ctx.completions.create 中会自动进行
       )
       # 和推理链路的diff
       wrap_async_client_inplace(ctx, proxy=proxy)
       await ctx.init()
       # 每次调用messages新增即可, ctx会帮忙维护history, 以及执行tool_call, 回填tool role(工具执行结果)
       # NOTE: 当模型输出错误的工具入参时，Arkitect会自动将报错作为一条tool role 返回模型让其自行修正。训练中可能无法获得有关错误入参的负样本，TODO: 提前返回reward 0 分
       _ = await ctx.completions.create(messages=messages, stream=False)
       await cleanup()
       return
   ```

* **例3：通过对话(Chat) API+ 调用搜索api工具 实现deep search**
   * 在初始化项目`rl_search_mcp_demo`中`rollout.py`模版：
   ```Bash
   @rollout(
       name="code_sandbox_grader_single_grader",
       description="code_sandbox_grader",
       runtime=Runtime(
           instance=PluginInstance.CPU2MEM4,
           timeout=900,
       ),
   )
   @coze_monitor
   async def demo_rollout(
       context: PluginContext,
       proxy: RolloutInferenceProxy,
       sample: ChatCompletionSample,
   ) -> Optional[RolloutResult]:
       """
       训练时，每个样本会调用 n_sample 次rollout函数，每次调用rollout函数当前仅支持返回一条轨迹（wrap_client_inplace的client最后一次调用上下文）
       """
       # NOTE: 由于search mcp的封装，无法修改tool定义，使用直接调用api的方式
       # from arkitect.core.component.tool.tool_pool import ToolPool, build_tool_pool
       # from arkitect.core.component.tool.builder import build_mcp_clients_from_config
       # mcp_clients, cleanup = build_mcp_clients_from_config(CONFIG_FILE_PATH)
       # tool_pool = build_tool_pool(list(mcp_clients.values()))
       # tools = [tool.model_dump() for tool in await tool_pool.list_tools()]
       tools = [search_tool]
       schema = search_tool["function"]["parameters"]
       search_api_key = os.getenv("SEARCH_API_KEY")
       assert search_api_key, "env SEARCH_API_KEY not set"
       async with httpx.AsyncClient(
           base_url="https://open.feedcoopapi.com",
           headers={
               "Authorization": f"Bearer {search_api_key}",
               "Content-Type": "application/json",
           },
       ) as tool_client:
           req = sample.model_dump()
           messages = req.pop("messages")
           client = AsyncArk(base_url=proxy.url, api_key=proxy.jwt_token)
   
           wrap_inplace_trace(client)
           wrap_async_client_inplace(client, proxy=proxy)
   
           step = 0
   
           while True:
               completion = await client.chat.completions.create(
                   # model 字段仅在本地测试时生效
                   model=LOCAL_TEST_MODEL,
                   messages=messages,
                   tools=tools,
               )
               messages.append(completion.choices[0].message.model_dump())
               step += 1
   
               if step >= MAX_STEPS:
                   break
               if completion.choices[0].finish_reason != "tool_calls":
                   break
   
               tool_calls = completion.choices[0].message.tool_calls
               for tool_call in tool_calls or []:
                   try:
                       name = tool_call.function.name
                       params = json.loads(tool_call.function.arguments)
                       assert name == "web_search", f"tool name not web_search: {name}"
                       jsonschema.validate(instance=params, schema=schema)
                   except (
                       json.JSONDecodeError,
                       AssertionError,
                       jsonschema.ValidationError,
                   ) as e:
                       # NOTE: extra字段用于传递额外的信息给reward函数，此处通知reward需要惩罚为-1分
                       logger.error(f"tool call format check failed: {repr(e)}")
                       return RolloutResult(
                           status=PluginStatus.SUCCESS,
                           extra={
                               "reward": -1,
                           },
                       )
                   res = await exec_tool_call(
                       tool_client=tool_client,
                       name=name,
                       arguments=params,
                   )
                   messages.append(
                       {
                           "role": "tool",
                           "content": res,
                           "tool_call_id": tool_call.id,
                       }
                   )
   
       return RolloutResult(
           status=PluginStatus.SUCCESS,
       )
   ```

#### **Grader函数**
**签名要求**
```Python
@dataclass
class RewardFunctionResult:
    rewards: List[float]
    metrics: Dict[str, float]
    status: str # success/failure/discard
    error: str

def reward_fn(
    context: Dict[str, Any],
    sample: Dict[str, Any], 
    trajectories: list[Dict[str,Any]])-> RewardFunctionResult#group grader为trajectories ；ingle grader为 trajectory
```

**入参**

- 字段 | 类型 | 描述 | 样例
- context
- context: Dict[str, Any]
- * 该请求对应的任务信息，包含
- * 任务 Id
- * 模型名
- * 模型版本
- * 训练方式
- * phase: 样本来自什么阶段，train/test(验证集）
- * 是否是 mock 请求
- ```JSON
- {
- "modle_customization_job_id": "mcj_xxxxxx_xxx",
- "foundation_model_name": "doubao-1-5-lite-32k",
- "foundation_model_version": "250115",
- "customization_type": "GRPO"，
- "phase":"train",
- "is_mock": false,
- }
- ```
- sample
- Dict[str, Any]
- *  rollout 输入的样本，与数据集内容完全一致
- * **注意，1.6 模型或其他多模态模型，content 字段会转换为 list，如：**
- ```JSON
- [
- {
- "type": "text",
- "text": "1+1=？"
- }
- ]
- ```
- ```JSON
- {
- "messages": [
- {
- "role": "system",
- "content": "你是一个擅长数据计算的人工智能助手。"
- },
- {
- "role": "user",
- "content": "1+1=？"
- }
- ],
- "tools": [],
- "extra": {
- "answer": 1234
- }
- }
- ```
- trajectories
- list[Dict[str,Any]]
- * 一条样本的所有 rollout 输出的结果
- * 假设一次 rollout 采样 n 次，trajectory 长度就为 n
- * messages 类型为数组，非 agent rl 场景长度固定为 1
- ```JSON
- [
- {
- "messages": [
- {
- "content": "等于 2",
- "role": "assistant"
- }
- ],
- "finish_reason": "stop",
- "usage": {
- "completion_tokens": 3,
- "prompt_tokens:": 20,
- "total_tokens": 23
- }
- },
- {
- "messages": [
- {
- "reasoning_content": "嗯，用户问的是 1 加 1 等于多少。首先，我需要确认这是一个基本的算术问题。在常规的十进制数学中，1 加 1 的结果是 2。这是最基础的加法运算，应该没有其他复杂的情况需要考虑。用户可能是在测试我的基本计算能力，或者是刚开始学习数学的小朋友。所以直接回答 2 就可以了。",
- "content": "1 + 1 等于 2。这是基础的算术加法运算，在十进制计数系统中，1 和 1 相加的结果是 2。",
- "role": "assistant"
- }
- ],
- "finish_reason": "stop",
- "usage": {
- "completion_tokens": 109,
- "prompt_tokens:": 20,
- "total_tokens": 129
- }
- }
- ]
- ```

**返回**

- 参数 | 类型 | 描述 | 样例
- rewards
- list[float]
- * 按照 trajectories 的顺序返回每个采样的得分
- ```JSON
- [
- 0.0,
- 1.0,
- 0.5
- ]
- ```
- metrics
- Dict[str, float]
- * 支持返回 reward 过程的自定义指标，如计算耗时等。训练框架将把每个 step 的指标按 key 聚合出最大值，最小值和平均值。
- ```Thrift
- {
- "avg_length_reward": 0.49,
- "avg_formtat_reward": 0.9,
- }
- ```
- status | str | * success/failure/discard
- error | str
- * 如果status 为 failure，可携带具体失败原因或错误栈信息，会打印到训练日志中

**函数模版**

* **例1:字符串包含**
   * random_reward_fn：对一组样本通过rulebase实现判断模型生成内容是否包含数据集中给定的标准答案。以下是示例代码：
   ```Python
   @group_grader(
       name="randaom_reward",
       runtime=Runtime(
           instance=PluginInstance.CPU1MEM2,
           max_concurrency=100,
           timeout=300,
       ),
   )
   def random_reward_fn(
       context: PluginContext,
       sample: ChatCompletionSample,
       trajectories: List[Trajectory],
   ) -> GroupGraderResult:
       """
       奖励函数：返回随机奖励
   
       参数:
       - trajectories: 完整的对话历史
       - sample: 样本数据，包含标准答案的字典
   
       返回:
       - list[float]: 奖励分数列表，每个分数对应一个候选回复（1.0表示完全匹配，0.0表示不匹配）
   
       依赖:
       - 数据集里的字典字段 extra 内需要携带 answer 字段。
       """
       rewards = [
           t.extra["reward"] if (t.extra and "reward" in t.extra) else random.random()
           for t in trajectories
       ]
       return GroupGraderResult(
           rewards=rewards, status=PluginStatus.SUCCESS, error="", metrics={}
       )
   ```

* **例2:LLM as a judge**
   * llm_grader：利用大模型作为评分专家，来对模型生成的内容进行评分。完整例子在rl_search_mcp_demo中，以下是关键部分代码：
   ```Python
   SYSTEM_PROMPT = """你是一位严谨的内容评估专家。
   你的核心任务是：依据提供的【问题】和一份【正确答案列表】（列表中的每个答案都被视为该问题的有效解答），来判断我提供的【预测答案】是否正确回答了该【问题】。你应该首先给出你的判断依据，然后给出你的判断结果（即“正确”或“错误”）。
   判断标准如下：
   1. 【预测答案】不需要与【正确答案列表】中的任何答案在字面上完全相同，但应该在语义上相同。
   2. 【正确答案列表】中的每个答案都可以被视为问题的正确答案，【预测答案】应该至少与其中之一在语义上是相同的。
   3. 你必须仔细阅读【问题】和【预测答案】，并仔细考虑【预测答案】是否真的正确回答了【问题】，不可以因为【预测答案】中包含了正确答案的字眼而认为【预测答案】正确。
   4. 对于模型没有认真回答问题，只是通过列举很多答案骗分的情况，请你判断为错误。
   """
   
   USER_PROMPT_TEMPLATE = """
   ###问题###
   {}
   ###正确答案列表###
   {}
   ###预测答案###
   {}
   现在请开始你的判断：
   """
   
   @group_grader(
       name="llm_group_grader",
       description="llm_group_grader",
       runtime=Runtime(
           instance=PluginInstance.CPU1MEM2,
           max_concurrency=32,
           timeout=900,
       ),
   )
   async def llm_grader(
       context: PluginContext,
       sample: ChatCompletionSample,
       trajectories: List[Trajectory],
   ):
       if (
           not sample.extra
           or "answer" not in sample.extra
           or sample.messages[-1].role != "user"
       ):
           return GroupGraderResult(
               rewards=[],
               metrics={},
               status=PluginStatus.DISCARD,
               error="sample is not valid",
           )
   
       ark = AsyncArk()
       reward_list = []
       tasks = []
   
       @backoff.on_exception(
           wait_gen=backoff.expo,
           exception=(
               ArkAPIConnectionError,
               ArkRateLimitError,
               ArkInternalServerError,
               json.JSONDecodeError,
           ),
           max_time=100,
           jitter=backoff.full_jitter,  # full jitter
           base=2,
           factor=0.4,
           max_value=20,  # 退避重试最大时长（秒）
       )
       async def single_grader(traj: Trajectory) -> float:
           # 返回rollout过程中提前设置的reward
           if traj.extra and "reward" in traj.extra:
               return traj.extra["reward"]
   
           format_score = validate_tool_calls(traj)
           if format_score is not None:
               logger.info(
                   f"prompt_id: {sample.extra.get('prompt_id')} eval_res: {format_score} format check failed. {traj.messages}"
               )
               return format_score
           resp = await ark.chat.completions.create(
               model="doubao-seed-1-6-250615",
               max_completion_tokens=MAX_COMPLETION_TOKENS,
               temperature=0,
               messages=[
                   {"role": "system", "content": SYSTEM_PROMPT},
                   {
                       "role": "user",
                       # NOTE: 仅reward最后的总结，并未考虑历史tool_calls
                       "content": USER_PROMPT_TEMPLATE.format(
                           sample.messages[-1].content,
                           sample.extra["answer"],
                           traj.messages[-1].content,
                       ),
                   },
               ],
               response_format={
                   "type": "json_schema",
                   "json_schema": {
                       "name": "评估结果",
                       "description": "根据你作为内容评估专家的角色和评估标准进行评估的结果",
                       "schema": {
                           "type": "object",
                           "properties": {
                               "rationale": {
                                   "type": "string",
                                   "description": "详细判断理由",
                               },
                               "judgement": {
                                   "type": "string",
                                   "enum": ["正确", "错误"],
                                   "description": "判断结果",
                               },
                           },
                           "required": ["rationale", "judgement"],
                       },
                       "strict": True,
                   },
               },
           )
   
           eval_res = json.loads(resp.choices[0].message.content)
           logger.info(f"prompt_id: {sample.extra.get('prompt_id')} eval_res: {eval_res}")
           if eval_res["judgement"] == "正确":
               return 1.0
           else:
               return 0.0
   
       for traj in trajectories:
           tasks.append(asyncio.create_task(single_grader(traj)))
   
       results = await asyncio.gather(*tasks, return_exceptions=True)
       for res in results:
           if isinstance(res, Exception):
               return GroupGraderResult(
                   rewards=[], metrics={}, status=PluginStatus.FAILURE, error=str(res)
               )
           reward_list.append(res)
   
       return GroupGraderResult(
           rewards=reward_list, metrics={}, status=PluginStatus.SUCCESS, error=""
       )
       
   ```

#### **创建plugin函数时关键注意事项**

1. 协程 / 线程并发选择与函数调用规范
* 核心原则：使用`async`定义协程并发函数时，需避免同步长阻塞函数调用，防止并发效率下降。
* 正确操作：
   * 协程并发场景：函数调用需匹配异步版本，比如`Ark()`，应该使用`AsyncArk()`。若使用`@rollout()`装饰器定义异步函数，示例如下：
   ```Python
   @rollout()
   async def demo_rollout():
   # 函数逻辑（需调用AsyncArk()等异步函数）
   ```

   * 同步长耗时场景：若必须使用耗时同步函数，需去掉`async`关键字，此时会自动启用线程并发，定义示例如下：
   ```Python
   @rollout()
   def demo_rollout():
   # 函数逻辑（可调用同步长耗时函数）
   ```

2. Rollout 与 Grader 并发能力压测要求
* 关键影响：Rollout 和 Grader 的并发能力直接决定训练效率，未达预期会导致任务耗时显著增加。
* 操作要求：正式提交训练任务前，建议在本地环境使用目标数据集进行并发压测，验证并发性能是否满足需求。
* 参考示例：压测方法可参考[deep search demo中draft_rollout_arkitect.py文件中main](https://www.volcengine.com/docs/82379/1852871?lang=zh#%E6%B5%8B%E8%AF%95plugin%E5%87%BD%E6%95%B0)函数的实现逻辑。
3. 推理 Agent 转训练 Agent 的逻辑优化策略
* 优化目标：将推理场景的 Agent 改造为训练场景 Agent 时，通过逻辑调整减少 badcase，提升模型训练下限与效果。
* 具体操作：针对性地将推理 Agent 中的 “容错逻辑”（如跳过错误、默认返回等），改造为 “错误扣分逻辑”（即对错误结果进行明确扣分，强化模型对错误的识别与规避）。

### 可观测性配置

1. **轨迹分析**：**在精调参数配置`job.py`文件中配置`enable_trajectory=True`，即可开启轨迹分析功能。**开启后，系统将记录强化学习训练轨迹，可视化展示于方舟控制台轨迹分析功能下。此记录有助于模型效果分析与问题排查，**对强化学习至关重要，建议训练前开启**。如需提前查看该功能效果，详见[轨迹分析](https://bytedance.larkoffice.com/docx/JXSnd9NLboSqyUxDQ8mcrSfHnYg#TBj3dIwXsoljYkxbdIRcRchwnRb)。（该功能依赖TLS日志服务，需先联系管理员，前往[TLS控制台](https://console.volcengine.com/tls/region:tls+cn-beijing/v2?)开通配置。）
2. **自定义函数日志**：根据用户自定义需求记录Rollout、Reward函数执行中的关键信息，以精准定位问题、提高排查效率。具体实现方面，在`rollout.py`文件内，通过`logger.info`、`logger.error`等方法完成日志记录操作。最终，这些日志将展示于方舟控制台的自定义日志功能模块下。（该功能依赖TLS日志服务开通，需先联系管理员，前往[TLS控制台](https://console.volcengine.com/tls/region:tls+cn-beijing/v2?)开通日志服务配置）
3. **Tracing** ：为分析模型效果、定位问题，建议使用 Tracing 系统采集样本 rollout 每轮推理和工具调用的输入输出及耗时信息。可选用任意 Tracing 系统，本文以 cozeloop为例介绍使用方法，可参考示例调整。
   **CozeLoop Tracing**
   1. **配置与环境设置：**cozeloop配置详情请参考https://loop.coze.cn/open/docs/cozeloop/python-sdk
      * **本地运行配置：**在终端中设置以下环境变量：
      ```Python
        export COZELOOP_WORKSPACE_ID=xxx 
        export COZELOOP_API_TOKEN=xxx
      ```

      * **训练环境配置：**需在 faas 的环境变量中添加如下内容：
      ```JSON
      rollout=PipelinePluginWrapper(
          plugin=demo_rollout,
          envs={
              "COZELOOP_API_TOKEN": "xxx",
              "COZELOOP_WORKSPACE_ID": "xxx",
          },
      ),
      ```

   2. **使用方法：**
      * **对Rollout函数的输入输出Tracing：**使用`@coze_monitor`装饰器包装 rollout 函数。该装饰器来自项目中的`plugins/coze_monitor.py`文件，它基于 cozeloop 库进行数据上报，会从`proxy`中取出 rollout 实际的轨迹返回，并以精调任务的 ID 作为`user_id`（本地测试时`user_id`默认为`test_service`），方便在 CozeLoop 平台上进行数据分析和筛选。示例如下：
      ```Plain Text
      @rollout(
          name="code_sandbox_grader_single_grader",
          description="code_sandbox_grader",
          runtime=Runtime(
              instance=PluginInstance.CPU2MEM4,
              timeout=900,
          ),
      )
      @coze_monitor
      async def demo_rollout(context, proxy, sample):
          # 函数实现...
      ```

      * **对LLM Chat的输入输出Tracing：**使用`wrap_inplace_trace`函数包装 LLM 客户端。示例如下：
      ```Plain Text
      client = AsyncArk(base_url=proxy.url, api_key=proxy.jwt_token)
      wrap_inplace_trace(client)
      ```

      * **对中间过程添加自定义Tracing：**使用`@observe()`装饰器记录任意函数的输入输出。示例如下：
      ```Plain Text
      @observe()  # type: ignore
      async def exec_tool_call(tool_client, name, arguments):
      ```

   3. **Cozeloop 效果：**若您完成了 cozeloop 配置，运行后可在 Cozeloop 平台观测 Rollout 过程，具体效果如下：
      * 分析各阶段耗时优化rollout链路
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/95bbf7dd2431453f8f26ad6fe04f85c4~tplv-goo7wpa0wc-image.image)

      * 指定任务筛选超长模型返回
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cd92576d88014de5a9872da4fbcbd644~tplv-goo7wpa0wc-image.image)

### 测试plugin函数
在创建`plugin`函数后，为确保其功能符合预期，可通过**本地运行**和**在线运行**两种方式对其进行测试：

1. **本地运行**：`draft_rollout_arkitect.py `文件中的`main` 函数展示了在本地对 `demo_rollout `与` llm_grader `结合使用进行测试。这一过程呈现了 “从单样本调试到批量验证” 的完整链路。
   * **单样本评估：**测试时会基于样本，调用 demo_rollout 执行推理，获取模型的回答；将AI模型的回答和原始问题、正确答案一起传递给 llm_grader 进行评估，并打印评估结果。
   * **批量验证：**main 函数还展示了如何使用 test_with_dataset 函数对一个数据集中的多个样本进行批量推理和评估，以获取平均奖励分数。代码如下：
   ```Python
   async def main():
       from ark_sdk.core.plugin.rollout.proxy import InferenceProxy, Mode
       from plugins.llm_grader import llm_grader
       import os
   
       # 调试模式，使用公共服务
       mode = Mode.Inference
       base_url = "https://ark.cn-beijing.volces.com/api/v3"
       api_key = os.getenv("ARK_API_KEY", "xxx")
   
       sample = ChatCompletionSample(
           **{
               "messages": [
                   {
                       "role": "user",
                       "content": "通过景栗科技的私域运营服务和与薪勤科技的产品共创，哪两个公司在各自的领域实现了用户增长或应用上架？",
                   }
               ],
               "thinking": {"type": "enabled"},
               "extra": {"answer": "景栗科技和薪勤科技", "prompt_id": "123"},
           }
       )
       proxy = InferenceProxy(sample, url=base_url, jwt_token=api_key, mode=mode)
       await demo_rollout({}, proxy, sample)
       logger.info(f"demo rollout done with result: {proxy.messages}")
       grader_res = await llm_grader(
           {},
           sample,
           [
               ChatCompletion(
                   messages=proxy.messages,
                   usage=proxy.usage,
                   finish_reason=proxy.finish_reason,
               )
           ],
       )
       logger.info(f"demo grader done with result: {grader_res}")
       #批量验证
       logger.info("small dataset")
   
       jsonl_file_path = "./data/search_dataset_dev_100.jsonl"
       from plugins.test_utils import test_with_dataset
   
       # NOTE: 可以针对其他模型测试数据集的reward分数
       rewards = await test_with_dataset(
           jsonl_file_path, demo_rollout, llm_grader, limit=10, max_concurrent=10
       )
       logger.info(f"avg rewards: {sum(rewards) / len(rewards)}")
   ```

   你可以执行下方命令，快速对plugin函数进行本地测试：
   ```Bash
   curl -LsSf https://astral.sh/uv/install.sh | sh 
   #windows系统下设置环境变量的命令为：set ARK_API_KEY=your_api_key 
   export ARK_API_KEY=your_api_key 
   #建议使用tracing系统记录模型调用过程，便于分析模型效果、定位问题，以cozeloop平台为例对相关环境变量进行配置，以确保 tracing 功能正常运行。详情请参考https://bytedance.larkoffice.com/docx/HKN2dX97MoPEZJxhBoQcRcuUncd#HsNQdtWH1oDK3oxdf8Rcg1NonBg 
   export COZELOOP_WORKSPACE_ID=xxx 
   export COZELOOP_API_TOKEN=xxx 
   PYTHONPATH=./ uv run plugins/rollout.py 
   ```

   ```Bash
   pip install bytedance.fornax --upgrade --index-url=https://bytedpypi.byted.org/simple/
   #建议使用tracing系统记录模型调用过程，便于分析模型效果、定位问题，以fornax 平台为例对相关环境变量进行配置，以确保 tracing 功能正常运行。 
   export BYTED_HOST_IPV6=1
   export CONSUL_HTTP_HOST=10.225.68.72
   export CONSUL_HTTP_PORT=2280
   export RUNTIME_IDC_NAME=boe
   export FORNAX_CUSTOM_REGION=BOE
   PYTHONPATH=./ uv run plugins/rollout.py
   ```

2. **在线运行**：用户可采用 SDK 或 CLI 方式对`plugin`函数进行测试，将拉起在线运行环境进行测试，更接近训练任务。具体测试方法如下：
   * SDK方式用法
   ```Python
   from ark_sdk.resources.pipeline_plugin.test_instance import (
       get_or_create_pipeline_plugin_test_instance,
   )
   # 定义一个 plugin
   @group_grader
   def length_grader(
       context: PluginContext,
       sample: ChatCompletionSample,
       completions: List[ChatCompletion],
   ):
       return GroupGraderResult(
           rewards=[1.0], metrics={}, status=PluginStatus.SUCCESS, error=""
       )
   
   # sdk 用法
   if __name__ == '__main__':
       request = {
           "context": {},
           "sample": {
               "messages": [
                   {
                       "role": "system",
                       "content": "你是一个擅长数据计算的人工智能助手。"
                   },
                   {
                       "role": "user",
                       "content": "1+1=？"
                   }
               ]
           },
           "completion": {"messages": [
               {
                   "content": "等于 2",
                   "role": "assistant"
               }
           ]}}
          
       instance = ark.get_or_create_plugin_test_instance(length_grader) # 传入一个 plugin 对象，创建或者获取一个已创建好的 plugin 测试实例对象
       instance.sync() # 手动触发同步
       response = instance.request(request, sync=False) #发送请求，可通过sync 参数控制发送前是否执行一次同步
       print(response) # 打印response
       instance.stop() # 停止实例
   ```

   * CLI方式用法
   在使用CLI方式时，需使用 `ark test plugin` 命令，该命令包含以下参数：
   `--fn`：必填参数，代表函数，identifier。
   `--request`：必填参数，是请求体，要求为json字符串格式 ，具体样例可参考SDK用法。
   `--sync`：选填参数，默认值为否，用于确定在执行前是否触发一次同步。
   以下为完整示例：
   ```Plain Text
   ark test plugin --fn plugin.code_agent_grader:grader --request '{&"sample":{...},"comploetion":{....}}' --sync
   ```

### 提交强化学习任务
参考[配置精调参数](https://www.volcengine.com/docs/82379/1852871?lang=zh#%E9%85%8D%E7%BD%AE%E7%B2%BE%E8%B0%83%E5%8F%82%E6%95%B0)，配置好所需的强化学习流程和plugin函数，即可提交精调任务开始强化学习训练。
##  查看并管理精调任务
创建完成的精调任务可以使用控制台和CLI方式进行查看与管理，可以查看**精调任务列表、精调任务详情（基础信息**、**状态、任务及函数日志、效果指标）、产物导出为自定义模型、复制精调任务等。**
**控制台**
你可以访问[火山方舟控制台-模型精调](https://console.volcengine.com/ark/region:ark+cn-beijing/finetune)，通过界面化操作管理任务，具体使用详见文档[查看并管理模型精调任务](https://bytedance.larkoffice.com/docx/JXSnd9NLboSqyUxDQ8mcrSfHnYg#JXSnd9NLboSqyUxDQ8mcrSfHnYg)
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b4e1fd397f544ee795f0f12aa393115c~tplv-goo7wpa0wc-image.image)

**CLI 方式**
整体使用格式为 `ark [verb] [noun] [arguments] [options]` ，常用命令如下：

1. **查看全部精调任务**

使用`ark list mcj `命令可以列举账号下的精调信息，包括精调任务ID，名称，训练方式，基础模型，现处阶段等。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bccc21fc31f74f48b205c2aed90826e5~tplv-goo7wpa0wc-image.image)

同时可选择以下参数：
`--page-size/-ps`：单页返回数量，默认为 10
`--page-number/-pn`：查询页数，默认为 1
`--customization_type/-t`：限制查询的任务训练方式，多个训练方式逗号分隔，默认不限制
`--phase/-p`：限制查询的任务的阶段，多个训练方式逗号分隔，默认不限制

2. **获取精调任务详细信息**

使用 `ark get mcj` 命令可获取精调任务详情、RL任务和FaaS（托管rollout/grader的服务）的关联方式，可查看faas服务跳转链接。例如要获取精调任务ID为mcj-xxxxxx-xxx的精调任务详细信息，命令为：
```Bash
ark get mcj mcj-xxxxxx-xxx
```

3. **拉取精调任务配置到本地**：

使用 `ark pull mcj` 命令。例如要拉取精调任务ID为mcj-xxxxxx-xxxxx的精调任务到本地，可使用命令：
```Bash
ark pull mcj mcj-xxxxxx-xxxxx
```

同时可选择以下参数：
`--include-data/--exclude-data`：用于选择是否拉取数据，默认不拉取数据。
`--include-plugin/--exclude-plugin`：用于选择是否拉取plugin代码，默认拉取。
完整示例：
```Bash
#拉去精调任务到本地，并且拉取数据和plugin代码
ark pull mcj mcj-xxxxxx-xxxxx --include-data --include-plugin
```

任务拉取到本地后可以进行修改，修改后提交会生成一个新的精调任务，而不会影响原有任务。
## 精调产物使用与管理
可以使用控制台对精调产出的模型进行使用与管理，支持**查看模型信息**、**模型推理、增量训练、在线体验、发起评测**等操作。
**控制台**
你可以访问[火山方舟控制台-模型仓库](https://console.volcengine.com/ark/region:ark+cn-beijing/customModel)，界面化使用与管理精调产物，具体使用详见[精调模型的使用与管理](https://www.volcengine.com/docs/82379/1582651)
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/66f2277332844d4bb9937a5d26adb7e6~tplv-goo7wpa0wc-image.image)

# 案例分享
## Demo-使用**Arkitect框架+MCP实现deep search 强化学习**
`rl_search_mcp_demo` 这个示例项目，展示如何利用 SDK 在融合信息搜索场景下，借助 Arkitect 框架和 MCP（Multi - Client Proxy）机制，实现基于强化学习的大模型微调。
### 快速开始
您可参考以下介绍，快速复现本demo。
#### 安装与配置
在使用demo之前您需要 完成前提条件、安装SDK、配置授权信息，具体操作详见[前提条件](/docs/82379/1852871#b7d616e6)。
#### 初始化 rl_search_mcp_demo 精调项目
执行 `ark init workspace <文件夹名> --template rl_search_mcp_demo` 命令，即可拉取 `rl_search_mcp_demo` 的代码。该 `demo` 是一个已具备基础结构和配置、可直接投入使用的精调项目。
例如：
```Plain Text
ark init workspace sdk --template rl_search_mcp_demo
```

#### 配置MCP服务
配置 MCP 服务是为了在文件里定义各种可以被大型语言模型（LLM）调用的 外部工具或能力 。这样做是为了在模型推理时，LLM能够根据需要 使用这些工具来执行特定任务 ，从而扩展其功能。在此demo中配置了一个搜索插件：融合信息搜索MCP。

* 访问<https://www.volcengine.com/mcp-marketplace>搜索框中输入「融合信息搜索」找到该MCP服务，点击云部署生成专属长效URL，获取MCP服务。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/689c8e07436e4ad7b7ff595a8d60ee6f~tplv-goo7wpa0wc-image.image)

* 配置`mcp_config.json`文件

你需要将获取到的长效URL配置到`mcp_config.json`文件中，示例代码如下：
```Plain Text
{
    "mcpServers": {
        "search": {
            "url": "获取到的长效URL"
        }
    }
}
```

#### 提交基础任务
此时你便可以通过以下命令提交强化学习精调任务。同时，您可对示例项目进行个性化修改，以满足自身业务需求，如对plugin函数进行修改，对参数配置进行修改等等，具体详情请参考下文[个性化调整](/docs/82379/1852871#eeabef25)。
```Python
#windows系统下设置环境变量的命令为：set ARK_API_KEY=your_api_key
export ARK_API_KEY=your_api_key
python job.py
```

### 个性化调整
#### 修改plugin函数
以下是本demo的plugin函数示例。您可以基于示例修改 rollout plugin、grader plugin，以适配您的业务场景。
##### `draft_rollout_arkitect.py` 和 `rollout.py `：LLM 推理与工具集成
此 demo 提供了两种 Rollout 插件函数的实现方式，分别在 draft_rollout_arkitect.py 和 rollout.py 文件中，它们都使用 @rollout 装饰器进行标记，主要实现了一个 AI 模型推理与推理轨迹记录的流程。你可以根据你的场景选择合适的实现进行修改。
**基于** [Arkitect ](https://github.com/volcengine/ai-app-lab/tree/main/arkitect)**框架+MCP的实现 (** **draft_rollout_arkitect.py )**
`draft_rollout_arkitect.py `主要实现了一个基于 Arkitect 框架和 MCP (Multi-Cloud Platform) 的模型推理流程。它通过 Context 类管理模型和工具调用，简化了工具的定义和执行。你可以根据你的场景进行修改，代码如下：
```Python
@rollout(
    name="code_sandbox_grader_single_grader",
    description="code_sandbox_grader",
    runtime=Runtime(
        instance=PluginInstance.CPU1MEM2,
        max_concurrency=100,
        min_replicas=1,
        max_replicas=10,
    ),
)
async def demo_rollout(
    context: PluginContext,
    proxy: RolloutInferenceProxy,
    sample: ChatCompletionSample,
) -> Optional[RolloutResult]:
    mcp_clients, cleanup = build_mcp_clients_from_config(CONFIG_FILE_PATH)
    req = sample.model_dump()
    messages = req.pop("messages")
    ctx = Context(
        model="doubao-seed-1-6-250615",
        tools=[
            *mcp_clients.values(),
            # get_current_weather,
        ],  # 直接在这个list里传入你的所有python 方法作为tool，tool的描述会自动带给模型推理，tool的执行在ctx.completions.create 中会自动进行
    )
    # 添加tracing
    wrap_async_client_inplace_trace(ctx)
    # 和推理链路的diff
    wrap_async_client_inplace(ctx, proxy=proxy)
    await ctx.init()

    # 每次调用messages新增即可, ctx会帮忙维护history, 以及执行tool_call, 回填tool role(工具执行结果)
    # NOTE: 当模型输出错误的工具入参时，Arkitect会自动将报错作为一条tool role 返回模型让其自行修正。训练中可能无法获得有关错误入参的负样本，TODO: 提前返回reward 0 分
    _ = await ctx.completions.create(messages=messages, stream=False)
    await cleanup()
    return
```

**注意与修改事项：**

1. 更换LLM模型： 如果你有其他LLM模型，可以修改 Context 初始化时的 model 参数。
2. 定义和集成自定义工具：
   * 该代码中从 mcp_config.json 文件中加载工具，你可以创建自己的 Python 函数作为工具，并将其添加到 Context 的 tools 列表中，这些工具可以被LLM调用以执行特定任务。
   * 如果你的工具定义在 mcp_config.json 中，你需要修改该文件来添加、删除或修改工具。
3. 调整推理逻辑：demo_rollout 函数内部的逻辑可以根据你的需求进行修改。

**直接调用插件 API 的实现 (** **rollout.py )**
如果您需要集成自有服务 API，`rollout.py `提供的 Rollout 插件示例将对您具有很高的参考价值。该示例更贴近实际生产环境，展示了如何直接调用外部工具 API（而非通过 MCP 封装），并处理其结果。以下为您详细介绍该实现的核心特点：

1. **直接 API 调用**：以 web_search 工具为例，它通过 httpx 库直接向外部 API 服务发送 HTTP 请求，完成工具的执行。
2. **健壮性增强**：集成退避重试 (backoff) 机制，能够有效应对外部服务可能出现的瞬时故障；同时引入限流 (AsyncLimiter) 机制，确保对外部 API 的调用频率符合服务提供商的限制。
3. **精细的错误处理**：当工具调用失败时，会抛出 RolloutFailedException 异常，该异常与重试机制协同工作。若工具调用最终失败，Rollout 框架会将当前数据样本标记为失败并丢弃，这与返回特定失败状态码的效果一致。

更完整的直接调用 API 的实现细节和错误处理逻辑，请参考 `rollout.py `文件。
##### `llm_grader.py`：LLM 评分器
此demo的grader plugin函数为`llm_grader.py`文件，实现了一个基于大模型的评分器（llm as a judge），用于判断大模型给出的“预测答案”是否正确回答了用户提出的“问题”，并与一份“正确答案列表”进行比对。最终给出相应的得分。下方为代码中的关键部分说明，你可以根据场景需要进行对应的修改。

1. 关键部分:`@group_grader`装饰器，用于将`single_grader`函数标记 rollout plugin 函数类型，声明 plugin 相关元信息与运行时配置。提交任务时将根据plugin类型校验是否满足函数签名和强化学习pipeline是否完备。示例代码如下：

```Plain Text
@group_grader(
    name="llm_group_grader",
    description="llm_group_grader",
    runtime=Runtime(
        instance=PluginInstance.CPU1MEM2,
        max_concurrency=32,
        timeout=900,
    ),
)
```

2. 关键部分：`SYSTEM_PROMPT`，这是定义评估LLM行为和评估规则的核心。你可以完全重写修改它的评估标准和角色来适应任何评估场景。

```Python
SYSTEM_PROMPT = """你是一位严谨的内容评估专家。
你的核心任务是：依据提供的【问题】和一份【正确答案列表】（列表中的每个答案都被视为该问题的有效解答），来判断我提供的【预测答案】是否正确回答了该【问题】。你应该首先给出你的判断依据，然后给出你的判断结果（即“正确”或“错误”）。
判断标准如下：
1. 【预测答案】不需要与【正确答案列表】中的任何答案在字面上完全相同，但应该在语义上相同。
2. 【正确答案列表】中的每个答案都可以被视为问题的正确答案，【预测答案】应该至少与其中之一在语义上是相同的。
3. 你必须仔细阅读【问题】和【预测答案】，并仔细考虑【预测答案】是否真的正确回答了【问题】，不可以因为【预测答案】中包含了正确答案的字眼而认为【预测答案】正确。
4. 对于模型没有认真回答问题，只是通过列举很多答案骗分的情况，请你判断为错误。
"""
```

3. 关键部分：`USER_PROMPT_TEMPLATE`，定义了问题、正确答案列表和预测答案如何呈现给评估LLM。如果你的数据结构或评估需求有变化，可以调整这个模板。

```Python
USER_PROMPT_TEMPLATE = """
###问题###
{}
###正确答案列表###
{}
###预测答案###
{}
现在请开始你的判断：
"""
```

4. 关键部分：`single_garder`函数中的逻辑：
   * 输入准备： 如何从 sample 和 comp 中提取信息并格式化为 messages 发送给评估LLM。
   * 结果解析： 如何解析评估LLM返回的 eval_res 并从中提取 judgement 来决定 reward 。
   * 其中`response_format`中`json_schema`字段： 定义了评估LLM输出JSON的结构。如果你需要评估LLM输出除了 rationale 和 judgement 之外的其他字段，你需要修改这个 schema。

通过修改这些关键部分，你可以将 llm_grader.py 适配到各种不同的LLM答案评估场景中。
#### 测试plugin函数
在创建`plugin`函数后，为确保其功能符合预期，可通过**本地运行**和**在线运行**两种方式对其进行测试，详情可见[测试plugin函数](/docs/82379/1852871#a2d0bf5e)。

* **本地运行**：`draft_rollout_arkitect.py`与`rollout.py`文件中的`main` 函数展示了在本地对 `demo_rollout `与` llm_grader `结合使用进行测试。您可以参考该方法对修改后的plugin函数进行测试。
* **在线运行**：用户可采用 SDK 或 CLI 方式对`plugin`函数进行测试，将拉起在线运行环境进行测试，更接近训练任务。

#### 强化学习相关配置

* **精调参数修改**：您可打开`job.py`文件修改精调参数，配置要求详情可参考[配置精调参数](/docs/82379/1852871#1d0f46ba)。
* **可观测性配置**：对强化学习精调至关重要，强烈建议您在提交强化学习精调任务前完成配置，详情可参考[可观测性配置](/docs/82379/1852871#69596831)。

#### 提交强化学习任务
配置好所需的强化学习流程和plugin函数，即可提交精调任务开始强化学习训练。
```Python
#windows系统下设置环境变量的命令为：set ARK_API_KEY=your_api_key
export ARK_API_KEY=your_api_key
python job.py
```

####
