---
ms.openlocfilehash: addf481fc3d7e87fde663ef5a7ac2ea82cd20a28
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何将关联组迁移到区域虚拟网络 (VNet)"
   description="了解如何将关联组迁移到区域 vnets"
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
   ms.date="09/02/2015"
   ms.author="telmos" />

# 如何将关联组迁移到区域虚拟网络 (VNet)

相似性组可用于确保在同一个关系组内创建资源实际由相距较近，使这些资源进行通信速度更快的服务器。 在过去，好友小组已创建虚拟网络 (VNets) 的要求。 在那时，托管 VNets 的网络管理器服务无法仅在一组物理服务器或刻度单位内工作。 体系结构改进，提高了网络管理区域范围内。

由于这些体系结构的改进，好友小组不再建议使用，或所需的虚拟网络。 通过区域正在更换 VNets 的好友小组使用。 与区域关联的 VNets 被称为区域 VNets。

此外，我们建议不要使用一般关系组。 除了 VNet 要求，好友小组也是一定要使用，以确保资源，如计算和存储，被放在彼此附近。 但是，与当前的 Azure 的网络体系结构中，这些位置要求不再需要。 几个剩余特定情况下您可能需要使用关联的组，请参阅[关联组和虚拟机](#Affinity-groups-and-VMs)。

## 创建和迁移到区域 VNets

今后，在创建新的 VNets 时，使用*区域*。 您将看到这作为管理门户中的选项。 请注意，在网络配置文件中，这显示*位置*。

>[AZURE.IMPORTANT] 虽然仍以创建与相似性组关联的虚拟网络技术上是可行，但没有任何令人信服理由这么做。 许多新功能，如网络安全组，请使用区域 VNet 时才可用，并不适用于与关系组相关联的虚拟网络。

### 关于目前与好友小组的 VNets

迁移到区域 VNets 启用当前与好友小组的 VNets。 若要迁移到区域 VNet，请执行以下步骤︰

1. 导出网络配置文件。 您可以使用 PowerShell 或管理门户网站。 有关使用管理门户的说明，请参阅[配置使用一个网络配置文件您 VNet](../virtual-networks-using-network-configuration-file/)。

1. 编辑您的网络配置文件，旧的值替换为新值。 

    > [AZURE.NOTE] **位置**是为关系组与您 VNet 指定的区域。 例如，如果您的 VNet 是与位于中西部的美国，在迁移时相似性组相关联，您所在的位置必须指向西部美国。 
    
    编辑替换为您自己的值是您的网络配置文件中的以下行︰ 

    **旧值︰**\<VirtualNetworkSitename ="VNetUSWest"AffinityGroup ="VNetDemoAG"\> 

    **新值︰**\<VirtualNetworkSitename ="VNetUSWest"地址 ="西部美国"\>

1. 将保存您的更改与[导入](../virtual-networks-using-network-configuration-file/)网络配置到 Azure。

>[AZURE.INFO]此迁移不会导致任何停机时间缩到您的服务。

## 好友小组和虚拟机

如上文所述，好友小组一般不再建议进行虚拟机。 仅当虚拟机的设置必须具有虚拟机之间的绝对最低网络延迟时，应使用关系组。 将虚拟机放在关联组中，虚拟机都将成为同一计算群集或刻度单位中的位置。

值得注意的是，使用关联组可以有两个，可能是负的后果︰

- 虚拟机大小的集将被限制为 VM 大小计算刻度单位所提供的一套。

- 没有不能够分配一个新的虚拟机的概率越高。 相似性组的特定比例单位超出容量时，将发生这种情况。

### 如果关联组中有一个虚拟机，怎么办

目前关系组中的虚拟机不需要从关系组中删除。

VM 部署之后，部署到单个刻度单位。 好友小组可以限制的一套新的 VM 部署，可用的虚拟机大小，但任何现有的 VM 部署已经限制在其中部署 VM 的刻度单位中可用的虚拟机大小的集中。 正因为如此，从仿射性组删除虚拟机将没有任何影响。
 
