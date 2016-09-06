---
ms.openlocfilehash: 579372ca34e4dd0bf7ddd1e78b9a6332fca49d63
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="配置 Azure 的自动化"
   description="描述配置初始使用 Azure 自动化时必须执行的步骤。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/10/2015"
   ms.author="bwren" />

# 配置 Azure 的自动化

本文介绍了最初开始使用 Azure 自动化时必须执行的操作。

## 自动化客户

当您首次启动了 Azure 自动化时，您必须创建至少一个自动化帐户。 自动化帐户允许您隔离您的自动化资源 （运行手册，资产） 从包含在其他自动化客户的自动化资源。 可以使用自动化客户自动化资源分为单独的逻辑环境。 例如，可以使用开发和生产的另一个帐户。

每个自动化客户的自动化资源与单个 Azure 区域，但自动化客户可以管理任何地区的 Azure 服务。 如果您有需要的数据和资源，以便能独立于特定区域的策略是在不同的区域中创建自动化客户的主要原因。

>[AZURE.NOTE] 自动化客户和它们所包含的资源创建的 Azure 预览与门户网站不能访问 Azure 门户中。 如果您想要管理这些帐户或它们与 Windows PowerShell 的资源，则必须使用 Azure 资源管理器模块。 
>
>使用 Azure 门户创建自动化客户可通过任一门户和或者进行管理组的 cmdlet。 创建帐户后，是如何创建和管理资源帐户内的没有区别。 如果您计划继续使用 Azure 的门户，然后您应使用它代替 Azure 预览门户创建任何自动化客户。


自动化的帐户可能被暂停，当使用 Azure 帐户，如逾期付款问题。 在这种情况下，您不能访问该帐户、 任何正在运行的作业将被挂起，和所有的计划都将被禁用。 您将能够查看该帐户，但您将无法查看任何资源。 一旦解决问题和自动帐户已启用，您必须启用您的日程并重新启动已暂停任何运行手册。


## 对 Azure 资源配置身份验证

当您访问使用[Azure cmdlet](http://msdn.microsoft.com/library/azure/jj554330.aspx)的 Azure 资源时，您需要提供身份验证到 Azure 订购。 在 Azure 自动化，这是作为管理员配置为您的订阅的 Azure Active Directory 中的组织帐户。 然后可以创建此用户帐户[的凭据](http://msdn.microsoft.com/library/dn940015.aspx)，并使用它[添加 AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx)在您的 runbook。

>[AZURE.NOTE] Microsoft 帐户，前身为 LiveIDs，不能使用 Azure 自动化。

## 创建一个新的 Azure Active Directory 用户来管理 Azure 的订阅

1. 要管理 Azure 订阅的服务管理员身份登录到 Azure 的门户。
2. 请选择**有效的目录**
3. 选择与您 Azure 订阅关联的目录名称。 如果有必要，您可以更改此关联从**设置 > 订阅 > 编辑目录**。
4. [创建新的活动目录用户](http://msdn.microsoft.com/library/azure/hh967632.aspx)。  为该**类型的用户**选择**组织中的新用户**，请不要不**启用多因素身份验证**。
5. 注意用户的全名和临时密码。
7. 选择**设置 > 管理员 > 添加**。
8. 键入您创建的用户的完整的用户名。
9. 请选择您希望用户以管理订阅。
10. 从 Azure 注销，然后重新登录您刚刚创建的帐户。 系统将提示您更改用户的密码。
11. 创建一个新[Azure 自动化凭据资产](http://msdn.microsoft.com/library/dn940015.aspx)为您创建的用户帐户。 **凭据类型**应为**Windows PowerShell 凭据**。


## 在 runbook 中使用的凭据

您可以检索中使用[Get AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx)活动 runbook 凭据和再使用它[添加 AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx)连接到 Azure 订购。 如果凭据是管理员的 Azure 的多个订阅，则您还应该使用[选择 AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx)指定一个正确。 Windows PowerShell 下面通常出现在顶部的大多数 Azure 自动化运行手册的示例所示。

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

您 runbook 中，应在所有[检查点](automation-runbook-execution#checkpoints)后重复这些行。 如果 runbook 挂起，然后继续在另一个工作人员，它将需要进行重新身份验证。

## 相关的文章
- [Azure 自动化︰ 对 Azure 使用 Azure Active Directory 进行身份验证](http://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)
 
测试
