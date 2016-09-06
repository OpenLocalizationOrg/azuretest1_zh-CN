---
ms.openlocfilehash: 0254735bd7f1326fbd2e8cada17b2992d82d43ce
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="使用标签来组织您的 Azure 资源" 
    description="演示如何应用于组织的计费和管理的资源的标记。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac"
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="AzurePortal" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/07/2015" 
    ms.author="tomfitz"/>


# 使用标签来组织您的 Azure 资源

资源管理器使您可以通过应用标签用逻辑方式组织的资源。 标记包含与您定义的属性标识资源的键/值对。 若要标记为属于同一类别的资源，用于那些资源相同的标记。 

查看具有特定标记的资源时，您会看到从所有资源组的资源。 您并不限于只可用于组织您的资源以一种独立于部署关系同一资源组中的资源。 标记可能会特别有用，当您需要组织帐单或管理资源。 

添加资源或资源组的每个标记会自动添加到该订阅范围分类。 您还可以订阅标记名称和您想要用作将来标记资源值的预设分类。

> [AZURE.NOTE] 您可以仅适用于支持资源管理器操作的资源的标记。 如果您创建的虚拟机、 虚拟网络或存储通过经典的部署模型 (如通过 Azure 的门户网站或[服务管理 API](https://msdn.microsoft.com/library/azure/dn948465.aspx))，不能将标记应用到该资源。 您必须重新部署这些资源通过资源管理器以支持标记。 所有其他资源支持标记。


## 在门户中预览标签

在门户中预览标记资源和资源组很容易。 使用浏览中心定位到您想要标记并单击标记概述部分顶部的刀片式服务器的资源组的资源。 

![标记部分资源和资源组刀片](./media/resource-group-using-tags/tag-icon.png)

这将打开刀片式服务器的已应用的标记的列表。 如果这是您第一个标记，则该列表将为空。 若要添加一个标记，只需指定名称和值，然后按 Enter。 您已经添加了几个标记后，您将看到自动完成选项根据预先存在的标记的名称和值以更好地跨您的资源确保一致的分类并避免常见错误，如拼写错误。

![标记名称/值对的资源](./media/resource-group-using-tags/tag-resources.png)

在门户中查看您的标记的分类，使用浏览中心查看的所有内容，然后选择标记。

![查找标记通过浏览集线器](./media/resource-group-using-tags/browse-tags.png)

固定到以便快速访问您 Startboard 最重要的标记，您就可以转。 玩得愉快！

![Pin Startboard 标记](./media/resource-group-using-tags/pin-tags.png)

## 使用 PowerShell 标记

如果您先前没有使用 Azure PowerShell 与资源管理器，请参阅[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。
对于这篇文章的目的，我们假设您已经添加的帐户并选择要标记的资源具有的订阅。

标记功能仅适用于资源和资源组从[资源管理器](http://msdn.microsoft.com/library/azure/dn790568.aspx)，因此我们需要做下一件事是切换到使用资源管理器。

    Switch-AzureMode AzureResourceManager

直接对资源和资源组，因此，若要查看已应用标记的标记存在，我们只是可以获得的资源或资源组`Get-AzureResource`或`Get-AzureResourceGroup`，分别。 让我们开始与资源组。

    PS C:\> Get-AzureResourceGroup tag-demo

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *

    Resources         :
                    Name                             Type                                  Location
                    ===============================  ====================================  ==============
                    CPUHigh ExamplePlan              microsoft.insights/alertrules         eastus
                    ForbiddenRequests tag-demo-site  microsoft.insights/alertrules         eastus
                    LongHttpQueue ExamplePlan        microsoft.insights/alertrules         eastus
                    ServerErrors tag-demo-site       microsoft.insights/alertrules         eastus
                    ExamplePlan-tag-demo             microsoft.insights/autoscalesettings  eastus
                    tag-demo-site                    microsoft.insights/components         centralus
                    ExamplePlan                      Microsoft.Web/serverFarms             southcentralus
                    tag-demo-site                    Microsoft.Web/sites                   southcentralus


此 cmdlet 返回元数据的几位上的资源组中包括已应用了哪些标记，如果有的话。 若要标记一个资源组，只需使用`Set-AzureResourceGroup`命令并指定标记名称和值。

    PS C:\> Set-AzureResourceGroup tag-demo -Tag @( @{ Name="project"; Value="tags" }, @{ Name="env"; Value="demo"} )

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name     Value
                    =======  =====
                    project  tags
                    env      demo

标记更新作为一个整体，因此如果要添加一个标记进行标记的资源，需要用于数组的所有标记您想要保留。 若要执行此操作，可以首先选择现有标签，并添加一个新。

    PS C:\> $tags = (Get-AzureResourceGroup -Name tag-demo).Tags
    PS C:\> $tags += @{Name="status";Value="approved"}
    PS C:\> Set-AzureResourceGroup tag-demo -Tag $tags

    ResourceGroupName : tag-demo
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name     Value
                    =======  ========
                    project  tags
                    env      demo
                    status   approved


若要删除一个或多个标记，只需保存而不是您想要删除的数组。 

该过程是相同的资源，不同之处在于，您将使用`Get-AzureResource`和`Set-AzureResource`的 cmdlet。 若要获取资源或资源组与一个特定的标记，请使用`Get-AzureResource`或`Get-AzureResourceGroup`使用的 cmdlet`-Tag`参数。

    PS C:\> Get-AzureResourceGroup -Tag @{ Name="env"; Value="demo" } | %{ $_.ResourceGroupName }
    rbacdemo-group
    tag-demo
    PS C:\> Get-AzureResource -Tag @{ Name="env"; Value="demo" } | %{ $_.Name }
    rbacdemo-web
    rbacdemo-docdb
    ...

若要获取使用 PowerShell 的订阅中的所有标记的列表，请使用`Get-AzureTag`cmdlet。

    PS C:/> Get-AzureTag
    Name                      Count
    ----                      ------
    env                       8
    project                   1

您可能会看到标记开头"隐藏的"和"链接:"。 这些都是内部的标记，它应忽略并避免更改。 

使用`New-AzureTag`cmdlet 将新标签添加到分类。 这些标记将包含在自动完成，即使他们还没有尚未应用于任何资源或资源组。 要删除标记名称/值，请首先移除标记从任何资源，它可以与一起使用，然后使用`Remove-AzureTag`cmdlet 若要删除该分类。

## 与 REST API 添加标签

门户和 PowerShell 同时使用[REST API，资源管理器](http://msdn.microsoft.com/library/azure/dn790568.aspx)在后台。 如果您需要集成标记到另一个环境，可以获得的资源 id 获取标记并更新的修补程序调用的标记集。


## 标记和计费

对于支持服务，您可以使用标记对记帐数据进行分组。 例如，[虚拟机集成使用 Azure 资源管理器中](/virtual-machines/virtual-machines-azurerm-versus-azuresm.md)，可以定义并应用标签来组织付费使用的虚拟机。 如果您正在对不同组织的多个虚拟机，可以使用按成本中心组用法的标记。  
您还可以使用标记对成本进行分类由运行时环境;例如，在生产环境中运行的虚拟机的付费使用。

您可以检索有关通过标记信息[使用 api](billing-usage-rate-card-overview.md)或可以下载在[Azure 帐户门户](https://account.windowsazure.com/)或[EA 门户](https://ea.azure.com)使用逗号分隔值 (CSV) 文件。 有关以编程方式访问记帐信息的详细信息，请参阅[深入了解您的 Microsoft Azure 资源消耗](billing-usage-rate-card-overview.md)。

下载使用 CSV 支持标记付费的服务时，标记将出现在**标记**列中。 有关详细信息，请参阅[了解 Microsoft Azure 的账单](billing-understand-your-bill.md)。

![看到帐单中的标记](./media/resource-group-using-tags/billing_csv.png)

## 下一步行动

- 部署资源时使用 Azure PowerShell 的简介，请参见[使用 Azure PowerShell 使用 Azure 资源管理器中](./powershell-azure-resource-manager.md)。
- 部署资源时使用 Azure CLI 的简介，请参见[使用 Azure CLI 的 Mac、 Linux、 Windows Azure 资源管理](./xplat-cli-azure-resource-manager.md)。
- 使用预览门户的简介，请参见[使用 Azure 预览门户管理 Azure 资源](./resource-group-portal.md)  
  

  



测试
