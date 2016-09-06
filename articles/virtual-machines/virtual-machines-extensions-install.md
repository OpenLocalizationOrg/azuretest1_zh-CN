---
ms.openlocfilehash: b4d5d719e67923a0823a5a49c04a20e5824b8d45
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
 pageTitle="管理虚拟机的扩展 |Microsoft Azure" 
 description="介绍如何添加、 查找、 更新和删除扩展。" 
 services="virtual-machines" 
 documentationCenter="" 
 authors="squillace" 
 manager="timlt" 
 editor=""/>
<tags 
 ms.service="virtual-machines" 
 ms.devlang="na" 
 ms.topic="article" 
 ms.tgt_pltfrm="vm-multiple" 
 ms.workload="infrastructure-services"
 ms.date="08/25/2015" 
 ms.author="rasquill"/>
#管理虚拟机的扩展
介绍如何查找、 添加、 修改或删除 Windows 或 Linux 虚拟机在 Azure 上的 VM 扩展。

##使用 VM 扩展

Azure VM 扩展可实现行为，或者任何一个帮助 Azure 的虚拟机上运行的其他程序的功能 （例如， **WebDeployForVSDevTest**扩展名允许 Visual Studio Azure VM 上的 Web 部署解决方案） 或提供支持某些其他行为的能力可以与虚拟机进行交互 （例如，您可以使用虚拟媒体访问扩展 Powershell，Azure CLI 从和其他客户端重新设置或修改远程访问在 Azure VM 上的值)。

>[AZURE.IMPORTANT] 扩展功能的完整列表他们支持，请参阅[Azure VM 扩展和功能](https://msdn.microsoft.com/library/dn606311.aspx)。 因为每个 VM 扩展支持特定功能，正好可以做什么您和扩展名不能取决于扩展。 因此，在修改 VM 之前, 请确保您已经阅读了您想要使用 VM 扩展的文档。 不支持删除一些虚拟机的扩展;其他人可以将从根本上改变虚拟机行为的属性。

最常见的任务是︰

1.  查找可用的扩展
    
2.  更新的已加载的扩展

3.  添加扩展

4.  删除扩展

##查找可用的扩展

Azure VM 扩展 （扩展功能的完整列表他们支持，请参阅[Azure VM 扩展和功能](https://msdn.microsoft.com/library/dn606311.aspx)。）您也可以找到扩展和扩展的信息使用︰

-   PowerShell
-   Azure 的跨平台接口 (Azure CLI)
-   服务管理 REST API，

[Azure PowerShell](https://msdn.microsoft.com/library/azure/dn495240.aspx) cmdlet 或[服务管理 REST Api](https://msdn.microsoft.com/library/ee460799.aspx)来了解可用的扩展。

###Azure PowerShell

某些扩展了 Powershell cmdlet 所特有，它可能使其配置从 PowerShell 更轻松;但以下 cmdlet 的所有 VM 扩展工作。

可以使用以下 cmdlet 以获取有关可用扩展的信息︰

-   对于实例的 web 角色或辅助角色，您可以使用[Get AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet。
-   对于实例的虚拟机，您可以使用[Get AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet。

     例如，下面的代码示例演示如何列出使用 PowerShell **IaaSDiagnostics**扩展的信息。
    
        PS C:\PowerShell> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
        VERBOSE: 5:09:01 PM - Begin Operation: Get-AzureVMAvailableExtension
        VERBOSE: 5:09:06 PM - Completed Operation: Get-AzureVMAvailableExtension

        Publisher                   : Microsoft.Azure.Diagnostics
        ExtensionName               : IaaSDiagnostics
        Version                     : 1.2
        Label                       : Microsoft Monitoring Agent Diagnostics
        Description                 : Microsoft Monitoring Agent Extension
        PublicConfigurationSchema   :
        PrivateConfigurationSchema  :
        IsInternalExtension         : False
        SampleConfig                :
        ReplicationCompleted        : True
        Eula                        :
        PrivacyUri                  :
        HomepageUri                 :
        IsJsonExtension             : True
        DisallowMajorVersionUpgrade : False
        SupportedOS                 :
        PublishedDate               :
        CompanyName                 :


###Azure 的命令行接口 (CLI Azure)

某些扩展具有特定于它们的 Azure CLI 命令 （Docker VM 扩展是一个示例），这可能会使其配置更容易;但以下命令适用于所有的 VM 扩展。

**Azure vm 扩展列表**命令可用于获取有关可用的扩展的信息，并使用**–-json**选项，以显示有关一个或多个扩展的所有可用信息。 如果不使用扩展名，则该命令将返回 json 的所有可用的扩展说明。

例如，下面的代码示例演示如何列出使用 Azure CLI **azure vm 扩展列表**命令扩展名为**IaaSDiagnostics**的信息，并使用**--json**选项返回的完整信息。


    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



###服务管理 REST Api

以下的 REST Api 可用于获取有关可用扩展的信息︰

-   对于实例的 web 角色或辅助角色，可以使用此[列表中可用的扩展](https://msdn.microsoft.com/library/dn169559.aspx)操作。 列出可用的扩展版本，您可以使用[列表扩展版本](https://msdn.microsoft.com/library/dn495437.aspx)。

-   对于实例的虚拟机，则可以使用[列表资源扩展](https://msdn.microsoft.com/library/dn495441.aspx)操作。 列出可用的扩展版本，您可以使用[列表资源扩展版本](https://msdn.microsoft.com/library/dn495440.aspx)。

##添加、 更新或禁用的扩展

创建实例，或将它们添加到正在运行的实例时，可以添加扩展。 可以更新、 禁用或删除扩展。 通过使用 Azure PowerShell cmdlet 或使用 REST API，服务管理操作，您可以执行这些操作。 参数需要安装和设置了一些扩展。 公共和私有参数的扩展支持。


###Azure PowerShell

使用 Azure PowerShell cmdlet 是最简单的方法来添加和更新扩展。 当您使用扩展的 cmdlet 时，是为您完成大部分的扩展的配置。 有时，您可能需要以编程方式添加扩展名。 当您需要执行此操作时，您必须提供扩展的配置。

您可以使用以下 cmdlet 知道扩展是否需要公钥和私钥参数的配置︰

-   对于实例的 web 角色或辅助角色，您可以使用**Get AzureServiceAvailableExtension** cmdlet。

-   对于实例的虚拟机，您可以使用**Get AzureVMAvailableExtension** cmdlet。

###服务管理 REST Api

当使用 REST Api 检索可用扩展的列表时，您会收到有关扩展的配置方式的信息。 返回的信息可能会显示参数信息由一个公共架构和专用架构。 有关实例的查询中返回公用参数值。 不返回私有参数值。

可以使用以下的 REST Api 知道扩展是否需要公钥和私钥参数的配置︰

-   对于 web 角色或辅助角色实例、 **PublicConfigurationSchema**和**PrivateConfigurationSchema**元素包含在[列表中可用的扩展](https://msdn.microsoft.com/library/dn169559.aspx)操作的响应的信息。

-   为虚拟机， **PublicConfigurationSchema**和**PrivateConfigurationSchema**元素的实例包含[列表资源扩展](https://msdn.microsoft.com/library/dn495441.aspx)操作的响应中的信息。

>[AZURE.NOTE]扩展也可以使用 JSON 定义配置。 当使用这些类型的扩展时，仅**SampleConfig**元素的用途。
