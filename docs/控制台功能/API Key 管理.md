# API Key 管理
当您通过代码调用大模型或应用时，需要获取API Key作为调用时的鉴权凭证。方舟控制台上的 API Key 管理页面可以帮助您创建和管理API Key。
## 功能入口
[API Key管理](https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey?apikey=%7B%7D)  
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b1e0cd3207584c86ac7195eaec730e04~tplv-goo7wpa0wc-image.image =2398x)
## 创建 API Key
创建一个 API Key，作为调用大模型或应用的鉴权凭证。
当前，火山方舟的 API Key 提供以下能力：

| | | \
|功能 |说明 |
|---|---|
| | | \
|控制所属项目 |当您的账号是多人使用，且使用了多个项目空间，如企业、团队的共有账号。您可以在左上角切换项目空间，这样创建的 API Key 只可以用于该项目空间下的资源使用的鉴权凭证。 |
| | | \
|控制权限 |全部：可访问项目下全部资源。 |\
| |自定义：可自定义指定部分资源的访问权限，灵活控制 API Key 对项目内特定资源的使用权限。 当前支持对 **Model ID**、**自定义推理接入点** 维度的资源进行细致的权限设置。 |
| | | \
|IP 调用白名单 |限制调用模型的 IP 地址。若配置，则仅已输入的 IP 地址能够调用模型。 |

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/666b63271fa744a08dba7bddcfe29747~tplv-goo7wpa0wc-image.image =1382x)

## **查询/获取 API Key**
支持在 API Key 管理页面查询已有的 API Key，查看当前 API Key 的状态、权限、最后使用时间、创建人等信息。这些信息有助于用户全面了解 API Key 的使用情况，以便更好地进行管理和维护。 
:::warning
请不要将API Key以任何方式公开，避免被他人使用造成安全风险或资金损失。
:::
## 编辑权限
支持随时调整 API Key 可访问的权限范围，包括 IP 调用白名单、可访问的具体资源等，以满足不同场景下对 API Key 使用权限的灵活配置需求，确保系统安全与业务需求的平衡。 
## 禁用/启用 API Key
禁用后，API Key 会变成 **已禁用** 状态，使用该 API Key 的请求将无法通过鉴权，请谨慎操作。
启用后，API Key 会变成 **生效中** 状态，可正常使用。
## 删除 API Key
当您出于安全角度更新或者不再使用API Key进行鉴权时，您可以删除您的API Key。
## 申请配额
一个主账号下支持创建 50 个API Key，如需更多配额可至 [配额管理](https://console.volcengine.com/ark/region:ark+cn-beijing/quota?quota=%7B%22business%22%3A%22Other%22%2C%22quota%22%3A%22Other%22%2C%22table%22%3A%7B%7D%7D) 页面申请配额。注意需要有配额中心的操作权限。
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c74c4527ac3341adb21541da31134509~tplv-goo7wpa0wc-image.image =2402x)

## 安全设置
控制项目成员只能管理自己创建的 API Key，从而增强 API Key 管理的安全性和规范性。 
默认情况下，项目成员（子账号）可查看并管理当前项目下的所有 API Key；如对权限有更严格管控要求，项目管理员（主账号）可开启 **限制成员权限**。开启后，项目成员仅可查看、编辑、禁用/启用、删除自己创建的 API Key，无权操作他人创建的 API Key。
> 您需要在顶部菜单栏选择资源组后，才能看到**安全设置**信息。

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/31d3c2efcfff47b1bfb6b0e219425683~tplv-goo7wpa0wc-image.image =2408x)