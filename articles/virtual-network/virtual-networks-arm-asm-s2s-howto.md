---
ms.openlocfilehash: 0d75d71a2622c790c520c54cd836c856ea1ea723
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="如何连接到 ARM VNets Azure 中的经典 VNets"
   description="了解如何创建经典的 VNets 和新 VNets 之间的 VPN 连接"
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
   ms.date="07/14/2015"
   ms.author="telmos" />

# 连接到新的 VNets 的经典 VNets

Azure 目前有两种管理模式︰ Azure 服务管理器 （称为为经典） 和 Azure 资源管理器 (ARM)。 如果您一直使用 Azure 的一些时间，您可能有 Azure Vm 和经典的 VNet 上运行的实例角色。 和您的新虚拟机和角色实例可以运行在 ARM 中创建的 VNet。

在这种情况下，要确保新的基础结构能够与传统资源进行通信。 您可以通过创建两个 VNets 之间的 VPN 连接来这样做。 

在本文中，您将学习如何创建站点到站点 (S2S) 经典 VNet 和 ARM VNet 之间的 VPN 连接。

>[AZURE.NOTE] 本文假定您已经拥有经典的 VNets，并且 ARM VNets，并且熟悉为典型的 VNets 配置 S2S VPN 连接。 详细的端到端解决方案之间经典的 S2S VPN 连接上，ARM VNets 访问[解决方案指南-连接到经典的 VNet 和 S2S VPN 使用 ARM VNet](../virtual-networks-arm-asm-s2s.md)。

您可以看到经典的 VNet 和 ARM VNet 之间的 S2S VPN 连接通过使用来创建以下 Azure 网关能完成的任务的概述。

1-[创建经典 VNet 的 VPN 网关](#Step-1:-Create-a-VPN-gateway-for-the-classic-VNet)

2-[创建 ARM VNet 的 VPN 网关](#Step-2:-Create-a-VPN-gateway-for-the-ARM-VNet)

3-[创建一个网关之间的连接](#Step-3:-Create-a-connection-between-the-gateways)

## 步骤 1︰ 为经典的 VNet 创建的 VPN 网关

要为经典的 VNet 创建的 VPN 网关，请按照下面的说明。

1. 从 https://manage.windowsazure.com，打开传统门户网站，输入您的凭据，如有必要。
2. 在屏幕的左下角，单击**新建**按钮，然后单击**网络服务**单击**虚拟网络**，然后再单击**添加本地网络**。
3. 在**指定您的本地网络的详细信息**窗口中，键入要连接到，然后在窗口的右下角，单击箭头按钮上 ARM VNet 的名称。
3. 在地址空间**起始 IP**的文本框中，键入您想要连接到 ARM VNet 网络前缀。 
4. 在**CIDR （地址计数）**下拉列表中，选择使用 CIDR 块由 ARM VNet，您想要连接到的网络部分的比特数。
5. 在**VPN 设备 IP 地址 （可选）**，输入任何有效的公共 IP 地址。 我们将在以后更改此 IP 地址。 然后单击复选标记按钮在屏幕的右下方。 下图显示了此页面的示例设置。

    ![本地网络设置](..\virtual-network\media\virtual-networks-arm-asm-s2s-howto\figurex1.png)

5. 在**网络**页面中，在**虚拟网络**上，请单击然后单击您经典的 VNet，然后单击**配置**。
6. 在**站点对站点连接**下启用**连接到本地网络**复选框。
7. 选择本地网络，则步骤 4 中创建从列表从**本地网络**下拉列表中，可用的网络，然后单击**保存**。
8. 一旦保存设置，单击上**的仪表板**，然后在页面底部单击**创建网关**，然后**动态路由**，请单击，然后单击**是**。
9. 等待创建并复制其公用 IP 地址的网关。 您将需要设置在 ARM VNet 网关。

## 第 2 步︰ 为 ARM VNet 创建的 VPN 网关

要为 ARM VNet 创建的 VPN 网关，请按照下面的说明进行操作。

1. 从 PowerShell 控制台，通过运行以下命令来切换到 ARM 模式。

        Switch-AzureMode AzureResourceManager

2. 通过运行以下命令创建一个本地网络。 经典的 VNet 您想要连接到的 CIDR 块和网关在上面的步骤 1 中创建的公用 IP 地址，则必须使用本地网络。

        New-AzureLocalNetworkGateway -Name VNetClassicNetwork `
            -Location "East US" -AddressPrefix "10.0.0.0/20" `
            -GatewayIpAddress "168.62.190.190" -ResourceGroupName RG1

3. 通过运行以下命令来创建网关的公共 IP 地址。

        $ipaddress = New-AzurePublicIpAddress -Name gatewaypubIP`
            -ResourceGroupName RG1 -Location "East US" `
            -AllocationMethod Dynamic

4. 检索通过运行下面的命令为该网关使用的子网。

        $subnet = Get-AzureVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureVirtualNetwork -Name VNetARM -ResourceGroupName RG1) 

    >[AZURE.IMPORTANT] 网关网必须存在，并且它必须命名为 GatewaySubnet。

5. 通过运行以下命令来创建网关的 IP 配置对象。 请注意一个网关的子网 id。 该子网必须存在于 VNet。

        $ipconfig = New-AzureVirtualNetworkGatewayIpConfig `
            -Name ipconfig -PrivateIpAddress 10.1.2.4 `
            -SubnetId $subnet.id -PublicIpAddressId $ipaddress.id

    >[AZURE.IMPORTANT] *SubnetId*和*PublicIpAddressId*参数必须从子网和 IP 地址对象，repectively 传递的 id 属性。 您不能使用简单的字符串。
    
5. 通过运行以下命令创建 ARM VNet 网关。

        New-AzureVirtualNetworkGateway -Name v1v2Gateway -ResourceGroupName RG1 `
            -Location "East US" -GatewayType Vpn -IpConfigurations $ipconfig `
            -EnableBgp $false -VpnType RouteBased

6. 创建 VPN 网关后，通过运行以下命令来检索其公用 IP 地址。 复制的 IP 地址，您将需要此为典型的 VNet 配置本地网络。

        Get-AzurePublicIpAddress -Name gatewaypubIP -ResourceGroupName RG1

## 步骤 3︰ 创建网关之间的连接

1. 从 https://manage.windowsazure.com，打开传统门户网站，输入您的凭据，如有必要。
2. 在经典的门户中，向下滚动**网络**，然后单击**局域网**，然后单击您想要连接，然后单击**编辑**按钮上的 ARM VNet。
3. 在**VPN 设备 IP 地址 （可选）**，同上，第 2 步中检索 ARM VNet 网关的 IP 地址的类型然后单击右下角上的右箭头，然后单击复选标记按钮。
4. 从 PowerShell 控制台，通过运行以下命令来切换到 Azure 服务管理器模式。

        Switch-AzureMode AzureServiceManager

5. 通过运行以下命令来设置一个共享的密钥。 确保将到 VNets 的名称更改为您自己的 VNet 名。

        Set-AzureVNetGatewayKey -VNetName VNetClassic `
            -LocalNetworkSiteName VNetARM -SharedKey abc123

6. 从 PowerShell 控制台，通过运行以下命令来切换到 Azure 资源管理器模式。

        Switch-AzureMode AzureResourceManager

7. 通过运行以下命令创建 VPN 连接。

        $vnet01gateway = Get-AzureLocalNetworkGateway -Name VNetClassic -ResourceGroupName RG1
        $vnet02gateway = Get-AzureVirtualNetworkGateway -Name v1v2Gateway -ResourceGroupName RG1
        
        New-AzureVirtualNetworkGatewayConnection -Name arm-asm-s2s-connection `
            -ResourceGroupName RG1 -Location "East US" -VirtualNetworkGateway1 $vnet02gateway `
            -LocalNetworkGateway2 $vnet01gateway -ConnectionType IPsec `
            -RoutingWeight 10 -SharedKey 'abc123'

## 下一步行动

- 了解更多关于[为 ARM 网络资源提供者 (NRP)](../resource-groups-networking.md)。
- 创建[连接到使用 S2S VPN ARM VNet 经典 VNet 的端到端解决方案](../virtual-networks-arm-asm-s2s.md)。