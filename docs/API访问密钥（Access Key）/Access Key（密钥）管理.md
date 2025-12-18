# Access Key（密钥）管理

> **说明**
> 
> 仅主账号和IAM用户可以拥有密钥，角色无法拥有密钥。密钥的最佳实践请参考[API密钥最佳实践文档](https://www.volcengine.com/docs/6257/1400244)。

## 获取当前身份的 Access Key

如果您需要获取当前身份的密钥，您可以进入[API访问密钥](https://console.volcengine.com/iam/keymanage/)页面（也可以从顶部导航中头像下拉菜单中的"API访问密钥"入口中进入页面），创建并获取 **Access Key ID** 和 **Secret Access Key**。

## 获取指定IAM用户的 Access Key

如果您需要获取指定IAM用户的 Access Key，参考以下步骤：

1. 进入[用户](https://console.volcengine.com/iam/identitymanage/user)页面。
2. 点击您需要获取密钥的用户名。进入**用户详情**页面。
3. 点击**密钥**选项卡。在**密钥**选项卡内创建并获取子账号的 **Access Key ID** 和 **Secret Access Key**。

## 禁用/启用与删除Access Key

您可以在密钥列表中对正在使用的Access Key进行禁用，禁用后密钥访问将失效，启用后密钥重新生效。密钥删除后密钥将彻底失效，不可恢复，请谨慎操作。
