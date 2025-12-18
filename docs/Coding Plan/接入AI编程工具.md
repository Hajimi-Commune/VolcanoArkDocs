# 接入AI编程工具
方舟 Code Plan 支持在多款主流的编程工具中使用，您可以参考本文对编程工具进行配置及使用。
:::tip
* 访问[方舟 Coding Plan活动](https://www.volcengine.com/activity/codingplan)，完成套餐订阅后，再进行编程工具的配置和使用。
* 接入工具的核心参数如下（**不同的工具配置的Base URL不同，请根据使用的工具选择**）：
   * 配置模型：支持`doubao-seed-code-preview-latest`、`ark-code-latest`(对应模型 DeepSeek V3.2)
   * 配置 Base URL：`https://ark.cn-beijing.volces.com/api/coding`
      * 适用工具：Claude Code。
   * 兼容OpenAI API 的 Base URL：`https://ark.cn-beijing.volces.com/api/coding/v3`
      * 适用工具：Cline、Cursor、Codex CLI、Kilo Code、Roo Code、OpenCode等。
* **部分工具自带 doubao provider，请确认模型、Base URL与上方信息一致。**
:::
## 接入Claude Code
### 安装步骤
前提条件：

* 安装 [Node.js 18 或更新版本环境](https://nodejs.org/en/download/)。
* Windows 用户需安装 [Git for Windows](https://git-scm.com/download/win)。

在命令行界面，执行以下命令安装 Claude Code。
```Bash
npm install -g @anthropic-ai/claude-code
```

安装结束后，执行以下命令查看安装结果，若显示版本号则安装成功。
```Bash
claude --version
```

### 配置工具
完成Claude Code安装后，配置以下信息。

* **ANTHROPIC_BASE_URL**：`https://ark.cn-beijing.volces.com/api/coding`
* **ANTHROPIC_AUTH_TOKEN**：[获取API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)
* **ANTHROPIC_MODEL**: 按需选择模型`doubao-seed-code-preview-latest`、`ark-code-latest`

**MacOS & Linux**

1. 在终端执行以下命令进入Claude Code配置文件。
   ```Bash
   vim ~/.claude/settings.json
   ```

2. 编辑配置文件，文件内容如下，请将\`<ARK_API_KEY>\`替换为方舟 API Key，\`<Model>\`替换为要使用的模型。
   ```JSON
   {
       "env": {
           "ANTHROPIC_AUTH_TOKEN": "<ARK_API_KEY>",
           "ANTHROPIC_BASE_URL": "https://ark.cn-beijing.volces.com/api/coding",
           "ANTHROPIC_MODEL": "<Model>"
       }
   }
   ```

3. 打开一个新的终端窗口，环境变量才能生效。

**Windows**

### CMD

1. 在CMD中执行以下命令，设置环境变量。
   ```Bash
   setx ANTHROPIC_AUTH_TOKEN <ARK_API_KEY>
   setx ANTHROPIC_BASE_URL https://ark.cn-beijing.volces.com/api/coding
   setx ANTHROPIC_MODEL <Model>
   ```

2. 在新的CMD窗口执行以下命令，检查环境变量是否生效。
   ```Bash
   echo %ANTHROPIC_AUTH_TOKEN%
   echo %ANTHROPIC_BASE_URL%
   echo %ANTHROPIC_MODEL%
   ```

### PowerShell

1. 在PowerShell中执行以下命令，设置环境变量。
   ```PowerShell
   [System.Environment]::SetEnvironmentVariable('ANTHROPIC_AUTH_TOKEN', '<ARK_API_KEY>', 'User')
   [System.Environment]::SetEnvironmentVariable('ANTHROPIC_BASE_URL', 'https://ark.cn-beijing.volces.com/api/coding', 'User')
   [System.Environment]::SetEnvironmentVariable('ANTHROPIC_MODEL', '<Model>', 'User')
   ```

2. 在新的PowerShell窗口执行以下命令，检查环境变量是否生效。
   ```PowerShell
   echo $env:ANTHROPIC_AUTH_TOKEN
   echo $env:ANTHROPIC_BASE_URL
   echo $env:ANTHROPIC_MODEL
   ```

### 使用 Claude Code

* 启动Claude Code：进入项目目录，执行`claude`命令，即可开始使用Claude Code。
   ```Bash
   cd my-project
   claude
   ```

* 模型状态验证：输入`/status`确认模型状态。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/66ac411648f34406ad71214db090d86d~tplv-goo7wpa0wc-image.image)
   如果提示信息如下，可能是由于之前已登录过Claude Code，可输入`/logout`退出登录，然后再启动Claude Code。
   ```JSON
   {
       "error": {
           "code": "AuthenticationError",
           "message": "The API key format is incorrect. Request id:0217xxxxxxx",
           "param": "",
           "type": "Unauthorized"
       }
   }
   ```

### 使用 Claude Code IDE 插件

1. 安装 Claude Code 并配置好环境变量，具体参考[接入Claude Code](/docs/82379/1928262#7432bab1)。
   Claude Code IDE 插件依赖 Claude Code CLI 工具，需先完成 Claude Code的安装及配置。
2. 安装并使用 IDE 插件。

> 因 IDE 插件会迭代演进，以下内容仅供参考，具体的安装及使用可参考 [Visual Studio Code](https://code.claude.com/docs/en/vs-code)、[JetBrains IDEs](https://code.claude.com/docs/en/jetbrains)。

**Claude Code VSCode 插件**

:::tip
Claude Code VSCode 插件支持在 VSCode 及基于 VSCode 的 IDE（如 Cursor、Trae 等）中使用。
:::
### 安装插件
打开 VSCode，在扩展市场搜索\`claude code\`进行安装。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6c39c8cb4ec84f03bb0ca2444804eac7~tplv-goo7wpa0wc-image.image)
### 开始使用
安装完成后，点击 VSCode 右上角的 Claude Code 图标，进入 Claude Code 页面，待初始化完成后即可开始使用。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/388665c0f9ed47379faa5291e180a6b2~tplv-goo7wpa0wc-image.image)

**Claude Code Jetbrains 插件**

:::tip
Claude Code Jetbrains 插件支持 Jetbrains 的系列 IDE 如 IntelliJ IDEA、PyCharm、WebStorm 等。
:::
### 安装插件
打开 Jetbrains IDE，在插件市场搜索\`claude code\`进行安装。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5859916985a34036bb41f5097ba58f6c~tplv-goo7wpa0wc-image.image)
### 开始使用
安装完成后，重启IDE后，单击Claude Code 图标，进入 Claude Code 页面开始使用。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/78fae54038524ffbbb0f9d0e677de393~tplv-goo7wpa0wc-image.image)

## 接入veCLI
### 安装步骤
在命令行界面，执行以下命令安装 veCLI。
```Bash
npm install -g @volcengine/vecli@latest
```

安装结束后，执行以下命令查看安装结果，若显示版本号则安装成功。
```Bash
vecli --version
```

### 配置工具
> 订阅 Coding Plan 套餐后，只需要配置订阅账号的 AK/SK 信息，即可在 veCLI中使用 Coding Plan 支持的模型。

1. 进入项目目录，在命令行界面执行`vecli`命令启动 veCLI。
2. 根据提示输入[当前账号的 AK/SK](https://www.volcengine.com/docs/6291/65568)。

### 开始使用

* 启动 veCLI：进入项目目录，执行`vecli`命令，即可开始使用 veCLI。
   ```Bash
   cd my-project
   vecli
   ```

* 模型状态验证：输入`/model` 确认当前使用的模型。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/29110f31a7884a90b66821d68a6af56a~tplv-goo7wpa0wc-image.image =100%x)

## 接入TRAE CN
:::warning
暂未支持 Coding Plan。
TRAE CN 最新版中可直接使用 Doubao-Seed-Code 模型，使用预置的 Doubao-Seed-Code 模型不会占用Coding Plan使用额度。
:::

1. 访问[TRAE CN官网](https://www.trae.cn/)，下载最新版本TRAE IDE。
2. 关闭Auto模式后，内置模型选择 Doubao-Seed-Code。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/211f9acc79d341499d41f10430be0eda~tplv-goo7wpa0wc-image.image)

## 接入Cline
### 安装步骤
打开 VSCode，在扩展市场搜索`Cline`安装。
### 配置工具
Cline插件安装完成后，您需要配置以下信息。

* **API Provider**：`OpenAI Compatible`（Coding Plan 接口兼容 OpenAI 标准）
* **Base URL**：`https://ark.cn-beijing.volces.com/api/coding/v3`
* **API Key**：[获取API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)
* **Model ID**：按需选择模型`doubao-seed-code-preview-latest`、`ark-code-latest`

配置完成后，就可以在输入框中输入需求，与模型进行交互。 
## 接入Cursor
### 安装步骤
官网下载安装包：通过[Cursor官网](https://cursor.com/features)下载并安装Cursor。
### 配置工具
Cursor安装完成后，需要登录付费账户才支持配置 Models。Models 模块的具体配置如下：

* OpenAI API Key：[获取API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)
* Override OpenAI Base URL：`https://ark.cn-beijing.volces.com/api/coding/v3`
* Add Custom Model：按需选择模型`doubao-seed-code-preview-latest`、`ark-code-latest`

配置完成后，即可在聊天面板中选择配置的模型进行交互。
## 接入Codex CLI
### 安装步骤
在命令行界面，执行以下命令安装Codex CLI。
```Bash
npm i -g @openai/codex
```

安装结束后，执行以下命令查看安装结果，若显示版本号则安装成功。
```Bash
codex --version
```

### 配置工具

1. 编辑Codex的配置文件`~/.codex/config.toml`，配置信息参考下面内容。
   支持的model：`doubao-seed-code-preview-latest`、`ark-code-latest`。
   ```Bash
   [model_providers.volcengine]  
   name = "volcengine"  
   base_url = "https://ark.cn-beijing.volces.com/api/coding/v3"  
   env_key = "ARK_API_KEY"
   wire_api = "chat" 
   requires_openai_auth = false
    
    
   [profiles.seed-code]
   model = "doubao-seed-code-preview-latest"
   model_provider = "volcengine"
   ```

2. 出于安全考虑，需要通过环境变量`ARK_API_KEY`设置 [API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)。
   ```Shell
   export ARK_API_KEY="<ARK_API_KEY>"
   ```

### 开始使用

* 启动Codex CLI：
   ```Shell
   codex --profile seed-code
   ```

* 状态验证：
   ```Bash
   codex status
   ```

## 接入Kilo Code
### 安装步骤
打开 VSCode，在扩展市场搜索`kilo code`进行安装，安装完成后选择信任发布者。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3e991c168c0c49bc9449f6f6efc29bd7~tplv-goo7wpa0wc-image.image)
### 配置工具
选择Use your own API key，然后配置以下信息。

* **API Provider**：`OpenAI Compatible`（Coding Plan 接口兼容 OpenAI 标准）
* **Base URL**：`https://ark.cn-beijing.volces.com/api/coding/v3`
* **API Key**：[获取API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)
* **Model**：按需选择模型`doubao-seed-code-preview-latest`、`ark-code-latest`

配置完成后，就可以在输入框中输入需求，与模型进行交互。
## 接入Roo Code
### 安装步骤
打开 VSCode，在扩展市场搜索`Roo Code`进行安装，安装完成后选择信任发布者。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/dd47c5150044469cb4e95b8de067d817~tplv-goo7wpa0wc-image.image)
### 配置工具
安装完成后，配置以下信息。

* **API Provider**：`OpenAI Compatible`（Coding Plan 接口兼容 OpenAI 标准）
* **Base URL**：`https://ark.cn-beijing.volces.com/api/coding/v3`
* **API Key**：[获取API Key](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)
* **Model**：按需选择模型`doubao-seed-code-preview-latest`、`ark-code-latest`

配置完成后，就可以在输入框中输入需求，与模型进行交互。
## 接入OpenCode
### 安装步骤
在命令行界面，执行以下命令安装 OpenCode。
```Bash
npm install -g opencode-ai
```

安装结束后，执行以下命令查看安装结果，若显示版本号则安装成功。
```Bash
opencode --version
```

### 配置工具

1. 编辑OpenCode的配置文件`~/.config/opencode/opencode.json`。
   以配置模型`doubao-seed-code-preview-latest`为例，配置信息如下。
   :::tip
   * 支持的model：`doubao-seed-code-preview-latest`、`ark-code-latest`。
   * 替换配置信息中的[<ARK_API_KEY>](https://console.volcengine.com/ark/region:ark+cn-beijing/apikey)。
:::
   ```JSON
   {
     "$schema": "https://opencode.ai/config.json",
     "provider": {
       "myprovider": {
         "npm": "@ai-sdk/openai-compatible",
         "name": "volcengine",
         "options": {
           "baseURL": "https://ark.cn-beijing.volces.com/api/coding/v3",
           "apiKey": "<ARK_API_KEY>"
         },
         "models": {
           "doubao-seed-code-preview-latest": {
             "name": "doubao-seed-code-preview-latest"
           }
         }
       }
     }
   }
   ```

### 开始使用

1. 启动OpenCode：
   ```Shell
   opencode
   ```

2. 输入`/models`，选择配置的`doubao-seed-code-preview-latest`模型并在OpenCode中使用。
   ![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9d36feaf0d9945a682b98fce8fe91b5c~tplv-goo7wpa0wc-image.image)

## 错误码
在编程工具中使用 Coding Plan 套餐时可能会遇到报错，您可以根据错误信息查看对应的报错场景并定位问题，具体可参考[错误码](/docs/82379/1299023)。
