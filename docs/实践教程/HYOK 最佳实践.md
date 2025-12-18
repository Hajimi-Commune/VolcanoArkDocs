# HYOK 最佳实践
HYOK（Hold Your Own Key）为用户提供了云环境中数据安全的最高级别控制权。该模式允许用户完全掌控密钥的所有权与管理权，并能够通过自有的软件或硬件系统对数据进行加密。在火山方舟平台上，用户可通过“托管外部密钥”功能接入外部密钥，并在执行精调任务时将其指定为数据加密密钥。该密钥将用于加密用户的精调数据等敏感信息，并在任务全生命周期内确保数据加密存储。
# HYOK核心原理

* **密钥生成**：在用户自有的可信环境（如HSM或可信的KMS）中生成并保存密钥。
* **完全控制**：用户自主管理密钥的全生命周期（创建、存储、轮换、撤销）。火山方舟仅在接受明确授权后执行临时且受控的加解密操作，无法持久持有密钥或访问明文数据。
* **临时访问**：数据密钥绝不会以明文形式传输或存储于火山方舟，仅在必要计算时短暂访问。用户可通过审计日志等方式对访问行为进行验证。

下图是HYOK的原理流程图（以加密存储为例）：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cb501c25f6804fc2bc82374633f1d65e~tplv-goo7wpa0wc-image.image)
需说明以下几点：

1. 火山方舟通过 AssumeRole 机制扮演用户身份以访问其 KMS，该操作需用户授权，且每次调用均会在火山方舟的安全审计页面及火山引擎云审计中记录日志。
2. KMS 在访问外部密钥存储代理（XKS Proxy）时，将使用用户创建外部密钥存储时指定的 CA 证书验证代理身份，以防止中间人攻击。
3. XKS Proxy 会通过用户在创建外部密钥存储时提供的访问密钥（ak/sk，仅用于签名验证）验证请求来源的合法性。
4. 所有数据均经过双重加密，以增强数据机密性。

# HYOK与传统云加密的区别

- **传统方式** | **HYOK 模式**
- 由CloudKMS管理密钥，平台具备访问用户明文数据的潜在条件。 | 用户作为密钥的唯一持有者，从根本上掌控数据访问权限。即使云服务提供商也无法在未经授权的情况下访问明文数据。

# HYOK典型应用场景

* **隐私保护**：精调数据中包含高度敏感信息，如个人身份信息（PII）、财务数据或商业机密。
* **合规要求**：需符合数据主权法规，要求数据或密钥不得超出特定地域或组织边界。
* **自主可控**：用户已具备外部KMS或CloudHSM等密钥管理基础设施，希望延续现有控制策略。

# XKS Proxy功能
火山方舟目前提供基础版本的XKS Proxy供用户参考和使用。后续将开放相关接入协议，用户可基于协议自行实现代理服务，并接入其他KMS或CloudHSM系统。
基础版XKS Proxy功能：

- 密钥来源 | aliyun/aws
- 加密算法 | AES_256

# HYOK使用流程

<img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI4ODVweCIgaGVpZ2h0PSIxNjVweCIgdmlld0JveD0iLTAuNSAtMC41IDg4NSAxNjUiPjxkZWZzLz48Zz48cmVjdCB4PSIxMTIiIHk9IjkyIiB3aWR0aD0iNzcwIiBoZWlnaHQ9IjcwIiBmaWxsLW9wYWNpdHk9IjAuMyIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBzdHJva2Utb3BhY2l0eT0iMC4zIiBzdHJva2UtZGFzaGFycmF5PSIzIDMiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSIxMTIiIHk9IjIiIHdpZHRoPSI3NzAiIGhlaWdodD0iNzAiIGZpbGwtb3BhY2l0eT0iMC4zIiBmaWxsPSIjZTFlYWZkIiBzdHJva2U9IiM1MDg0ZmIiIHN0cm9rZS1vcGFjaXR5PSIwLjMiIHN0cm9rZS1kYXNoYXJyYXk9IjMgMyIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMzgyIDM3IEwgNDY1LjYzIDM3IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNDcwLjg4IDM3IEwgNDYzLjg4IDQwLjUgTCA0NjUuNjMgMzcgTCA0NjMuODggMzMuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gNjMyIDM3IEwgNzEwLjYzIDM3IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNzE1Ljg4IDM3IEwgNzA4Ljg4IDQwLjUgTCA3MTAuNjMgMzcgTCA3MDguODggMzMuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMjAyIDM3IEwgMjk1LjYzIDM3IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzAwLjg4IDM3IEwgMjkzLjg4IDQwLjUgTCAyOTUuNjMgMzcgTCAyOTMuODggMzMuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjEyMiIgeT0iMTQuNSIgd2lkdGg9IjgwIiBoZWlnaHQ9IjQ1IiByeD0iNi43NSIgcnk9IjYuNzUiIGZpbGw9IiNlMWVhZmQiIHN0cm9rZT0iIzUwODRmYiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDc4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzdweDsgbWFyZ2luLWxlZnQ6IDEyM3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj4xLuWIm+W7ulZQQzwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHJlY3QgeD0iMzAyIiB5PSIxNC41IiB3aWR0aD0iODAiIGhlaWdodD0iNDUiIHJ4PSI2Ljc1IiByeT0iNi43NSIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogNzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAzN3B4OyBtYXJnaW4tbGVmdDogMzAzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxzcGFuIHN0eWxlPSJ0ZXh0LWFsaWduOmxlZnQiPjIu5Yib5bu6RUNTPC9zcGFuPjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHJlY3QgeD0iNDcyIiB5PSIxNC41IiB3aWR0aD0iMTcwIiBoZWlnaHQ9IjQ1IiByeD0iNi43NSIgcnk9IjYuNzUiIGZpbGw9IiNlMWVhZmQiIHN0cm9rZT0iIzUwODRmYiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE2OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDM3cHg7IG1hcmdpbi1sZWZ0OiA0NzNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PHNwYW4gc3R5bGU9InRleHQtYWxpZ246bGVmdCI+My7liJvlu7pDTELlubblhbPogZToh7PmjIflrppWUEM8L3NwYW4+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDc5MiA1OS41IEwgNzkyIDk4LjEzIiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNzkyIDEwMy4zOCBMIDc4OC41IDk2LjM4IEwgNzkyIDk4LjEzIEwgNzk1LjUgOTYuMzggWiIgZmlsbD0iIzAwMDAwMCIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSI3MTciIHk9IjE0LjUiIHdpZHRoPSIxNTAiIGhlaWdodD0iNDUiIHJ4PSI2Ljc1IiByeT0iNi43NSIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTQ4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzdweDsgbWFyZ2luLWxlZnQ6IDcxOHB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj48c3BhbiBzdHlsZT0idGV4dC1hbGlnbjpsZWZ0Ij40LuWIm+W7ulZQQ+e7iOerr+iKgueCueacjeWKoTwvc3Bhbj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxwYXRoIGQ9Ik0gNzEyIDEyNyBMIDY5OC4zNyAxMjciIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA2OTMuMTIgMTI3IEwgNzAwLjEyIDEyMy41IEwgNjk4LjM3IDEyNyBMIDcwMC4xMiAxMzAuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjcxMiIgeT0iMTA0LjUiIHdpZHRoPSIxNjAiIGhlaWdodD0iNDUiIHJ4PSI2Ljc1IiByeT0iNi43NSIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTU4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTI3cHg7IG1hcmdpbi1sZWZ0OiA3MTNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PHNwYW4gc3R5bGU9InRleHQtYWxpZ246bGVmdCI+NS7phY3nva7nu4jnq6/oioLngrnmnI3liqHnmb3lkI3ljZU8L3NwYW4+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDU4MiAxMjcgTCA1NjIgMTI3IEwgNTgyIDEyNyBMIDU2OC4zNyAxMjciIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA1NjMuMTIgMTI3IEwgNTcwLjEyIDEyMy41IEwgNTY4LjM3IDEyNyBMIDU3MC4xMiAxMzAuNSBaIiBmaWxsPSIjMDAwMDAwIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjU4MiIgeT0iMTA0LjUiIHdpZHRoPSIxMTAiIGhlaWdodD0iNDUiIHJ4PSI2Ljc1IiByeT0iNi43NSIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTA4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTI3cHg7IG1hcmdpbi1sZWZ0OiA1ODNweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogY2VudGVyOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PHNwYW4gc3R5bGU9InRleHQtYWxpZ246bGVmdCI+Ni5YS1MgUHJveHnpg6jnvbI8L3NwYW4+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0PjwvZz48cGF0aCBkPSJNIDM4MiAxMjcgTCAzNjguMzcgMTI3IiBmaWxsPSJub25lIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzYzLjEyIDEyNyBMIDM3MC4xMiAxMjMuNSBMIDM2OC4zNyAxMjcgTCAzNzAuMTIgMTMwLjUgWiIgZmlsbD0iIzAwMDAwMCIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSIzODIiIHk9IjEwNC41IiB3aWR0aD0iMTgwIiBoZWlnaHQ9IjQ1IiByeD0iNi43NSIgcnk9IjYuNzUiIGZpbGw9IiNlMWVhZmQiIHN0cm9rZT0iIzUwODRmYiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDE3OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDEyN3B4OyBtYXJnaW4tbGVmdDogMzgzcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDA7IHRleHQtYWxpZ246IGNlbnRlcjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxzcGFuIHN0eWxlPSJ0ZXh0LWFsaWduOmxlZnQiPjcu5byA6YCa5bm25o6I5p2D54Gr5bGx5byV5pOOS01T5pyN5YqhPC9zcGFuPjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHBhdGggZD0iTSAyNDIgMTI3IEwgMjI4LjM3IDEyNyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDIyMy4xMiAxMjcgTCAyMzAuMTIgMTIzLjUgTCAyMjguMzcgMTI3IEwgMjMwLjEyIDEzMC41IFoiIGZpbGw9IiMwMDAwMDAiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMjQyIiB5PSIxMDQuNSIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI0NSIgcng9IjYuNzUiIHJ5PSI2Ljc1IiBmaWxsPSIjZTFlYWZkIiBzdHJva2U9IiM1MDg0ZmIiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMjdweDsgbWFyZ2luLWxlZnQ6IDI0M3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj48c3BhbiBzdHlsZT0idGV4dC1hbGlnbjpsZWZ0Ij44LuWIm+W7uuWklumDqOWvhumSpeWtmOWCqDwvc3Bhbj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjEyMiIgeT0iMTA0LjUiIHdpZHRoPSIxMDAiIGhlaWdodD0iNDUiIHJ4PSI2Ljc1IiByeT0iNi43NSIgZmlsbD0iI2UxZWFmZCIgc3Ryb2tlPSIjNTA4NGZiIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogOThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMjdweDsgbWFyZ2luLWxlZnQ6IDEyM3B4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBjZW50ZXI7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiAjMDAwMDAwOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyB3b3JkLXdyYXA6IG5vcm1hbDsgIj48c3BhbiBzdHlsZT0idGV4dC1hbGlnbjpsZWZ0Ij45LuaJmOeuoeWklumDqOWvhumSpTwvc3Bhbj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjxyZWN0IHg9IjEyIiB5PSIxNC41IiB3aWR0aD0iNzAiIGhlaWdodD0iNDUiIGZpbGw9Im5vbmUiIHN0cm9rZT0ibm9uZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIj48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBmbGV4LXN0YXJ0OyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBmbGV4LXN0YXJ0OyB3aWR0aDogNjhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyMnB4OyBtYXJnaW4tbGVmdDogMTRweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMDsgdGV4dC1hbGlnbjogbGVmdDsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6ICMwMDAwMDA7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IHdvcmQtd3JhcDogbm9ybWFsOyAiPjxkaXYgc3R5bGU9InRleHQtYWxpZ246Y2VudGVyIj48c3BhbiBzdHlsZT0iYmFja2dyb3VuZC1jb2xvcjppbml0aWFsIj48Yj48Zm9udCBzdHlsZT0iZm9udC1zaXplOjE2cHgiPuWJjee9rui1hOa6kOWHhuWkhzwvZm9udD48L2I+PC9zcGFuPjwvZGl2PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48L2c+PHJlY3QgeD0iMiIgeT0iMTA1IiB3aWR0aD0iOTAiIGhlaWdodD0iNDUuNSIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJub25lIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGZsZXgtc3RhcnQ7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGZsZXgtc3RhcnQ7IHdpZHRoOiA4OHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDExMnB4OyBtYXJnaW4tbGVmdDogNHB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwOyB0ZXh0LWFsaWduOiBsZWZ0OyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogIzAwMDAwMDsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgd29yZC13cmFwOiBub3JtYWw7ICI+PGRpdiBzdHlsZT0idGV4dC1hbGlnbjpjZW50ZXIiPjxmb250IHN0eWxlPSJmb250LXNpemU6MTZweCI+PGI+5Yib5bu65bm25omY566h5aSW6YOo5a+G6ZKlPC9iPjwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PC9nPjwvZz48L3N2Zz4=" from="flow-chart" payload="{"data":{"mxGraphModel":{"dx":"1768","dy":"615","grid":"1","gridSize":"10","guides":"1","tooltips":"1","connect":"1","arrows":"1","fold":"1","page":"1","pageScale":"1","pageWidth":"827","pageHeight":"1169"},"mxCellMap":{"VRPv7Vq2":{"id":"VRPv7Vq2"},"ocT5TdFc":{"id":"ocT5TdFc","parent":"VRPv7Vq2"},"eMbub3F5":{"id":"eMbub3F5","value":"","style":"rounded=0;whiteSpace=wrap;html=1;fillColor=#E1EAFD;opacity=30;dashed=1;strokeColor=#5084FB;","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","parent":"ocT5TdFc","-0-mxGeometry":{"x":"40","y":"375","width":"770","height":"70","as":"geometry"}},"ZK79gXO0":{"id":"ZK79gXO0","value":"","style":"rounded=0;whiteSpace=wrap;html=1;fillColor=#E1EAFD;opacity=30;dashed=1;strokeColor=#5084FB;","parent":"ocT5TdFc","vertex":"1","diagramName":"Rectangle","diagramCategory":"general","-0-mxGeometry":{"x":"40","y":"285","width":"770","height":"70","as":"geometry"}},"qvMClkGT":{"id":"qvMClkGT","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","source":"chkuhEn1","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry","-0-mxPoint":{"x":"310","y":"320","as":"sourcePoint"},"-1-mxPoint":{"x":"400","y":"320","as":"targetPoint"}}},"w2SMVMx7":{"id":"w2SMVMx7","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","target":"jHWgBOyy","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry","-0-mxPoint":{"x":"560","y":"320","as":"sourcePoint"},"-1-mxPoint":{"x":"640","y":"320","as":"targetPoint"}}},"8AM66bxq":{"id":"8AM66bxq","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","source":"LazgmCxJ","target":"chkuhEn1","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"LazgmCxJ":{"id":"LazgmCxJ","value":"1.创建VPC","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"50","y":"297.5","width":"80","height":"45","as":"geometry"}},"chkuhEn1":{"id":"chkuhEn1","value":"2.创建ECS","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"230","y":"297.5","width":"80","height":"45","as":"geometry"}},"z858rEwg":{"id":"z858rEwg","value":"3.创建CLB并关联至指定VPC","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"400","y":"297.5","width":"170","height":"45","as":"geometry"}},"dlOVieAt":{"id":"dlOVieAt","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;","parent":"ocT5TdFc","source":"jHWgBOyy","target":"rsoemwV4","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"jHWgBOyy":{"id":"jHWgBOyy","value":"4.创建VPC终端节点服务","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"645","y":"297.5","width":"150","height":"45","as":"geometry"}},"8vJyyeB6":{"id":"8vJyyeB6","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0;exitY=0.5;exitDx=0;exitDy=0;entryX=1;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","source":"rsoemwV4","target":"IzTa81CH","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"rsoemwV4":{"id":"rsoemwV4","value":"5.配置终端节点服务白名单","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"640","y":"387.5","width":"160","height":"45","as":"geometry"}},"yGqidaJ1":{"id":"yGqidaJ1","value":"","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;","parent":"ocT5TdFc","source":"IzTa81CH","target":"LBSAsj9w","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"IzTa81CH":{"id":"IzTa81CH","value":"6.XKS Proxy部署","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"510","y":"387.5","width":"110","height":"45","as":"geometry"}},"EgftwBDI":{"id":"EgftwBDI","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0;exitY=0.5;exitDx=0;exitDy=0;entryX=1;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","source":"LBSAsj9w","target":"iBsibUbh","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"LBSAsj9w":{"id":"LBSAsj9w","value":"7.开通并授权火山引擎KMS服务","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"310","y":"387.5","width":"180","height":"45","as":"geometry"}},"swsprUyN":{"id":"swsprUyN","style":"edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;html=1;exitX=0;exitY=0.5;exitDx=0;exitDy=0;entryX=1;entryY=0.5;entryDx=0;entryDy=0;","parent":"ocT5TdFc","source":"iBsibUbh","target":"xVRRbOpW","edge":"1","-0-mxGeometry":{"relative":"1","as":"geometry"}},"iBsibUbh":{"id":"iBsibUbh","value":"8.创建外部密钥存储","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"170","y":"387.5","width":"120","height":"45","as":"geometry"}},"xVRRbOpW":{"id":"xVRRbOpW","value":"9.托管外部密钥","style":"rounded=1;whiteSpace=wrap;html=1;strokeColor=#5084fb;fillColor=#E1EAFD;strokeWidth=1;","parent":"ocT5TdFc","vertex":"1","diagramName":"RoundedRectangle","diagramCategory":"general","-0-mxGeometry":{"x":"50","y":"387.5","width":"100","height":"45","as":"geometry"}},"llqxvJ3f":{"id":"llqxvJ3f","value":"<b><font style=\"font-size:16px\">前置资源准备</font></b>","style":"text;whiteSpace=wrap;html=1;","parent":"ocT5TdFc","vertex":"1","-0-mxGeometry":{"x":"-60","y":"297.5","width":"70","height":"45","as":"geometry"}},"fNlLrIaX":{"id":"fNlLrIaX","value":"<meta /><meta /><font style=\"font-size:16px\"><b>创建并托管外部密钥</b></font>","style":"text;whiteSpace=wrap;html=1;","parent":"ocT5TdFc","vertex":"1","-0-mxGeometry":{"x":"-70","y":"388","width":"90","height":"45.5","as":"geometry"}}},"mxCellList":["VRPv7Vq2","ocT5TdFc","eMbub3F5","ZK79gXO0","qvMClkGT","w2SMVMx7","8AM66bxq","LazgmCxJ","chkuhEn1","z858rEwg","dlOVieAt","jHWgBOyy","8vJyyeB6","rsoemwV4","yGqidaJ1","IzTa81CH","EgftwBDI","LBSAsj9w","swsprUyN","iBsibUbh","xVRRbOpW","llqxvJ3f","fNlLrIaX"]},"lastEditTime":0,"snapshot":""}" />

:::tip
本节将介绍如何通过VPC终端节点服务托管外部密钥至外部密钥存储。其中，第1至4步为创建外部密钥存储所需的前置资源准备（本文以火山引擎资源为例）。**如果您已具备相应资源，可跳过相关步骤**。第5至9步为在火山方舟平台上创建外部密钥存储并托管外部密钥的具体操作指引。
:::
## 1.创建VPC
VPC 终端节点服务需与外部密钥管理器所在 VPC 连接。您可使用符合外部密钥存储要求的现有 VPC；如无可用 VPC，请参照下图于[火山引擎-私有网络](https://console.volcengine.com/vpc/region:vpc+cn-beijing/vpc?)中创建。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1392eb6504b54191ad9995112a36916e~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d1647ec2cdd14e35a39009dd94867997~tplv-goo7wpa0wc-image.image)

## 2.创建ECS
在创建所需的VPC终端节点服务前，您需要有一台可部署XKS Proxy的ECS实例。下图为在[火山引擎-云服务器](https://console.volcengine.com/ecs/region:ecs+cn-beijing/instance?)创建ECS实例的示例
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/97af06c677e24bf793c18cbe6431d712~tplv-goo7wpa0wc-image.image)

* 基础配置：**地域及可用区**需要与**步骤1**中所创建的 VPC 保持一致

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3f89b901e9b64cd8a3ae50b38d5afb2f~tplv-goo7wpa0wc-image.image)

* 网络配置：选择**步骤1**中创建的 VPC 和子网，并绑定公网 IP（新建或使用已有）

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4122c2edec7d4475bef83f8531f4375f~tplv-goo7wpa0wc-image.image)

* 自定义配置：配置 SSH 密钥或密码作为登录凭证

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c2c1f663639f452db5f3d6f01fabb4b6~tplv-goo7wpa0wc-image.image)

* 确认订单：阅读并同意《云服务器服务条款》后完成创建。

## **3.创建CLB并关联至指定VPC**
CLB 用于将火山引擎 KMS 的访问流量路由至XKS Proxy。
您可根据下图指引在[火山引擎-传统型负载均衡](https://console.volcengine.com/clb/region:clb+cn-beijing/LoadBalancer?)中创建CLB并配置监听端口：

* 点击**实例管理-创建负载均衡**进行配置，选择**步骤1**中创建的VPC和子网

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/16cd873a1a464b108807744207a4cb02~tplv-goo7wpa0wc-image.image)
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/86f46d5fb5f3423ab1805572cfc48e53~tplv-goo7wpa0wc-image.image)

* 添加监听器：在**实例管理界面**，选择上一步创建的负载均衡，配置监听器

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/45710b6c13704c6ab275340e3026377c~tplv-goo7wpa0wc-image.image)

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ea16912ac094445ca12804db02167032~tplv-goo7wpa0wc-image.image)

* **后端服务器组：**挂载**步骤2**中创建的 ECS 实例

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/14607d10124347bf917874cc24e1f075~tplv-goo7wpa0wc-image.image)

* **添加后端服务器**：选择具备公网访问能力的 ECS，确保负载均衡端口与服务器端口一致

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/322d7ec003414cc0a3825a350b4ddb65~tplv-goo7wpa0wc-image.image)

* **健康检查**：启用健康检查功能

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/07206a09e6fd4614ad7a67e2d85af1e6~tplv-goo7wpa0wc-image.image)

:::tip
具体配置请根据实际需要确定。若仅用于方舟托管外部密钥，建议选用基础规格即可。
:::
## **4.创建VPC终端节点服务**

* 完成前述资源准备后，即可创建 [VPC终端节点服务](https://console.volcengine.com/pl/region:pl+cn-beijing/endpointServices?)。
   * **服务资源**需选择**步骤3**中创建的负载均衡。
   * **在高级配置中，**请勾选“自动接受连接”。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1e1e1a9a79af41cc8cc53aad8720fac8~tplv-goo7wpa0wc-image.image)

* 创建完成后，请在**终端节点服务列表页**中记录**服务域名**，该信息将在后续创建托管外部密钥时使用。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6a9dae3b2e144791965c9a89ce89d957~tplv-goo7wpa0wc-image.image)

## **5.配置终端节点服务白名单**

* 点击已创建的终端节点服务，选择**服务白名单**，点击**添加服务白名单**，输入KMS服务账号 `2100052433`。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4bc3306a8c374ae084f23ac9a5134662~tplv-goo7wpa0wc-image.image)

## 6.XKS Proxy 部署
请返回至**步骤2**中已创建的 ECS 实例，部署 XKS Proxy 并手动配置鉴权信息，包括：外部 KMS 的 ak/sk、用于验证火山引擎 KMS 身份的 ak/sk、保障连接安全的 CA 证书等元数据。
您可通过 SSH 或控制台连接到ECS 实例，并按下图指引完成 XKS Proxy 部署（本文以控制台连接为例）：
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d2aeab982d4149c1837968f1c14d1318~tplv-goo7wpa0wc-image.image)

* **连接 ECS 实例**：若需其他连接方式，请参见[相关操作指南](https://www.volcengine.com/docs/6396/81032)。
* **获取 XKS Proxy**：
   * 下载 XKS Proxy 压缩包。

```Plain Text
# 下载压缩包
wget https://ark-public-cn-beijing.tos-cn-beijing.volces.com/xksproxy/xksproxy.tar.gz
# 解压xksproxy压缩包
tar -zxvf xksproxy.tar.gz
# 进入解压的文件夹
cd xksproxy
```

   * 其主要目录结构如下：

```Plain Text
xksproxy/
├── conf
│   └── config.yaml # 配置文件
├── scripts
│   ├── README.md # CA/服务端证书生成指南
│   ├── ca.cnf
│   ├── cert_gen.sh
│   ├── generate_ca.sh
│   ├── generate_server_cert.sh
│   └── server.cnf
└── xksproxy # 主程序
```

* **证书配置**：
   * 若您已持有 CA 或服务端证书，请将其保存至指定目录并在 `config.yaml` 中配置路径。
   * 若无相应证书或仅需功能体验，可参照 `scripts/README.md` 文档创建。

```Plain Text
# 1.记录上述Step4「创建VPC终端节点服务」步骤中的服务域名
# 假设域名为epsvc-xxx.cn-beijing.privatelink.volces.com

# 2.打开scripts目录
cd xksproxy/scripts

# 3.生成根证书和服务端证书
./cert_gen.sh all --dns "*.epsvc-xxx.cn-beijing.privatelink.volces.com" --output /path/to/cert/dir
```

   * 生成后的证书目录结构如下：

```Plain Text
certs
├── ca.key        # 私有CA私钥
├── ca.pem        # 私有CA证书
├── ca.srl        # 私有CA序列号
├── cert.csr      # 服务端证书签名请求
├── cert.pem      # 服务端证书
├── key.pem       # 服务端私钥
└── openssl.cnf   # 服务端证书配置
```

* **编辑配置文件**：进入 `conf` 目录，编辑 `config.yaml` 文件。

```Plain Text
# cd conf
server:
  host: "0.0.0.0"
  port: 443
  read_timeout: 30 
  write_timeout: 30
  path_prefix: ""               # 路径前缀（选填）
  tls:
    enabled: true               # 是否开启tls（目前必须开启）
    cert_path: ""               # 服务端证书路径（必填）
    key_path: ""                # 服务端密钥路径（必填）
    ca_path: ""                 # CA证书路径（必填）
    ca_base64: ""               # CA证书der格式base64编码 (启动服务时自动填充)

# SignV4 authentication configuration
auth:
  access_key_id: "YOUR_ACCESS_KEY_ID"         # 认证ak（留空则启动自动填充）
  secret_access_key: "YOUR_SECRET_ACCESS_KEY" # 认证sk（留空则启动自动填充）

kms:
  - id: "local-kms"                        # 外部kms唯一标识
    provider: "local"                      # 根据kms服务商填写，目前支持aliyun、aws和local（本地调试模式）
    region: ""                             # kms region
    endpoint: ""                           # kms服务endpoint
    access_key: ""                         # kms服务鉴权ak
    secret_key: ""                         # kms服务鉴权sk
    key_mapping:                           # 密钥映射
      "external-key-1": "real-key-id-1"    # key为xksproxy中该密钥别称，vaule为kms服务真实密钥id

log:
  level: "info"
  file_path: ""
  max_size: 100
  max_age: 7
  max_backups: 5 

# XKS proxy configuration
xks_proxy:                                # xksproxy版本信息，保持不变即可
  fleet_size: 1
  vendor: "Volc XKS Proxy"
  model: "Volc XKS Proxy v1.0"
```

- **字段** | **描述** | **示例** | 是否必填
- server.path_prefix | 路径前缀 | myxks | 否
- server.tls.cert_path | 服务端证书路径 | /root/cert.pem | 是
- server.tls.key_path | 服务端密钥路径 | /root/key.pem | 是
- server.tls.ca_path | CA证书路径 | /root/ca.pem | 是
- server.tls.ca_base64 | CA证书der格式base64编码 | / | 否（启动时自动填充）
- auth.access_key_id | 用于验证kms侧身份的ak，kms会使用该ak/sk做请求签名 | XT7II5R7F4OORP5Z4PRB | 否（留空则自动填充）
- auth.secret_access_key | 用于验证kms侧身份的ak，kms会使用该ak/sk做请求签名 | hHnacnaJEUa0AlYd6WZXUlPg9QG0Gzasrfjd4VkWIpp | 否（留空则自动填充）
- kms.id | 用于唯一标识外部kms | mykms | 是
- kms.provider | kms的提供方，目前支持：
- * aliyun：阿里云kms
- * aws：aws kms
- * local：本地调试模式，加解密发生在本地（不可用于生产环境） | local | 是
- log.level | 日志级别
- * debug
- * info
- * warn
- * error | error | 否
- log.file_path | 日志存储位置 | /mypath/xksproxy.log | 否
- log.max_size | 日志最大容量（MB） | 100 | 否（默认100MB）
- log.max_age | 日志最大存储时间（天） | 7 | 否（默认7天）
- log.max_backups | 日志最大存储数量 | 100 | 否（默认5个）
- xks_proxy.* | xksproxy的版本元信息 | / | 否

* **启动代理**：执行启动命令，运行 XKS Proxy。**仅当成功启动后，方可连接至外部密钥存储**。

```Plain Text
./xksproxy server
```

## 7.开通并授权火山引擎 KMS 服务
您需手动开通 KMS 服务并授予火山方舟相应访问权限，以便其代理您完成外部密钥的托管。

* 进入[火山引擎 KMS 控制台](https://console.volcengine.com/kms/region:kms+cn-beijing/primary?)，开通服务。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/78d4c617d7c041a79185ca864c37982f~tplv-goo7wpa0wc-image.image)

* 前往[访问控制授权页面](https://console.volcengine.com/iam/service/attach_role/?ServiceName=kms)。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f7218891bf4e46578c42b48b2b1a55cb~tplv-goo7wpa0wc-image.image)

* 查看权限内容并完成授权。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/51152e10b17a4d17ae2b9c2773dc0b68~tplv-goo7wpa0wc-image.image)

## 8.创建外部密钥存储
在火山方舟控制台中，选择对应的 VPC 终端节点服务，并配置代理连接信息。所需配置内容请参照**步骤6**中 `config.yaml` 文件的相应条目。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/71ce6d31cf6742bfa20ebac8a59be16d~tplv-goo7wpa0wc-image.image)

## 9.托管外部密钥
托管外部密钥将由方舟代理您在火山引擎 KMS 中创建一款用户主密钥。请选择**步骤8**中创建的外部密钥存储，并可按需自定义密钥环与密钥信息。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/add5ecef640c4672921249f53e828db0~tplv-goo7wpa0wc-image.image)

# 常见问题
**Q：可以使用其他厂商的KMS服务接入方舟的托管外部密钥功能吗？**
**A**：可以，您使用的其他厂商KMS服务的ak/sk需具备以下权限：`kms:Encrypt`、`kms:Decrypt` 和 `kms:DescribeKey`。
**Q：如果我已经拥有相关的云服务资源，是否还需要重新创建？**
**A**：如果您已拥有相应的云服务资源（如ECS、CLB），并确保其与目标资源处于同一可用区且能正常访问外部密钥存储，则可以跳过相关创建步骤。
**Q：XKS Proxy 无法连接到外部密钥存储应如何排查？**
**A**：请按以下步骤依次检查：

   1. 确认终端节点服务是否已开启“自动接受连接”选项；
   2. 检查是否已完成对火山引擎KMS服务的授权；
   3. 验证XKS Proxy是否已正确部署并运行。

**Q：如果密钥已被删除，为何精调任务仍在进行？**
**A**：精调任务仅在加密数据的输出和访问时需要访问密钥。若该阶段已完成，精调任务进程将不受密钥删除影响；若任务仍在进行且需访问加密数据，则会因密钥不可用而提示错误信息。
**Q：使用托管外部密钥是否会带来时延性能的损失？**
**A**：该功能的时延取决于火山引擎KMS、XKS Proxy及外部密钥存储接口的延时和网络情况。与直接使用火山引擎KMS相比，会有百毫秒级别的延迟差距，但不会对精调任务的整体性能产生影响。
