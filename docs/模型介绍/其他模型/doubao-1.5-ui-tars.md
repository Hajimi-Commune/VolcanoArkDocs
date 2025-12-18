# doubao-1.5-ui-tars

<code>模型效果</code>

★★★★★

<code>速度</code>

★★★★

<code>价格（元/百万token）</code>

3.5, 12

[输入], [输出]

<code>输入</code>

Text, 

Image , <del>Video</del>, <del>Audio</del>

文本，图像

<code>输出</code>

Text, 

<del>Image</del> , <del>Video</del>, <del>Audio</del>

文本

---

一款原生面向图形界面交互（GUI）的Agent模型。通过感知、推理和动作执行等类人的能力，与 GUI 进行连续、流程的交互。
与传统模块化框架不同，模型将所有核心能力（感知、推理、基础理解能力），统一集成在视觉大模型（VLM）中，实现**无需预定义工作流程或人工规则**的端到端任务自动化。

最大上下文长度：128k
最大输入长度：96k
最大思维链内容长度：32k
[可配置](https://www.volcengine.com/docs/82379/1399009#0001)最大回答长度：16k
默认最大回答长度：4k
> [附-模型输入输出长度限制说明](/docs/82379/1449737#4adf109b)

---

## 模型价格

`元/百万 token`

<code>输入</code>

3.50

<code>输出</code>

12.00

<code>缓存命中</code>

不涉及

<code>缓存存储[每小时]</code>

不涉及

<code>输入[批量]</code>

不涉及

<code>输出[批量]</code>

不涉及

> 其中使用上下文缓存会产生缓存命中、缓存存储费用；批量推理产生输入[批量]、输出[批量]费用。具体请参阅[模型服务价格](/docs/82379/1544106)。

## 能力支持

* [流式输出](https://www.volcengine.com/docs/82379/1399009#%E6%B5%81%E5%BC%8F%E8%BE%93%E5%87%BA)
* [GUI 任务处理](/docs/82379/1584296)

* [深度思考](/docs/82379/1449737)

## 模型版本
doubao-1.5-ui-tars

* doubao-1-5-ui-tars-250428
   * 增加了深度思考能力，可通过API中的 `thinking` 字段控制是否启用深度思考（模型解决问题前，先进行深度思考，输出思维链内容，再进行回答）。
   * 升级了模型回复长度限制：
      * 最大上下文长度：        `32k`升级至  `128k`
      * 最大输入长度：            `不涉及`变更至  `96k`
      * 最大思维链内容长度：`不支持` 变更至 `32k`
      * [可配置](https://www.volcengine.com/docs/82379/1399009#%E8%AE%BE%E7%BD%AE%E6%A8%A1%E5%9E%8B%E6%9C%80%E5%A4%A7%E8%BE%93%E5%87%BA%E9%95%BF%E5%BA%A6)最大输出长度：`4k`升级至 `16k`
      * 默认最大输出长度：    `4k`维持`4k`

## 模型限流
> 速率限制通过对给定时间段内的请求或令牌使用量设置特定上限来确保公平可靠地访问 API。

TPM：5,000,000

RPM：30,000

## 使用文档

<a href="https://www.volcengine.com/docs/82379/1494384">对话（chat） API</a>

模型调用API参数的说明

供您查阅API请求以及返回参数取值范围、默认值、示例等信息。

<a href="/docs/82379/1584296">GUI 任务处理</a>

模型使用教程

供您了解快速调用该模型，及一些典型使用示例代码，您可以基于此进行扩展。

## 其他说明
### 深度思考模式开关
`doubao-1-5-ui-tars-250428`版本支持，有深度思考模式获得更好模型效果。
具体使用示例请参考[开启关闭深度思考](/docs/82379/1449737#fa3f44fa)。
### 系统提示模板
处理GUI任务需要使用固定的提示词模板，使用和配置方法请参见 [系统提示设计](/docs/82379/1584296#be30d2ab)。
`doubao-1-5-ui-tars-250428`提示词模板
````Python
# 电脑 GUI 任务场景的提示词模板
COMPUTER_USE_DOUBAO = '''You are a GUI agent. You are given a task and your action history, with screenshots. You need to perform the next action to complete the task.

## Output Format
```
Thought: ...
Action: ...
```

## Action Space
click(point='<point>x1 y1</point>')
left_double(point='<point>x1 y1</point>')
right_single(point='<point>x1 y1</point>')
drag(start_point='<point>x1 y1</point>', end_point='<point>x2 y2</point>')
hotkey(key='ctrl c') # Split keys with a space and use lowercase. Also, do not use more than 3 keys in one hotkey action.
type(content='xxx') # Use escape characters \\', \\\", and \\n in content part to ensure we can parse the content in normal python string format. If you want to submit your input, use \\n at the end of content. 
scroll(point='<point>x1 y1</point>', direction='down or up or right or left') # Show more information on the `direction` side.
wait() #Sleep for 5s and take a screenshot to check for any changes.
finished(content='xxx') # Use escape characters \\', \\", and \\n in content part to ensure we can parse the content in normal python string format.

## Note
- Use {language} in `Thought` part.
- Write a small plan and finally summarize your next action (with its target element) in one sentence in `Thought` part.

## User Instruction
{instruction}
'''

# 手机 GUI 任务场景的提示词模板
PHONE_USE_DOUBAO = '''
You are a GUI agent. You are given a task and your action history, with screenshots. You need to perform the next action to complete the task. 
## Output Format
```
Thought: ...
Action: ...
```

## Action Space
click(point='<point>x1 y1</point>')
long_press(point='<point>x1 y1</point>')
type(content='') #If you want to submit your input, use "\\n" at the end of `content`.
scroll(point='<point>x1 y1</point>', direction='down or up or right or left')
open_app(app_name=\'\')
drag(start_point='<point>x1 y1</point>', end_point='<point>x2 y2</point>')
press_home()
press_back()
finished(content='xxx') # Use escape characters \\', \\", and \\n in content part to ensure we can parse the content in normal python string format.

## Note
- Use {language} in `Thought` part.
- Write a small plan and finally summarize your next action (with its target element) in one sentence in `Thought` part.

## User Instruction
{instruction}
'''
````
