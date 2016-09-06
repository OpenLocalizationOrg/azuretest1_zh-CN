---
ms.openlocfilehash: 6787af04b1412f89a097eb1c18f5c4130326e3b6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理基于角色的访问控制与 Windows PowerShell"
    description="管理基于角色的访问控制与 Windows PowerShell"
    services="azure-portal"
    documentationCenter="na"
    authors="IHenkel"
    manager="stevenpo"
    editor="mollybos"/>

<tags
    ms.service="azure-portal"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/14/2015"
    ms.author="inhenk"/>

# 管理基于角色的访问控制与 Windows PowerShell #

> [AZURE.SELECTOR]
- [Windows PowerShell](role-based-access-control-powershell.md)
- [Azure CLI](role-based-access-control-xplat-cli.md)


基于角色的访问控制 (RBAC) 在 Azure 门户和 Azure 资源管理 API 允许您管理您的订阅级别较细粒度访问。 使用此功能，您可以通过在特定的范围内，向其分配一些角色授予访问 Active Directory 用户、 组或服务主体。

在本教程中，您将学习如何使用 Windows PowerShell 管理 RBAC。 它将引导您完成创建和检查角色分配的过程。

**估计完成时间︰** 15 分钟

## 先决条件

您可以使用 Windows PowerShell 管理 RBAC 之前，您必须具有以下︰

- Windows PowerShell，3.0 或 4.0 版本。 若要查找 Windows PowerShell 的版本，请键入︰`$PSVersionTable` ，并验证的值`PSVersion`是 3.0 或 4.0。 要安装的兼容版本，请参阅[Windows 管理框架 3.0](http://www.microsoft.com/download/details.aspx?id=34595)或[Windows 管理框架 4.0](http://www.microsoft.com/download/details.aspx?id=40855)。

- Azure PowerShell 版本 0.8.8 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)。

本教程为 Windows PowerShell 初学者而设计，但它假定您了解的基本概念，如模块、 cmdlet 和会话。 有关 Windows PowerShell 的详细信息，请参阅[Windows PowerShell 入门知识](http://technet.microsoft.com/library/hh857337.aspx)。

要获得任何在本教程中，您看到的 cmdlet 的详细的帮助，请使用获取帮助 cmdlet。

    Get-Help <cmdlet-name> -Detailed

例如，要添加 AzureAccount cmdlet 获取帮助，请键入︰

    Get-Help Add-AzureAccount -Detailed

请请参阅下面的教程来熟悉设置和使用的 Windows PowerShell Azure 资源管理器︰

- [如何安装和配置 Azure PowerShell](../install-configure-powershell.md)
- [使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)


## 连接到您的订阅

RBAC 只适用与 Azure 资源管理器中，由于做第一件事就是切换到 Azure 资源管理器模式下，类型︰

    PS C:\> Switch-AzureMode -Name AzureResourceManager

有关详细信息，请参阅[使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)。

若要连接 o Azure 的订阅，请键入︰

    PS C:\> Add-AzureAccount

在弹出的浏览器控件中，输入您的 Azure 帐户用户名和密码。 PowerShell 将获得与此帐户和图 PowerShell 使用第一个作为默认值的所有订阅。 请注意，使用 RBAC，您将只能获取订阅您拥有某些权限通过被其 co 管理员或有一些角色分配。

如果多个订阅，并且想要切换到另一个，请键入︰

    # This will show you the subscriptions under the account.
    PS C:\> Get-AzureSubscription
    # Use the subscription name to select the one you want to work on.
    PS C:\> Select-AzureSubscription -SubscriptionName <subscription name>

有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)。

## 检查现有角色分配

现在让我们来检查工作分配中订阅已存在何种角色。 类型：

    PS C:\> Get-AzureRoleAssignment

这将返回订阅中的所有角色分配。 要注意以下两点︰

1. 您将需要在订阅级别具有读取访问权限。
2. 如果订阅了大量的角色分配，它可能需要一段时间才能得到所有这些。

此外可以查看有关特定角色定义，特定用户的特定范围的现有角色分配。 类型：

    PS C:\> Get-AzureRoleAssignment -ResourceGroupName group1 -Mail <user email> -RoleDefinitionName Owner

这将在您的 AD 租户，具有"所有者"的资源组"group1"角色分配返回特定用户的所有角色分派。 角色分配可以来自两个位置︰

1. 角色分配的"所有者"给资源组的用户。
2. 角色分配中的"所有者"为用户提供资源的父组 （在此例中的订阅），因为如果您在某一级有任何权限，您必须为所有其子相同的权限。

此 cmdlet 的所有参数都是可选的。 可以组合使用不同的筛选器检查角色分配。

## 创建角色分配

若要创建角色分配，您需要考虑︰

谁您想要分配到该角色︰ 可以使用下面的 Azure 的活动目录 cmdlet 以查看哪些用户、 组和服务主体具有在 AD 租户。

    PS C:\> Get-AzureADUser
    PS C:\> Get-AzureADGroup
    PS C:\> Get-AzureADGroupMember
    PS C:\> Get-AzureADServicePrincipal

什么您想要分配的角色︰ 您可以使用以下 cmdlet 以查看受支持的角色定义。

    PS C:\> Get-AzureRoleDefinition

什么您要分配的范围︰ 有三种级别的作用域

    - The current subscription
    - A resource group, to get a list of resource groups, type `PS C:\> Get-AzureResourceGroup`
    - A resource, to get a list of resources, type `PS C:\> Get-AzureResource`

然后使用`New-AzureRoleAssignment`创建角色分配。 例如︰


这将在当前订阅级别的用户创建角色分配，作为读取器。

     PS C:\> New-AzureRoleAssignment -Mail <user email> -RoleDefinitionName Reader

这将创建资源组级别的角色分配。

    PS C:\> New-AzureRoleAssignment -Mail <user email> -RoleDefinitionName Contributor -ResourceGroupName group1

在资源组级别，这将创建一组角色分配。

    PS C:\> New-AzureRoleAssignment -ObjectID <group object ID> -RoleDefinitionName Reader -ResourceGroupName group1

这将在资源级别创建角色分配。

    PS C:\> $resources = Get-AzureResource
    PS C:\> New-AzureRoleAssignment -Mail <user email> -RoleDefinitionName Owner -Scope $resources[0].ResourceId


## 验证权限

这些角色分配您检查您的帐户具有某些角色分配，则可以实际看见权限后通过运行向您授予

    PS C:\> Get-AzureResourceGroup
    PS C:\> Get-AzureResource

这些两个 cmdlet 将仅返回资源的资源组具有读取权限的位置。 然后它将向您展示以及拥有的权限。

然后当您尝试运行像其他 cmdlet `New-AzureResourceGroup`，您将收到拒绝访问错误如果您没有权限。

## 下一步行动

若要了解有关管理基于角色的访问控制与 Windows PowerShell 和相关的主题的更多信息︰

- [基于角色的访问控制在 Azure 中](../role-based-access-control-configure.md)
- [Azure 资源管理器的 Cmdlet](http://go.microsoft.com/fwlink/?LinkID=394765&clcid=0x409)︰ 了解如何使用 AzureResourceManager 模块中的 cmdlet。
- [使用资源组来管理 Azure 资源](../azure-preview-portal-using-resource-groups.md)︰ 了解如何创建和管理 Azure 管理门户中的资源组。
- [Azure 博客](http://blogs.msdn.com/windowsazure)︰ 了解 Azure 中的新功能。
- [Windows PowerShell 博客](http://blogs.msdn.com/powershell)︰ 了解 Windows PowerShell 中的新功能。
- ["的您好，脚本专家 ！"博客](http://blogs.technet.com/b/heyscriptingguy/)︰ 从 Windows PowerShell 社区获取真实的提示和技巧。
- [配置基于角色的访问控制使用 Azure CLI](role-based-access-control-xplat-cli.md)
- [故障诊断的基于角色的访问控制](role-based-access-control-troubleshooting.md)

测试
