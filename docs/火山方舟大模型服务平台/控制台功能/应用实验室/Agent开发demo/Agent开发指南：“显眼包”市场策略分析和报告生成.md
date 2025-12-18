# Agent开发指南：“显眼包”市场策略分析和报告生成
本文通过火山方舟应用实验室的 [DeepSearch](https://console.volcengine.com/ark/region:ark+cn-beijing/application/detail?id=bot-20250414112950-6x4km-procode-preset&prev=application&projectName=default) 应用，以 “撰写研究报告、生成 PPT 并自动发送邮件” 为例，展示如何集成 MCP 服务实现复杂任务的全流程自动化。
## **任务目标**
火山方舟应用实验室 DeepSearch 应用依托 **日志服务、联网搜索、ChatPPT、浏览器控制** 等 MCP 能力，深度整合私域数据与公开信息，解决企业级场景中的高效生产力需求。

* **场景设定**：显眼包团队需分析 AI 玩具市场动态与用户行为日志（近 14 天数据），生成《2025 年 AI 玩具市场分析及显眼包迭代建议》报告、配套 PPT，并通过 163 邮箱自动发送至指定邮件组。
* **能力介绍**：
   * **日志服务**：可存储并检索私域“显眼包抽样数据”中的用户行为与访问日志；
   * **联网搜索**： 用于搜索互联网公开域信息；
   * **必优-ChatPPT**：智能 PPT 服务，本案例中用于生成《显眼包下一代迭代方案建议》PPT； 
   * **浏览器使用**：可自动执行浏览器任务，本案例中使用浏览器自动撰写并发送 163 邮件；
   * **PromptPilot**：广场 DeepSearch 内置 PromptPilot，开启后自动实现 Prompt个性化优化。
* **演示视频**：以下视频展示了使用DeepSearch撰写研究报告，生成PPT并自动发送邮件的示例

<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cd329a51d8494adfa63e31679e16edc8~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cd329a51d8494adfa63e31679e16edc8~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer>
## 前置准备

* **注册账号**：[注册火山引擎账号](https://console.volcengine.com/auth/signup) 并完成企业或个人认证，详见 [账号注册流程](https://www.volcengine.com/docs/6261/64925)。

## 操作步骤
### 启动任务

1. 访问并登录 [火山方舟控制台](https://console.volcengine.com/ark)，进入 **应用广场 -** [DeepSearch](https://console.volcengine.com/ark/region:ark+cn-beijing/application/detail?id=bot-20250414112950-6x4km-procode-preset&prev=application&projectName=default)  **- 体验应用** 页面。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2b034a6b21eb47be93b08f29e5155420~tplv-goo7wpa0wc-image.image)
2. （可选）DeepSearch支持自定义设置问题拆解层数与MCP服务，建议开启Prompt个性化优化。您也可直接使用默认配置。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/fc6984356a584ea0b55c74cb53dd1a69~tplv-goo7wpa0wc-image.image)
3. 输入研究问题，等待任务执行完成。例如，在输入框输入以下内容：
   ```Plain Text
   请撰写一份《2025年AI玩具市场分析及显眼包迭代建议》，分析日志情况（开始时间：1747123403000，结束时间：1748335823000），并结合近期AI玩具市场的最新动态和最受欢迎的新功能进行研究。在此基础上，生成一份“显眼包下一代迭代方案建议” PPT。最终将报告内容和PPT预览链接整理为一份汇报邮件通过我的163邮箱发送至XXX
   ```

> 请将上述的邮箱地址替换为自定义收件人。

   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e794a8e8e23246d0acad414edc0db275~tplv-goo7wpa0wc-image.image)

### 任务执行
点击“运行”按钮后，系统将开始执行任务，其大致可分为以下几个阶段：

1. **任务规划**：DeepSearch 在分析完需求后，会进行任务拆解。如果任务计划不符合预期，可以点击 **修改任务**。确认需求后点击 **开始任务**。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/90ee3481de234578bc92fb5339233581~tplv-goo7wpa0wc-image.image)
   任务执行过程中，可实时展开左下角 **任务列表** 查看执行进度。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/13d73d91c7164e3cb10f637455ea85b2~tplv-goo7wpa0wc-image.image)
2. **调用 日志服务 MCP**：工具将检索广场预置的示例日志 ——「显眼包抽样数据」，并分析用户行为与访问日志。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0bd7ea99a6ec47c6b6debc0f7ceb12ef~tplv-goo7wpa0wc-image.image)
3. **调用 联网搜索 MCP**：搜索互联网公开域资料，例如近期AI玩具市场的最新动态和最受欢迎的新功能。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/80390eac252d43d69711e9c2612806a3~tplv-goo7wpa0wc-image.image)
4. **调用 ChatPPT MCP**：根据分析研究内容，并生成 “显眼包下一代迭代方案建议” PPT。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b3de60ed5ee54998b56cb34451a6bbca~tplv-goo7wpa0wc-image.image)
5. **调用 浏览器使用 MCP**：将汇总内容通过163邮箱发送至用户指定邮箱地址。
   * **支持用户接管**：浏览器使用遇到登陆场景支持用户进行主动接管，用户登陆操作完成后，可退出接管模式。
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3a041ce95d534cb3a8b881ab0f587166~tplv-goo7wpa0wc-image.image)
   * **成功发送邮件**：等待浏览器使用MCP 依次执行填入收件人、邮件主题、邮件正文和邮件发送操作。
      ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3224431f6f47421ca983a437c5b01bb3~tplv-goo7wpa0wc-image.image)

### 个性化优化 Prompt (可选)

* 通过 DeepSearch 内置的 [PromptPilot](https://www.volcengine.com/docs/82379/1399497)，用户可对任务结果点赞 / 踩、评论反馈，系统自动优化 Prompt 参数，提升后续任务的响应精准度，反馈三次即可开启个性化优化。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e773a81eeacf4097a0dc5405d329d0a0~tplv-goo7wpa0wc-image.image)
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/87a16c01a2de4ad1a4f1b43b050e97da~tplv-goo7wpa0wc-image.image)

* 进入 [控制台-Prompt 实验室](https://console.volcengine.com/ark/region:ark+cn-beijing/autope/workbench/ta-20250520131804-r5C6F) ，您可以查看实时优化结果及智能优化报告。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8dc37762328b4d8f8b098c8a511fce79~tplv-goo7wpa0wc-image.image)

* 在 DeepSearch 中使用个性化优化后的Prompt，可以获得更佳的模型回复效果。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c19c15fba03241e39e30443dada0b89c~tplv-goo7wpa0wc-image.image)
## 创建更多同款应用

* 点击应用右上角 **复制应用**，在随后弹出的 **开通及创建授权** 窗口中，确认相关服务是否一键开通，确认无误后点击 **立即创建** （注：创建同款功能即将对个人用户开放）。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3d445261a7e2419f95d50f17d3c3cc1e~tplv-goo7wpa0wc-image.image)
* 在 **创建并部署函数** 的配置页面，按需配置以下内容：
   * 若开启 **日志服务 MCP** 服务，需选择可用于检索的日志项目和主题。
   * 若开启 **知识库 MCP** 服务，需选择可用于检索的知识库，并填写知识库描述作为 MCP Server 的描述，以便模型判断是否使用。
   * 推荐开启 **日志服务-Trace** 功能，方便后续线上问题定位和排查，同时支持通过可视化的调试面板查看会话执行细节。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3cf43932167e46b080ad03fa6264d415~tplv-goo7wpa0wc-image.image)
6. 完成配置后，点击 **确定** 开始自动部署。等待部署成功后，点击 **立即体验** 即可使用该应用。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9a056031d4d94e758efc424a703e9015~tplv-goo7wpa0wc-image.image)

## 附录：开源应用介绍
该案例中用到的 [DeepSearch](https://console.volcengine.com/ark/region:ark+cn-beijing/application/detail?id=bot-20250414112950-6x4km-procode-preset&prev=application&projectName=default) 为火山方舟应用实验室的开源原型应用，您可在 [应用广场](https://console.volcengine.com/ark/region:ark+cn-beijing/application) 免费体验。DeepSearch 专为应对复杂问题而设计的高效工具，集成了浏览器使用、联网搜索、知识库、网页解析等丰富的 MCP 服务。无论是学术研究、企业决策，还是产品调研场景，它都能助力用户深入挖掘信息，提出切实可行的解决策略。
此外，应用实验室汇集了更多高难度、高价值的解决方案案例，并开放源代码，助力企业快速构建大模型应用。
**开源信息：**

* DeepSearch开源地址：https://github.com/volcengine/ai-app-lab/tree/main/demohouse/deep_search_mcp/backend
* 应用实验室开源地址：https://github.com/volcengine/ai-app-lab
* 开源协议：Apache License 2.0 https://github.com/volcengine/ai-app-lab/blob/main/APACHE_LICENSE
