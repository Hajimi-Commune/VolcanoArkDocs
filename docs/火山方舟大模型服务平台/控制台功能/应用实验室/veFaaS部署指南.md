# veFaaS部署指南
高代码智能体支持企业开发者通过[veFaaS控制台](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/overview)进行在线开发与部署。本文为您介绍使用代码部署方式进行veFaas部署时的操作指南。（注：目前主要支持基于自定义镜像代码开发和部署）

# 1. 初始化配置
	

## 1.1. 产品开通与子账号授权
	

### **1.1.1. 产品服务开通与前置准备**
	

- 在使用veFaaS部署服务前，需开通相关产品服务。若首次使用，请按需开通或申请以下产品：
	

- **产品名称** | **产品概述** | **是否为计费项目** | **是否需要申请开白** | **备注**
- [veFaaS](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/overview) | 函数服务（Volcano Engine Function as a Service，veFaaS）是事件驱动的无服务器函数托管计算平台，支持快速创建和部署函数，连接云上中间件和数据库产品，帮助企业低成本构建复杂应用。 | [产品计费](https://www.volcengine.com/docs/6662/107454) | 否 | 必选
- [veFaaS Native函数](https://www.volcengine.com/docs/6662/116699) | Native 运行时支持用户基于原生的 HTTP 框架进行代码开发。使用 Native 运行时开发函数，需要遵循火山引擎的函数服务 Native 运行时规范。本文介绍 Native 运行时函数（下文简称 “Native 函数”）的运行环境、开发方法、日志采集及部署方法。 | ^^ | 否 | 必选
- [veFaaS 异步函数](https://www.volcengine.com/docs/6662/1158775) | 异步任务是函数服务全新推出的函数运行机制，通过异步模式响应调用请求，在完成事件调度后立即返回 RequestId 结束调用操作，无需阻塞调用端资源。同时，异步任务支持追踪并保存任务各阶段的状态，提供丰富的任务控制和可观测能力。 | ^^ | 否 | 可选
- [Serverless 应用托管](https://www.volcengine.com/docs/6662/97175) | 对原生 HTTP 框架应用进行托管。无需修改业务代码，在仅修改服务监听端口及服务启动脚本的情况下，便可平滑迁移至函数服务。 | ^^ | 否 | 必选
- [CR（镜像仓库）](https://console.volcengine.com/cr/region:cr+cn-beijing/instances) | 火山引擎镜像仓库（Container Registry，CR）提供安全高可用的容器镜像、Helm Chart 等符合 OCI 标准的云原生制品托管服务，方便企业用户管理容器镜像和 Helm Chart 的全生命周期。 | [产品计费](https://www.volcengine.com/docs/6420/79158) | 否 | 必选
- [VPC](https://console.volcengine.com/vpc/region:vpc+cn-beijing/vpc) | 私有网络（VPC，Virtual Private Cloud）为云上资源构建隔离的、自主配置和管理的虚拟网络环境。不同私有网络之间网络相互隔离，您可在自己的私有网络中创建和管理云服务器、负载均衡等云资源。 | [产品计费](https://www.volcengine.com/docs/6401/75314) | 否 | 必选
- [TLS（日志服务）](https://console.volcengine.com/tls/region:tls+cn-beijing/) | 火山引擎日志服务 TLS（Tinder Log Service）是日志和 Trace 类数据的一站式服务平台，提供数据采集、存储、检索分析、加工、消费、投递、监控告警、可视化等功能，适用于业务运维监控、数据统计分析等场景。 | [产品计费](https://www.volcengine.com/docs/6470/1215813) | 否 | 必选
- [APIG](https://console.volcengine.com/veapig/region:veapig+cn-beijing/gateway) | API 网关（API Gateway，APIG）是基于云原生的、高扩展、高可用的云上网关托管服务。在传统流量网关的基础上，集成丰富的服务发现和服务治理能力，打通微服务架构的内外部网络，快速实现各服务之间、服务与客户端之间的安全通信。 | [产品计费](https://www.volcengine.com/docs/6569/185249) | 否 | 必选
- [云监控](https://console.volcengine.com/cloud-monitor/overview) | 云监控服务是云上一站式监控告警解决方案。 云监控可以收集并可视化展示各类云产品的资源状态，帮助您全面了解其健康状况，及时识别异常状态并发送告警通知，确保业务平稳运行、提升运维效率。 | [产品计费](https://www.volcengine.com/docs/6408/79155) | 否 | 必选

- 若使用子用户进行产品服务开通，则需为其配置权限：`iam:CreateServiceLinkedRole`，步骤如下：
	
	  **步骤一：** 通过主账号，进入[火山引擎访问控制台](https://console.volcengine.com/iam/identitymanage/user)
	  **步骤二：** 增加最小权限可在[决策管理](https://console.volcengine.com/iam/policymanage?scope=All)处新建自定义策略`CreateServiceLinkedRole`
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_789937f0842699968ccf0df5c798179b.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_07771f10e44dd9dd9b4eddcaaa60dba8.png)
**步骤三：** 授权子账号用户产品开通权限
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e92e4083bfd46dbbfcf0db344428741b.png)

### **1.1.2 子用户产品使用权限配置**
	

- 产品开通完成后，为子用户添加vsFass部署相关产品使用权限的方法如下，如需了解火山引擎[IAM用户创建与授权](https://www.volcengine.com/docs/6257/94013)可详见产品文档。
	
	  **步骤一**：进入[访问控制台](https://console.volcengine.com/iam/identitymanage/user)，点击导航栏的“身份管理-用户”处，对所需授权的子账号用户进行【管理】操作。
	![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1685311e2fe5e8e4bc23934334907018.png)
	  **步骤二：** 进入“用户详情”页面后点击【权限→ 添加权限→选择策略】, 搜索并选择以下策略：
	- VeFaaSFullAccess
		
	- CRFullAccess
		
	- VPCFullAccess
		
	- TLSFullAccess
		
	- APIGFullAccess
		
	- CloudMonitorReadOnlyAccess
		
	  选择后点击【确定】，则该步骤授权完成。
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bc1d541de3395160ced34a2c096b186c.png)

## 1.2 创建函数配置
	

以下内容介绍用户在[创建函数](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/function/create)页面中如何操作配置：

### 1.2.1. 运行时配置

为满足不同场景下的用户需求，函数服务提供多种类型的函数，详细选择说明见[函数创建方式选型](https://www.volcengine.com/docs/6662/97175)。
	

#### （1）创建事件函数

- 如果您希望按照函数服务定义的接口编写程序，请选择【创建「事件函数」】。
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_85d61bbdf4ec0b34b12cde2d2e5bd5f8.png)

- 用户可灵活选择编程语言、部署方式
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_0f309a31ba4687934e8b012774275890.png)

- 用户可配置内存规格、超时时间、单实例并发数等选项
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a8503d859a02ec0fe7dc76788f21bebb.png)

#### （2）创建 Web 应用

Web 应用帮助用户托管原生 Web 框架应用，支持基于各语言的流行框架（Golang Gin 等）或自定义容器镜像编写 Web 程序，以及迁移已有的 Web 应用。

- 如果您希望使用Web 应用托管，请选择【创建「Web」应用】。
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_131f94e1712af064722137c1e4ef3794.png)

- 用户可配置编程语言、部署方式、Webserver 模式、启动命令、监听端口
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_eb877ec2cc1cfaa40108f15a3748fdff.png)

- 用户可配置计算模式、内存规格、异步任务、超时时间（建议3min以上）、单实例并发数等选项
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_7020507dd0b16087b08d89bd5fd020c6.png)

#### （3）创建微服务应用

微服务应用可托管任意语言和框架的容器应用，提供代码包和容器镜像部署两种部署方式，支持自动弹性，按量计费，主要应用于微服务应用，MQ 消费服务等领域。

- 如果您希望使用微服务应用托管，请选择【创建「Web」应用】。
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_4d253dea9c384d9cea64003a8191f267.png)

- 用户可配置编程语言、部署方式、Webserver 模式、启动命令
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_418f8fd76ed70f41ee1adccac793d41d.png)

- 用户可配置计算模式、内存规格
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_72fd82c56a9053af1f9ac01e38701131.png)

#### （4）创建任务

任务帮助用户对一次性执行的脚本任务进行托管，通过任务模式响应异步调用请求，并追踪和保存任务各个阶段的状态，提供丰富的任务控制和可观测能力。

- 如果您希望追踪和保存任务各个阶段的状态，请选择【创建「任务」】。
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_6968a1626d9f2fcd7c6ac21d899f36ab.png)

- 用户可配置编程语言、部署方式、Webserver 模式、启动命令
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d7a9ccd7b9cc793767072a9a926d8a98.png)

- 用户可配置计算模式、内存规格、超时时间（建议3min以上）等选项
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_61252d37335bd57f06df1b69ed7c1196.png)

### 1.2.2. 网络配置
	

- 如需访问用户私有网络（VPC）下资源（mysql/redis）或在使用知识库场景下对数据安全有较高需求，请在[创建函数](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/function/create)时勾选启用VPC访问
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_742001ece37bbc60b9e29a8737e9d91d.png)

### 1.2.3. 可观测性配置
	

#### （1）日志配置

- 日志功能启用与配置步骤如下：
	
	  **步骤一：** 勾选启用日志功能
	  **步骤二：** 创建相应日志项目（如已创建，可忽略此步骤）
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d4173f5e92e7cad3a9dd0858f1f12a4b.png)
**步骤三：** 配置对应日志项目
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a0c268b2b92ecca9e552b45d328f8431.png)

#### （2）trace配置

**步骤一：** 进入“[日志服务](https://console.volcengine.com/tls/region:tls+cn-beijing/trace-service?)”点击导航栏的“trace服务”后点击【创建trace实例】（可选择与日志配置在同一项目下）
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_6223b07a5c086432f8beffa801a6d702.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_58a34636007be506f889c1c2607575f0.png)
**步骤二：** 如图，获取tls私网访问地址和trace日志主题ID
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_0bead972adc3f7757036d5d592376885.png)
**步骤三：** 进行trace索引配置，用户可选择在Attributes下配置二级索引: request\_id, input, output
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d1ef23e6b7a6fe1ac23cdfd1f0a19913.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_eac0e3488e4a96209a88f82e1f9c1aee.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_67b6531ca3fabcf409f0be9902a615dc.png)
<br>

### 1.2.4. 环境变量配置
	

- 单击 【新增环境变量】，通过设置 “键” 和 “值”，增加环境变量
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ff652cc8c08b6ba1ecc517af3b644d2b.png)

- 类型 | 键 / Key | 值 / Value
- 鉴权 | VOLC\_ACCESSKEY | 火山ak
- ^^ | VOLC\_SECRETKEY | 火山sk
- Trace依赖 | REGION（tls地域） | e.g. cn-beijing
（Optional，配置后可查看各函数调用时间）
- ^^ | TRACE\_ENDPOINT（tls私网访问地址:4317） | e.g. https://tls-cn-beijing.ivolces.com:4317
- ^^ | TRACE\_TOPIC（trace日志主题id） | e.g. a8adc928-69e1-4839-a4a0-929f4f188888
- 自定义信息 | ACCOUNT\_ID | e.g. 2100xxxxxx
（Optional）
- ^^ | RESOURCE\_TYPE | e.g. bot
- ^^ | RESOURCE\_ID | e.g. browsing-pipeline

### 1.2.5. 镜像相关配置
	

以下介绍镜像相关配置步骤，更多可详见[产品文档](https://www.volcengine.com/docs/6420/68643)（注：用户若已创建镜像仓库，以下步骤可跳过）
**步骤一：** 在部署方式中选择：【容器镜像→镜像仓库】，部署前需通过火山的[镜像仓库](https://console.volcengine.com/cr/region:cr+cn-beijing/instances)上传镜像。如不存在则需点击【创建镜像】，跳转至[镜像仓库](https://console.volcengine.com/cr/region:cr+cn-beijing/instances)页面后点击【创建实例】
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_da09615c2ec7dd3008a40b028b976c5c.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bd1e7611774563b9ff200bc8697c83c6.png)
**步骤二：** 点击【创建命名空间】，并输入信息进行创建
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_54da6764773f5d723e7859f97e9ff607.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f6a743849db9c2bc07c34b0dac426ae5.png)
**步骤三：** 创建完成后，请进入实例【设置仓库实例密码】
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_8084d3783697d728f3aaf4ae83809e32.png)
**步骤四：** 点击导航栏中的“访问控制”，开启公网访问（注：配置0.0.0.0/0放行所有IP访问，如需安全控制可自行配置IP段）
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_abc08f64bceeb14fc1a2c498041c30b2.png)

### 1.2.6. 配置API网关触发器
	

#### （1）创建VPC与子网

下面为您介绍创建VPC与子网步骤，更多可详见[产品文档](https://www.volcengine.com/docs/6401/69467)（注：如用户账号下已经有VPC，可跳过以下步骤）
**步骤一：** 进入[私有网络控制台](https://console.volcengine.com/vpc/region:vpc+cn-beijing/vpc)，点击【创建私有网络】
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cb5bd23fcee351db38a7cf0e950b0a89.png)
**步骤二：** 配置可用区A、可用区B两个子网
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_9433389963f97bf096abd3f63a04dc73.png)

#### （2）创建API网关

下面为您介绍创建API网关步骤，更多可详见[产品文档](https://www.volcengine.com/docs/6569/85693)（注：如用户账号下已经有api网关，可跳过以下步骤）
**步骤一：** 进入[API网关控制台](https://console.volcengine.com/veapig/region:veapig+cn-beijing/gateway)，点击【创建实例】。按需创建apig实例（网关类型：标准网关和serverless网关均可），需要配置双可用区
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_3380854a42b7cac0d61a2916f9489af2.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_43d58bb923acea2578014a0bfaa70968.png)
**步骤二：** 进入对应实例，点击【创建服务】
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_fa76d84bdabb62ef1ad491cf3c1d1f24.png)
<br>

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bb10ca846c2e6f812558e5386567a110.png)
**步骤三：** 在对应服务下开启JWT认证以支持鉴权
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e9bebaa95578f778e8bb0bf0fc047ad6.png)

#### （3）配置API网关触发器

**步骤一：** 完成上述前置工作后，在函数创建控制台发布函数
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_38f355f14d362051b816cd5b058aa3f7.png)
**步骤二：** 选择已建立的api网关

- 智能体：配置智能体api路由路径（e.g. /api/v2/bots/chat）
	
- 知识库（可选）：配置 knowledge api 路由路径 (e.g. /api/v2/kb)
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_4e5c8f15b748c18ddc80748ac6e34584.png)
**步骤三：** 建立完成后可通过公网/私网地址访问api，以访问智能体chat接口及知识库文档上传接口

- 访问智能体chat接口
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_000d1627e2e7c1c139f86d4f42f6d4bf.png)

- 访问知识库文档上传接口
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bd113cb2791042c947cbe45286fe332c.png)

# 2. 代码开发与部署
	

以下为您介绍基于自定义镜像代码与基于Python函数代码的两种代码部署方式（注：使用知识库场景下建议使用Python函数代码开发和部署并在2.3提供简单示例）

## 2.1 基于自定义镜像代码开发和部署
	

### 2.1.1 目录结构
	

```shell
├── Dockerfile
├── code
│   ├── __init__.py
│   └── main.py
└── run.sh
```

#### （1） run.sh

```shell
#!/bin/bash
set -ex
#  shellcheck  disable=SC2046
cd `dirname $0`

exec python3 code/main.py
```

#### （2） main.py

```python
import os
from typing import AsyncIterable
from ark.core.task import task
from ark.core.idl.maas_protocol import MaasChatRequest, MaasChatResponse
from ark.core.launcher.local.serve import launch_serve

@task(distributed=False)
async def main(request: MaasChatRequest) -> AsyncIterable[MaasChatResponse]:
   # code here
   yield response

if __name__ == "__main__":
    port = os.getenv('_FAAS_RUNTIME_PORT')
    launch_serve(
        package_path="main",
        endpoint_path="/api/v2/bots/chat",
        port=int(port) if  port else  8080,
        health_check_path="/v1/ping")
```

#### （3）可观测性代码修改

- 在需要监测的函数前增加装饰器`@task()` 即可使用tls的trace功能查看函数的调用时长、开始时间、结束时间、输入输出、request id等信息

#### （4）Dockerfile

- Version = 0.1.x
	

```dockerfile
FROM python:3.9.5

ARG SDK_WHEEL="ark-0.1.4-py3-none-any.whl"
ARG SDK_URL

COPY code /opt/application/code
COPY run.sh /opt/application/run.sh

RUN wget ${SDK_URL} -O ${SDK_WHEEL}
RUN pip install ${SDK_WHEEL}

WORKDIR /opt/application
USER root

RUN chmod a+x /opt/application/run.sh
CMD /opt/application/run.sh
```

### 2.1.2 打包发布
	

打包发布可选择使用命令行工具发布或逐步操作两种方式：

#### （1）方式一：**使用命令行工具发布**

**步骤一：** 安装SDK依赖
**步骤二：** 创建工程项目，具体指令可参考[命令行工具](https://www.volcengine.com/docs/82379/1262349)\- **ark** **create**
**步骤三：** 一键推送发布镜像，具体指令可参考[命令行工具](https://www.volcengine.com/docs/82379/1262349)\- **ark** **push**

#### **（2）方式二：逐步操作**

**步骤一：** 启动docker daemon

- mac环境：启动docker desktop即可
	
- linux环境：执行sudo systemctl start docker
	

**步骤二：** 登录[镜像仓库控制台](https://console.volcengine.com/cr/region:cr+cn-beijing/instance/)，进入实例，点击【命名空间】
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_285d705019f7bdce7e692c4488cd5a7e.png)

- 登陆实例命令可在cr界面上获得
	

```shell
docker login --username={userName}@{userId} {CRHost}
```

**步骤三：** 编译镜像

- CRHost: 镜像仓库实例访问域名
	
- CRNamespace: 镜像仓库实例命名空间
	
- ImageName: 镜像名
	
- ImageTag: 镜像版本（可通过填写latest/1.0/2.0来区分不同版本代码，如没有版本管理需求可直接填写latest）
	

```shell
DOCKER_DEFAULT_PLATFORM="linux/x86_64" docker build  . -f ./Dockerfile -t {CRHost}/{CRNamespace}/{ImageName}:{ImageTag}
```

**步骤四：** 上传镜像

```shell
docker push {CRHost}/{CRNamespace}/{ImageName}:{ImageTag}
```

**步骤五：** 同步与发布新版本代码

- 修改代码后需在镜像仓库示例处点击【镜像修改】同步修改后的镜像
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_819642a2120074a6d4f2ccb407ae5b20.png)

- 选择最新打包的镜像同步至vefaas
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_8c770e053526ba3275b9330270d0140b.png)

- 根据需求进行全量发布/灰度发布，并配置函数版本（注：建议使用灰度发布，防止变更失误影响线上服务）
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1862816238c3aa2de2e14d3dcebe26fe.png)

- 如选择灰度发布，用户可进行配置比例
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_db2eeed909c6db9cae418e5b2caa203d.png)
<br>

## 2.2. 基于Python函数代码开发和部署
	

### 2.2.1. 安装SDK依赖
	

更多可参见[依赖安装](https://www.volcengine.com/docs/6662/160687)文档

- 登陆devbox（linux环境开发机），基于conda创建**python3.8.17环境**
	

```shell
conda create --name python3.8 python==3.8.17
```

- 启用虚拟环境
	

```shell
conda activate python3.8
```

- 在项目目录下安装SDK依赖包

### 2.2.2. 编写index.py
	

```python
from ark.core.launcher.vefaas import faas_handler

@task()
def main(request: MaasChatRequest) -> Iterable[MaasChatResponse]:
    # wite your code here
    yield response

def handler(event, context):
    return faas_handler(event=event, context=context, runnable_func=main)
```

### 2.2.3. 打包上传更新代码
	

- 打包index.py和site-packages到压缩包deployment.zip中
	

```shell
zip -r ./deployment.zip .
```

- 上传更新后的代码压缩包
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b7bb5ec7b453a36d731c2ff5521fd8bc.png)

### 2.2.4. 发布新版本代码
	

- 根据需求进行全量发布/灰度发布，并配置函数版本（注：建议使用灰度发布，防止变更失误影响线上服务）
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_3263fd7885d15d384798f025e5aa8cf1.png)

- 如选择灰度发布，用户可进行配置比例
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d30f519c44bdfb426a6192dfb0a0a6ef.png)

## 2.3. 知识库场景示例：基于Python函数代码开发和部署
	

使用知识库场景下建议使用Python函数代码开发和部署，可参照2.2，本节提供简单示例。

### 2.3.1. 安装SDK依赖
	

该步骤同2.2

### 2.3.2. 编写index.py
	

```python
def handler(event, context):
    # Log incoming request.
    #
    # NOTE: the log here is only for debug purpose. It's recommended to delete those debug/info
    # logs and only print log where error occurs after your business logic is verified and ready to
    # go production, nor the log amount may be too huge.
    LOGGER.info("received new request, event content: %s", event)

    body = event["body"]
    action = event["path"]
    LOGGER.info("body: %s action: %s", body, action)

    params = json.loads(body)
    ret_code, msg = asyncio.run(main(params))

    if ret_code == 0:
        result = {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': msg
        }
    else:
        result = {
            'statusCode': 500,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': msg
        }
    return result
    
    @task()
async def main(params: Dict[str, Any]):
    """
        main func
        """
    ak = os.getenv("VOLC_ACCESSKEY")
    sk = os.getenv("VOLC_SECRETKEY")
```

### 2.3.3. 打包上传更新代码及发布函数
	

该步骤同2.2
<br>

# 3. FaaS 使用和运维
	

## 3.1 API调用
	

### 3.1.1 API直接调用
	

- 用户可进入“[API网关控制台](https://console.volcengine.com/veapig/region:veapig+cn-beijing/gateway)\-服务列表”，点击认证管理获得JWTToken，有效期为7天。具体步骤可参见[文档](https://www.volcengine.com/docs/6569/120515)
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_9020c3441fbe7ac37efa65a8c116cd74.png)

- 如使用API调用获取jwttoken[可参考文档](https://www.volcengine.com/docs/6369/67265)
	

#### （1）GetJwtToken

Version: 2021-03-03

- **请求参数**
	

| **参数名称** | **类型** | **是否必选** | **示例值** | **描述** |
| --- | --- | --- | --- | --- |
| Action | String | 是 | GetJwtToken | 公共参数，本接口值：GetJwtToken |
| ServiceId | String | 选填 |  | 服务Id（即将下线这个字段，之前是通过ServiceId来获取toke，兼容考虑） |
| GatewayId | String | 选填 |  | 网关Id，Token是全网关生效 |

ServiceId、GatewayId获取方法：进入[API网关控制台](https://console.volcengine.com/veapig/region:veapig+cn-beijing/gateway)，点击导航栏中的“路由管理-服务列表”，点击对应服务获取
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e57c21a47bf9d89af112607d5b02356c.png)

- **请求示例**
	

```json
POST /?Action=GetJwtToken&Version=
Content-Type:application/json
{
    "ServiceId": "xxxxxxxx",
    "GatewayId": "xxxxxxxx"
}
```

- **返回参数**
	

- **参数名称** | **类型** | **示例值** | **描述**
- JwtToken | String

- **返回示例**
	

```json
HTTP/1.1 200 OK
Content-Type:application/json
{
    "ResponseMetadata": {
        "RequestId": "202211302208xxxx",
        "Action": "GetJwtToken",
        "Version": "",
        "Service": "",
        "Region": "cn-beijing"
    },
    "Result": {
        "JwtToken": "xxxxxxxx"
    }
}
```

- **错误码**
	- 500：内部错误
		
	- 400：参数错误
		
	- 429：限流
		
	- 403：无权限
		
- **调用****`GetJwtToken`** **方法**
	
	> 注：调用api前需构造签名填充header，签名构造demo源码
	> 
	> - Java: https://github.com/volcengine/volc-openapi-demos/blob/main/signature/java/Sign.java
	> 	
	> - Golang: https://github.com/volcengine/volc-openapi-demos/tree/main/signature/golang
	> 	
	> - Python: https://github.com/volcengine/volc-openapi-demos/blob/main/signature/python/sign.py
	> 	
	> - c#: https://github.com/volcengine/volc-openapi-demos/blob/main/signature/csharp/Program.cs
	> 	
	
	<br>
	
	- Endpoint: open.volcengineapi.com
		
	- Path: /
		
	- Version: 2021-03-03
		
	- Action: GetJwtToken
		
	- Service: apig
		
	- Service id： API网关的服务ID（路径：【[API网关控制台](https://console.volcengine.com/veapig/region:veapig+cn-beijing/gateway)→ 路由管理→ 服务列表】，进入对应服务获取）
		
	![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_74a40b9abcb1f4eb0c73281d4afd4c87.png)
	
	```shell
	curl --location 'https://open.volcengineapi.com?Version=2021-03-03&Action=GetJwtToken'
	--header 'Region: cn-beijing'
	--header 'ServiceName: apig'
	--header 'Content-Type: application/json'
	--data '{
	    "ServiceId": {{service id}}
	}'
	```
	

<br>

**For Java 开发场景，也可使用下面的Sample Code来实现JwtToken的自动获取和缓存：** 

- **引入SDK**
	

```java
<dependency>
    <groupId>com.volcengine</groupId>
    <artifactId>volc-sdk-java</artifactId>
    <!-- 使用最新版本 -->
    <version>1.0.155</version>
</dependency>
```

- **项目工程中增加实现类**
	

```java
package com.volce.demo;

import com.alibaba.fastjson.JSONObject;
import com.volcengine.error.SdkError;
import com.volcengine.model.ApiInfo;
import com.volcengine.model.Credentials;
import com.volcengine.model.ServiceInfo;
import com.volcengine.model.response.RawResponse;
import com.volcengine.service.BaseServiceImpl;
import org.apache.http.Header;
import org.apache.http.NameValuePair;
import org.apache.http.message.BasicHeader;
import org.apache.http.message.BasicNameValuePair;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

public class ApiGatewayAuthService extends BaseServiceImpl {

    private static final int CONNECT_TIMEOUT = 5000;
    private static final int SOCKET_TIMEOUT = 5000;
    private static final long TOKEN_EXPIRE_TIME = 604800 * 1000;

    // key: gatewayId#serviceId
    private Map<String, Long> _expireTimes = new HashMap<>();
    private Map<String, String> _cache = new HashMap<>();

    public ApiGatewayAuthService(String host, String region) {
        super(buildServiceInfo(host, region), buildApiInfo());
    }

    public ApiGatewayAuthService(String host, String region, String ak, String sk) {
        super(buildServiceInfo(host, region), buildApiInfo());
        this.credentials.setAccessKeyID(ak);
        this.credentials.setSecretAccessKey(sk);
    }

    public String getGatewayJwtTokenWithAutoRefresh(String gatewayId, String serviceId) {
        String cacheKey = String.format("%s#%s", gatewayId, serviceId);
        Long expireTime = _expireTimes.get(cacheKey);
        long now = System.currentTimeMillis();
        if (expireTime == null || expireTime <= now) {
            String token = getGatewayJwtToken(gatewayId, serviceId);
            _cache.put(cacheKey, token);
            expireTime = now + TOKEN_EXPIRE_TIME;
            _expireTimes.put(cacheKey, expireTime);
            return token;
        } else {
            return _cache.get(cacheKey);
        }
    }

    public String getGatewayJwtToken(String gatewayId, String serviceId) throws RuntimeException {
        JSONObject reqJson = new JSONObject();
        reqJson.put("GatewayId", gatewayId);
        reqJson.put("ServiceId", serviceId);
        RawResponse response = this.json("get_jwttoken", null, reqJson.toJSONString());
        if (response.getCode() != SdkError.SUCCESS.getNumber()) {
            throw new RuntimeException("fail to get jwt token, got response = " + response);
        } else {
            JSONObject rspJson = JSONObject.parseObject(new String(response.getData()));
            JSONObject resultJson = rspJson.getJSONObject("Result");
            if (resultJson != null) {
                return resultJson.getString("JwtToken");
            }
        }
        return null;
    }

    private static ServiceInfo buildServiceInfo(String host, String region) {
        Map<String, Object> serviceInfoMap = new HashMap<>();
        serviceInfoMap.put("ConnectionTimeout", CONNECT_TIMEOUT);
        serviceInfoMap.put("SocketTimeout", SOCKET_TIMEOUT);
        serviceInfoMap.put("Host", host);
        serviceInfoMap.put("Header", new ArrayList<Header>() {
            {
                this.add(new BasicHeader("Accept", "application/json"));
            }
        });
        serviceInfoMap.put("Credentials", new Credentials(region, "apig"));
        return new ServiceInfo(serviceInfoMap);
    }

    private static Map<String, ApiInfo> buildApiInfo() {
        Map<String, ApiInfo> apiInfoMap = new HashMap<>();
        Map<String, Object> getJwtTokenParam = new HashMap<>();
        getJwtTokenParam.put("Method", "POST");
        getJwtTokenParam.put("Path", "/");
        getJwtTokenParam.put("Query", new ArrayList<NameValuePair>() {
            {
                this.add(new BasicNameValuePair("Action", "GetJwtToken"));
                this.add(new BasicNameValuePair("Version", "2021-03-03"));
            }
        });
        getJwtTokenParam.put("Form", new ArrayList<>());
        getJwtTokenParam.put("Header", new ArrayList<>());
        ApiInfo getJwtTokenApi = new ApiInfo(getJwtTokenParam);
        apiInfoMap.put("get_jwttoken", getJwtTokenApi);
        return apiInfoMap;
    }
}
```

- **使用示范**
	

```java
import com.volce.demo.ApiGatewayAuthService;

public class Test {
    public static void main(String[] args) {
        ApiGatewayAuthService auth = new ApiGatewayAuthService("open.volcengineapi.com",
                "cn-beijing",
                "{yourAK}",
                "{yourSK}");
        // 直接获取JwtToken（无缓存，每次都调用）
        auth.getGatewayJwtToken("{gatewayId}", "{serviceId}");
        // 自动缓存获取JwtToken（默认七天轮转一次）
        auth.getGatewayJwtTokenWithAutoRefresh("{gatewayId}", "{serviceId}");
    }
}
```

#### （2）智能体API请求

- 请求智能体 api时带上JWT Token即可完成请求
	
- APIHost：api触发器的公网域名（如果走私网访问可以配置私网域名）
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1ee5883145ba48099e314065b958414f.png)

```shell
curl --location '{APIHost}/api/v2/bots/chat'
--header 'Authorization: Bearer {JWTToken}'
--header 'Content-Type: application/json'
--data '{
    "messages": [
        {"role": "user", "content": "查今天的新闻"}
    ]
}'
```

## 3.2 可观测性
	

### 3.2.1 查看Trace
	

- 用户可通过进入“[日志服务](https://console.volcengine.com/tls/region:tls+cn-beijing/trace-service?)”控制台查看trace
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ba3a30f30f850cffed562e4bad23039e.png)

- 在已配置的trace对应日志项目下可选择trace日志主题进行检索
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_29ffdb46ec43cc80353e185d12e7ebaa.png)

- 可根据response header中的“x-tt-logid”作为request\_id索引查询trace的各指标
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b240e0d7dc05bdfb6dec0a74965919fe.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1751f4f048f598876bba4ce817452495.png)

- trace各字段含义
	

| 字段 | 含义 |
| :-- | :-- |
| Name | 函数名 |
| SpanID | 所调用函数ID |
| ParentSpanID | 父函数ID |
| StatusCode | 函数运行返回状态 |
| StatusDescription | 运行失败状态描述 |
| Attributes.input | 函数输入 |
| Attributes.output | 函数输出 |
| Attributes.request\_id | x-tt-logid |
| StartTime | 函数执行起始时间 |
| EndTime | 函数执行终止时间 |
| Duration | 函数执行时长（ms） |

### 3.2.2 查看日志
	

- 用户可通过[faas控制台](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/overview)的日志页面查看stdout/stderr输出
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_4e046b33d269bd16be5fb3cb15c178f4.png)

### 3.2.3 查看监控
	

- 用户可在faaks控制台的监控模块查看运行各指标
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_fa2caaf3329e6c8f5ce84713243a2d1c.png)

## 3.3 Webshell
	

- 实例未销毁时可通过webshell进入容器内排查问题
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_092ff72c2920f941107e283edab71ab4.png)

## 3.4 知识库异步任务日志观测
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b8aeed96ff2b504c3236ec514fcf6e12.png)

# 4. 云监控运维
	

## 4.1 云监控增加联系人与认证
	

- 用户可进入[云监控控制台](https://console.volcengine.com/cloud-monitor/alert/contact)，点击导航栏中的“联系人”，进行联系人的创建与认证
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_68db48858743c95a8598c10454ad60e6.png)

## 4.2 veFaas监控中创建告警策略
	

- 用户可进入[veFaaS控制台](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/overview)，选择已创建的函数点击【监控】，并创建告警策略
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_40ecd57f6945f7e981f62b00cdb93f34.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d897d8499960a622121c89d0aa19b35f.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_97f8de82f861a9c317608c861379ba6e.png)

## 4.3 观测报警触达

- 若触发请求，则观测报警触达
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_7311cae51965f4b4937caebeef1549c6.png)
<br>
