---
ms.openlocfilehash: 14be1f7a061992c7f81a8265cbb072b37f47b2ad
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理与 PowerShell 的服务总线"
    description="管理使用 PowerShell 脚本而不是.NET 服务总线"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/18/2015"
    ms.author="sethm"/>

# 管理与 PowerShell 的服务总线

## 概述

Microsoft Azure PowerShell 是一种可用于控制和自动化的部署和管理您的工作负载在 Azure 中的脚本环境。 本文介绍如何使用 PowerShell 调配和管理服务总线的实体，如命名空间、 队列和使用本地的 Azure PowerShell 控制台事件集线器。

## 先决条件

在开始这篇文章之前，您必须具有以下︰

- Azure 的订阅。 Azure 是一个基于订阅的平台。 有关获取订阅的详细信息，请参阅[购买选项]，[成员提供]，或[免费试用版]。

- 带有 Azure PowerShell 的计算机。 有关说明，请参阅[安装和配置 Azure PowerShell]。

- PowerShell 脚本，NuGet 程序包和.NET Framework 大概了解。

## 包括对服务总线的.NET 程序集的引用

可用于管理服务总线是有限的数量的 PowerShell cmdlet。 若要设置实体通过现有的 cmdlet 不公开的可以用于.NET 客户端服务总线[服务总线 NuGet 程序包]中。

首先，确保脚本可以找到使用 NuGet 程序包安装的**Microsoft.ServiceBus.dll**程序集。 为了保持灵活，该脚本执行下列步骤︰

1. 确定在调用它的路径。
2. 遍历路径，直到它找到一个文件夹命名为`packages`。 NuGet 程序包安装时，会创建此文件夹。
3. 以递归方式搜索`packages`名为**Microsoft.ServiceBus.dll**的程序集的文件夹。
4. 引用程序集，以使类型可供以后使用。

以下是这些步骤 PowerShell 脚本中的实现方式︰

```powershell

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

## 规定服务总线命名空间

两个 PowerShell cmdlet 支持服务总线命名空间操作。 而不是.NET SDK Api，您可以使用[Get AzureSBNamespace]和[新建 AzureSBNamespace]。

本示例创建几个本地变量在脚本;`$Namespace` and `$Location`.

- `$Namespace` 名称是我们想要的服务总线命名空间。
- `$Location` 标识脚本顺序提供命名空间的数据中心。
- `$CurrentNamespace` 存储脚本检索 （或创建） 的引用命名空间。

在实际的脚本中， `$Namespace` ，`$Location`可以作为参数传递。

脚本的这一部分将执行以下操作︰

1. 尝试检索服务总线命名空间具有提供的名称。
2. 如果找到该命名空间，则它会报告找到的内容。
3. 如果找不到该命名空间，它创建的命名空间，然后检索新创建的命名空间。

    ``` powershell
    
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }

    ```

若要设置其他服务总线实体，创建的实例`NamespaceManager`类的 SDK。
可以使用[Get AzureSBAuthorizationRule] cmdlet 来检索授权规则，用于提供的连接字符串。 我们会将存储在参考`NamespaceManager`实例的`$NamespaceManager`变量。 我们将使用`$NamespaceManager`稍后在脚本中提供其他实体。

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## 提供其他服务总线实体

要为其他实体，如队列、 主题和事件集线器使用[.NET API 的服务总线]。 本文仅侧重事件集线器，但是其他实体的步骤相似。 此外，更详细的示例，包括其他实体，引用这篇文章的末尾。

脚本的这一部分中创建四个更多的本地变量。 这些变量用来实例化`EventHubDescription`对象。 该脚本执行以下任务︰

1. 使用`NamespaceManager`对象，请先检查事件中心是否由`$Path`存在。
2. 如果不存在，则创建`EventHubDescription`和传递给`NamespaceManager`类`CreateEventHubIfNotExists`方法。
3. 在确定事件集线器可用之后, 创建者组使用`ConsumerGroupDescription`， `NamespaceManager`。

    ``` powershell
        
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## 下一步行动

这篇文章提供供应服务总线实体使用 PowerShell 的基本轮廓。 使用.NET 客户端库可以执行的任何操作，您还可以执行 PowerShell 脚本中。

更详细的示例上有这些博客文章︰

- [如何创建服务总线队列、 主题和订阅使用 PowerShell 脚本](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [如何创建服务总线 Namespace，使用 PowerShell 脚本事件中心](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

有些现成的脚本，还有可供下载︰
- [服务总线 PowerShell 脚本](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[成员服务]: http://azure.microsoft.com/pricing/member-offers/
[免费试用版]: http://azure.microsoft.com/pricing/free-trial/
[安装和配置 Azure PowerShell]: ../install-configure-powershell.md
[服务总线 NuGet 程序包]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[获得 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[新 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[获得 AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[服务总线的.NET API]: https://msdn.microsoft.com/library/microsoft.servicebus.aspx
 