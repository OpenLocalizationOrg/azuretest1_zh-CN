---
ms.openlocfilehash: 3bac7e4be24c18f9020af334a685ac65057bcf17
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理与 PowerShell Azure 媒体服务帐户" 
    description="了解如何管理使用 PowerShell cmdlet 的 Azure 媒体服务帐户。" 
    authors="Juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015"
    ms.author="juliako"/>


#管理与 PowerShell Azure 媒体服务帐户

> [AZURE.SELECTOR]
- [门户](media-services-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](https://msdn.microsoft.com/library/azure/dn167014.aspx)

##概述 

本文演示了如何使用 PowerShell cmdlet 管理 Azure 媒体服务帐户。

>[AZURE.NOTE]
> 若要完成本教程，您需要一个 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure 免费试用版</a>。

##安装 Microsoft Azure PowerShell Cmdlet

若要安装最新的 Azure PowerShell cmdlet，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)

##请选择 Azure 订阅

一旦您安装并配置 PowerShell 的 cmdlet，应指定要使用哪种订阅中。 

若要获取可用的订阅运行以下 cmdlet 的列表︰

    PS C:\> Get-AzureSubscription

然后，选择一个这样做的︰

    PS C:\> Select-AzureSubscription "TestSubscription"

 
##获取存储帐户名称

Azure 的介质服务使用 Azure 存储来存储媒体内容。 创建新的媒体服务帐户时，您必须将其与存储帐户关联。 存储帐户必须属于同一订阅与您打算使用的介质服务帐户。 

在此示例中，使用现有的存储帐户。 [获取 AzureStorageAccount](https://msdn.microsoft.com/library/azure/dn495134.aspx) cmdlet 获取在当前订阅的存储帐户。 获取您想要将您的媒体帐户相关联的存储帐户的名称 (StorageAccountName)。

    StorageAccountDescription : 
    AffinityGroup             :
    Location                  : East US
    GeoReplicationEnabled     : True
    GeoPrimaryLocation        : East US
    GeoSecondaryLocation      : West US
    Label                     : storagetest001
    StorageAccountStatus      : Created
    StatusOfPrimary           : Available
    StatusOfSecondary         : Available
    Endpoints                 : {https://storagetest001.blob.core.windows.net/,
                                https://storagetest001.queue.core.windows.net/,
                                https://storagetest001.table.core.windows.net/}
    AccountType               : Standard_GRS
    StorageAccountName        : storatetest001
    OperationDescription      : Get-AzureStorageAccount
    OperationId               : e919dd56-7691-96db-8b3c-2ceee891ae5d
    OperationStatus           : Succeeded

##创建新的媒体服务帐户

若要创建新的 Azure 媒体服务帐户，请使用[New AzureMediaServicesAccount](https://msdn.microsoft.com/library/azure/dn495286.aspx) cmdlet 提供媒体服务帐户名、 数据中心位置，它将被创建，并存储帐户名称。 


    PS C:\> New-AzureMediaServicesAccount -Name "amstestaccount001" -StorageAccountName "storagetest001" -Location "East US"

##获得媒体服务帐户

一旦您创建了一个或多个介质服务帐户可以使用[Get AzureMediaServicesAccount](https://msdn.microsoft.com/library/azure/dn495286.aspx)列出的信息

    
    PS C:\> Get-AzureMediaServicesAccount
    
    AccountId       Name                State
    ---------       ----                 -----
    xxxxxxxxxx      amstestaccount001   Active

通过提供名称参数中，您将获得更多详细的信息，包括帐户密钥。

    PS C:\> Get-AzureMediaServicesAccount -Name amstestaccount001

##重新生成媒体服务访问键

如果您想要更新介质服务主要或次要的访问键，请使用[New AzureMediaServicesKey](https://msdn.microsoft.com/library/azure/dn495215.aspx)。 您需要提供帐户名称，并指定您希望重新生成 （主要或辅助） 的密钥。 

指定如果您不希望为确认提问 PowerShell-Force 开关。

    PS C:\> New-AzureMediaServicesKey -Name "amstestaccount001" -KeyType "Primary" -Force

##删除介质服务帐户

准备就绪，可删除 Azure 媒体帐户后，使用[删除 AzureMediaServicesAccount](https://msdn.microsoft.com/library/azure/dn495220.aspx)。

    PS C:\> Remove-AzureMediaServicesAccount -Name "amstestaccount001" -Force

 
测试
