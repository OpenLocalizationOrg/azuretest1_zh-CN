---
ms.openlocfilehash: f4c79483f807101524454683308421aed8d063e9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="部署和管理使用 PowerShell 通知集线器" 
    description="如何创建和管理自动化使用 PowerShell 通知集线器" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/18/2015" 
    ms.author="wesmc"/>

# 部署和管理使用 PowerShell 通知集线器

##概述

这篇文章演示了如何使用创建和管理使用 PowerShell Azure 通知集线器。 本主题中显示以下常见的自动化任务。

+ 创建通知中心
+ 设置凭据

如果您还需要创建通知集线器的新服务总线命名空间，请参阅[使用 PowerShell 管理服务总线](../service-bus/service-bus-powershell-how-to-provision.md)。

管理通知集线器不受直接附带 Azure PowerShell 的 cmdlet。 从 PowerShell 的最佳方法是引用 Microsoft.ServiceBus.dll 程序集。 程序集被随[服务总线 NuGet 程序包](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)。


## 先决条件

在开始这篇文章之前，您必须具有以下︰

- Azure 的订阅。 Azure 是一个基于订阅的平台。 有关获取订阅的详细信息，请参阅[购买选项]，[成员提供]，或[免费试用版]。

- 带有 Azure PowerShell 的计算机。 有关说明，请参阅[安装和配置 Azure PowerShell]。

- PowerShell 脚本，NuGet 程序包和.NET Framework 大概了解。


## 包括对服务总线的.NET 程序集的引用

管理 Azure 通知集线器尚不包含在 Azure PowerShell PowerShell cmdlet。 若要设置通知集线器和其他服务总线实体通过现有的 cmdlet 不公开的可以用于在[服务总线 NuGet 程序包](http://www.nuget.org/packages/WindowsAzure.ServiceBus/)的服务总线使用.NET 客户端。

首先，确保您的脚本可以找到作为 Visual Studio 项目中 NuGet 程序包安装的**Microsoft.ServiceBus.dll**程序集。 为了保持灵活，该脚本执行下列步骤︰

1. 确定在调用它的路径。
2. 遍历路径，直到它找到一个文件夹命名为`packages`。 当您安装的 Visual Studio 项目的 NuGet 程序包，则创建此文件夹。
3. 以递归方式搜索`packages`名为**Microsoft.ServiceBus.dll**的程序集的文件夹。
4. 引用程序集，以使类型可供以后使用。

以下是这些步骤 PowerShell 脚本中的实现方式︰

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## 创建 NamespaceManager 类

若要设置通知集线器和其他服务总线实体，请从 SDK 创建[NamespaceManager](http://msdn.microsoft.com/library/microsoft.servicebus.namespacemanager.aspx)类的一个实例。 

可以使用 Azure PowerShell 包含[Get AzureSBAuthorizationRule] cmdlet 来检索授权规则，用于提供的连接字符串。 我们会将存储在参考`NamespaceManager`实例的`$NamespaceManager`变量。 我们将使用`$NamespaceManager`提供通知中心。

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## 资源调配新的通知中心 

若要设置新的通知中心，使用[服务总线的.NET API]。 本文仅侧重通知集线器。 若要使用其他服务总线实体，请参阅[使用 PowerShell 管理服务总线](../service-bus/service-bus-powershell-how-to-provision.md)。

脚本的这一部分在您设置了四个局部变量。 

1. `$Namespace` ︰ 设置为命名空间的名称要创建通知中心的位置。
2. `$Path` ︰ 将此路径设置为新的通知中心的名称。  例如，"MyHub"。    
3. `$WnsPackageSid` ︰ 设置为 SID 的包为您的 Windows 应用程序从[Windows 开发人员中心](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409)。
4. `$WnsSecretkey`︰ 设置为密钥对您的 Windows 应用程序从[Windows 开发人员中心](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409)。

这些变量用于连接到服务总线名称空间并创建新通知集线器配置为能够处理与 WNS 凭据的 Windows 应用程序的 Windows 通知服务 (WNS) 通知。 有关获取软件包信息 SID 和机密密钥请参阅，[通知集线器入门](notification-hubs-windows-store-dotnet-get-started.md)教程。 

+ 脚本片段将使用`NamespaceManager`对象来检查以查看是否通知中心标识由`$Path`存在。

+ 如果不存在，脚本将创建`NotificationHubDescription`与 WNS 凭据并传递给`NamespaceManager`类`CreateNotificationHub`方法。

``` powershell

$Namespace = "<Enter your namespace>
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.ServiceBus.Notifications.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.ServiceBus.Notifications.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## 其他资源

- [管理与 PowerShell 的服务总线](../service-bus/service-bus-powershell-how-to-provision.md)
- [如何创建服务总线队列、 主题和订阅使用 PowerShell 脚本](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [如何创建服务总线 Namespace，使用 PowerShell 脚本事件中心](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

有些现成的脚本，还有可供下载︰
- [服务总线 PowerShell 脚本](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[成员服务]: http://azure.microsoft.com/pricing/member-offers/
[免费试用版]: http://azure.microsoft.com/pricing/free-trial/
[安装和配置 Azure PowerShell]: ../install-configure-powershell.md
[服务总线的.NET API]: https://msdn.microsoft.com/library/microsoft.servicebus.aspx
[获得 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[新 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[获得 AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 