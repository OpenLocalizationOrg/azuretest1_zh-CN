---
ms.openlocfilehash: a97e635405c0387cba30a02428ff6c88901ed09d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理 DNS 服务器使用的虚拟网络 (VNet)"
   description="了解如何添加和移除虚拟网络 (vnet) 中的 DNS 服务器"
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

# 管理 DNS 服务器使用的虚拟网络 (VNet)

您可以管理在管理门户中，或在网络配置文件 VNet 中所用的 DNS 服务器的列表。 您可以添加最多 12 个 DNS 服务器的每个 VNet。 当指定 DNS 服务器，务必验证您列出您的 DNS 服务器为您的环境的正确顺序。 DNS 服务器列表无效循环。 它们用于指定它们时的顺序。 如果能够达到列表上的第一个 DNS 服务器，客户端将使用该 DNS 服务器，DNS 服务器是否工作正确与否不论。 若要更改您的虚拟网络的 DNS 服务器顺序，请从列表中删除 DNS 服务器，并将其添加回中所需的顺序。

>[AZURE.WARNING] 更新 DNS 列表后，您必须重新启动虚拟机位于虚拟网络中，使他们拿起新的 DNS 服务器设置。 虚拟机将继续使用其当前的配置，直到他们重新启动。

## 编辑虚拟网络使用管理门户的 DNS 服务器列表

1. 登录到**管理门户**。

1. 在导航窗格中，单击**网络**，，然后单击虚拟网络**名称**列中的名称。

1. 单击**配置**。

1. 在**DNS 服务器**中，您可以进行下列配置︰

    - **若要注册 （添加） 一个新的 DNS 服务器 –**只是在框中键入的名称和 IP 地址。 这将 DNS 服务器添加到您的虚拟网络的 DNS 服务器列表，还将与 Azure 注册的 DNS 服务器。

    - **若要添加 DNS 服务器以前注册的 –**如果您已经注册 Azure 的 DNS 服务器，您可以从预填充的列表中选择。

    - **要删除 DNS 服务器从虚拟网络 —**单击要删除的服务器旁边的 X。 请注意，这仅服务器从列表中删除此虚拟网络。 DNS 服务器保留注册的其他虚拟网络的 Azure 使用中。 要从您的订阅中删除 DNS 服务器，请转到**网络-> DNS 服务器**页面。

    - **若要重新排序的 DNS 服务器 –**删除所有 DNS 服务器都已列出，并将它们添加回所需的顺序。 请记住，这不是循环复用 DNS 列表。

    - **若要重命名一个 DNS 服务器 –**突出显示的 DNS 服务器，在列表中，然后键入新名称。 这将在 Azure，注册新的 DNS 服务器，以及将其添加到您的虚拟网络的 DNS 服务器列表。 旧的 DNS 服务器和它的 IP 地址将一直保持注册使用 Azure。 如果您在不使用任何其他的虚拟网络，您可以在**DNS 服务器**页上删除它。

1. 单击**保存**页面底部以保存新的 DNS 服务器配置。

1. 重新启动虚拟机中的虚拟网络，让他们可以获得新的 DNS 设置。

## 编辑 DNS 服务器列表中使用的网络配置文件

若要通过一个网络配置文件编辑 DNS 服务器列表中，将首先从管理门户导出配置设置。 然后将编辑网络配置文件，并将其导回通过管理门户。 下面是完成此过程的步骤的高级列表。

1. 将虚拟网络设置导出到一个网络配置文件。 有关详细信息和步骤导出网络配置设置，请参阅[导出虚拟网络设置应用于网络配置文件](virtual-networks-using-network-configuration-file.md)。

1. 指定 DNS 服务器信息的虚拟网络。 有关指定 DNS 服务器的详细信息，请参阅[指定 DNS 服务器在虚拟的网络配置文件](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)。 有关网络配置文件的其他信息，请参阅[Azure 虚拟网络配置架构](https://msdn.microsoft.com/library/azure/jj157100.aspx)和[配置虚拟网络使用网络配置文件](virtual-networks-using-network-configuration-file.md)。

1. 网络配置文件导入。 有关详细信息和步骤来导入您的网络配置文件，请参阅[导入网络配置文件](virtual-networks-using-network-configuration-file.md)。

1. 重新启动虚拟机中的虚拟网络，让他们可以获得新的 DNS 设置。

## 下一步行动

[如何管理虚拟网络 (VNet) 属性](../virtual-networks-settings)

[如何在虚拟的网络中使用公用 IP 地址](../virtual-networks-public-ip-within-vnet)

[如何删除虚拟网络 (VNet)](../virtual-networks-delete-vnet) 
