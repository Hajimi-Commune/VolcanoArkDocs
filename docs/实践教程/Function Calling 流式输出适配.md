# Function Calling 流式输出适配
Doubao 1.5 升级了流式输出中的返回，使所有返回结果都能以流式输出，其中包括Function Calling的返回。如果您业务中已使用了Function Calling 流式输出，您可以调整对返回信息的处理方式，适配原代码处理逻辑。
# 升级前后对比
## 基本信息差异

| | | | \
|流式输出 |升级前 |升级后 |
|---|---|---|
| | | | \
|对应的模型名称 |`doubao-pro-**` |\
| |`doubao-lite-**` |`doubao-1.5-pro-**` |\
| | |`doubao-1.5-lite-**` |
| | | | \
|对应的模型版本 |241215 及之前 |250115及以后 |
| | | | \
|是否支持流式 |部分支持： |\
| | |\
| |* 支持：assistant的content流式返回。 |\
| |* 不支持：tool_calls 信息在一个chunk返回。 |是，支持`content`、`tool_calls` 字段内容流式返回。 |

## 响应示例差异
### 升级前
```Shell
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="从前", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-***",
    object="chat.completion.chunk",
)
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="xxxx", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-***",
    object="chat.completion.chunk",
)
...
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="\n",
                function_call=None,
                role="assistant",
                tool_calls=[
                    ChoiceDeltaToolCall(
                        index=0,
                        id="call_leigeybrw25cwlk87byg8l3v",
                        function=ChoiceDeltaToolCallFunction(
                            arguments='{"prompt": "有道词典", "region": "CN"}',
                            name="AppFinder",
                        ),
                        type="function",
                    )
                ],
            ),
            finish_reason="tool_calls",
            index=0,
        )
    ],
    created=1737884441,
    model="doubao-***",
    object="chat.completion.chunk",
)
```

### 升级后
```Shell
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="从前", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="xxxx", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
...
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
...
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="",
                function_call=None,
                role="assistant",
                tool_calls=[
                    ChoiceDeltaToolCall(
                        index=0,
                        id="call_05i***",
                        function=ChoiceDeltaToolCallFunction(
                            arguments="", name="AppFinder"
                        ),
                        type="function",
                    )
                ],
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
...
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="",
                function_call=None,
                role="assistant",
                tool_calls=[
                    ChoiceDeltaToolCall(
                        index=0,
                        id=None,
                        function=ChoiceDeltaToolCallFunction(arguments='"}', name=None),
                        type=None,
                    )
                ],
            ),
            finish_reason=None,
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
ChatCompletionChunk(
    id="0217***",
    choices=[
        Choice(
            delta=ChoiceDelta(
                content="", function_call=None, role="assistant", tool_calls=None
            ),
            finish_reason="tool_calls",
            index=0,
        )
    ],
    created=1737882725,
    model="doubao-1-5-***",
    object="chat.completion.chunk",
)
```

# 客户端代码适配示例
当您业务中使用之前版本的流式输出，且含有调用Function Calling，您希望切换到`doubao-1.5- *** 250115`及未来版本上来，您需对模型调用代码进行适当调整，以保障原业务代码正常运行。
以下是流式输出 Function Calling 升级前后的示例代码比较。
> 此代码不支持`n`字段。

| | | | \
|升级前后 |升级前 |升级后 |
|---|---|---|
| | | | \
|客户端处理代码示例 |```Python |\
| |import os |\
| |from volcenginesdkarkruntime import Ark |\
| |# 从环境变量中读取方舟API Key |\
| |client = Ark(api_key=os.environ.get("ARK_API_KEY")) |\
| |stream = client.chat.completions.create( |\
| |    model="ep-2025********", |\
| |    messages=[ |\
| |        { |\
| |            "role": "user", |\
| |            "content": "给我讲个故事，然后告诉我北京今天的天气", |\
| |        } |\
| |    ], |\
| |    tools=[ |\
| |        # 您要调用的工具信息 |\
| |        {...} |\
| |    ], |\
| |    stream=True, |\
| |) |\
| |tool_calls = [] |\
| |for chunk in stream: |\
| |    if not chunk.choices: |\
| |        continue |\
| |    print(chunk.choices[0].delta.content, end="") |\
| |    # 原先使用Function Call能力返回信息 |\
| |    if chunk.choices[0].delta.tool_calls: |\
| |        tool_calls.extend(chunk.choices[0].delta.tool_calls) |\
| | |\
| | |\
| | |\
| | |\
| |print("Tools: ", tool_calls) |\
| |``` |\
| | |```Python |\
| | |import os |\
| | |from volcenginesdkarkruntime import Ark |\
| | |# 从环境变量中读取方舟API Key |\
| | |client = Ark(api_key=os.environ.get("ARK_API_KEY")) |\
| | |stream = client.chat.completions.create( |\
| | |    model="ep-2025********", |\
| | |    messages=[ |\
| | |        { |\
| | |            "role": "user", |\
| | |            "content": "给我讲个故事，然后告诉我北京今天的天气", |\
| | |        } |\
| | |    ], |\
| | |    tools=[ |\
| | |        # 您要调用的工具信息 |\
| | |        {...} |\
| | |    ], |\
| | |    stream=True, |\
| | |) |\
| | |final_tool_calls = {} |\
| | |for chunk in stream: |\
| | |    if not chunk.choices: |\
| | |        continue |\
| | |    print(chunk.choices[0].delta.content, end="") |\
| | |    # 使用新版Function Call能力返回信息代码适配，将流式返回的信息拼装后返回 |\
| | |    for tool_call in chunk.choices[0].delta.tool_calls or []: |\
| | |        index = tool_call.index |\
| | |        if index not in final_tool_calls: |\
| | |            final_tool_calls[index] = tool_call |\
| | |        final_tool_calls[index].function.arguments += tool_call.function.arguments |\
| | | |\
| | |print("Tools: ", final_tool_calls) |\
| | |``` |\
| | | |\
| | | |