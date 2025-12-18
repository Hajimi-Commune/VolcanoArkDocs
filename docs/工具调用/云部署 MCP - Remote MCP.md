# 云部署 MCP / Remote MCP

**核心功能**
丰富MCP生态兼容：对接“[MCP MarketPlace](https://www.volcengine.com/mcp-marketplace)”，支持调用市场内各类垂直领域MCP工具（如巨量千川、知识库），无需自行开发工具逻辑。
多轮工具调用：复杂任务（如多步数据查询+分析）支持多轮MCP调用，上一轮工具输出结果自动作为下一轮模型输入（例：MCP数据查询→模型分析→二次补充查询）。
灵活混合调用：可与用户自定义Function、Web Search工具混合使用（如联合巨量千川MCP与Web Search实现“数据+热点”交叉分析），暂不限制工具组合数量。
说明
测试期间调用此工具需要增加 header '`ark-beta-mcp: true`'，调用方式请参见[快速开始](/docs/82379/1827534#d20c754a)。
前提条件
[获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey)
[开通模型服务](https://console.volcengine.com/ark/openManagement)
在 [模型列表](/docs/82379/1330310) 获取所需 Model ID
通过 Endpoint ID 调用模型服务，请参考 [获取 Endpoint ID（创建自定义推理接入点）](/docs/82379/1099522)。
**快速入门**
以下提供两种常用调用场景示例，需先替换 `<ARK_API_KEY>` 为实际密钥，`server_label`/`server_url` 为实际MCP Server 信息。
获取 MCP URL
您可以访问 [MCP MarketPlace](https://www.volcengine.com/mcp-marketplace)，选择想要使用的云部署 MCP / Remote MCP。
在 MCP 详情页，点击生成获取带授权的个人 MCP 服务端URL。
部分 MCP 通过`Authorization`字段来配置授权，详情请参考[2. 配置鉴权信息](/docs/82379/1827534#8cf42728)。
快速开始
**参数说明**
详情请参见 [创建Responses模型请求](https://www.volcengine.com/docs/82379/1569618)。
**支持模型列表**
参见[MCP 调用能力](/docs/82379/1330310#f449e5cb)。
模型调用需确保已完成MCP服务开通及对应模型权限申请，未开通用户无法触发MCP工具功能。
使用指南
本节介绍了常见的使用场景及其字段设置方法，以帮助您更高效地运用 MCP 工具。
1. 过滤 MCP 工具
一个 MCP 服务器可能提供多种工具。您可以在该服务的详情页查看完整的工具列表。在 API 调用时，您可以使用 `allowed_tools` 参数来指定要使用的工具，从而节省时间并获得更符合预期的结果。
2. 配置鉴权信息
目前部分 MCP 工具需要在header中配置身份验证。一般通过`Authorization`字段来配置，示例参考如下：
**最佳实践**
**多轮调用示例**
**场景**：调用巨量千川MCP分析直播数据，需用户确认后执行，支持多轮补充查询。
**注意事项**
目前云部署 MCP / Remote MCP 仅支持通过 Responses API 调用，同时**只支持通过 Streamable HTTP 链接的 MCP**。
**Function命名冲突**：若用户自定义 Function 与 MCP 内置工具（如`mcp_call`）重名，由模型自动判断调用优先级，无需额外配置。
**默认限流规则**：账号维度默认限流为`1000 RPM`（每分钟请求数），超出限流将导致请求失败，需调整可提交工单。
**错误信息**：暂不支持`caching` 参数，使用该参数会返回400错误信息。
**计费说明**
消耗模型基础Tokens，不收取MCP工具调用附加费。
相关参考
Responses API：[创建Responses模型请求](https://www.volcengine.com/docs/82379/1569618)
[联网搜索 Web Search](/docs/82379/1756990)
[MCP MarketPlace](https://www.volcengine.com/mcp-marketplace)
from volcenginesdkarkruntime import Ark
import os
api_key = os.getenv('ARK_API_KEY')
client = Ark(
base_url='https://ark.cn-beijing.volces.com/api/v3',
api_key=api_key
)
# 1. 首轮请求：发起MCP调用审批
first_response = client.responses.create(
model="doubao-seed-1-6-251015",
tools=[
{
"type": "mcp",
"server_label": "qianchuan-mcp",
"server_url": "https://mcp.qianchuan.com/mcp",
"require_approval": "always"
}
],
input=[
{
"role": "user",
"content": [{"type": "input_text", "text": "分析今日直播间竞价推广数据"}]
}
],
extra_headers={"ark-beta-mcp": "true"},
stream=True
)
# 提取审批ID与上一轮响应ID
last_chunk = None
for chunk in first_response:
last_chunk = chunk
previous_response_id = last_chunk.response.id
approval_id = last_chunk.response.output[-1].id
print(f"上一轮响应ID：{previous_response_id}，审批ID：{approval_id}")
# 2. 二轮请求：同意审批并继续执行
second_response = client.responses.create(
model="doubao-seed-1-6-251015",
tools=[
{
"type": "mcp",
"server_label": "qianchuan-mcp",
"server_url": "https://mcp.qianchuan.com/mcp",
"require_approval": "always"
}
],
input=[
{
"type": "mcp_approval_response",
"approve": True,  # 同意审批
"approval_request_id": approval_id
}
],
extra_headers={"ark-beta-mcp": "true"},
previous_response_id=previous_response_id,  # 关联上一轮请求
stream=True
)
# 打印二轮响应结果
for chunk in second_response:
if hasattr(chunk, 'delta'):
print(chunk.delta, end="", flush=True)
package com.example.ark;
import com.volcengine.ark.runtime.model.responses.constant.ResponsesConstants;
import com.volcengine.ark.runtime.model.responses.event.StreamEvent;
import com.volcengine.ark.runtime.model.responses.request.ResponsesInput;
import com.volcengine.ark.runtime.model.responses.request.CreateResponsesRequest;
import com.volcengine.ark.runtime.model.responses.tool.ToolMCP;
import com.volcengine.ark.runtime.model.responses.tool.mcp.MCPRequireApproval;
import com.volcengine.ark.runtime.model.responses.event.reasoningsummary.ReasoningSummaryTextDeltaEvent;
import com.volcengine.ark.runtime.model.responses.event.outputitem.OutputItemDoneEvent;
import com.volcengine.ark.runtime.model.responses.event.outputtext.OutputTextDeltaEvent;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;
import com.volcengine.ark.runtime.model.responses.common.ResponsesThinking;
import com.volcengine.ark.runtime.model.responses.event.response.ResponseCompletedEvent;
import java.time.Duration;
import java.util.Collections;
import java.util.concurrent.TimeUnit;
publicclassMCPToolCallExample {
// 固定使用的模型
privatestaticfinalStringmodel="doubao-seed-1-6-251015";
publicstaticvoidmain(String[] args) {
// 1. 校验鉴权凭证（必须配置 ARK_API_KEY 环境变量）
StringapiKey= System.getenv("ARK_API_KEY");
if (apiKey == null) {
System.out.println("ARK_API_KEY environment variable not set");
return;
// 2. 初始化 ArkService（配置连接池、并发、超时等）
ConnectionPoolconnectionPool=newConnectionPool(5, 1, TimeUnit.SECONDS);
Dispatcherdispatcher=newDispatcher();
dispatcher.setMaxRequests(5000);
dispatcher.setMaxRequestsPerHost(5000);
ArkServiceservice= ArkService.builder()
.dispatcher(dispatcher)
.timeout(Duration.ofHours(1))
.connectionPool(connectionPool)
.apiKey(apiKey)
.build();
// 3. 构造 MCP 工具调用请求
CreateResponsesRequestmcpRequest= CreateResponsesRequest.builder()
.model(model)
.stream(true) // 流式响应，实时获取结果
// 配置 MCP 工具：指定服务标签、服务地址、审批模式
.tools(Collections.singletonList(
ToolMCP.builder()
.serverLabel("deepwiki-test") // MCP 服务标签（需提前在平台注册）
.serverUrl("https://mcp.deepwiki.com/mcp") // MCP 服务接口地址
// 审批模式：NEVER（无需审批）、ALWAYS（必须审批）、AUTO（自动审批）
.requireApproval(MCPRequireApproval.builder()
.mode(ResponsesConstants.MCP_APPROVAL_MODE_NEVER)
.build())
.build()
))
// 输入查询指令：让 MCP 工具查询指定仓库结构
.input(ResponsesInput.builder()
.stringValue("查看这个 repo的structure expressjs/express")
.build())
// 启用模型思考过程（可在流式输出中查看）
.thinking(ResponsesThinking.builder()
.type(ResponsesConstants.THINKING_TYPE_ENABLED)
.build())
.build();
// 4. 执行 MCP 工具调用并处理流式响应
try {
System.out.println("----- [MCP Tool Call] 开始调用 -----");
service.streamResponse(mcpRequest)
.doOnError(Throwable::printStackTrace) // 异常处理
.blockingForEach(MCPToolCallExample::printStreamEvent); // 监听流式事件
} catch (Exception e) {
System.err.println("MCP Tool Call Error: " + e.getMessage());
// 5. 关闭服务 executor，释放资源
service.shutdownExecutor();
// 流式事件处理：打印思考过程、响应内容、工具调用状态
privatestaticvoidprintStreamEvent(StreamEvent event) {
// 打印思考过程增量（如模型分析如何调用 MCP 工具）
if (event instanceof ReasoningSummaryTextDeltaEvent) {
System.out.print("[思考过程] " + ((ReasoningSummaryTextDeltaEvent) event).getDelta());
// 打印文本响应增量（MCP 工具返回结果的实时输出）
if (event instanceof OutputTextDeltaEvent) {
System.out.print("[工具返回] " + ((OutputTextDeltaEvent) event).getDelta());
// 工具调用项结束标识
if (event instanceof OutputItemDoneEvent) {
System.out.println("\n[调用结束] 工具类型：" + ((OutputItemDoneEvent) event).getItem().getType() + "\n");
// 整体响应完成，打印调用用量
if (event instanceof ResponseCompletedEvent) {
System.out.println("[响应完成] 调用用量：" + ((ResponseCompletedEvent) event).getResponse().getUsage());
"tools": [
"server_label": "info-quest-mcp",
"server_url": "https://mcp.infoquest.bytepluses.com/mcp",  # 某MCP工具提供的MCP服务地址
"require_approval": "never",
"headers": {
"Authorization": "******"  # 替换为MCP工具提供的认证信息
]
import (
"context"
"fmt"
"github.com/volcengine/volcengine-go-sdk/service/arkruntime"
"github.com/volcengine/volcengine-go-sdk/service/arkruntime/model/responses"
"io"
"os"
/**
* Authentication
* If you authorize your endpoint using an API key, you can set your api key to environment variable "ARK_API_KEY"
* client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
* Note: If you use an API key, this API key will not be refreshed.
* To prevent the API from expiring and failing after some time, choose an API key with no expiration date.
*/
funcmain() {
stream()
nonStream()
funcnonStream() {
fmt.Println("non stream")
client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
ctx := context.Background()
fmt.Println("----- round 1 message -----")
// round 1 message
serverDescription := "test desc"
inputMessage := &responses.ItemInputMessage{
Role: responses.MessageRole_user,
Content: []*responses.ContentItem{
Union: &responses.ContentItem_Text{
Text: &responses.ContentItemText{
Type: responses.ContentItemType_input_text,
Text: "查一下 mark3labs/mcp-go 这个仓库的结构",
},
},
},
},
createResponsesReq := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_InputMessage{
InputMessage: inputMessage,
},
}}},
},
},
//Caching: &responses.ResponsesCaching{Type: responses.CacheType_enabled.Enum()},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
},
},
},
},
resp, err := client.CreateResponses(ctx, createResponsesReq, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
var responseId string
var approvalRequestId string
fmt.Printf("response: %v\n", resp)
responseId = resp.Id
approvalRequestId = resp.Output[len(resp.Output)-1].GetFunctionMcpApprovalRequest().GetId()
fmt.Println()
fmt.Println("-----round 2---------")
createResponsesReq2 := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_McpApprovalResponse{
McpApprovalResponse: &responses.ItemFunctionMcpApprovalResponse{
Type:              responses.ItemType_mcp_approval_response,
ApprovalRequestId: approvalRequestId,
Approve:           true,
},
},
}}},
},
},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
},
},
},
},
PreviousResponseId: &responseId,
resp2, err := client.CreateResponses(ctx, createResponsesReq2, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
fmt.Printf("response: %v\n", resp2)
fmt.Println("----- round 3 never require approval -----")
createResponsesReq3 := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_InputMessage{
InputMessage: inputMessage,
},
}}},
},
},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
RequireApproval: &responses.McpRequireApproval{
Union: &responses.McpRequireApproval_Mode{
Mode: responses.ApprovalMode_never,
},
},
},
},
},
},
resp3, err := client.CreateResponses(ctx, createResponsesReq3, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
fmt.Printf("response: %v\n", resp3)
funcstream() {
fmt.Println("stream")
client := arkruntime.NewClientWithApiKey(os.Getenv("ARK_API_KEY"))
ctx := context.Background()
fmt.Println("----- round 1 message -----")
// round 1 message
serverDescription := "test desc"
inputMessage := &responses.ItemInputMessage{
Role: responses.MessageRole_user,
Content: []*responses.ContentItem{
Union: &responses.ContentItem_Text{
Text: &responses.ContentItemText{
Type: responses.ContentItemType_input_text,
Text: "查一下 mark3labs/mcp-go 这个仓库的结构",
},
},
},
},
createResponsesReq := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_InputMessage{
InputMessage: inputMessage,
},
}}},
},
},
//Caching: &responses.ResponsesCaching{Type: responses.CacheType_enabled.Enum()},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
},
},
},
},
resp, err := client.CreateResponsesStream(ctx, createResponsesReq, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
var responseId string
var approvalRequestId string
for {
event, err := resp.Recv()
if err == io.EOF {
break
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
handleEvent(event)
if responseEvent := event.GetResponse(); responseEvent != nil {
responseId = responseEvent.GetResponse().GetId()
if event.GetItem().GetItem().GetFunctionMcpApprovalRequest() != nil {
approvalRequestId = event.GetItem().GetItem().GetFunctionMcpApprovalRequest().GetId()
fmt.Println()
fmt.Println("-----round 2---------")
createResponsesReq2 := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_McpApprovalResponse{
McpApprovalResponse: &responses.ItemFunctionMcpApprovalResponse{
Type:              responses.ItemType_mcp_approval_response,
ApprovalRequestId: approvalRequestId,
Approve:           true,
},
},
}}},
},
},
//Caching: &responses.ResponsesCaching{Type: responses.CacheType_enabled.Enum()},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
},
},
},
},
PreviousResponseId: &responseId,
resp2, err := client.CreateResponsesStream(ctx, createResponsesReq2, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
for {
event, err := resp2.Recv()
if err == io.EOF {
break
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
handleEvent(event)
fmt.Println("----- round 3 never require approval -----")
createResponsesReq3 := &responses.ResponsesRequest{
Model: "doubao-seed-1-6-251015",
Input: &responses.ResponsesInput{
Union: &responses.ResponsesInput_ListValue{
ListValue: &responses.InputItemList{ListValue: []*responses.InputItem{{
Union: &responses.InputItem_InputMessage{
InputMessage: inputMessage,
},
}}},
},
},
Tools: []*responses.ResponsesTool{
Union: &responses.ResponsesTool_ToolMcp{
ToolMcp: &responses.ToolMcp{
Type:              responses.ToolType_mcp,
ServerLabel:       "deepwiki",
ServerDescription: &serverDescription,
ServerUrl:         "https://mcp.deepwiki.com/mcp",
RequireApproval: &responses.McpRequireApproval{
Union: &responses.McpRequireApproval_Mode{
Mode: responses.ApprovalMode_never,
},
},
},
},
},
},
resp3, err := client.CreateResponsesStream(ctx, createResponsesReq3, arkruntime.WithCustomHeader("ark-beta-mcp", "true"))
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
for {
event, err := resp3.Recv()
if err == io.EOF {
break
if err != nil {
fmt.Printf("stream error: %v\n", err)
return
handleEvent(event)
funchandleEvent(event *responses.Event) {
switch event.GetEventType() {
case responses.EventType_response_reasoning_summary_text_delta.String():
print(event.GetReasoningText().GetDelta())
case responses.EventType_response_reasoning_summary_text_done.String(): // aggregated reasoning text
fmt.Printf("\naggregated reasoning text: %s\n", event.GetReasoningText().GetText())
case responses.EventType_response_output_text_delta.String():
print(event.GetText().GetDelta())
case responses.EventType_response_output_text_done.String(): // aggregated output text
fmt.Printf("\naggregated output text: %s\n", event.GetText().GetText())
case responses.EventType_response_output_item_done.String():
if event.GetItem().GetItem().GetFunctionMcpApprovalRequest() != nil {
fmt.Printf("\nmcp call: %s; arguments %s\n", event.GetItem().GetItem().GetFunctionMcpApprovalRequest().GetName(), event.GetItem().GetItem().GetFunctionMcpApprovalRequest().GetArguments())
case responses.EventType_response_mcp_call_arguments_delta.String():
fmt.Printf("\nmcp call arguments: %s\n", event.GetResponseMcpCallArgumentsDelta().GetDelta())
case responses.EventType_response_mcp_call_completed.String():
fmt.Printf("\nmcp call completed.\n")
case responses.EventType_response_mcp_list_tools_in_progress.String():
fmt.Printf("\nlisting mcp tools.\n")
case responses.EventType_response_mcp_list_tools_completed.String():
fmt.Printf("\nDone listing mcp tools.\n")
default:
return
curl --location 'https://ark.cn-beijing.volces.com/api/v3/responses' \\
--header "Authorization: Bearer <ARK_API_KEY>" \\
--header 'Content-Type: application/json' \\
--header 'ark-beta-mcp: true' \\
--data '{
"model": "doubao-seed-1-6-251015",
"stream": true,
"server_label": "deepwiki",
"server_url": "https://mcp.deepwiki.com/mcp",
"require_approval": "never"  # 无需审批，直接调用
"input": [
"content": [
"type": "input_text",
"text": "看一下volcengine/ai-app-lab这个repo的文档"
}'
from openai import OpenAI
# 从环境变量获取API密钥（配置方法：https://www.volcengine.com/docs/82379/1399008）
# 初始化客户端，启用MCP功能
client = OpenAI(
api_key=api_key,
default_headers={"ark-beta-mcp": "true"}
# 发起基础MCP调用请求
response = client.responses.create(
"require_approval": "never"
stream=True# 流式获取结果
# 打印响应结果
for chunk in response:
ifhasattr(chunk, 'delta'):
"server_label": "mcp_tools",
"server_url": "https://mcpserver/mcp",
"allowed_tools": {
"tool_names": [ # 列举需要使用的 MCP 工具
"tool1",
"tool2"
