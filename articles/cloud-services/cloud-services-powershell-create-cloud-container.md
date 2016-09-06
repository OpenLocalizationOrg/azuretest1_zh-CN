---
ms.openlocfilehash: 32b9248af8f5d97bf3dc964d6737ace2ad0a1299
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="如何使用 Azure PowerShell 命令来创建一个空的云服务容器"
   description="本文介绍了如何创建云服务容器和执行云服务相关管理操作使用 PowerShell 脚本"
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="bscholl" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="06/19/2015"
   ms.author="cawa"/>

# 如何使用 Azure PowerShell 命令来创建一个空的云服务容器
1. 从[下载 Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)安装 Microsoft Azure PowerShell cmdlet。 有关进一步的说明安装 Azure PowerShell cmdlet 和连接到您的 Azure 订阅，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。

2. **新 AzureService**是用于创建一个空的云服务容器。

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

   调用该 cmdlet 的一个示例是︰
```
New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
```

3. 有关创建 Azure 云服务的详细信息，请运行︰
```
Get-help New-AzureService
```

4. 下一步︰

   - 若要管理云服务部署，请参阅[获取 AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx)、[删除 AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)和[一组 AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx)命令。 此外请参阅[如何配置云服务](cloud-services-how-to-configure.md)

    - 要发布到 Azure 的云服务项目，请参考**PublishCloudService.ps1**代码示例从[不断在 Azure 的云服务的提供](cloud-services-dotnet-continuous-delivery.md)
 

测试
