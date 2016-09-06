---
ms.openlocfilehash: 8ea3566e9f9b2ab8fbfcf82f51f0e385f4901faa
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="导航并选择使用 PowerShell 和 Azure CLI 的 Azure 虚拟机映像"
   description="了解如何创建与资源管理器的 Azure 的虚拟机的图像确定发布服务器、 优惠和 SKU。"
   services="virtual-machines"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="command-line-interface"
   ms.workload="infrastructure"
   ms.date="08/25/2015"
   ms.author="rasquill"/>

# 导航并选择使用 Windows PowerShell 和 Azure CLI 的 Azure 虚拟机映像

> [AZURE.NOTE] 当您要搜索在本主题中的虚拟机映像时，您最近安装或 Windows PowerShell 的 Mac、 Linux、 和 Windows Azure 的任一命令行界面使用[Azure 资源管理器模式](../resource-group-overview.md)。 使用 Azure CLI，该模式通过键入来输入`azure config mode arm`。 使用 PowerShell，键入`Switch-AzureMode AzureResourceManager`。 请参阅[使用 Azure CLI 使用资源管理器](xplat-cli-azure-resource-manager.md)和完成更新和配置的详细信息的详细信息的[使用 Azure PowerShell 使用 Azure 资源管理器中](../powershell-azure-resource-manager.md)。

## 常用的图像的表


| PublisherName                        | 提供                                 | Sku                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| OpenLogic                        | CentOS                                     | 7                                |
| OpenLogic                        | CentOS                                     | 7.1                              |
| CoreOS                           | CoreOS                                     | Beta                             |
| CoreOS                           | CoreOS                                     | 稳定                           |
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| Microsoft                        | Oracle-Database-12c-Weblogic-Server-12c    | 标准                         |
| Microsoft                        | Oracle-Database-12c-Weblogic-Server-12c    | 企业                       |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | 企业-优化-的-数据仓库      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | 企业-优化-的-OLTP    |
| 规范                        | UbuntuServer                               | 12.04.5-LTS                      |
| 规范                        | UbuntuServer                               | 14.04.2-LTS                      |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 数据中心                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 R2 数据中心               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows 服务器技术预览 |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


## Azure CLI

最简单和最快的方式来定位图像使用与`azure vm quick-create`或创建资源组模板文件是调用`azure vm image list`命令并传递位置，发布服务器名称 （不区分大小写 ！），并提供 — 如果您知道此项优惠活动。 例如，下面的列表只是简短示例--许多列表都很长，如果您知道"Canonical"的发布者为"UbuntuServer"优惠。

    azure vm image list westus canonical ubuntuserver
    info:    Executing command vm image list
    warn:    The parameter --sku if specified will be ignored
    + Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
    data:    Publisher  Offer         Sku          Version          Location  Urn
    data:    ---------  ------------  -----------  ---------------  --------  --------------------------------------------------
    data:    canonical  ubuntuserver  12.04-DAILY  12.04.201504201  westus    canonical:ubuntuserver:12.04-DAILY:12.04.201504201
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201302250  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201302250
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201303250  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201303250
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201304150  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201304150
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201305160  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201305160
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201305270  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201305270
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201306030  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201306030
    data:    canonical  ubuntuserver  12.04.2-LTS  12.04.201306240  westus    canonical:ubuntuserver:12.04.2-LTS:12.04.201306240

该**Urn**列将窗体传递给`azure vm quick-create`。

通常情况下，但是，还不知道什么是可用。 在这种情况下，您可以通过使用第一次发现出版商导航图像`azure vm image list-publishers`和响应与预期使用的资源组的数据中心位置的位置提示。 例如，下面列出了中西部美国位置 (pass 位置参数由小写和从标准的位置删除空格) 的所有图像出版商

    azure vm image list-publishers
    info:    Executing command vm image list-publishers
    Location: westus
    + Getting virtual machine image publishers (Location: "westus")
    data:    Publisher                                       Location
    data:    ----------------------------------------------  --------
    data:    a10networks                                     westus  
    data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
    data:    alertlogic                                      westus  
    data:    AlertLogic.Extension                            westus  


这些列表可能很长，因此上面的示例列表是刚才的片段。 让我们假设我注意到，Canonical 是，事实上，在美国西部的位置图像发布服务器。 您现在可以找到其提供通过调用`azure vm image list-offers`和传递和发布服务器的位置处的提示，如下例所示︰

    azure vm image list-offers
    info:    Executing command vm image list-offers
    Location: westus
    Publisher: canonical
    + Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
    data:    Publisher  Offer         Location
    data:    ---------  ------------  --------
    data:    canonical  UbuntuServer  westus  
    info:    vm image list-offers command OK

现在我们知道，在美国西部地区，Canonical 发布**UbuntuServer**提议在 Azure 上。 但哪种 Sku 吗？ 若要实现这些，需要调用`azure vm image list-skus`并做出响应的提示与位置、 发布者和已经发现的优惠。

    azure vm image list-skus
    info:    Executing command vm image list-skus
    Location: westus
    Publisher: canonical
    Offer: ubuntuserver
    + Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
    data:    Publisher  Offer         sku          Location
    data:    ---------  ------------  -----------  --------
    data:    canonical  ubuntuserver  12.04-DAILY  westus  
    data:    canonical  ubuntuserver  12.04.2-LTS  westus  
    data:    canonical  ubuntuserver  12.04.3-LTS  westus  
    data:    canonical  ubuntuserver  12.04.4-LTS  westus  
    data:    canonical  ubuntuserver  12.04.5-LTS  westus  
    data:    canonical  ubuntuserver  12.10        westus  
    data:    canonical  ubuntuserver  14.04-beta   westus  
    data:    canonical  ubuntuserver  14.04-DAILY  westus  
    data:    canonical  ubuntuserver  14.04.0-LTS  westus  
    data:    canonical  ubuntuserver  14.04.1-LTS  westus  
    data:    canonical  ubuntuserver  14.04.2-LTS  westus  
    data:    canonical  ubuntuserver  14.10        westus  
    data:    canonical  ubuntuserver  14.10-beta   westus  
    data:    canonical  ubuntuserver  14.10-DAILY  westus  
    data:    canonical  ubuntuserver  15.04        westus  
    data:    canonical  ubuntuserver  15.04-beta   westus  
    data:    canonical  ubuntuserver  15.04-DAILY  westus  
    info:    vm image list-skus command OK

使用此信息，您现在可以找到完全通过顶部调用原始调用所需的图像。

    azure vm image list westus canonical ubuntuserver 14.04.2-LTS
    info:    Executing command vm image list
    + Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "14.04.2-LTS" Location:"westus")
    data:    Publisher  Offer         Sku          Version          Location  Urn
    data:    ---------  ------------  -----------  ---------------  --------  --------------------------------------------------
    data:    canonical  ubuntuserver  14.04.2-LTS  14.04.201503090  westus    canonical:ubuntuserver:14.04.2-LTS:14.04.201503090
    data:    canonical  ubuntuserver  14.04.2-LTS  14.04.20150422   westus    canonical:ubuntuserver:14.04.2-LTS:14.04.20150422
    data:    canonical  ubuntuserver  14.04.2-LTS  14.04.201504270  westus    canonical:ubuntuserver:14.04.2-LTS:14.04.201504270
    info:    vm image list command OK

现在，您可以精确地想要使用的图像。 若要通过使用 URN 信息，只需找到快速创建虚拟机或模板使用该 URN 信息，请参阅[使用 Azure CLI Mac、 Linux 和 Windows Azure 资源管理器使用](xplat-cli-azure-resource-manager.md)。

### 视频的演练

该视频演示使用 CLI 执行上述步骤。

[AZURE.VIDEO resource-groups-vm-searching-cli]


## PowerShell

当使用 Azure 资源管理器中创建新的虚拟机，在某些情况下您需要指定图像下面的图像属性的组合︰

- Publisher
- 提供
- SKU

例如，这些值需要**设置 AzureVMSourceImage** PowerShell cmdlet 或与资源组模板文件必须在其中指定要创建的虚拟机的类型。

如果您需要确定这些值，您可以导航的图像，以确定这些值︰

1. 列出映像出版。
2. 为给定的发布服务器，列出了它们的优惠。
3. 对于给定的报价，列出其 Sku。

要继续执行此操作，首先切换到 Azure PowerShell 的资源管理器模式。

    Switch-AzureMode AzureResourceManager

对于上面的第一步，列出使用这些命令的发布者。

    $locName="<Azure location, such as West US>"
    Get-AzureVMImagePublisher -Location $locName | Select PublisherName

填写您选择发布者的名称和运行这些命令。

    $pubName="<publisher>"
    Get-AzureVMImageOffer -Location $locName -Publisher $pubName | Select Offer

填写您选择的提供的名称和运行这些命令。

    $offerName="<offer>"
    Get-AzureVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

从**获取 AzureVMImageSku**命令显示，您必须指定为新的虚拟机的图像所需的所有信息。

下面是一个示例。

    PS C:\> $locName="West US"
    PS C:\> Get-AzureVMImagePublisher -Location $locName | Select PublisherName

    PublisherName
    -------------
    a10networks
    aiscaler-cache-control-ddos-and-url-rewriting-
    alertlogic
    AlertLogic.Extension
    Barracuda.Azure.ConnectivityAgent
    barracudanetworks
    basho
    boxless
    bssw
    Canonical
    ...

"MicrosoftWindowsServer"发布者︰

    PS C:\> $pubName="MicrosoftWindowsServer"
    PS C:\> Get-AzureVMImageOffer -Location $locName -Publisher $pubName | Select Offer

    Offer
    -----
    WindowsServer

为"WindowsServer"优惠︰

    PS C:\> $offerName="WindowsServer"
    PS C:\> Get-AzureVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

    Skus
    ----
    2008-R2-SP1
    2012-Datacenter
    2012-R2-Datacenter
    Windows-Server-Technical-Preview

从此列表中复制选定的 SKU 名称，并具有**一组 AzureVMSourceImage** PowerShell cmdlet 或要求您为图像指定发布服务器、 优惠和 SKU 的资源组模板文件的所有信息。

### 视频的演练

该视频演示使用 PowerShell 的上述步骤。

[AZURE.VIDEO resource-groups-vm-searching-posh]


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/
