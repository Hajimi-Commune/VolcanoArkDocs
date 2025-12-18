# 图像处理 Image Process

Image Process 图像处理工具支持通过 Responses API 调用对输入图片执行画点、画线、旋转、缩放、框选/裁剪关键区域等基础操作，适用于需模型通过视觉处理提升图片理解的场景（如图文内容分析、物体定位标注、多轮视觉推理等）。工具通过模型自动判断图像处理逻辑，支持与自定义 Function 混合使用，且可处理多轮视觉输入（上一轮输出图片作为下一轮输入）。
说明
当前处于 beta 测试阶段，测试期间免费使用。
测试期间调用此工具需要增加 header '`ark-beta-image-process: true`'，调用方式请参见[快速开始](/docs/82379/1798161#f444abc9)。
**核心功能**
丰富的图像处理工具：支持启用/禁用画点（POINT）、框选（GROUNDING）、缩放（ZOOM）、旋转（ROTATE）等子功能，满足不同视觉处理需求。
支持多轮图像处理：复杂视觉任务（如多步缩放+旋转）支持多轮工具调用，上一轮输出图片自动作为下一轮输入（例：image0→image1→image2）。
支持混合调用：可与用户自定义 Function 混合使用。（暂不支持与 联网搜索工具 Web Search 混合调用）
广泛的图片格式兼容：支持 Base64 编码的 .gif、.jpg、.jpeg 等主流图片格式，明确限制图片大小、像素、长宽比例（避免处理异常）。
前提条件
[获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey)
[开通模型服务](https://console.volcengine.com/ark/openManagement)
在 [模型列表](/docs/82379/1330310) 获取所需 Model ID
通过 Endpoint ID 调用模型服务，请参考 [获取 Endpoint ID（创建自定义推理接入点）](/docs/82379/1099522)。
**快速开始**
以下提供两种常用调用方式示例，需先替换 `<ARK_API_KEY>` 为实际密钥。
说明
提示词支持图片和文字混排，但图文顺序可能对模型的输出效果产生影响。在提示词构成为多张图片+1段文字，建议将文字放在提示词最后。
示例一：缩放（zoom）工具
以下代码展示了在视觉问答场景下，如何启用缩放（zoom）工具，让模型在放大图片后读取前方路牌文字，并以流式方式实时返回答案。
示例二：画点（point）工具
以下代码展示了如何启用画点（point）工具，让模型在视觉问答中进行计数，并以流式方式返回答案。
**参数说明**
详情请参见 [创建 Responses 模型请求](https://www.volcengine.com/docs/82379/1569618)。
**支持模型列表**
参见[图像处理工具](/docs/82379/1330310#7e79a172)。
**计费说明**
**公测期间**：暂时免费使用，无额外收费。
我们将会提前 2 周通过官方渠道告知用户具体收费标准，保障您的使用权益。
**注意事项**
**Function 命名冲突**：若用户自定义 Function 与 “image_process” 重名，由模型自行判断调用工具（无需额外配置）。
**图片规格限制**：输入图片需满足以下条件：
文件体积≤10MB、≤36000000像素、宽和高的长度>14像素、长宽比<150:1，超出规格将导致处理失败；
文件格式支持情况如下：
**支持** .gif，.jpg，.jpeg，.png，.webp，.bmp，.tiff，.ico， .icns， .jp2 格式。
**不支持** .dib、.sgi、.heic、.heif 格式。
**暂未支持功能**：
当前不支持与 Web Search 工具混合使用，也不支持通过 `tool_choice` 参数指定调用 image_process，后续将逐步上线。
暂不支持`caching` 参数，使用该参数会返回400错误信息。
**Tokens 消费提示**：多轮图像处理会增加 Tokens 消费（上一轮图片输入计入下一轮 Tokens），需注意调用成本。
package main
import (
"context"
"fmt"
"github.com/openai/openai-go/v3"
"github.com/openai/openai-go/v3/option"
"github.com/openai/openai-go/v3/responses"
)
funcmain() {
client: = openai.NewClient(option.WithBaseURL("https://ark.cn-beijing.volces.com/api/v3"))
ctx: = context.Background()
tools: = [] map[string] any {
{
"type": "image_process",
"point": {
"type": "enabled"// 启用画点（point）工具
},
"grounding": {
"type": "disabled"
},
"zoom": {
"type": "disabled"
},
"rotate": {
"type": "disabled"
}
}
}
stream: = client.Responses.NewStreaming(ctx, responses.ResponseNewParams {
Input: responses.ResponseNewParamsInputUnion {
OfInputItemList: [] responses.ResponseInputItemUnionParam {
{
OfInputMessage: & responses.ResponseInputItemMessageParam {
Role: string(responses.ResponseInputMessageItemRoleUser),
Content: [] responses.ResponseInputContentUnionParam {
{
OfInputImage: & responses.ResponseInputImageParam {
ImageURL: openai.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_2.jpg"),
},
}, {
OfInputText: & responses.ResponseInputTextParam {
Text: "数一数有多少颗草莓？",
},
},
},
},
},
}
},
Model: "doubao-seed-1-6-vision-250815",
}, option.WithJSONSet("tools", tools), option.WithHeaderAdd("ark-beta-image-process", "true"))
for stream.Next() {
data: = stream.Current()
fmt.Println(data.RawJSON())
}
if stream.Err() != nil {
panic(stream.Err())
}
}
"type": "enabled"// 启用缩放（zoom）工具
ImageURL: openai.String("https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_1.jpg"),
Text: "前方路牌写了什么？",
curl --location 'https://ark.cn-beijing.volces.com/api/v3/responses' \\
--header 'Authorization: Bearer <ARK_API_KEY>' \\
--header 'Content-Type: application/json' \\
--header 'ark-beta-image-process: true' \\
--data '{
"model": "doubao-seed-1-6-vision-250815",
"stream": true,
"tools": [
"type": "enabled"  // 启用 zoom（缩放）工具
],
"input": [
"type": "message",
"role": "user",
"content": [
"type": "input_image",
"image_url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_1.jpg"
"type": "input_text",
"text": "前方路牌写了什么？"
]
]
}'
from openai import OpenAI
import os
# 从环境变量获取API密钥，配置方法见：https://www.volcengine.com/docs/82379/1399008
api_key = os.getenv('ARK_API_KEY')
# 初始化客户端，配置API地址与工具启用头
client = OpenAI(
base_url='https://ark.cn-beijing.volces.com/api/v3',
api_key=api_key,
default_headers={"ark-beta-image-process": "true"}
# 发起图像处理请求
response = client.responses.create(
model="doubao-seed-1-6-vision-250815",
tools=[
"type": "enabled"# 启用缩放（zoom）工具
input=[
"image_url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_1.jpg"# 输入图片 URL
"text": "前方路牌写了什么？"# 系统提示文本
stream=True# 启用流式响应，实时获取处理结果
# 打印流式响应结果
for chunk in response:
ifhasattr(chunk, 'delta'):
print(chunk.delta, end="", flush=True)
"type": "enabled"# 启用画点（point）工具
"image_url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_2.jpg"# 输入图片 URL
"text": "数一数有多少颗草莓？"# 系统提示文本
"type": "enabled"  // 启用画点（point）工具
"image_url": "https://ark-project.tos-cn-beijing.volces.com/doc_image/image_process_2.jpg"
"text": "数一数有多少颗草莓？"
