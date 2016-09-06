---
ms.openlocfilehash: 3d1e8fc1afe2f9b293224ca8579aca15086be568
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="使用 PowerShell 管理服务总线和事件集线器资源"
   description="使用 PowerShell 创建和管理服务总线和事件集线器资源"
   services="service-bus"
   documentationCenter=".NET"
   authors="sethmanheim"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="08/14/2015"
   ms.author="sethm"/>

# 使用 PowerShell 管理服务总线和事件集线器资源

Microsoft Azure PowerShell 是一种可用于控制和自动部署和管理 Azure 服务的脚本环境。 本文介绍如何使用 PowerShell 调配和管理服务总线的实体，如命名空间、 队列和使用本地的 Azure PowerShell 控制台事件集线器。

## 先决条件

在开始之前，您将需要以下︰

- Azure 的订阅。 Azure 是一个基于订阅的平台。 有关获取订阅的详细信息，请参阅[购买选项]，[成员提供]，或[免费试用版]。

- 带有 Azure PowerShell 的计算机。 有关说明，请参阅[安装和配置 Azure PowerShell]

- PowerShell 脚本，NuGet 程序包和.NET Framework 大概了解。

## 包括对服务总线的.NET 程序集的引用

可用于管理服务总线是有限的数量的 PowerShell cmdlet。 若要设置实体通过现有的 cmdlet 不公开的可以用于.NET 客户端服务总线从 PowerShell 中通过引用[服务总线 NuGet 程序包]。

首先，确保脚本可以找到使用 NuGet 程序包安装的**Microsoft.ServiceBus.dll**程序集。 为了保持灵活，该脚本执行下列步骤︰

1. 确定调用它的路径。
2. 遍历路径，直到它找到一个文件夹命名为`packages`。 NuGet 程序包安装时，会创建此文件夹。
3. 以递归方式搜索`packages`名为**Microsoft.ServiceBus.dll**的程序集的文件夹。
4. 引用程序集，以使类型可供以后使用。

以下是这些步骤 PowerShell 脚本中的实现方式︰

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
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

当使用服务总线命名空间时，有两个 cmdlet，您可以使用而不是.NET SDK︰[获取 AzureSBNamespace]和[新建 AzureSBNamespace]。

本示例创建几个本地变量在脚本;`$Namespace` and `$Location`.

- `$Namespace` 名称是我们想要的服务总线命名空间。
- `$Location` 标识数据中心，我们将配置命名空间。
- `$CurrentNamespace` 存储我们检索 （或创建） 的引用命名空间。

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
若要设置其他服务总线实体，创建的实例`NamespaceManager`SDK 中的对象。 可以使用[Get AzureSBAuthorizationRule] cmdlet 来检索授权规则，用于提供的连接字符串。 本示例将一个引用存储`NamespaceManager`实例的`$NamespaceManager`变量。 之后，该脚本将使用`$NamespaceManager`提供其他实体。

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## 提供其他服务总线实体

要设置其他实体，如队列、 主题和事件集线器可以使用[.NET API 的服务总线]。 更详细的示例，包括其他实体，引用这篇文章的末尾。

### 创建事件中心

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

    # Check if the Event Hub already exists
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

### 创建队列

若要创建一个队列或主题，执行如前一节中所示的命名空间检查。

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### 创建主题

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## 下一步行动

这篇文章提供了一个基本轮廓调配使用 PowerShell 的服务总线实体。 尽管有有限的数量的 PowerShell cmdlet 可用于管理服务总线消息通过几乎任何操作的 Microsoft.ServiceBus.dll 程序集引用的实体，您可以执行使用.NET 客户端库，您也可以执行 PowerShell 脚本中。

更详细的示例上有这些博客文章︰

- [如何创建服务总线队列、 主题和订阅使用 PowerShell 脚本](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [如何创建服务总线 Namespace，使用 PowerShell 脚本事件中心](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

一些现成的脚本，还有可供下载︰
- [服务总线 PowerShell 脚本](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[购买选项]: http://azure.microsoft.com/pricing/purchase-options/
[成员服务]: http://azure.microsoft.com/pricing/member-offers/
[免费试用版]: http://azure.microsoft.com/pricing/free-trial/
[服务总线 NuGet 程序包]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[获得 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[新 AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[获得 AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[服务总线的.NET API]: https://msdn.microsoft.com/library/microsoft.servicebus.aspx
[安装和配置 Azure PowerShell]: ../install-configure-powershell.md
