---
ms.openlocfilehash: ad809b35e1991fc09b83ec53822897f05c4c2639
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何管理访问控制列表 (Acl) 为终结点使用 PowerShell"
   description="了解如何管理使用 PowerShell 的 Acl"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/08/2015"
   ms.author="telmos" />

# 如何管理访问控制列表 (Acl) 为终结点使用 PowerShell

您可以创建和管理网络访问控制列表 (Acl) 为终结点使用 Azure PowerShell 或管理门户中。 在本主题中，您将找到您可以完成使用 PowerShell 的 ACL 常见任务过程。 Azure PowerShell cmdlet 的列表请参阅[Azure 管理 Cmdlet](http://go.microsoft.com/fwlink/?LinkId=317721)。 有关 Acl 的详细信息，请参阅[网络访问控制列表 (ACL) 是什么？](../virtual-networks-acl)。 如果您想要通过管理门户管理您的 Acl，请参阅[如何设置设置终结点的虚拟机](../virtual-machines-set-up-endpoints/)。

## 通过使用 Azure PowerShell 管理网络 Acl

Azure PowerShell cmdlet 可用于创建、 删除和配置 （设置） 网络访问控制列表 (Acl)。 我们提供了一些方法，您可以配置 ACL 使用 PowerShell 的几个示例。

若要检索的 ACL PowerShell cmdlet 的完整列表，可以使用下列任一︰

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### 允许从远程子网的访问的规则创建了网络的 ACL

下面的示例演示如何创建一个包含规则的新 ACL。 此 ACL 将被应用到虚拟机的终结点。 在以下示例中的 ACL 规则将允许来自远程子网的访问。 若要创建新的网络 ACL 允许规则，用于远程子网，请打开 Azure PowerShell ISE。 复制和粘贴下面，使用您自己的值，配置该脚本的脚本以及然后运行该脚本。

1. 创建新的网络 ACL 对象。

        $acl1 = New-AzureAclConfig

1. 设置允许从远程子网的访问规则。 在下面的示例中，您可以设置规则*100* （它的优先级高于 200 或更高版本的规则） 来允许远程子网*10.0.0.0/8*访问虚拟机终结点。 值替换为您自己的配置要求。 名称"SharePoint ACL 配置"应被替换想要调用此规则的友好名称。

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. 其他规则，请重复该 cmdlet，值替换为您自己的配置要求。 请务必更改规则顺序以反映您想要应用规则的顺序进行编号。 较低的规则编号将优先于较大的数字。

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. 接下来，可以创建新的终结点 （添加） 或设置的 ACL （设置） 现有的终结点。 在此示例中，我们将添加新的虚拟机终结点称为"web"并使用 ACL 设置更新虚拟机终结点。

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. 接下来，将 cmdlet 合并，然后运行该脚本。 对于此示例，这些组合的 cmdlet 将如下所示︰

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### 删除网络 ACL 规则，允许从远程子网的访问

下面的示例演示如何删除网络 ACL 规则的方法。  要删除允许的规则，用于远程子网的网络 ACL 规则，请打开 Azure PowerShell ISE。 复制和粘贴下面，使用您自己的值，配置该脚本的脚本以及然后运行该脚本。

1. 第一步是为虚拟机端点获取网络 ACL 对象。 然后，您将删除 ACL 规则。 在这种情况下，我们会删除它按规则 id。 这将仅从 ACL 中删除规则 ID 0。 它不会删除该 ACL 对象从虚拟机终结点。 

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. 下一步，必须将网络 ACL 对象应用到虚拟机的终结点并更新该虚拟机。 

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### 从虚拟机终结点中删除网络 ACL

在某些情况下，您可能需要从一个虚拟机的终结点中删除网络 ACL 对象。 为此，请打开 Azure PowerShell ISE。 复制和粘贴下面，使用您自己的值，配置该脚本的脚本以及然后运行该脚本。

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## 请参见

[网络访问控制列表 (ACL) 是什么？](../virtual-networks-acl)

[如何设置与虚拟机进行通信](http://go.microsoft.com/fwlink/?LinkId=303938) 