# API概览

本文介绍费用中心提供的所有 OpenAPI 接口列表。

## 服务信息

| 服务名称 | 版本 | 服务地址 |
| --- | --- | --- |
| billing | 2022-01-01 | https://billing.volcengineapi.com |

## 实例相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [UnsubscribeInstance](UnsubscribeInstance.md) | 退订实例 |
| [ListAvailableInstances](ListAvailableInstances.md) | 批量查询可用实例 |
| [SetRenewalType](SetRenewalType.md) | 设置实例续费类型 |
| [RenewInstance](RenewInstance.md) | 实例续费 |

## 账单中心相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [ListBillDetail](ListBillDetail.md) | 分页查询账单明细 |
| [ListBillOverviewByCategory](ListBillOverviewByCategory.md) | 查询账单总览-账号汇总信息 |
| [ListBillOverviewByProd](ListBillOverviewByProd.md) | 分页查询账单总览-产品汇总信息 |
| [ListBill](ListBill.md) | 分页查询账单 |
| [ListSplitBillDetail](ListSplitBillDetail.md) | 分页查询分账账单 |
| [ListAmortizedCostBillDetail](ListAmortizedCostBillDetail.md) | 查询成本账单明细 |
| [ListAmortizedCostBillMonthly](ListAmortizedCostBillMonthly.md) | 查询成本账单总览 |
| [ListAmortizedCostBillDaily](ListAmortizedCostBillDaily.md) | 查询成本账单按天 |

## 资金服务相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [QueryBalanceAcct](QueryBalanceAcct.md) | 查询用户账户余额信息 |

## 企业财务相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [CreateFinancialRelation](CreateFinancialRelation.md) | 建立财务关系 |
| [ListFinancialRelation](ListFinancialRelation.md) | 查询财务关系 |
| [CancelInvitation](CancelInvitation.md) | 取消企业财务邀约 |
| [HandleInvitation](HandleInvitation.md) | 接受/拒绝企业财务邀约 |
| [ListInvitation](ListInvitation.md) | 查询企业财务邀约 |
| [DeleteFinancialRelation](DeleteFinancialRelation.md) | 解除财务关系 |
| [UpdateAuth](UpdateAuth.md) | 变更财务管理授权点 |
| [CleanUpFinancialRelation](CleanUpFinancialRelation.md) | 删除企业财务关联记录 |

## 订单相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [CancelOrder](CancelOrder.md) | 取消订单 |
| [PayOrder](PayOrder.md) | 支付订单 |
| [ListOrderProductDetails](ListOrderProductDetails.md) | 批量查询订单商品信息 |
| [ListOrders](ListOrders.md) | 批量查询订单信息 |
| [GetOrder](GetOrder.md) | 查询订单详情 |

## 资源包相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [ListResourcePackages](ListResourcePackages.md) | 查询资源包列表 |
| [ListPackageUsageDetails](ListPackageUsageDetails.md) | 查询资源包抵扣明细列表 |

## 算价相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [QueryPriceForRenew](QueryPriceForRenew.md) | 续费询价 |
| [QueryPriceForPayAsYouGo](QueryPriceForPayAsYouGo.md) | 后付费询价 |
| [QueryPriceForSubscription](QueryPriceForSubscription.md) | 预付费询价 |

## 代金券相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [ListCoupons](ListCoupons.md) | 查询代金券信息 |
| [ListCouponUsageRecords](ListCouponUsageRecords.md) | 查询代金券核销记录 |

## 费用管理相关接口

| 接口名称 | 接口功能 |
| --- | --- |
| [ListRecipientInformation](ListRecipientInformation.md) | 查询报警接收人信息 |
| [DeleteBudget](DeleteBudget.md) | 删除预算 |
| [QueryBudgetDetail](QueryBudgetDetail.md) | 查询预算详情 |
| [CreateBudget](CreateBudget.md) | 创建预算 |
| [UpdateBudget](UpdateBudget.md) | 更新预算 |
| [ListBudget](ListBudget.md) | 查询预算列表 |
| [ListBudgetAmountByBudgetID](ListBudgetAmountByBudgetID.md) | 根据预算ID查询预算金额列表 |
| [ListBudgetFilterSubjectInfo](ListBudgetFilterSubjectInfo.md) | 查询预算服务主体筛选项 |
| [ListBudgetFilterTagValue](ListBudgetFilterTagValue.md) | 查询标签value筛选项 |
| [ListBudgetFilterTagKey](ListBudgetFilterTagKey.md) | 查询标签key筛选项 |
| [ListBudgetFilterProduct](ListBudgetFilterProduct.md) | 查询预算的产品信息筛选项 |
| [ListBudgetFilterZoneCode](ListBudgetFilterZoneCode.md) | 查询预算区域筛选项 |
| [ListBudgetFilterRegionCode](ListBudgetFilterRegionCode.md) | 查询预算地域信息筛选项 |
| [ListBudgetFilterPayerID](ListBudgetFilterPayerID.md) | 查询预算的payer账号筛选项 |
| [ListBudgetFilterProject](ListBudgetFilterProject.md) | 查询预算项目信息筛选项 |
| [ListBudgetFilterOwnerID](ListBudgetFilterOwnerID.md) | 查询预算owner账号的筛选项 |
| [ListBudgetFilterBillingMode](ListBudgetFilterBillingMode.md) | 查询预算计费模式筛选项 |

## 相关链接

- [API调用说明](API调用说明.md) - API请求方式和签名机制
- [火山引擎OpenAPI中心](https://api.volcengine.com/api-docs/view?serviceCode=billing&version=2022-01-01) - 在线API文档和调试工具
