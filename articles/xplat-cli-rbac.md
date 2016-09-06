---
ms.openlocfilehash: e4758f625279a6d3caa87c36e0b487367a1e0b24
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理基于角色的访问控制 Mac、 Linux、 和 Windows Azure CLI"
    description="管理与 Azure CLI 的基于角色的访问控制。"
    services=""
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tomfitz"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2015"
    ms.author="tomfitz"/>

# 管理基于角色的访问控制与 Azure 命令行界面 (CLI Azure)#

<div class="dev-center-tutorial-selector sublanding"><a href="/documentation/articles/powershell-rbac.md" title="Windows PowerShell" class="current">Windows PowerShell</a><a href="/documentation/articles/xplat-cli-rbac.md" title="Azure CLI">Azure CLI</a></div>

基于角色的访问控制 (RBAC) 在 Azure 门户和 Azure 资源管理器 API 允许您管理对细粒度级别上订阅的访问。 使用此功能，您可以通过在特定的范围内，向其分配一些角色授予访问权限活动目录的用户、 组或服务主体。

在本教程中，您将学习如何使用 Azure CLI 管理 RBAC。 它将引导您完成创建和检查角色分配的过程。

**估计完成时间︰** 15 分钟

## 先决条件 ##

您可以使用 Azure CLI 管理 RBAC 之前，必须具有下列︰

- Azure CLI 版本 0.8.8 或更高版本。 若要安装最新版本，并将其与您 Azure 的订阅，请参阅[安装](xplat-cli-install.md)。
- 请请参阅下面的教程来熟悉设置并在 Azure CLI 使用 Azure 资源管理器︰[使用 Azure CLI 使用资源管理器](xplat-cli-azure-resource-manager.md)

## 在本教程中 ##

* [连接到您的订阅](#connect)
* [检查现有角色分配](#check)
* [创建角色分配](#create)
* [验证权限](#verify)
* [下一步行动](#next)

## <a id="connect"></a>连接到您的订阅 ##

RBAC 只适用与 Azure 资源管理器中，由于做第一件事就是切换到 Azure 资源管理器模式下，类型︰

    azure config mode arm

有关详细信息，请参阅[使用 Azure CLI 使用资源管理器](xplat-cli-azure-resource-manager.md)

若要连接 o Azure 的订阅，请键入︰

    azure login -u <username>

在命令行提示符下，输入您的 Azure 帐户密码 （仅支持工作或学校 Id，也称为**组织 ID**）。 Azure CLI 会得到与此帐户和图本身可以使用为默认值的第一个的所有订阅。 请注意，使用 RBAC，您将只能获取订阅您拥有某些权限通过被其 co 管理员或有一些角色分配。

如果多个订阅，并且想要切换到另一个，请键入︰

    # This will show you the subscriptions under the account.
    azure account list
    # Use the subscription name to select the one you want to work on.
    azure account set <subscription name>

有关详细信息，请参阅[Azure CLI 命令](azure-cli-arm-commands.md)。

## <a id="check"></a>检查现有角色分配 ##

现在让我们来检查工作分配中订阅已存在何种角色。 类型：

    azure role assignment list

这将返回订阅中的所有角色分配。 要注意以下两点︰

1. 您将需要在订阅级别具有读取访问权限。
2. 如果订阅了大量的角色分配，它可能需要一段时间才能得到所有这些。

此外可以查看有关特定角色定义，特定用户的特定范围的现有角色分配。 类型：

    azure role assignment list -g group1 --mail <user's email> -o Owner

这将在您的 AD 租户，具有"所有者"的资源组"group1"角色分配返回特定用户的所有角色分派。 角色分配可以来自两个位置︰

1. 角色分配的"所有者"给资源组的用户。
2. 角色分配中的"所有者"为用户提供资源的父组 （在此例中的订阅），因为如果您在某一级有任何权限，您必须为所有其子相同的权限。

此 cmdlet 的所有参数都是可选的。 可以组合使用不同的筛选器检查角色分配。

## <a id="create"></a>创建角色分配 ##

若要创建角色分配，您需要考虑

- 谁您想要分配到该角色︰ 可以使用下面的 Azure 的活动目录 cmdlet 以查看哪些用户、 组和服务主体具有在 AD 租户。

    `azure ad user list
    azure ad user show
    azure ad group list
    azure ad group show
    azure ad group member list
    azure sp list
    azure sp show`

- 什么您想要分配的角色︰ 您可以使用以下 cmdlet 以查看受支持的角色定义。

    `azure role list`

- 什么您要分配的范围︰ 有三种级别的作用域

    - 当前订阅
    - 资源组，以获取资源组类型的列表 `azure group list`
    - 资源，以获取资源，类型的列表 `azure resource list`

然后使用`azure role assignment create`创建角色分配。 例如︰

 - 这将在当前订阅级别的用户创建角色分配，作为读取器。

    `azure role assignment create --mail <user's email> -o Reader`

- 这将创建角色分配在资源组级别

    `PS C:\> azure role assignment create --mail <user's email> -o Contributor -g group1`

- 这将创建角色分配在资源级别

    `azure role assignment create --mail <user's email> -o Owner -g group1 -r Microsoft.Web/sites -u site1`

## <a id="verify"></a>验证权限 ##

这些角色分配您检查您的帐户具有某些角色分配，则可以实际看见权限后通过运行向您授予

    PS C:\> azure group list
    PS C:\> azure resource list

这些两个 cmdlet 将仅返回资源的资源组具有读取权限的位置。 然后它将向您展示以及拥有的权限。

然后当您尝试运行像其他 cmdlet `azure group create`，您将收到拒绝访问错误如果您没有权限。

## <a id="next"></a>下一步行动 ##

若要了解有关管理 Azure CLI 和相关的主题的基于角色的访问控制的详细信息︰

- [安装和配置的 Azure CLI](xplat-cli-install.md)
- [使用 Azure CLI 使用资源管理器](xplat-cli-azure-resource-manager.md)
- [使用资源组来管理 Azure 资源](resource-groups-overview.md)︰ 了解如何创建和管理 Azure 管理门户中的资源组。

测试
