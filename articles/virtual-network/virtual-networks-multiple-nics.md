---
ms.openlocfilehash: 790665a2ddd3459aa74f6fdaba77861f324d95d5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建具有多个 Nic 的 VM"
   description="了解如何创建和使用多个 nic 配置虚拟机"
   services="virtual-network, virtual-machines"
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
   ms.date="08/10/2015"
   ms.author="telmos" />

# 创建具有多个 Nic 的 VM

多网卡功能允许您创建和管理在 Azure 的虚拟机 (Vm) 上多个虚拟网络接口卡 (Nic)。 多个 NIC 是许多网络虚拟装置，例如应用程序交付和 WAN 优化解决方案的要求。 多个 NIC 还提供了多个网络通信管理功能，包括之间的 NIC 前端和后端 nic 进行通信，通信的隔离或数据平面通信与管理平面通信分开。 

![虚拟机的多网卡](./media/virtual-networks-multiple-nics/IC757773.png)

上图显示三个 nic，虚拟机的每个连接到不同的子网。

## 要求和约束

到目前为止，多网卡具有以下要求和约束︰ 

- 必须在 Azure 虚拟网络 (VNets) 中创建多个 NIC 的虚拟机。 不支持非 VNet 虚拟机。 
- 在一个云服务，允许使用下面的设置︰ 
    - 在云服务中的所有虚拟机都必须是的多网卡启用，或 
    - 在云服务中的所有虚拟机都必须安装有一个 NIC 

>[AZURE.IMPORTANT] 如果您尝试将多个 NIC VM 添加到已包含单个 NIC VM （或相反） 的部署 （云服务），您将收到以下错误︰ 具有辅助网络接口的虚拟机和虚拟机没有辅助网络接口不支持在同一部署中，也无法更新虚拟机无任何辅助网络接口具有辅助网络接口，反之亦然。
 
- 在"默认值"NIC 上才支持面向 Internet 的 VIP 没有默认 NIC 的 IP 到只有一个 VIP 
- 到目前为止，实例级公用 IP (LPIP) 地址不支持多个 NIC 的虚拟机。 
- 从 VM 内部 Nic 的顺序将随机的并且也可以在 Azure 基础结构更新更改。 但是，IP 地址，以及相应的以太网 MAC 地址将保持不变。 例如，假定**Eth1**具有 10.1.0.100 的 IP 地址和 MAC 地址 00-0D-3A-B0-39-0D;在 Azure 的基础结构后更新并重新启动时，它会更改为 Eth2，但 IP 和 MAC 配对将保持不变。 重新启动，则客户发出，NIC 订单将保持不变。 
- 在每个虚拟机上每个网卡的地址必须位于子网中，单个 VM 上的多个 Nic 可以每一个分配都位于同一子网的地址。 
- VM 大小确定 NIC，您可以为虚拟机创建的数。 下表列出的 Nic 对应的虚拟机大小的数字︰ 

|VM 大小 (标准 Sku)|Nic （最大允许每个虚拟机）|
|---|---|
|所有的基本尺寸|1|
|A0\extra 小|1|
|A1\small|1|
|A2\medium|1|
|A3\large|2|
|A4\extra 大|4|
|A5|1|
|A6|2|
|A7|4|
|A8|2|
|A9|4|
|A10|2|
|A11|4|
|D1|1|
|D2|2|
|D3|4|
|D4|8|
|D11|2|
|D12|4|
|D13|8|
|D14|16|
|DS1|1|
|DS2|2|
|DS3|4|
|DS4|8|
|DS11|2|
|DS12|4|
|DS13|8|
|DS14|16|
|G1|1|
|G2|2|
|G3|4|
|G4|8|
|G5|16|
|所有其他大小|1|

## 网络安全组 (NSGs)
在虚拟机上的所有 NIC 都可能与网络安全组 (NSG)，包括任何 Nic 上一个已启用的多个 Nic 的虚拟机相关联。 如果某个 NIC 分配所在 NSG 与关联子网的子网内的地址，然后中子网的 NSG 的规则也应用于该网卡。 除了将子网与 NSGs 相关联，您可以将 NIC 关联的 NSG。 

如果子网关联 NSG，并且 NIC 在该子网内的分别与 NSG，NSG 中的关联的规则应用在"**流顺序**"根据进出网卡传递的通信的方向︰ 

- 其目标是问题中的 NIC**传入通信**流动首先子网，触发子网的 NSG 规则，传递到 NIC，然后触发网卡的 NSG 规则之前。 
- 从网卡，触发网卡的 NSG 规则，通过子网，然后触发子网的 NSG 规则之前**传出通信**源是网卡问题第一次流出。 

上图表示应用程序完成的 NSG 规则如何基于 traffice 流 （从虚拟机到子网，或到虚拟机的子网）。

## 如何配置多个网卡 VM

以下说明将帮助您创建多个包含 3 Nic 网卡 VM︰ 默认的 NIC 以及两个额外的 Nic。 配置步骤将创建一个将根据下面的服务配置文件片段配置的虚拟机︰

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


尝试运行该示例中的 PowerShell 命令之前，您需要以下系统必备组件。

- Azure 的订阅。
- 已配置的虚拟网络。 有关 VNets 的详细信息，请参阅[虚拟网络概述](virtual-networks-overview.md)。
- 最新版本的 Azure PowerShell 下载和安装。 了解[如何安装和配置 Azure PowerShell](../install-configure-powershell)。

若要创建具有多个 Nic 的 VM，请按照下面的步骤操作︰

1. Azure VM 映像库中选择虚拟机映像。 注意图像频繁更改，且有按区域。 下面的示例中指定的图像可能会更改或可能不会在您的地区，因此请确保指定所需的图像。 
        
        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. 创建虚拟机的配置。 

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. 创建默认的管理员身份登录。 

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. 为虚拟机配置中添加其他 Nic。 

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm 
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. 指定的子网和 IP 地址的默认网卡。 

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm 
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. 在虚拟网络中创建虚拟机。 

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] 您在此处指定 VNet 必须已经存在 （如系统必备组件中所述）。 下面的示例指定了名为**MultiNIC VNet**的虚拟网络。 

## 到其他子网的辅助 NIC 访问

Azure 中的当前模型是一个虚拟机中的所有 Nic，的默认网关都设置。 这样，与 IP 地址的子网外进行通信的 Nic。 在使用弱主机路由模型如 Linux 的操作系统，如果入口和出口流量使用其他 Nic 将中断互联网连接。

为了解决此问题，Azure 将推出 7 月 2015年的第一周在更新到的平台，这将从辅助 Nic 删除默认网关。 这不会影响现有的虚拟机，直到它们被重新启动。 在重新启动后新的设置将生效，此时将有限点，辅助 Nic 上的通信流在同一子网内。 如果用户想要启用辅助 Nic 来谈自己的子网之外，他们将不得不按如下所述配置网关的路由表中添加一个条目。

### 配置 Windows 虚拟机

假设您有一个 Windows 虚拟机使用两个 Nic，如下所示︰

- 主要的 NIC 的 IP 地址︰ 192.168.1.4
- 辅助 NIC 的 IP 地址︰ 192.168.2.5

此 VM IPv4 路由表如下所示︰

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

请注意，默认工艺路线 (0.0.0.0) 是仅供主网卡。 您将不能访问资源子网之外的辅助 nic，如下所示︰

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
     
    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

若要添加辅助 NIC 上的默认路由，请按照下面的步骤︰

1. 从命令提示符处，运行以下命令来辅助 nic 标识索引号︰

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. 请注意具有 27 （在本例中） 的索引表中的第二项。
3. 在命令提示符下，运行**路由添加**的命令，如下所示。 在此示例中，将 192.168.2.1 指定为默认网关辅助 nic:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. 要测试连接，请返回到命令提示符并尝试 ping 辅助 NIC 不同的子网为显示 int eh 下面示例︰

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
         
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. 您还可以检查路由表中，以检查该新添加的路由，如下所示︰

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### 配置 Linux 虚拟机

对于 Linux 虚拟机，该默认行为将使用弱主机路由，因为建议的辅助 Nic 被限制为仅在同一子网内的通信流。 但是某些情况下要求子网之外的连接，如果用户应启用基于策略的路由，以确保入站和出站通信都使用相同的网卡。
