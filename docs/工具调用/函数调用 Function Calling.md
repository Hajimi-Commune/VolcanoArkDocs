# 函数调用 Function Calling
## 功能概述
### 功能简介
函数调用（Function Calling）是一种将大模型与外部工具和 API 相连的关键功能，作为自然语言与信息接口之间的“翻译官”，它能够将用户的自然语言请求智能地转化为对特定工具或 API 的调用，从而高效满足用户的特定需求。

* **核心价值**：实现大模型与外部工具的无缝衔接，使大模型能够借助外部工具处理实时数据查询、任务执行等复杂场景，推动大模型在实际产业中的落地应用。
* **工作原理**：开发者通过自然语言向模型描述函数的功能和定义，模型在对话过程中自主判断是否需要调用函数。当需要调用时，模型会返回符合要求的工具函数及入参，开发者负责实际调用函数并将结果回填给模型，模型再根据结果进行总结或继续规划子任务。

### 适用场景
Function Calling 适用于以下需要大模型与外部工具协同的场景：

| | | | | \
|**场景分类** |**核心特征** |**核心价值** |**典型应用** |
|---|---|---|---|
| | | | | \
|实时数据交互场景 |需大模型与外部工具协同处理动态信息 |处理动态信息查询需求 |天气/股票/航班实时状态查询、数据库检索与 API 数据调用 |
| | | | | \
|任务自动化场景 |单次函数调用完成操作 |提升操作效率 |邮件/消息自动发送、设备控制指令执行（如智能家居开关） |
| | | | | \
|复杂流程编排场景 |多工具串并联调用 |跨工具参数传递、子任务依赖关系管理 |先查天气再发通知等需跨工具传递参数及管理子任务依赖的场景 |
| | | | | \
|智能系统集成场景 |与业务系统深度耦合 |实现系统智能化联动 |智能座舱多设备联动控制、企业级 Bot 工作流（如飞书会议创建→群组管理→任务生成） |

### 典型示例
用户：北京今天的天气如何？适合穿什么衣服？
模型思考：
  1. 需要调用天气查询工具获取实时数据（location=北京，unit=摄氏度）
  2. 天气信息包含温度、天气状况（晴/雨等），需结合数据给出穿衣建议
函数调用结果：
北京今天晴，气温 18-25℃，北风3级，湿度45%
模型回答：
北京今天天气晴朗，气温18-25℃，建议穿薄长袖衬衫或短袖T恤，搭配薄外套应对早晚温差。
### 工作原理图

<img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI4MDVweCIgaGVpZ2h0PSI0ODVweCIgdmlld0JveD0iLTAuNSAtMC41IDgwNSA0ODUiPjxkZWZzLz48Zz48cmVjdCB4PSIyIiB5PSIyIiB3aWR0aD0iODAwIiBoZWlnaHQ9IjQ4MCIgZmlsbD0iI2ZmZmZmZiIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMjIiIHk9IjIyIiB3aWR0aD0iMjQwIiBoZWlnaHQ9IjQ0MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtZGFzaGFycmF5PSIzIDMiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgZmxleC1zdGFydDsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMjM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjlweDsgbWFyZ2luLWxlZnQ6IDIzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxiPuW3peWFtzwvYj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjI4MiIgeT0iMjIiIHdpZHRoPSIyNDAiIGhlaWdodD0iNDQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1kYXNoYXJyYXk9IjMgMyIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBmbGV4LXN0YXJ0OyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAyMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyOXB4OyBtYXJnaW4tbGVmdDogMjgzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxiPueoi+W6jzwvYj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjU0MiIgeT0iMjIiIHdpZHRoPSIyNDAiIGhlaWdodD0iNDQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1kYXNoYXJyYXk9IjMgMyIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBmbGV4LXN0YXJ0OyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAyMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyOXB4OyBtYXJnaW4tbGVmdDogNTQzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxiPuaooeWeizwvYj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gNDAyIDEwMiBMIDQwMiAxMjIgTCA0MDIgMTAyIEwgNDAyIDExNS42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQwMiAxMjAuODggTCAzOTguNSAxMTMuODggTCA0MDIgMTE1LjYzIEwgNDA1LjUgMTEzLjg4IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMzIyIiB5PSI2MiIgd2lkdGg9IjE2MCIgaGVpZ2h0PSI0MCIgcng9IjIwIiByeT0iMjAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE1OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDgycHg7IG1hcmdpbi1sZWZ0OiAzMjNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+5pS25Yiw55So5oi36Zeu6aKYPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDQ4MiAxNDIgTCA1MzIgMTQyIEwgNTMyIDE2MiBMIDU3NS42MyAxNjIiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA1ODAuODggMTYyIEwgNTczLjg4IDE2NS41IEwgNTc1LjYzIDE2MiBMIDU3My44OCAxNTguNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMyMiIgeT0iMTIyIiB3aWR0aD0iMTYwIiBoZWlnaHQ9IjQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxNThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxNDJweDsgbWFyZ2luLWxlZnQ6IDMyM3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj7lj5HotbfmqKHlnovosIPnlKg8YnIgLz5Ub29scyDlrZfmrrUr5Y6f5aeL55So5oi36Zeu6aKYPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDY2MiAxODIgTCA2NjIgMjAyIEwgNjYyIDE4MiBMIDY2MiAxOTUuNjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA2NjIgMjAwLjg4IEwgNjU4LjUgMTkzLjg4IEwgNjYyIDE5NS42MyBMIDY2NS41IDE5My44OCBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gNzQyIDE2MiBMIDc2MiAxNjIgTCA3NjIgMzgyIEwgNzQ4LjM3IDM4MiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDc0My4xMiAzODIgTCA3NTAuMTIgMzc4LjUgTCA3NDguMzcgMzgyIEwgNzUwLjEyIDM4NS41IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHBhdGggZD0iTSA2NjIgMTQyIEwgNzQyIDE2MiBMIDY2MiAxODIgTCA1ODIgMTYyIFoiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTU4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTYycHg7IG1hcmdpbi1sZWZ0OiA1ODNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+5piv5ZCm6LCD55So5bel5YW3PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDU4MiAyMjIgTCA1MzIgMjIyIEwgNTMyIDI0MiBMIDQ4OC4zNyAyNDIiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODMuMTIgMjQyIEwgNDkwLjEyIDIzOC41IEwgNDg4LjM3IDI0MiBMIDQ5MC4xMiAyNDUuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjU4MiIgeT0iMjAyIiB3aWR0aD0iMTYwIiBoZWlnaHQ9IjQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxNThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyMjJweDsgbWFyZ2luLWxlZnQ6IDU4M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj7ovpPlh7rlt6XlhbflkI3np7Dku6Xlj4rlj4LmlbA8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjY2MiIgeT0iMTgzIiB3aWR0aD0iNDAiIGhlaWdodD0iMjAiIGZpbGw9Im5vbmUiIHN0cm9rZT0ibm9uZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDFweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxOTNweDsgbWFyZ2luLWxlZnQ6IDY4MnB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm93cmFwOyAiPuaYrzwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHBhdGggZD0iTSA2NjIgNDAyIEwgNjYyIDQyMiBMIDQ4OC4zNyA0MjIiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODMuMTIgNDIyIEwgNDkwLjEyIDQxOC41IEwgNDg4LjM3IDQyMiBMIDQ5MC4xMiA0MjUuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjU4MiIgeT0iMzYyIiB3aWR0aD0iMTYwIiBoZWlnaHQ9IjQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxNThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAzODJweDsgbWFyZ2luLWxlZnQ6IDU4M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj7nlJ/miJDpl67popjlm57nrZQ8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gMzIyIDI0MiBMIDI3MiAyNDIgTCAyNzIgMjYyIEwgMjI4LjM3IDI2MiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDIyMy4xMiAyNjIgTCAyMzAuMTIgMjU4LjUgTCAyMjguMzcgMjYyIEwgMjMwLjEyIDI2NS41IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMzIyIiB5PSIyMjIiIHdpZHRoPSIxNjAiIGhlaWdodD0iNDAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE1OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDI0MnB4OyBtYXJnaW4tbGVmdDogMzIzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPuino+aekOi/lOWbnuS/oeaBrzxiciAvPuWPkei1t+W3peWFt+iwg+eUqDwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHBhdGggZD0iTSAxNDIgMjgyIEwgMTQyIDMwMiBMIDMxNS42MyAzMDIiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSAzMjAuODggMzAyIEwgMzEzLjg4IDMwNS41IEwgMzE1LjYzIDMwMiBMIDMxMy44OCAyOTguNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjYyIiB5PSIyNDIiIHdpZHRoPSIxNjAiIGhlaWdodD0iNDAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE1OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDI2MnB4OyBtYXJnaW4tbGVmdDogNjNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+5a6M5oiQ6K+35rGCPGJyIC8+6L+U5Zue5L+h5oGvPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDQ4MiAzMDIgTCA1MzIgMzAyIEwgNTMyIDMyMiBMIDU3NS42MyAzMjIiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA1ODAuODggMzIyIEwgNTczLjg4IDMyNS41IEwgNTc1LjYzIDMyMiBMIDU3My44OCAzMTguNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMyMiIgeT0iMjgyIiB3aWR0aD0iMTYwIiBoZWlnaHQ9IjQwIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxNThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAzMDJweDsgbWFyZ2luLWxlZnQ6IDMyM3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj7lj5HotbfmqKHlnovosIPnlKg8YnIgLz7ljp/lp4vnlKjmiLfpl67popgr5bel5YW36L+U5Zue57uT5p6cPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cmVjdCB4PSIzMjIiIHk9IjQwMiIgd2lkdGg9IjE2MCIgaGVpZ2h0PSI0MCIgcng9IjIwIiByeT0iMjAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE1OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDQyMnB4OyBtYXJnaW4tbGVmdDogMzIzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPui/lOWbnuaooeWei+WbnuetlDwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHBhdGggZD0iTSAyMDYgMTIyIEwgMjcyIDEyMiBMIDI3MiAxNDIgTCAzMTUuNjMgMTQyIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzIwLjg4IDE0MiBMIDMxMy44OCAxNDUuNSBMIDMxNS42MyAxNDIgTCAzMTMuODggMTM4LjUgWiIgZmlsbD0iIzAwMDAwMCIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cGF0aCBkPSJNIDYyIDE0MiBMIDk0IDEwMiBMIDIyMiAxMDIgTCAxOTAgMTQyIFoiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTU4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTIycHg7IG1hcmdpbi1sZWZ0OiA2M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj7lt6Xlhbfkv6Hmga88L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gNjYyIDMwMiBMIDY2MiAyNDguMzciIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA2NjIgMjQzLjEyIEwgNjY1LjUgMjUwLjEyIEwgNjYyIDI0OC4zNyBMIDY1OC41IDI1MC4xMiBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gNzQyIDMyMiBMIDc2MiAzMjIgTCA3NjIgMzgyIEwgNzQ4LjM3IDM4MiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDc0My4xMiAzODIgTCA3NTAuMTIgMzc4LjUgTCA3NDguMzcgMzgyIEwgNzUwLjEyIDM4NS41IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHBhdGggZD0iTSA2NjIgMzAyIEwgNzQyIDMyMiBMIDY2MiAzNDIgTCA1ODIgMzIyIFoiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTU4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzIycHg7IG1hcmdpbi1sZWZ0OiA1ODNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+5piv5ZCm6LCD55So5bel5YW3PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cmVjdCB4PSI2NjIiIHk9IjI2MiIgd2lkdGg9IjQwIiBoZWlnaHQ9IjIwIiBmaWxsPSJub25lIiBzdHJva2U9Im5vbmUiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxcHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjcycHg7IG1hcmdpbi1sZWZ0OiA2ODJweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vd3JhcDsgIj7mmK88L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjczMiIgeT0iMTQyIiB3aWR0aD0iNDAiIGhlaWdodD0iMjAiIGZpbGw9Im5vbmUiIHN0cm9rZT0ibm9uZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDFweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxNTJweDsgbWFyZ2luLWxlZnQ6IDc1MnB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm93cmFwOyAiPuWQpjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHJlY3QgeD0iNzIyIiB5PSIyOTIiIHdpZHRoPSI0MCIgaGVpZ2h0PSIyMCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMXB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDMwMnB4OyBtYXJnaW4tbGVmdDogNzQycHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3dyYXA7ICI+5ZCmPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48L2c+PC9zdmc+" from="flow-chart" payload="{"data":{"mxGraphModel":{"dx":"976","dy":"609","grid":"1","gridSize":"10","guides":"1","tooltips":"1","connect":"1","arrows":"1","fold":"1","page":"1","pageScale":"1","pageWidth":"827","pageHeight":"1169"},"mxCellMap":{"qjiNImIL":{"id":"qjiNImIL"},"JshK4DVV":{"id":"JshK4DVV","parent":"qjiNImIL"},"lsuU37Ty":{"id":"lsuU37Ty","value":"","style":"rounded=0;whiteSpace=wrap;html=1;strokeColor=none;","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","parent":"JshK4DVV","-0-mxGeometry":{"x":"40","y":"100","width":"800","height":"480","as":"geometry"}},"ALwC5ZBF":{"id":"ALwC5ZBF","value":"<b>工具</b>","style":"rounded=0;whiteSpace=wrap;html=1;verticalAlign=top;fillColor=none;dashed=1;","parent":"JshK4DVV","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","-0-mxGeometry":{"x":"60","y":"120","width":"240","height":"440","as":"geometry"}},"JJvTNGGv":{"id":"JJvTNGGv","value":"<b>程序</b>","style":"rounded=0;whiteSpace=wrap;html=1;verticalAlign=top;fillColor=none;dashed=1;","parent":"JshK4DVV","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","-0-mxGeometry":{"x":"320","y":"120","width":"240","height":"440","as":"geometry"}},"HerNl4Am":{"id":"HerNl4Am","value":"<b>模型</b>","style":"rounded=0;whiteSpace=wrap;html=1;verticalAlign=top;fillColor=none;dashed=1;","parent":"JshK4DVV","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","-0-mxGeometry":{"x":"580","y":"120","width":"240","height":"440","as":"geometry"}},"AzImFBqq":{"id":"AzImFBqq","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"JshK4DVV","source":"1tnUQpLf","target":"a4ttJ9li","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"1tnUQpLf":{"id":"1tnUQpLf","value":"收到用户问题","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=50;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"360","y":"160","width":"160","height":"40","as":"geometry"}},"Rz0Vnx00":{"id":"Rz0Vnx00","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0;entryY=0.5;entryDx=0;entryDy=0;exitX=1;exitY=0.5;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"a4ttJ9li","target":"kQtnq2G9","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry","-0-mxPoint":{"x":"620","y":"260","as":"targetPoint"},"-1-Array":{"as":"points","-0-mxPoint":{"x":"570","y":"240"},"-1-mxPoint":{"x":"570","y":"260"}}}},"a4ttJ9li":{"id":"a4ttJ9li","value":"发起模型调用<br />Tools 字段+原始用户问题","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"360","y":"220","width":"160","height":"40","as":"geometry"}},"ipVMooqw":{"id":"ipVMooqw","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0.5;entryY=0;entryDx=0;entryDy=0;","parent":"JshK4DVV","source":"kQtnq2G9","target":"V80viOhd","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry","-0-mxPoint":{"x":"700","y":"400","as":"targetPoint"}}},"egs1h4iP":{"id":"egs1h4iP","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=1;entryY=0.5;entryDx=0;entryDy=0;exitX=1;exitY=0.5;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"kQtnq2G9","target":"SnIyUNHO","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"kQtnq2G9":{"id":"kQtnq2G9","value":"是否调用工具","style":"rhombus;whiteSpace=wrap;html=1;fillColor=none;","parent":"JshK4DVV","vertex":"1","diagramName":"Diamond","diagramCategory":"general","-0-mxGeometry":{"x":"620","y":"240","width":"160","height":"40","as":"geometry"}},"3fGUT9DD":{"id":"3fGUT9DD","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=1;entryY=0.5;entryDx=0;entryDy=0;","parent":"JshK4DVV","source":"V80viOhd","target":"jc1g9pHF","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"V80viOhd":{"id":"V80viOhd","value":"输出工具名称以及参数","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"620","y":"300","width":"160","height":"40","as":"geometry"}},"adkWaAlq":{"id":"adkWaAlq","value":"是","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;","parent":"JshK4DVV","vertex":"1","-0-mxGeometry":{"x":"700","y":"281","width":"40","height":"20","as":"geometry"}},"txJ7EXgl":{"id":"txJ7EXgl","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=1;entryY=0.5;entryDx=0;entryDy=0;exitX=0.5;exitY=1;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"SnIyUNHO","target":"jqaGcsh4","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"SnIyUNHO":{"id":"SnIyUNHO","value":"生成问题回答","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"620","y":"460","width":"160","height":"40","as":"geometry"}},"dcianNJl":{"id":"dcianNJl","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=1;entryY=0.5;entryDx=0;entryDy=0;","parent":"JshK4DVV","source":"jc1g9pHF","target":"yump7Jj5","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"jc1g9pHF":{"id":"jc1g9pHF","value":"解析返回信息<br />发起工具调用","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"360","y":"320","width":"160","height":"40","as":"geometry"}},"QRs31Xv7":{"id":"QRs31Xv7","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0;entryY=0.5;entryDx=0;entryDy=0;exitX=0.5;exitY=1;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"yump7Jj5","target":"MikdKlim","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"yump7Jj5":{"id":"yump7Jj5","value":"完成请求<br />返回信息","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"100","y":"340","width":"160","height":"40","as":"geometry"}},"pvM7y1Qi":{"id":"pvM7y1Qi","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0;entryY=0.5;entryDx=0;entryDy=0;exitX=1;exitY=0.5;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"MikdKlim","target":"P1lA33A0","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"MikdKlim":{"id":"MikdKlim","value":"发起模型调用<br />原始用户问题+工具返回结果","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=0;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"360","y":"380","width":"160","height":"40","as":"geometry"}},"jqaGcsh4":{"id":"jqaGcsh4","value":"返回模型回答","style":"rounded=1;whiteSpace=wrap;html=1;fillColor=none;arcSize=50;","parent":"JshK4DVV","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"360","y":"500","width":"160","height":"40","as":"geometry"}},"pbEfD5m8":{"id":"pbEfD5m8","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0;entryY=0.5;entryDx=0;entryDy=0;","parent":"JshK4DVV","source":"xunMzPAw","target":"a4ttJ9li","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"xunMzPAw":{"id":"xunMzPAw","value":"工具信息","style":"shape=parallelogram;perimeter=parallelogramPerimeter;whiteSpace=wrap;html=1;fillColor=none;","parent":"JshK4DVV","vertex":"1","diagramName":"Parallelogram","diagramCategory":"general","-0-mxGeometry":{"x":"100","y":"200","width":"160","height":"40","as":"geometry"}},"6p0yqPY1":{"id":"6p0yqPY1","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=0.5;entryY=1;entryDx=0;entryDy=0;","parent":"JshK4DVV","source":"P1lA33A0","target":"V80viOhd","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"jyyqNEWR":{"id":"jyyqNEWR","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;entryX=1;entryY=0.5;entryDx=0;entryDy=0;exitX=1;exitY=0.5;exitDx=0;exitDy=0;","parent":"JshK4DVV","source":"P1lA33A0","target":"SnIyUNHO","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"P1lA33A0":{"id":"P1lA33A0","value":"是否调用工具","style":"rhombus;whiteSpace=wrap;html=1;fillColor=none;","parent":"JshK4DVV","vertex":"1","diagramName":"Diamond","diagramCategory":"general","-0-mxGeometry":{"x":"620","y":"400","width":"160","height":"40","as":"geometry"}},"hBl2D1xZ":{"id":"hBl2D1xZ","value":"是","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;","parent":"JshK4DVV","vertex":"1","-0-mxGeometry":{"x":"700","y":"360","width":"40","height":"20","as":"geometry"}},"de50YbNh":{"id":"de50YbNh","value":"否","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;","parent":"JshK4DVV","vertex":"1","-0-mxGeometry":{"x":"770","y":"240","width":"40","height":"20","as":"geometry"}},"8DogO6fS":{"id":"8DogO6fS","value":"否","style":"text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;","parent":"JshK4DVV","vertex":"1","-0-mxGeometry":{"x":"760","y":"390","width":"40","height":"20","as":"geometry"}}},"mxCellList":["qjiNImIL","JshK4DVV","lsuU37Ty","ALwC5ZBF","JJvTNGGv","HerNl4Am","AzImFBqq","1tnUQpLf","Rz0Vnx00","a4ttJ9li","ipVMooqw","egs1h4iP","kQtnq2G9","3fGUT9DD","V80viOhd","adkWaAlq","txJ7EXgl","SnIyUNHO","dcianNJl","jc1g9pHF","QRs31Xv7","yump7Jj5","pvM7y1Qi","MikdKlim","jqaGcsh4","pbEfD5m8","xunMzPAw","6p0yqPY1","jyyqNEWR","P1lA33A0","hBl2D1xZ","de50YbNh","8DogO6fS"]},"lastEditTime":0,"snapshot":""}" />

## 支持模型
全量支持函数调用的模型，请参见[函数调用能力](/docs/82379/1330310#6e3339cf)。
基于效果/准确性和时延的权衡原则，推荐选型建议如下：

1. **效果优先型（高准确性，时延较高）**
* **doubao-seed-1-6-thinking-250615** ：强制开启思考模式，不可关闭，专注于 **Coding、Math 和逻辑推理** 任务，在基础能力上显著优于前代模型（如 doubao-1-5-thinking-pro），适用于高准确性要求的复杂场景。
* **doubao-seed-1-6-250615** ：默认开启深度思考模式（支持 auto/thinking/non-thinking），具有自适应思考机制（根据问题难度自动开关思考），适用于通用场景，尤其在 **非思考模式下效果大幅提升** 。
* **deepseek-v3-250324**：在**数学推理**和**代码生成**场景超越GPT-4.5，支持HTML前端任务的高可用性代码生成。
* **deepseek-r1-250528**：通过强化学习优化参数生成，在**多步骤函数调用**场景中表现优异，需配合系统提示干预参数幻觉。（参考文末FAQ部分）
2. **时延优先型（快速响应，效果相对宽松）**
* **doubao-seed-1-6-flash-250615** ：适用于延迟敏感场景（如实时控制或高速调用），文本理解能力超上一代 lite 模型，且视觉理解比肩旗舰模型。
* **doubao-1-5-lite-32k-250115**：平均响应时延**降低40%** ，适用于智能客服、实时控制等场景，支持并行函数调用。

## 使用流程
### 环境准备
调用方舟模型前，需先进行权限配置获取API Key并配置到环境变量。若通过火山引擎官方SDK或OpenAI SDK调用，需提前安装对应SDK，方舟已提供Go、Python、Java的SDK，便于快速集成模型服务。
环境配置参考:
* [获取 API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey) 
* [开通模型服务](https://console.volcengine.com/ark/openManagement)
* 在 [模型列表](/docs/82379/1330310) 获取所需 Model ID 
   * 通过 Endpoint ID 调用模型服务，请参考 [获取 Endpoint ID（创建自定义推理接入点）](/docs/82379/1099522)。

* [安装及升级 SDK](/docs/82379/1541595)。

### 基本使用流程
#### 步骤 1：定义函数
通过 `tools` 字段向模型描述可用函数，支持 JSON 格式，包含函数名称、描述、参数定义等信息。
**定义工具函数**
```Python
def get_current_weather(location, unit="摄氏度"):
    # 实际调用天气查询 API 的逻辑
    # 此处为示例，返回模拟的天气数据
    return f"{location}今天天气晴朗，温度 25 {unit}。"
```

* 代码中定义了一个名为 `get_current_weather` 的工具函数，用于获取指定地点的天气信息。
   * `location`：必需的参数，表示地点。
   * `unit`：可选参数，默认值为 `摄氏度`，表示温度单位。
* 函数内部目前只是返回模拟的天气数据，实际应用中需要调用真实的天气查询 API。

**定义 Tools**
```Python
{
  "type": "function",
  "function": {
    "name": "get_current_weather",
    "description": "获取指定地点的天气信息，支持摄氏度和华氏度两种单位",
    "parameters": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "地点的位置信息，例如北京、上海"
        },
        "unit": {
          "type": "string",
          "enum": ["摄氏度", "华氏度"],
          "description": "温度单位，可选值为摄氏度或华氏度"
        }
      },
      "required": ["location"]
    }
  }
}
```

* `tools` 是一个列表，其中每个元素代表一个工具。这里定义了一个名为 `get_current_weather` 的函数。
* `type`：工具的类型，这里是 `function`，表示这是一个函数调用工具。
* `function`：包含函数的详细信息，如名称、描述和参数。
   * `name`：函数的名称，即 `get_current_weather`。
   * `description`：函数的描述，说明该函数用于获取指定地点的天气信息。
   * `parameters`：函数所需的参数，这里是一个对象，包含 `location` 和 `unit` 两个属性。
      * `location`：地点的位置信息，是一个字符串类型的参数。
      * `unit`：温度单位，是一个字符串类型的参数，可选值为 `摄氏度` 或 `华氏度`。
      * `required`：指定必需的参数，这里只有 `location` 是必需的。

更多关于函数 构造相关的规范和注意事项，请参见 [附1：工具参数构造规范](/docs/82379/1262342#4d571c97)。
#### 步骤 2：发起模型请求
在请求中包含用户问题和函数定义，模型会根据需求返回需要调用的函数及参数。
```Python
from volcenginesdkarkruntime import Ark

# 从环境变量中获取您的API KEY，配置方法见：https://www.volcengine.com/docs/82379/1399008
api_key = os.getenv('ARK_API_KEY')
# 初始化Ark客户端
client = Ark(
    api_key = api_key,
    base_url="https://ark.cn-beijing.volces.com/api/v3"
)

# 用户问题 
messages = [
    {"role": "user", "content": "北京今天的天气如何？"}
]
tools = [
    {
        # 参见步骤1中定义的tools
    }
]
# 发起模型请求
completion = client.chat.completions.create(
    # 替换 <MODEL> 为模型的Model ID
    model="<MODEL>",
    messages=messages,
    tools=tools
)
```

#### 步骤 3：调用外部函数
根据模型返回的函数名称和参数，调用对应的外部函数或 API，获取函数执行结果。
```Python
# 解析模型返回的函数调用信息
tool_call = completion.choices[0].message.tool_calls[0]
# 函数名称
tool_name = tool_call.function.name
# 如果判断需要调用查询天气函数，则运行查询天气函数
if tool_name == "get_current_weather":
    # 提取的用户参数
    arguments = json.loads(tool_call.function.arguments)
    # 调用函数
    tool_result = get_current_weather(**arguments)
```

* `tool_calls`：获取模型调用的工具列表。
* 如果工具函数名称是 `get_current_weather`，则解析函数调用的参数并调用 `get_current_weather` 函数获取工具执行结果。

#### 步骤 4：回填结果并获取最终回复
将工具执行结果以 `role=tool` 的消息形式回填给模型，模型根据结果生成最终回复。
```Python
messages.append(completion.choices[0].message)
messages.append({
    "role": "tool",
    "tool_call_id": tool_call.id,
    "content": tool_result
})

# 再次调用模型获取最终回复
final_completion = client.chat.completions.create(
    model="doubao-1-5-pro-32k-250115",
    messages=messages
)

print(final_completion.choices[0].message.content)
```

### 完整代码示例

**Python - Arkiteck SDK**

##### （推荐）使用方舟智能体SDK Arkitect
```Python
from arkitect.core.component.context.context import Context
from enum import Enum
import asyncio
from pydantic import Field
def get_current_weather(location: str = Field(description="地点的位置信息，例如北京、上海"), unit: str=Field(description="温度单位, 可选值为摄氏度或华氏度")):
    """
    获取指定地点的天气信息
    """
    return f"{location}今天天气晴朗，温度 25 {unit}。"
async def chat_with_tool():
    ctx = Context(
            model="doubao-1-5-pro-32k-250115",
            tools=[
                get_current_weather
            ],  # 直接在这个list里传入你的所有python 方法作为tool，tool的描述会自动带给模型推理，tool的执行在ctx.completions.create 中会自动进行
        )
    await ctx.init()
    completion = await ctx.completions.create(messages=[
        {"role": "user", "content": "北京和上海今天的天气如何？"}
    ],stream =False)
    return completion
completion = asyncio.run(chat_with_tool())
print(completion.choices[0].message.content)
```

**Python - Ark SDK**

##### 方舟基础SDK
```Python
from volcenginesdkarkruntime import Ark
from volcenginesdkarkruntime.types.chat import ChatCompletion
import json
client = Ark()
messages = [
    {"role": "user", "content": "北京和上海今天的天气如何？"}
]
# 步骤1: 定义工具
tools = [{
  "type": "function",
  "function": {
    "name": "get_current_weather",
    "description": "获取指定地点的天气信息",
    "parameters": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "地点的位置信息，例如北京、上海"
        },
        "unit": {
          "type": "string",
          "enum": ["摄氏度", "华氏度"],
          "description": "温度单位"
        }
      },
      "required": ["location"]
    }
  }
}]
def get_current_weather(location: str, unit="摄氏度"):
    # 实际调用天气查询 API 的逻辑
    # 此处为示例，返回模拟的天气数据
    return f"{location}今天天气晴朗，温度 25 {unit}。"
while True:
    # 步骤2: 发起模型请求，由于模型在收到工具执行结果后仍然可能有函数调用意愿，因此需要多次请求
    completion: ChatCompletion = client.chat.completions.create(
    model="doubao-1-5-pro-32k-250115",
    messages=messages,
    tools=tools
    )
    resp_msg = completion.choices[0].message
    # 展示模型中间过程的回复内容
    print(resp_msg.content)
    if completion.choices[0].finish_reason != "tool_calls":
        # 模型最终总结，没有调用工具意愿
        break
    messages.append(completion.choices[0].message.model_dump())
    tool_calls = completion.choices[0].message.tool_calls
    for tool_call in tool_calls:
        tool_name = tool_call.function.name
        if tool_name == "get_current_weather":
            # 步骤 3：调用外部工具
            args = json.loads(tool_call.function.arguments)
            tool_result = get_current_weather(**args)
            # 步骤 4：回填工具结果，并获取模型总结回复
            messages.append(
                {"role": "tool", "content": tool_result, "tool_call_id": tool_call.id}
            )
```

**Java**

```Java
package com.ark.sample;

import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.service.ArkService;

import java.util.*;

public class VolcEngineFunctionCallChat {

    // 用于解析 get_current_weather 函数参数的类
    public static class WeatherArgs {
        @JsonProperty("location")
        private String location;

        @JsonProperty("unit")
        private String unit;

        // Jackson 需要默认构造函数
        public WeatherArgs() {
        }

        public String getLocation() {
            return location;
        }

        public void setLocation(String location) {
            this.location = location;
        }

        public String getUnit() {
            return unit;
        }

        public void setUnit(String unit) {
            this.unit = unit;
        }
    }

    // 用于定义工具函数参数模式的类 (类似于Python中的parameters字典结构)
    public static class FunctionParameterSchema {
        public String type;
        public Map<String, Object> properties;
        public List<String> required;

        public FunctionParameterSchema(String type, Map<String, Object> properties, List<String> required) {
            this.type = type;
            this.properties = properties;
            this.required = required;
        }

        public String getType() {
            return type;
        }

        public Map<String, Object> getProperties() {
            return properties;
        }

        public List<String> getRequired() {
            return required;
        }
    }

    private static final ObjectMapper objectMapper = new ObjectMapper();

    // 工具函数实现: get_current_weather
    public static String getCurrentWeather(String location, String unit) {
        // 此处应为实际调用天气查询 API 的逻辑
        // 这里是示例，返回模拟的天气数据
        String currentUnit = (unit == null || unit.isEmpty()) ? "摄氏度" : unit;
        System.out.println(String.format("调用工具 get_current_weather: location=%s, unit=%s", location, currentUnit));
        return String.format("%s今天天气晴朗，温度 25 %s。", location, currentUnit);
    }

    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");

        if (apiKey == null || apiKey.isEmpty()) {
            System.err.println("错误: ARK_API_KEY 环境变量未设置。");
            return;
        }

        ArkService service = ArkService.builder()
                .apiKey(apiKey)
                .build();

        List<ChatMessage> messages = new ArrayList<>();
        messages.add(ChatMessage.builder().role(ChatMessageRole.USER).content("北京和上海今天的天气如何？").build());

        // 步骤 1: 定义工具
        Map<String, Object> locationProperty = new HashMap<>();
        locationProperty.put("type", "string");
        locationProperty.put("description", "地点的位置信息，例如上海，北京");

        Map<String, Object> unitProperty = new HashMap<>();
        unitProperty.put("type", "string");
        unitProperty.put("enum", Arrays.asList("摄氏度", "华氏度"));
        unitProperty.put("description", "温度单位");

        Map<String, Object> schemaProperties = new HashMap<>();
        schemaProperties.put("location", locationProperty);
        schemaProperties.put("unit", unitProperty);

        FunctionParameterSchema functionParams = new FunctionParameterSchema(
                "object",
                schemaProperties,
                Collections.singletonList("location") // 'location' 是必需参数
        );

        List<ChatTool> tools = Collections.singletonList(
                new ChatTool(
                        "function", // 工具类型
                        new ChatFunction.Builder()
                                .name("get_current_weather")
                                .description("获取指定地点的天气信息")
                                .parameters(functionParams) // 工具函数的参数模式
                                .build()));

        String modelId = "doubao-1-5-pro-32k-250115";

        while (true) {
            // 步骤 2: 发起模型请求
            ChatCompletionRequest request = ChatCompletionRequest.builder()
                    .model(modelId)
                    .messages(messages)
                    .tools(tools)
                    .build();

            ChatCompletionResult completionResult;
            try {
                completionResult = service.createChatCompletion(request);
            } catch (Exception e) {
                System.err.println("调用 Ark API 时发生错误: " + e.getMessage());
                e.printStackTrace();
                break;
            }

            if (completionResult == null || completionResult.getChoices() == null
                    || completionResult.getChoices().isEmpty()) {
                System.err.println("从模型收到空的或无效的响应。");
                break;
            }

            ChatCompletionChoice choice = completionResult.getChoices().get(0);
            ChatMessage responseMessage = choice.getMessage();

            // 展示模型中间过程的回复内容
            System.out.println("模型回复: " + responseMessage.stringContent());

            // 将模型的回复（含函数调用请求）添加到消息历史中
            messages.add(responseMessage);
            if (choice.getFinishReason() == null || !"tool_calls".equalsIgnoreCase(choice.getFinishReason())) {
                // 模型最终总结，没有调用工具意愿，或者发生错误等其他终止原因
                break;
            }

            List<ChatToolCall> toolCalls = responseMessage.getToolCalls();
            if (toolCalls == null || toolCalls.isEmpty()) {
                // 如果 finish_reason 是 "tool_calls"，但 toolCalls 为空，这可能是一个异常情况
                System.err.println("警告: Finish reason 是 'tool_calls' 但未在消息中找到 tool_calls。");
                break;
            }

            for (ChatToolCall toolCall : toolCalls) {
                String toolName = toolCall.getFunction().getName();
                if ("get_current_weather".equals(toolName)) {
                    // 步骤 3：调用外部工具
                    String argumentsJson = toolCall.getFunction().getArguments();
                    WeatherArgs tool_args;
                    try {
                        tool_args = objectMapper.readValue(argumentsJson, WeatherArgs.class);
                    } catch (JsonProcessingException e) {
                        System.err.println("解析 get_current_weather 参数时出错: " + argumentsJson + " - " + e.getMessage());
                        // 将错误信息作为工具结果回填
                        messages.add(ChatMessage.builder()
                                .role(ChatMessageRole.TOOL)
                                .content("解析参数时出错: " + e.getMessage())
                                .toolCallId(toolCall.getId())
                                .build());
                        continue;
                    }

                    String toolResult = getCurrentWeather(tool_args.getLocation(), tool_args.getUnit());
                    System.out.println("工具执行结果 (" + toolCall.getId() + "): " + toolResult);

                    // 步骤 4：回填工具结果，并获取模型总结回复
                    messages.add(ChatMessage.builder()
                            .role(ChatMessageRole.TOOL)
                            .content(toolResult)
                            .toolCallId(toolCall.getId()) // 关联函数调用 ID
                            .build());
                }
            }
        }

        service.shutdownExecutor();
        System.out.println("会话结束。");
    }
}
```

**Golang**

```Go
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "os"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/volcengine"
)

type WeatherArgs struct {
    Location string \`json:"location"\`
    Unit     string \`json:"unit,omitempty"\` // omitempty 允许 unit 为可选
}

// 目标工具
func getCurrentWeather(location string, unit string) string {
    if unit == "" {
        unit = "摄氏度" // 默认单位
    }
    // 此处为示例，返回模拟的天气数据
    return fmt.Sprintf("%s今天天气晴朗，温度 25 %s。", location, unit)
}

func main() {
    // 从环境变量中获取 API Key，请确保已设置 ARK_API_KEY
    apiKey := os.Getenv("ARK_API_KEY")
    if apiKey == "" {
        fmt.Println("错误：请设置 ARK_API_KEY 环境变量。")
        return
    }

    client := arkruntime.NewClientWithApiKey(
        apiKey,
    )

    ctx := context.Background()

    // 初始化消息列表
    messages := []*model.ChatCompletionMessage{
        {
            Role: model.ChatMessageRoleUser,
            Content: &model.ChatCompletionMessageContent{
                StringValue: volcengine.String("北京和上海今天的天气如何？"),
            },
        },
    }

    // 步骤 1: 定义工具
    tools := []*model.Tool{
        {
            Type: model.ToolTypeFunction,
            Function: &model.FunctionDefinition{
                Name:        "get_current_weather",
                Description: "获取指定地点的天气信息",
                Parameters: map[string]interface{}{
                    "type": "object",
                    "properties": map[string]interface{}{
                        "location": map[string]interface{}{
                            "type":        "string",
                            "description": "地点的位置信息，例如北京、上海",
                        },
                        "unit": map[string]interface{}{
                            "type": "string",
                            "enum": []string{
                                "摄氏度",
                                "华氏度",
                            },
                            "description": "温度单位",
                        },
                    },
                    "required": []string{"location"},
                },
            },
        },
    }

    for {
        // 步骤 2: 发起模型请求
        req := model.CreateChatCompletionRequest{
            Model:    "doubao-1-5-pro-32k-250115", // 与 Python 版本一致的模型
            Messages: messages,
            Tools:    tools,
        }

        resp, err := client.CreateChatCompletion(ctx, req)
        if err != nil {
            fmt.Printf("模型请求错误: %v
", err)
            return
        }

        if len(resp.Choices) == 0 {
            fmt.Println("模型未返回任何 choice。")
            return
        }

        respMsg := resp.Choices[0].Message

        // 展示模型中间过程的回复内容 (如果存在)
        if respMsg.Content.StringValue != nil && *respMsg.Content.StringValue != "" {
            fmt.Println("模型回复:", *respMsg.Content.StringValue)
        }

        if resp.Choices[0].FinishReason != model.FinishReasonToolCalls || len(respMsg.ToolCalls) == 0 {
            break
        }

        // 将模型的回复（包含函数调用请求）添加到消息历史中
        messages = append(messages, &respMsg)

        for _, toolCall := range respMsg.ToolCalls {
            fmt.Printf("模型尝试调用工具: %s, ID: %s
", toolCall.Function.Name, toolCall.ID)
            fmt.Println("  参数:", toolCall.Function.Arguments)

            var toolResult string
            if toolCall.Function.Name == "get_current_weather" {
                // 步骤 3：调用外部工具
                var args WeatherArgs
                err := json.Unmarshal([]byte(toolCall.Function.Arguments), &args)
                if err != nil {
                    fmt.Printf("解析工具参数错误 (%s): %v
", toolCall.Function.Name, err)
                    toolResult = fmt.Sprintf("解析参数失败: %v", err)
                } else {
                    toolResult = getCurrentWeather(args.Location, args.Unit)
                    fmt.Println("  工具执行结果:", toolResult)
                }

                // 步骤 4：回填工具结果
                messages = append(messages, &model.ChatCompletionMessage{
                    Role:       model.ChatMessageRoleTool,
                    Content:    &model.ChatCompletionMessageContent{StringValue: volcengine.String(toolResult)},
                    ToolCallID: toolCall.ID,
                })
            }
        }
        fmt.Println("--- 下一轮对话 ---")
    }
}
```

## 推荐配置与优化
### 新业务接入 

1. 推荐优选`doubao-seed-1.6`系列模型。 
   1. FC场景下，关闭 thinking 会提高效率，具体操作请参见[开启关闭深度思考](/docs/82379/1449737#fa3f44fa)。
   2. 对速度有较高要求，可以选择 `doubao-seed-1-6-flash-******`模型。
2. 准备评测集，在FC模型测试，看看准确率，以及业务预期上线准确率。 
3. 常规调优手段： 
   1. Functions的params、description等字段准确填写。 System prompt中不用再重复介绍函数，（可选）可以描述在何种情况下调用某函数 
   * 优化函数、参数的描述， 明确不同函数的边界情况； 避免歧义；增加示例等。 
   * 对模型进行sft（建议至少50条数据，模型越多、参数越多、情况越多，则所需要的数据越多），见[精调优化](/docs/82379/1262342#f118e5a2)。  
4. 速度优化： 对于简单无歧义的函数或参数，适当精简输入输出内容。 

### prompt最佳实践 
 原则： Treat LLM as a kid 

1. 能不用大模型完成的任务，就不要调用大模型，尽量代码完成。 
2. 和任务无关的信息，避免输入，避免信息干扰。 

| | | | | \
|类别  |问题  |错误示例  |改正后示例  |
|---|---|---|---|
| | | | | \
|函数  |命名不规范、描述不规范  |```Go |\
| | |{ |\
| | |   "type": "function", |\
| | |    "function": { |\
| | |        "name": "GPT1", |\
| | |        "description": "新建日程", |\
| | |     } |\
| | |} |\
| | |``` |\
| | | |```Go |\
| | | |{ |\
| | | |   "type": "function", |\
| | | |    "function": { |\
| | | |        "name": "CreateEvent", |\
| | | |        "description": "当需要为用户新建日程时，此工具将创建日程，并返回日程ID", |\
| | | |     } |\
| | | |} |\
| | | |``` |\
| | | | |
|^^| | | | \
| |  |  |  |
| | | | | \
|参数  |\
|  |避免不必要的复杂格式（或嵌套）  |\
| |  |```Go |\
| | |{ |\
| | |    "time": { |\
| | |        "type": "object", |\
| | |        "description": "事件时间", |\
| | |        "properties": { |\
| | |            "timestamp": { |\
| | |                "description": "事件时间" |\
| | |            } |\
| | |        } |\
| | |    } |\
| | |} |\
| | |``` |\
| | | |```Go |\
| | | |{ |\
| | | |    "time": { |\
| | | |        "type": "string", |\
| | | |        "description": "事件时间", |\
| | | |    } |\
| | | |} |\
| | | |``` |\
| | | | |\
| | | |  |
|^^| | | | \
| |避免固定值  |\
| |  |```Go |\
| | |{ |\
| | |    "time": { |\
| | |        "type": "object", |\
| | |        "description": "事件时间", |\
| | |        "properties": { |\
| | |            "timestamp": { |\
| | |                "description": "固定传2024-01-01即可" |\
| | |            } |\
| | |        } |\
| | |    } |\
| | |} |\
| | |``` |\
| | | |既然参数值固定，删去该参数，由代码处理。  |
| | | | | \
|业务流程  |\
|  |尽量缩短LLM调用轮次  |\
| |  |System prompt:  |\
| | |```Go |\
| | |你正在与用户Alan沟通，你需要先查询用户ID，再通过ID创建日程…… |\
| | |``` |\
| | | |System prompt:  |\
| | | |```Go |\
| | | |你正在与用户Alan（ID=abc123）沟通，你可以通过ID创建日程…… |\
| | | |``` |\
| | | | |
|^^| | | | \
| |歧义消解  |System prompt:  |\
| | |```Go |\
| | |可以通过ID查找用户，并获得用户的日程ID |\
| | |``` |\
| | | |\
| | |这里两个ID未明确，模型可能会混用  |System prompt:  |\
| | | |```Go |\
| | | |每个用户具有唯一的用户ID；每个日程有对应的日程ID，两者独立的ID。 |\
| | | |可以通过用户ID查找用户，并获得用户的所有日程ID |\
| | | |``` |\
| | | | |\
| | | |  |

### 函数调用异常处理
JSON 格式容错机制：对于轻微不合法的 JSON 格式，可尝试使用 `json-repair` 库进行容错修复。
```Python
import json_repair

invalid_json = '{"location": "北京", "unit": "摄氏度"}'
valid_json = json_repair.loads(invalid_json)
```

### 需求澄清
需求澄清（确认需求），不依赖与FC，可独立使用。
可在 System prompt 中加入：
```Python
如果用户没有提供足够的信息来调用函数，请继续提问以确保收集到了足够的信息。
在调用函数之前，你必须总结用户的描述并向用户提供总结，询问他们是否需要进行任何修改。
......
```

在函数的 description 中加入：
```Python
函数参数除了提取a和b， 还应要求用户提供c、d、e、f和其他相关细节。
```

或在系统提示中加入参数校验逻辑，当模型生成的参数缺失时，引导模型重新生成完整的参数。
```Python
如果用户提供的信息缺少工具所需要的必填参数，你需要进一步追问让用户提供更多信息。
```

### 流式输出适配 
从 Doubao-1.5 系列模型开始支持流式输出，逐步获取函数调用信息，提升响应效率。
详见[Function Calling 流式输出适配](/docs/82379/1449722)。
```Python
def function_calling_stream():
    completion = client.chat.completions.create(
        model="doubao-1-5-pro-32k-250115",
        messages=messages,
        tools=tools,
        stream=True
    )
    for chunk in completion:
        if chunk.choices[0].delta.tool_calls:
            tool_call = chunk.choices[0].delta.tool_calls[0]
            print(f"名称：{tool_call.function.name}，参数：{tool_call.function.arguments}")

function_calling_stream()
```

### 多轮函数调用
当用户需求需要多次调用工具函数时，维护对话历史上下文，逐轮处理函数调用和结果回填。
#### 示例流程

1. **用户提问**：“查询北京的天气，并将结果发送给张三”。
2. **第一轮**：模型调用 `get_current_weather` 工具获取北京天气。
3. **第二轮**：模型调用 `send_message` 工具将天气结果发送给张三。
4. **第三轮**：模型总结任务完成情况，返回最终回复。

#### 代码示例 
:::tip
多轮Function Calling：指用户query需要多次调用工具函数和大模型才能完成的情况， 是多轮对话的子集。 
:::
调用Response细节图： 
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/71d9c354a2db4bd8a1959f43043cb944~tplv-goo7wpa0wc-image.image =2018x)

##### Golang 
```Go
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "os"
    "strings"

    "github.com/volcengine/volcengine-go-sdk/service/arkruntime"
    "github.com/volcengine/volcengine-go-sdk/service/arkruntime/model"
    "github.com/volcengine/volcengine-go-sdk/byteplus"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
       os.Getenv("ARK_API_KEY"),
       arkruntime.WithBaseUrl("${BASE_URL}"),
    )

    fmt.Println("----- function call multiple rounds request -----")
    ctx := context.Background()
    // Step 1: send the conversation and available functions to the model
    req := model.CreateChatCompletionRequest{
       Model: "${Model_ID}",
       Messages: []*model.ChatCompletionMessage{
          {
             Role: model.ChatMessageRoleSystem,
             Content: &model.ChatCompletionMessageContent{
                StringValue: volcengine.String("你是豆包，是由字节跳动开发的 AI 人工智能助手"),
             },
          },
          {
             Role: model.ChatMessageRoleUser,
             Content: &model.ChatCompletionMessageContent{
                StringValue: volcengine.String("上海的天气怎么样？"),
             },
          },
       },
       Tools: []*model.Tool{
          {
             Type: model.ToolTypeFunction,
             Function: &model.FunctionDefinition{
                Name:        "get_current_weather",
                Description: "Get the current weather in a given location",
                Parameters: map[string]interface{}{
                   "type": "object",
                   "properties": map[string]interface{}{
                      "location": map[string]interface{}{
                         "type":        "string",
                         "description": "The city and state, e.g. Beijing",
                      },
                      "unit": map[string]interface{}{
                         "type":        "string",
                         "description": "枚举值有celsius、fahrenheit",
                      },
                   },
                   "required": []string{
                      "location",
                   },
                },
             },
          },
       },
    }
    resp, err := client.CreateChatCompletion(ctx, req)
    if err != nil {
       fmt.Printf("chat error: %v\n", err)
       return
    }
    // extend conversation with assistant's reply
    req.Messages = append(req.Messages, &resp.Choices[0].Message)
    
    // Step 2: check if the model wanted to call a function.
    // The model can choose to call one or more functions; if so,
    // the content will be a stringified JSON object adhering to
    // your custom schema (note: the model may hallucinate parameters).
    for _, toolCall := range resp.Choices[0].Message.ToolCalls {
       fmt.Println("calling function")
       fmt.Println("    id:", toolCall.ID)
       fmt.Println("    name:", toolCall.Function.Name)
       fmt.Println("    argument:", toolCall.Function.Arguments)
       functionResponse, err := CallAvailableFunctions(toolCall.Function.Name, toolCall.Function.Arguments)
       if err != nil {
          functionResponse = err.Error()
       }
       // extend conversation with function response
       req.Messages = append(req.Messages,
          &model.ChatCompletionMessage{
             Role:       model.ChatMessageRoleTool,
             ToolCallID: toolCall.ID,
             Content: &model.ChatCompletionMessageContent{
                StringValue: &functionResponse,
             },
          },
       )
    }
    // get a new response from the model where it can see the function response
    secondResp, err := client.CreateChatCompletion(ctx, req)
    if err != nil {
       fmt.Printf("second chat error: %v\n", err)
       return
    }
    fmt.Println("conversation", MustMarshal(req.Messages))
    fmt.Println("new message", MustMarshal(secondResp.Choices[0].Message))
}
func CallAvailableFunctions(name, arguments string) (string, error) {
    if name == "get_current_weather" {
       params := struct {
          Location string `json:"location"`
          Unit     string `json:"unit"`
       }{}
       if err := json.Unmarshal([]byte(arguments), ¶ms); err != nil {
          return "", fmt.Errorf("failed to parse function call name=%s arguments=%s", name, arguments)
       }
       return GetCurrentWeather(params.Location, params.Unit), nil
    } else {
       return "", fmt.Errorf("got unavailable function name=%s arguments=%s", name, arguments)
    }
}

// GetCurrentWeather get the current weather in a given location.
// Example dummy function hard coded to return the same weather.
// In production, this could be your backend API or an external API
func GetCurrentWeather(location, unit string) string {
    if unit == "" {
       unit = "celsius"
    }
    switch strings.ToLower(location) {
    case "beijing":
       return `{"location": "Beijing", "temperature": "10", "unit": unit}`
    case "北京":
       return `{"location": "Beijing", "temperature": "10", "unit": unit}`
    case "shanghai":
       return `{"location": "Shanghai", "temperature": "23", "unit": unit}`
    case "上海":
       return `{"location": "Shanghai", "temperature": "23", "unit": unit}`
    default:
       return fmt.Sprintf(`{"location": %s, "temperature": "unknown"}`, location)
    }
}
func MustMarshal(v interface{}) string {
    b, _ := json.Marshal(v)
    return string(b)
}
```

##### Python 
```Python
from volcenginesdkarkruntime import Ark
import time
client = Ark(
    base_url="${BASE_URL}",
)

print("----- function call multiple rounds request -----")
messages = [
    {
        "role": "system",
        "content": "你是豆包，是由字节跳动开发的 AI 人工智能助手",
    },
    {
        "role": "user",
        "content": "北京今天的天气",
    },
]
req = {
    "model": "${YOUR_ENDPOINT_ID}",
    "messages": messages,
    "temperature": 0.8,
    "tools": [
        {
            "type": "function",
            "function": {
                "name": "MusicPlayer",
                "description": """歌曲查询Plugin，当用户需要搜索某个歌手或者歌曲时使用此plugin，给定歌手，歌名等特征返回相关音乐。\n 例子1：query=想听孙燕姿的遇见， 输出{"artist":"孙燕姿","song_name":"遇见","description":""}""",
                "parameters": {
                    "properties": {
                        "artist": {"description": "表示歌手名字", "type": "string"},
                        "description": {
                            "description": "表示描述信息",
                            "type": "string",
                        },
                        "song_name": {
                            "description": "表示歌曲名字",
                            "type": "string",
                        },
                    },
                    "required": [],
                    "type": "object",
                },
            },
        },
        {
            "type": "function",
            "function": {
                "name": "get_current_weather",
                "description": "",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "地理位置，比如北京市",
                        },
                        "unit": {"type": "string", "description": "枚举值 [摄氏度,华氏度]"},
                    },
                    "required": ["location"],
                },
            },
        },
    ],
}

ts = time.time()
completion = client.chat.completions.create(**req)
if completion.choices[0].message.tool_calls:
    print(
        f"Bot [{time.time() - ts:.3f} s][Use FC]: ",
        completion.choices[0].message.tool_calls[0],
    )
    # ========== 补充函数调用的结果 =========
    req["messages"].extend(
        [
            completion.choices[0].message.dict(),
             {
                "role": "tool",
                "tool_call_id": completion.choices[0].message.tool_calls[0].id,
                "content": "北京天气晴，24~30度",  # 根据实际调用函数结果填写，最好用自然语言。
                "name": completion.choices[0].message.tool_calls[0].function.name,
            },
        ]
    )
    # 再请求一次模型，获得总结。 如不需要，也可以省略
    ts = time.time()
    completion = client.chat.completions.create(**req)
    print(
        f"Bot [{time.time() - ts:.3f} s][FC Summary]: ",
        completion.choices[0].message.content,
    )
```

##### Java 
```Java
package com.volcengine.ark.runtime;

com.volcengine.ark.runtime

import com.volcengine.ark.runtime.model.completion.chat.*;
import com.volcengine.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.*;
import java.util.concurrent.TimeUnit;

public class FunctionCallChatCompletionsExample {
    static String apiKey = System.getenv("ARK_API_KEY");
    static ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
    static Dispatcher dispatcher = new Dispatcher();
    static ArkService service = ArkService.builder().dispatcher(dispatcher).connectionPool(connectionPool).baseUrl("${BASE_URL}").apiKey(apiKey).build();

    public static void main(String[] args) {
        System.out.println("\n----- function call multiple rounds request -----");
        final List<ChatMessage> messages = new ArrayList<>();
        final ChatMessage userMessage = ChatMessage.builder().role(ChatMessageRole.USER).content("北京今天天气如何？").build();
        messages.add(userMessage);

        final List<ChatTool> tools = Arrays.asList(
                new ChatTool(
                        "function",
                        new ChatFunction.Builder()
                                .name("get_current_weather")
                                .description("获取给定地点的天气")
                                .parameters(new Weather(
                                        "object",
                                        new HashMap<String, Object>() {{
                                            put("location", new HashMap<String, String>() {{
                                                put("type", "string");
                                                put("description", "T地点的位置信息，比如北京");
                                            }});
                                            put("unit", new HashMap<String, Object>() {{
                                                put("type", "string");
                                                put("description", "枚举值有celsius、fahrenheit");
                                            }});
                                        }},
                                        Collections.singletonList("location")
                                ))
                                .build()
                )
        );

        ChatCompletionRequest chatCompletionRequest = ChatCompletionRequest.builder()
                .model("${YOUR_ENDPOINT_ID}")
                .messages(messages)
                .tools(tools)
                .build();

        ChatCompletionChoice choice = service.createChatCompletion(chatCompletionRequest).getChoices().get(0);
        messages.add(choice.getMessage());
        choice.getMessage().getToolCalls().forEach(
                toolCall -> {
                messages.add(ChatMessage.builder().role(ChatMessageRole.TOOL).toolCallId(toolCall.getId()).content("北京天气晴，24~30度").name(toolCall.getFunction().getName()).build());
        });
        ChatCompletionRequest chatCompletionRequest2 = ChatCompletionRequest.builder()
                .model("${YOUR_ENDPOINT_ID}")
                .messages(messages)
                .build();

        service.createChatCompletion(chatCompletionRequest2).getChoices().forEach(System.out::println);

        // shutdown service
        service.shutdownExecutor();
    }

    public static class Weather {
        public String type;
        public Map<String, Object> properties;
        public List<String> required;

        public Weather(String type, Map<String, Object> properties, List<String> required) {
            this.type = type;
            this.properties = properties;
            this.required = required;
        }

        public String getType() {
            return type;
        }

        public void setType(String type) {
            this.type = type;
        }

        public Map<String, Object> getProperties() {
            return properties;
        }

        public void setProperties(Map<String, Object> properties) {
            this.properties = properties;
        }

        public List<String> getRequired() {
            return required;
        }

        public void setRequired(List<String> required) {
            this.required = required;
        }
    }

}
```

#### 响应示例 
```Python
========== Round 1 ==========
user: 先查询北京的天气，如果是晴天微信发给Alan，否则发给Peter...

assistant [FC Response]:
name=GetCurrentWeather, args={"location": "\u5317\u4eac"} 
[elpase=2.607 s]
========== Round 2 ==========
tool: 北京今天20~24度，天气：阵雨。...

assistant [FC Response]:
name=SendMessage, args={"content": "\u4eca\u5929\u5317\u4eac\u7684\u5929\u6c14", "receiver": "Peter"} 
[elpase=3.492 s]
========== Round 3 ==========
tool: 成功发送微信消息至Peter...

assistant [Final Answer]:
好的，请问还有什么可以帮助您？ 
[elpase=0.659 s]
```

#### 多轮输出注意事项

1. **轮次输出顺序**

触发函数调用时，系统先输出`content`给用户，再生成`tool_calls`并结束当前输出；**当前轮次内容不可依赖工具结果**，后续指令需待工具返回`message`后执行。 

2. **多轮输出响应完整性**

严格遵循`assistant（含tool_calls）→ tool（含message）→ assistant`的顺序，禁止跳过`tool message`响应直接发送新的`assistant`消息。每次`tool_calls`需对应`tool`角色的 `message`（含成功/错误结果），缺失可能因`prefill`机制触发重复调用或流程中断。
### 精调优化
如果 Function Calling 效果未达预期，可通过精调（SFT）提升模型表现。
详情请参见[模型精调概述](/docs/82379/1099459)。
#### 精调场景

* 提高函数选择的准确性，确保模型在合适的时机调用正确的函数。
* 优化参数提取能力，使模型能够准确解析用户需求并生成正确的函数入参。
* 改善结果总结质量，让模型能够更自然、准确地总结工具执行结果。

#### 精调样本格式
```SQL

{
  "messages": [
    {
      "role": "system",
      "content": "你是豆包AI助手"
    },
    {
      "role": "user",
      "content": "把北京的天气发给李四"
    },
    {
      "role": "assistant",
      "content": "",
      "tool_calls": [
        {
          "type": "function",
          "function": {
            "name": "get_current_weather",
            "arguments": "{\"location\": \"北京\"}"
          }
        }
      ],
      "loss_weight": 1.0
    },
    {
      "role": "tool",
      "content": "北京今天晴，25摄氏度"
    },
    {
      "role": "assistant",
      "content": "",
      "tool_calls": [
        {
          "type": "function",
          "function": {
            "name": "send_message",
            "arguments": "{\"receiver\": \"李四\", \"content\": \"北京今天晴，25摄氏度\"}"
          }
        }
      ],
      "loss_weight": 1.0
    },
    {
      "role": "tool",
      "content": "消息发送成功"
    },
    {
      "role": "assistant",
      "content": "已将北京的天气信息发送给李四"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_current_weather",
        "description": "获取指定地点的天气",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {"type": "string", "description": "地点的位置信息"}
          },
          "required": ["location"]
        }
      }
    }
  ]
}
```

## 常见问题
### Q：FC & MCP 的核心区别是什么，使用时如何选型？
**核心区别与典型使用场景**

| | | | \
|**维度** |**FC（Function Calling）** |**MCP（Model Context Protocol）** |
|---|---|---|
| | | | \
|**本质** |模型调用外部工具 / 函数的能力（功能扩展） |定义模型与外部系统交互时的上下文管理协议（流程规范） |
| | | | \
|**目标** |弥补模型自身能力不足（如计算、数据查询、操作执行） |标准化交互过程中上下文的传递、解析与状态维护 |
| | | | \
|**核心作用** |实现模型与外部工具的 “能力集成” |确保多轮交互中上下文（如对话历史、参数、状态）的一致性 |
| | | | \
|**协议标准** |厂商自定义格式（如 OpenAI 的 JSON 参数结构） |强制遵循 JSON-RPC 2.0 标准，强调协议统一性 |
| | | | \
|**架构** |直接集成于模型 API，用户定义函数后由模型触发调用 |客户端 - 服务器模式，分离 MCP Host（客户端）与 MCP Server（服务端） |
| | | | \
|**上下文管理** |单次请求 - 响应模式，上下文依赖需开发者自行处理 |支持多轮对话、历史状态维护，适用于长序列依赖任务 |
| | | | \
|**典型使用场景** |外部数据调用与系统操作（如天气查询、复杂运算）；示例：调用天气 API 回复 “东京今日气温” |多轮对话与跨系统上下文管理（如订餐流程、客服对话）；示例：连贯处理订餐菜品与配送地址 |

**两者之间的关系**

* **互补而非互斥：**
   * MCP 可能在流程中包含 FC 的调用逻辑（如定义何时、如何触发函数调用）
   * FC 的输入输出需遵循 MCP 的上下文规范（如参数格式、返回值解析规则）
* **分层协作**：
   * **MCP 解决连接问题**：标准化协议打通数据孤岛，提供基础设施（如整合用户订单数据）。
   * **FC 解决执行问题**：在协议层之上调用具体函数完成任务（如调用库存 API 生成补货建议）。
* **架构定位**：FC 是**能力接口**，MCP 是**交互框架**，二者常结合使用以构建复杂应用（如智能助手同时需要调用工具和管理多轮对话状态）。

**References**

* **MCP 官方协议规范（草案）**：明确协议核心架构、JSON - RPC 消息格式、状态管理机制与安全策略，是接口设计的权威参考。[Model Context Protocol Specification](https://spec.modelcontextprotocol.io/specification/draft/)
* **MCP 社区与生态发展**：介绍 MCP 发展规划，涵盖远程连接支持、沙箱安全机制及多模态扩展等发展计划。[MCP Development Roadmap](https://modelcontextprotocol.io/development/roadmap)

### **Q：Deepseek R1 模型是否支持并行函数调用？**
**A：暂不支持** **`parallel_tool_calls` 控制字段，该模型默认采用自动触发并行调用**机制，开发者无需额外配置即可实现多工具并行调用。
### **Q：如何处理 Deepseek R1 的参数幻觉问题？**
**A：该模型在复杂参数解析时可能出现嵌套调用异常**（如 `get_weather:{city: get_location()}`），建议通过以下方式干预： 

* 在 `system prompt` 中明确要求**分步调用**（如“请先调用定位函数，再调用天气查询函数”） 
* 使用 **JSON Schema 校验**强制参数格式

### Q：如何判断模型是否需要调用函数？
A：模型会根据用户问题和工具定义自主判断，若返回结果中包含 `tool_calls` 字段，则表示需要调用工具；若 `content` 字段有直接回复，则无需调用工具。
### Q：支持并行调用多个函数吗？
A：支持并行函数调用，通过设置 `parallel_tool_calls` 参数为 `true`，模型可同时返回多个函数调用信息，提高处理效率。
### Q：为什么模型返回的工具参数存在幻觉？
A：这是大模型常见的问题，可通过精调（SFT）优化模型的参数生成能力，或在系统提示中明确参数的格式和范围，减少幻觉现象。
部分模型（特别是Deepseek R1）存在一些参数幻觉问题。 如预期先调用get_location获得城市，再调用get_weather查询，R1模型有概率直接返回get_weather:{city: **get_location()**} 这种嵌套调用， 请在system prompt中进行干预解决，分步完成调用。
### Q：如何处理函数调用失败的情况？
A：将工具失败信息以 `role=tool` 的消息回填给模型，模型会根据错误信息生成相应的回复，例如“抱歉，函数调用失败，请稍后重试。”
通过以上优化，开发者能够更高效地使用 Function Calling 功能，实现大模型与外部工具的深度整合，快速构建智能应用。

## 附1：工具函数参数构造规范
为确保模型正确调用工具功能，需按以下规范构造 `tools` 对象，遵循 JSON Schema 标准。
本文主要讲解如何构造 Function Calling 工具，关于工具函数的使用请参考[基本使用流程](/docs/82379/1262342#db7321a0)。
### 总体结构
```JSON
{
  "type": "function",
  "function": {
    "name": "...",          // 函数名称（小写+下划线）
    "description": "...",   // 功能描述
    "parameters": { ... }   // 参数定义（JSON Schema格式）
  }
}
```

* `type`：工具的类型，这里是固定值 `function`，表示这是一个函数调用工具。
* `function`：含函数名称、描述和参数等详细配置。

### 字段解释
#### function 字段

| | | | | \
|**字段** |**类型** |**是否必填** |**说明** |
|---|---|---|---|
| | | | | \
|name |string |是 |函数名称，唯一标识，建议使用小写加下划线 |
| | | | | \
|description |string |是 |函数作用的描述 |
| | | | | \
|parameters |object |是 |函数参数定义，需要符合 JSON Schema 格式 |

#### parameters 字段
`parameters` 须为符合 JSON Schema 格式的对象：
```JSON
{
  "type": "object",
  "properties": {
    "参数名": {
      "type": "string | number | boolean | object | array",
      "description": "参数说明"
    }
  },
  "required": ["必填参数"]
}
```

* `type`：必须是 `"object"`。
* `properties`：列出支持的所有参数名及其类型。
   * `参数名`须为英文字符串，且不能重复。
      * 参数`type`需遵循[json规范](https://json-schema.org/docs)，支持类型包括string、number、boolean、integer、object、array。
      * `required`：指定函数中必填的参数名。
      * 其它参数根据`type`的不同稍有区别，详情请参见下表。

| | | \
|`type`类型 |示例 |
|---|---|
| | | \
|string、integer、number、boolean |略 |\
| | |
| | | \
|object （对象） |\
| |\
|* `description`：简要说明 |\
|* `properties` 描述该对象所有属性 |\
|* `required` 描述必填属性 |\
| |\
| |* 示例1: 查询特点用户画像（根据年龄、性别、婚姻状况等） |\
| | |\
| |```Python |\
| |"person": { |\
| |    "type": "object", |\
| |    "description": "个人特征", |\
| |    "properties": { |\
| |        "age": {"type": "integer", "description": "年龄"}, |\
| |        "gender": {"type": "string", "description": "性别"}, |\
| |        "married": {"type": "boolean", "description": "是否已婚"} |\
| |    }, |\
| |    "required": ["age"], |\
| |} |\
| |``` |\
| | |
| | | \
|array （列表） |\
| |\
|* `description`:简要说明 |\
|* `"items": {"type": ITEM_TYPE}`来表达数组元素的数据类型 |\
| |\
| |* 示例1：文本数组，若干个网页链接 |\
| | |\
| |```Bash |\
| |"url": {                       |\
| |    "type": "array",          |\
| |    "description": "需要解析网页链接,最多3个",               |\
| |    "items": {"type": "string"}  |\
| |``` |\
| | |\
| | |\
| |* 示例2： 二维数组 |\
| | |\
| |```Go |\
| |"matrix": { |\
| |    "type": "array", |\
| |    "description": "需要计算的二维矩阵", |\
| |    "items": {"type": "array", "items": {"type": "number"}}, |\
| |} |\
| |``` |\
| | |\
| | |\
| |* 示例3: 通过array来实现多选 |\
| | |\
| |```JSON |\
| | |\
| |"grade": { |\
| |    "description": "年级, 支持多选", |\
| |    "type": "array", |\
| |    "items": { |\
| |        "type": "string", |\
| |        "description": """枚举值有 |\
| |            "一年级", |\
| |            "二年级", |\
| |            "三年级", |\
| |            "四年级", |\
| |            "五年级", |\
| |            "六年级"。 """, |\
| |    }, |\
| |} |\
| |``` |\
| | |

### 完整示例
```JSON
{
  "type": "function",
  "function": {
    "name": "get_weather",
    "description": "获取指定位置的天气信息",
    "parameters": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "城市和国家，例如：北京，中国"
        }
      },
      "required": ["location"]
    }
  }
}
```

### 注意事项

1. **大小写敏感**：所有字段名、参数名严格区分大小写（建议统一用小写）。
2. **中文处理**：字段名用英文，中文说明置于 `description` 中（如 `location` 的描述为 “城市和国家”）。
3. **Schema 合规性**：`parameters` 必须是有效的 JSON Schema 对象，可通过JSON Schema 校验工具验证。

### 最佳实践

1. **工具描述核心准则** 
   * 详细说明工具功能、适用场景（及禁用场景）、参数含义与影响、限制条件（如输入长度限制），单工具描述建议3-4句。 
   * 优先完善功能、参数等基础描述，示例仅作为补充（推理模型需谨慎添加）。 
2. **函数设计要点** 
   * **命名与参数**：函数名直观（如`parse_product_info`），参数说明包含格式（如`city: string`）和业务含义（如“目标城市拼音全称”），明确输出定义（如“返回JSON格式天气数据”）。 
   * **系统提示**：通过提示指定调用条件（如“用户询问商品详情时触发`get_product_detail`”）。 
   * **工程化设计**： 
      * 使用枚举类型（如`StatusEnum`）避免无效参数，确保逻辑直观（最小惊讶原则）。 
      * 确保仅凭文档描述，人类可正确调用函数（补充潜在疑问解答）。 
   * **调用优化**： 
      * 已知参数通过火山方舟代码能力隐式传递（如`submit_order`无需重复声明`user_id`）。 
      * 合并固定流程函数（如`query_location`与`mark_location`整合为`query_and_mark_location`）。