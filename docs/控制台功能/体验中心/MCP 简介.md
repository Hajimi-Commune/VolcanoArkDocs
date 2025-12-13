# MCP 简介
## 什么是 MCP
MCP（Model Context Protocol，模型上下文协议）是由 Anthropic 公司提出并开源的一项标准协议框架，旨在标准化大型语言模型与外部数据源和工具之间的交互方式。通过 MCP，AI 应用可以以统一的方式访问本地和远程资源，实现更高效、灵活的集成。
MCP 被誉为 AI 应用的“TYPE-C 接口”，就像 TYPE-C 为设备连接外设提供了标准化方式一样，MCP 为 AI 模型连接不同的数据源和工具提供了标准化接口。
## 技术架构
MCP 采用模块化的客户端-服务器架构，解耦了 AI 助手与后端服务之间的关系。典型的 MCP 部署包括：

* **主机进程（Host）**：运行大模型的应用程序，如 Claude desktop、Cursor、方舟体验中心等。
* **MCP 客户端（Client）**：作为主机进程与服务器之间的抽象接口层，负责规范化通信并处理协议转换。
* **MCP 服务器（Server）**：通过标准化的 MCP 协议，向客户端提供特定的能力。

MCP 客户端通过 JSON-RPC 接口与服务器通信，支持多种传输机制，包括标准输入 / 输出（stdio）和流式传输 HTTP（Streamable HTTP）。
## 工作原理
当用户以自然语言提出请求时，MCP 的工作流程如下：

1. **请求解析**：主机进程识别用户意图，并将请求传递给 MCP 客户端。
2. **能力匹配**：客户端查询可用的 MCP 服务器，确定能够处理该请求的资源或工具。
3. **任务执行**：客户端向选定的服务器发送请求，服务器执行相应操作（查询数据库、调用 API 等）。
4. **结果返回**：服务器将结果返回给客户端，客户端将其传递给主机进程。
5. **响应生成**：主机进程整合上下文信息，生成自然语言响应呈现给用户。

整个过程对用户透明，提供了流畅的自然语言交互体验。
## 核心能力
### MCP 服务器提供的能力

*  **资源（Resources）**：提供静态或可查询的数据集，如文件、文档、数据库等。
*  **工具（Tools）**：暴露可调用的函数或 API，供 AI 模型执行操作。
*  **提示词（Prompts）**：提供结构化的提示模板，指导模型生成特定格式的响应。

### 客户端向服务器提供的能力

* **大模型调用（Sampling）**：支持服务器主动发起大模型请求，实现多轮对话、递归推理等复杂处理。
*  **文件系统访问（Root）**：向服务器暴露受限的文件系统视图，便于识别和访问可操作的目录与文件。

## 应用价值
MCP协议的价值在于通过标准化统一的接口，打破了大语言模型的能力边界，使用户能够通过自然语言交互方便地调用各类工具和资源，极大提高了人机交互的体验。更多的MCP技术原理可参考 [官方文档](https://docs.anthropic.com/zh-CN/docs/agents-and-tools/mcp)。
## 如何使用 MCP
### 体验中心快速体验
目前可在 [火山方舟体验中心](https://www.volcengine.com/experience/ark) 体验 MCP 能力，点击对话框下方的「MCP」按钮，即可体验 MCP 能力，其支持多模型、多MCP服务器及工具选择。
:::tip
为获得更佳的 Canvas 体验效果，建议使用深度思考模型：Doubao-1.5-thinking-vision-pro，Doubao-1.5-thinking-pro，DeepSeek-R1。
:::
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/67685a030d884481894080197779d4c3~tplv-goo7wpa0wc-image.image =695x)
在 MCP 服务器管理界面点击按钮即可开启服务，访问密钥已预置，如无预置密钥，需自行去服务提供商官网获取。目前已支持对象存储、数据湖等火山引擎 MCP 应用及飞常准，水滴信用等多个三方 MCP 应用，暂不支持自建MCP接入。更多应用正在开发和接入中，敬请期待。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/607db64eeb7140a890e6308e0f8fe1a0~tplv-goo7wpa0wc-image.image =695x)
下面是一个用 Doubao-1.5-thinking-pro 调用”飞常准-Aviation“MCP 服务，实现自然语言查询航班的应用案例 ：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6e2373343dfe4ba586f6bfd531127d82~tplv-goo7wpa0wc-image.image =655x)
### 
### 
##