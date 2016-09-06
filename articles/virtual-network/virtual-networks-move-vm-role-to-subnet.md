---
ms.openlocfilehash: a0c2dc6f6eeb0df38480cccde8392e96974f183d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何将虚拟机或角色实例移动到不同的子网"
   description="了解如何将虚拟机和角色实例移动到不同的子网"
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
   ms.date="05/29/2015"
   ms.author="telmos" />

# 如何将虚拟机或角色实例移动到不同的子网

您可以使用 PowerShell 将虚拟机从一个子网移动到另中相同的虚拟网络 (VNet)。 可以通过编辑 CSCFG，而不是使用 PowerShell 移动角色实例。

为什么将虚拟机移动到另一个子网？ 旧的网太小并且无法扩展由于现有的子网中运行中的 Vm 时，子网迁移非常有用。 在这种情况下，可以创建一个新的、 更大的子网，并将虚拟机迁移到新的子网，然后完成迁移后，您可以删除旧空的子网。

## 如何将虚拟机移动到另一个子网

若要移动虚拟机，请运行组 AzureSubnet PowerShell cmdlet，使用下面的示例作为模板。 在下面的示例中，我们将转向 TestVM 从现网，子网 2。 请务必编辑的示例，以反映您的环境。 请注意，每当您运行更新 AzureVM cmdlet 作为过程的一部分，它将重新启动 VM 更新过程的一部分。

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

如果您指定静态 DIP 的 VM，您必须清除该设置，然后可以将 VM 移动到新的子网。 在这种情况下，使用以下命令︰

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## 将角色实例移动到另一个子网

若要移动角色实例，请编辑 CSCFG 文件。 在下面的示例中，我们将转向"Role0"虚拟网络*VNETName*中从其存在的子网*子网 2*。 因为已部署角色实例，您将更改子网名称 = 子网 2。 请务必编辑的示例，以反映您的环境。

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 