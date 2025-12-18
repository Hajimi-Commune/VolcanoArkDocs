# 使用IAM进行访问控制教程
为确保您的云资源使用安全，如非必要都应避免直接使用火山引擎账号（即主账号）来访问火山方舟。推荐的做法是使用IAM身份（即IAM用户和IAM角色）来访问火山方舟。

# 基本概念

- 名称 | 说明
- 主账号 | 火山主账号
- * 账号（又称为主账号）可以看作是一个特殊的用户（被称为根用户，root user），是云服务资源的拥有者，也是资源计量、资源计费的主体。主账号默认拥有账号下所有权限。
- IAM用户或子账号 | IAM用户即IAM账号，是IAM的一种实体身份类型。
- * IAM用户由账号（主账号）或具有管理员权限的其他IAM用户、IAM角色创建，创建成功后，归属于该火山主账号，它不是独立的火山账号。
- * IAM用户不拥有资源，不能独立计量计费，由所属的主账号统一付费。
- * IAM用户必须在获得授权后，才能登录控制台或使用API访问火山主账号下的资源。
- > 更多请查看：[IAM用户](https://www.volcengine.com/docs/6257/64977)
- 用户组 | 用户组是IAM的一种实体身份类型，用户组可以对职责相同的IAM用户进行分类并授权，从而更高效地管理IAM用户及其权限。
- * 在IAM用户职责发生变化时，只需将其移动到相应职责的用户组下，不会对其他IAM用户产生影响。
- * 当用户组的权限发生变化时，只需修改用户组的权限策略，即可应用到所有IAM用户。
- > 更多请查看：[用户组管理](https://www.volcengine.com/docs/6257/104988)
- 角色 | IAM角色是一种虚拟用户，可以被授予一组权限策略。与IAM用户不同，IAM角色没有永久身份凭证（登录密码或访问密钥），需要被一个可信实体扮演。扮演成功后，可信实体将获得IAM角色的临时身份凭证，即安全令牌（STS Token），使用该安全令牌就能以IAM角色身份访问被授权的资源。
- > 更多请查看：[角色免密登录控制台](https://www.volcengine.com/docs/6257/160179)

# 基于身份的系统预设策略
## 什么是系统预设策略
权限策略是用语法结构描述的一组权限的集合，可以精确地描述被授权的资源集、操作集以及授权条件。火山引擎访问控制（IAM）产品提供了两种类型的权限策略：系统策略和自定义策略。系统策略统一由火山引擎各产品团队创建，策略的版本更新由火山引擎维护，用户只能使用不能修改。自定义策略由用户管理，策略的版本更新由用户维护。用户可以自主创建、更新和删除自定义策略。
## 产品预设策略
### ArkFullAccess
火山方舟(Ark)管理员用户，拥有所有Ark服务的权限。适合算法、研发等角色，可查看和配置全部资源。主账号或账号管理员可以通过下面链接查看策略详情：[权限策略-ArkFullAccess](https://console.volcengine.com/iam/policymanage/System/ArkFullAccess?tab=content)。
### ArkStandardGlobalAccess
火山方舟（Ark）基础功能使用权限，可查看并配置除模型开通外的所有资源。主账号或账号管理员可以通过下面链接查看策略详情：[权限策略-ArkStandardGlobalAccess](https://console.volcengine.com/iam/policymanage/System/ArkStandardGlobalAccess?tab=content)。
### ArkReadOnlyAccess
火山方舟(Ark)只读用户，拥有Ark服务的只读权限。适合产品、运营等角色，可查看全部资源，包括精调任务、仓库模型、推理接入点等。主账号或账号管理员可以通过下面链接查看策略详情：[权限策略-ArkReadOnlyAccess](https://console.volcengine.com/iam/policymanage/System/ArkReadOnlyAccess?tab=content)。
### ArkExperienceAccess
火山方舟（Ark）体验中心权限，可查看并体验模型广场中的模型，适合前期体验方舟以及模型能力阶段的用户。 主账号或账号管理员可以通过下面链接查看策略详情：[权限策略-ArkExperienceAccess](https://console.volcengine.com/iam/policymanage/System/ArkExperienceAccess?tab=content)。
### ArkGlobalInitAccess
火山方舟（Ark）**项目隔离**基本权限，用于搭配标准策略实现项目权限隔离，适合有按项目隔离需求的用户。 主账号或账号管理员可以通过下面链接查看策略详情：[权限策略-ArkGlobalInitAccess](https://console.volcengine.com/iam/policymanage/System/ArkGlobalInitAccess?tab=content)。
## 服务关联角色策略
### ServiceRoleForArk
火山方舟(Ark)的服务关联角色，授权后允许方舟平台访问您账号下的其他服务资源。主账号或账号管理员可以通过下面链接查看信任策略详情：[角色详情-ServiceRoleForArk](https://console.volcengine.com/iam/identitymanage/role/ServiceRoleForArk/)。

# 自定义权限策略
如果系统权限策略不能满足您的要求，您可以创建自定义权限策略实现最小授权。使用自定义权限策略有助于实现权限的精细化管控，是提升资源访问安全的有效手段。
## 什么是自定义权限策略
在基于IAM的访问控制体系中，自定义权限策略是指在系统权限策略之外，您可以自主创建、更新和删除的权限策略。自定义权限策略的版本更新需由您来维护。
## 典型场景-赋予某用户组ArkFullAccess
本用例演示了创建一条模型接入，并且拥有 模型接入的停止/删除/编辑/API调用的权限。
```json
{
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ark:GetEnd",
                "ark:List*",
                "tag:GetTagValues",
                "tag:GetTagKeys",
```

```json
{
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ark:CreateEndpoint",
                "ark:ListFoundationModels",
                "ark:ListCustomModels",
                "ark:ListFoundationModelVersions",
                "ark:ListEndpoints",
                "ark:StopEndpoint",
                "ark:StartEndpoint",
                "ark:GetEndpoint",
                "ark:DeleteEndpoint",
                "ark:UpdateEndpoint",
                "ark:GetModelChargeItem",
                "ark:InnerGetModelUnitPrice",
                "ark:GetEndpointExampleCode",
                "ark:GetApiKey",
                "tag:GetTagValues",
                "tag:GetTagKeys"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

若您需要限制使用范围，可以通过修改Resource范围实现。即可以通过限定用户范围、用户组、Project或资源ID来缩小权限。
```json
"Resource": [
                "trn:iam::2100000xxx:user/username",
                "trn:iam::2100000xxx:group/usergroupname",
                "trn:iam::2100000xxx:project/projectname",
                "trn:ark:cn-beijing:2100000xxx:endpoint/ep-20240407133918-vdsmd3"
                ]
```

## 典型场景-赋予账号项目级权限
当您需要配置IAM账号，使其拥有账号下A项目的权限，但是不具备B项目的权限，即限定用户只在A项目开发/查看/体验。您可以通过组合策略来达成。

- 全局策略 | 项目策略
- 需配置生效的项目 | 账号权限描述
- ArkGlobalInitAccess | ArkExperienceAccess | 火山方舟体验中心权限，可**在项目内**查看并体验模型广场中的模型。
- ArkGlobalInitAccess | ArkReadOnlyAccess | 火山方舟只读权限，可**在项目内**使用体验中心，并查看所有资源，包括精调任务、仓库模型、推理接入点等。
- ArkGlobalInitAccess | ArkStandardGlobalAccess | 火山方舟基础功能使用权限，可**在项目内**查看并配置除模型开通外的所有资源。
- ArkGlobalInitAccess | ArkFullAccess | 火山方舟管理员权限，可**在项目内**查看并配置管理所有资源。

下面以为用户配置`default （默认项目）`的管理员`ArkFullAccess`权限为例，演示完整操作流程。

1. 访问[访问控制-用户](https://console.volcengine.com/iam/identitymanage/user)页面，在需要配置权限的用户名右侧的用户列表的 操作 列中，为单击 添加权限 。
2. 在 授权策略 栏搜索并勾选 ArkGlobalInitAccess 策略，限制到项目资源 选择 否。
3. 在 授权策略 栏搜索并勾选 ArkFullAccess 策略，限制到项目资源 选择 是，并选择用户需要拥有权限的项目，这里选择`default （默认项目）`。
4. 单击提交按钮，生效权限。

同上步骤，您也可以为用户组配置策略，使其拥有某项目的权限。
# 项目管理
项目可以根据资源的用途、权限和归属等维度对您拥有的云资源进行分组，从而实现企业内部多用户、多项目的资源分级管理。每个云资源只能属于一个项目，加入项目不会改变云资源之间的关联关系。
基于项目（即一组资源）进行IAM授权，有利于维护资源独立、数据安全；同时可以从项目维度查看资源消费账单，便于计算云资源使用成本。
## **项目管理使用流程**
本文以实例为例，介绍如何对云资源进行项目管理。项目管理功能的一般使用流程如下：

1. [创建项目](https://www.volcengine.com/docs/6649/94336#%E6%96%B0%E5%BB%BA%E9%A1%B9%E7%9B%AE)：创建一个项目，用于对实例进行项目管理。
2. [为IAM用户添加项目权限](https://www.volcengine.com/docs/6649/173422#%E6%B7%BB%E5%8A%A0%E9%A1%B9%E7%9B%AE%E6%9D%83%E9%99%90)：为子用户授予项目权限。
3. 创建资源并且归属于项目中（资源会放入默认项目组，您可以在创建资源时选择资源所属项目）。
4. （可选）[按项目查看账单](https://www.volcengine.com/docs/6269/94010#%E4%B8%80%E3%80%81%E8%B4%A6%E5%8D%95%E6%98%8E%E7%BB%86)：您可以根据项目筛选账单。
   ![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_43789e6a623c89c97a3c83577de176fb.png)

## 通过项目管理分账
火山方舟已完成项目的对接，因此您可以在 [火山引擎-费用中心](https://console.volcengine.com/finance/bill/split-bill) 根据**项目**查看分账账单。
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f26c56e7c0292f7437e471d389d5348e.png)
更多信息请查看文档[项目分账](https://www.volcengine.com/docs/6649/174601)。
# 标签管理
资源标签是由一组KV键值对组成，您可以通过资源标签从不同维度对云资源进行分类和聚合管理，并且使用于标签制授权和资源分账等场景。火山方舟已经完成了系统标签 `create_by` 的对接，因此您可以在费用中心可以按照用户维度做分账。
## 使用步骤
您可前往“[费用中心-账单管理-费用标签](https://console.volcengine.com/finance/bill/tag/)”，启用 `sys:ark:createdBy` 作为费用标签，启用后，该费用标签将会在账单明细数据中的“标签”列体现；
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e83c4ba7d2ebe7ab1de1d87e8fc41ad5.png)
