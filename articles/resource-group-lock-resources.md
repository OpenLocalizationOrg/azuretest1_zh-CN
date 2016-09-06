---
ms.openlocfilehash: d7ab1d862cdf5ba0829cd194a0a59dd526cdeef2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="锁定资源使用 Azure 资源管理器" 
    description="锁定资源，以防用户更新或删除某些资源。" 
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
    ms.date="08/10/2015" 
    ms.author="tomfitz"/>

# 锁定资源使用 Azure 资源管理器

作为管理员，有需要将锁放置在资源或资源组，以防止其他用户在您的组织提交写入操作或意外删除重要的资源的方案。 

Azure 的资源管理器提供的功能来限制对资源管理锁通过资源的操作。 资源锁是强制在特定范围内锁定级别的策略。 锁定级别标识类型的强制执行的策略，目前有两个值-- **CanNotDelete**和**只读**。 范围表示为一个 URI，可以是资源或资源组。

锁是不同于指派用户权限才能执行某些操作。 若要了解有关设置用户和角色的权限，请参阅[预览门户中基于角色的访问控制](role-based-access-control-configure.md)和[管理和审核对资源的访问](resource-group-rbac.md)。

## 常见方案

一个常见的情形是必须带有一些灭-和-亮模式中使用的资源的资源组。  VM 资源是定期对过程数据为开启一个给定的时间间隔，则已关闭。 在这种情况下，希望能够关闭关闭虚拟机的但必须不删除存储帐户。 在这种情况下，将使用**CanNotDelete**的存储帐户的锁定级别资源锁。

另一种情况，您的业务可能有期需更新投入生产。 **只读**锁定级别停止创建或更新。 如果您是零售公司，可能不希望允许在假日购物期间更新。  如果您是一家金融服务公司，您可能必须在某些市场时间与部署相关的约束。 资源锁可以提供策略锁定为适当的资源。 这可能只是某些资源或资源组的全部应用。

## 在模板中创建一个锁定

下面的示例显示一个模板创建存储帐户的锁定。 对其应用锁定的存储帐户提供作为参数，并使用 concat （） 函数相结合。  其结果是 Microsoft.Authorization，然后锁定，此案例**myLock**名称追加的资源名称。

提供的类型是特定于资源类型。 对于存储，这种类型是"Microsoft.Storage/storageaccounts/providers/locks"。

作用域级别的资源使用**级别**设置。 随着则致力于帮助避免意外删除，将级别设置为**CannotDelete**。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "lockedResource": {
                "type": "string"
            }
        },
        "resources": [
            {
                "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
                "type": "Microsoft.Storage/storageAccounts/providers/locks",
                "apiVersion": "2015-01-01",
                "properties": {
                    "level": "CannotDelete"
                }
            }
        ]
    }

## 使用 REST API 创建锁

您可以锁定使用[REST API，用于管理锁](https://msdn.microsoft.com/library/azure/mt204563.aspx)已部署的资源。 REST API，使您能够创建和删除锁，并检索有关现有的锁的信息。

若要创建一个锁定，请运行︰

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

作用域可能订购、 资源组或资源。 锁的名称是任何您想调用该锁。 有关 api 版本，使用**2015年-01-01**。

在请求中，包括指定锁定的属性的 JSON 对象。

    {
        "properties": {
            "level": {lock-level},
            "notes": "Optional text notes."
        }
    } 

锁定级别指定**CanNotDelete**或**只读**。

有关示例，请参阅[管理锁的 REST API](https://msdn.microsoft.com/library/azure/mt204563.aspx)。

## 使用 Azure PowerShell 创建锁

您可以通过使用**New AzureResourceLock** ，如下所示锁定部署的资源与 Azure PowerShell。 通过 PowerShell，可以只设置为**CanNotDelete** **LockLevel** 。

    PS C:\> New-AzureResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName ExampleGroup

PowerShell 提供用于处理锁，例如**集 AzureResourceLock**更新锁和**删除 AzureResourceLock**删除锁定的其他命令。

## 下一步行动

- 有关如何使用资源的锁的详细信息，请参阅[下您 Azure 资源锁](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- 若要了解如何以逻辑方式组织您的资源，请参阅[使用标签来组织您的资源](resource-group-using-tags.md)
- 若要更改资源所在的资源组，请参阅[移动到新的资源组的资源](resource-group-move-resources.md)

测试
