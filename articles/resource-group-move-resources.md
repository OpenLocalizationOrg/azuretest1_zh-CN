---
ms.openlocfilehash: 285106fdf7a04731021ff4645cd2f4f69431e1c2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将资源移动到新的资源组" 
    description="使用 Azure PowerShell 或 REST API 将资源移动到一个新的资源组的 Azure 资源管理器中。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/20/2015" 
    ms.author="tomfitz"/>

# 将资源移动到新的资源组或订阅

本主题演示如何将资源从一个资源组移动到另一个资源组。 您还可以将资源移到新的订阅。 您可能需要移动资源，当您决定的︰

1. 为了记帐，资源需要住在不同的订阅。
2. 资源不能共享作为先前组合使用的资源相同的生命周期。 您想要将其移动到新的资源组中，以便您可以单独管理该资源与其他资源。
3. 现在，资源共享作为不同的资源组中其他资源相同的生命周期。 您想要将其移动到其他资源的资源组，以便共同管理。

移动资源时，有一些重要的注意事项︰

1. 您不能更改资源的位置。 移动资源仅将其移动到新的资源组。 新的资源组可能具有不同的位置，但它不会更改资源的位置。
2. 目标资源组应包含共享相同的应用程序生命周期，为您要移动的资源的资源。
3. 如果您正在使用 Azure PowerShell，请确保您使用的最新版本。 **移动 AzureResource**命令会经常更新。 若要更新您的版本，请运行 Microsoft Web 平台安装程序，检查新版本是否可用。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。
4. 移动操作可能需要一段时间才能完成，期间的时间提示操作完成之前将等待您 PowerShell。

## 支持的服务

不是所有的服务目前支持移动资源的能力。

现在，支持移动到一个新的资源组和订阅的服务包括︰

- API 管理
- Azure 搜索
- 数据工厂
- 密钥存储库
- 移动服务
- 操作建议
- Redis 高速缓存
- Azure 的 Web 应用程序 （应用某些[限制](app-service-web/app-service-move-resources.md)）

支持移动到新的资源组，但不是一个新的订阅的服务有︰

- 计算 （传统）
- 存储 （传统）

目前不支持移动资源的服务有︰

- 虚拟网络

在使用 web 应用程序，则不能移动只有一个应用程序服务计划。 若要移动 web 应用程序，您的选项是︰

- 所有的资源从一个资源组移到不同的资源组，如果目标资源组不具有 Microsoft.Web 资源。
- 将 web 应用程序移动到不同的资源组，但保留原始资源组中的应用程序服务计划。

## 使用 PowerShell 移动资源

若要将现有资源移到另一个资源组或订阅，请使用**移动 AzureResource**命令。

第一个示例演示如何将一个资源移动到一个新的资源组。

    PS C:\> Move-AzureResource -DestinationResourceGroupName TestRG -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OtherExample/providers/Microsoft.ClassicStorage/storageAccounts/examplestorage

第二个示例演示如何将多个资源移动到一个新的资源组。

    PS C:\> $webapp = Get-AzureResource -ResourceGroupName OldRG -ResourceName ExampleSite -ResourceType Microsoft.Web/sites
    PS C:\> $plan = Get-AzureResource -ResourceGroupName OldRG -ResourceName ExamplePlan -ResourceType Microsoft.Web/serverFarms
    PS C:\> Move-AzureResource -DestinationResourceGroupName NewRG -ResourceId ($webapp.ResourceId, $plan.ResourceId)

若要移动到新的订阅，包括**DestinationSubscriptionId**参数的值。

## 使用 REST API 将资源移

若要将现有资源移到另一个资源组或订阅，请运行︰

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

替换当前包含想要移动的资源的资源并预订组**{源订阅 id}**和**{源-资源-组-名称}** 。 使用 {api 版本} **2015年-01-01** 。

在请求中，包括一个 JSON 对象，用于定义目标资源组和您想要移动的资源。

    {
        "targetResourceGroup": "/subscriptions/{target-subscription-id}/resourceGroups/{target-resource-group-name}", "resources": [
            "/subscriptions/{source-id}/resourceGroups/{source-group-name}/providers/{provider-namespace}/{type}/{name}",
            "/subscriptions/{source-id}/resourceGroups/{source-group-name}/providers/{provider-namespace}/{type}/{name}",
            "/subscriptions/{source-id}/resourceGroups/{source-group-name}/providers/{provider-namespace}/{type}/{name}",
            "/subscriptions/{source-id}/resourceGroups/{source-group-name}/providers/{provider-namespace}/{type}/{name}"
        ]
    }

## 下一步行动
- [使用资源管理器中使用 Azure PowerShell](./powershell-azure-resource-manager.md)
- [使用 Azure CLI 使用资源管理器](./virtual-machines/xplat-cli-azure-resource-manager.md)
- [使用 Azure 门户管理资源](azure-portal/resource-group-portal.md)
- [使用标签来组织您的资源](./resource-group-using-tags.md)

测试
