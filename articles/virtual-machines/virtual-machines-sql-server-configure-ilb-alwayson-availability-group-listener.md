---
ms.openlocfilehash: 9e2bfb39424a1f7c0ea6c83af3532f1298cb8350
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 中配置 AlwaysOn 可用性组的 ILB 侦听器"
    description="本教程将指导您完成在 Azure 使用内部负载平衡器 (ILB) 中创建一个 AlwaysOn 可用性组侦听器的步骤。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" />
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/11/2015"
    ms.author="jroth" />

# 在 Azure 中配置 AlwaysOn 可用性组的 ILB 侦听器

> [AZURE.SELECTOR]
- [内部的侦听器](virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md)
- [外部侦听器](virtual-machines-sql-server-configure-public-alwayson-availability-group-listener.md)

## 概述

本主题演示如何通过使用**内部负载平衡器 (ILB)**配置的侦听器 AlwaysOn 可用性组。 

可用性组可以包含复制副本的内部，Azure 只，或跨内部和 Azure 的混合配置。 Azure 副本可以驻留在同一个区域内或跨多个使用多个虚拟网络 (VNets) 的地区。 以下步骤假定您已[配置可用性组](virtual-machines-sql-server-alwayson-availability-groups-gui.md)但不是配置了一个侦听器。 

请注意以下限制使用 ILB Azure 中的可用性组侦听器︰

- Windows Server 2008 R2、 Windows Server 2012，以及 Windows Server 2012 R2 支持可用性组侦听器。

- 客户端应用程序必须驻留在包含您的虚拟机的可用性组比不同的云服务。 Azure 不支持直接服务器返回客户端和服务器在相同的云服务。

- 每个云服务支持只有一个可用性组侦听器，因为该侦听器配置为使用云服务 VIP 地址或内部负载平衡器的 VIP 地址。 请注意，此限制仍在影响尽管 Azure 现在支持多个 VIP 地址创建一个给定的云服务中。

>[AZURE.NOTE] 本教程重点介绍使用 PowerShell 创建包括 Azure 副本可用性组的侦听器。 有关如何配置侦听器使用 SSMS 或事务处理 SQL 的详细信息，请参阅[创建或配置可用性组侦听器](https://msdn.microsoft.com/library/hh213080.aspx)。

## 确定侦听器的可访问性

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

本文着重于创建侦听程序使用**内部负载平衡器 (ILB)**。 如果您需要公共/外部侦听器，请参阅本文提供的步骤设置[外部侦听器](virtual-machines-sql-server-configure-public-alwayson-availability-group-listener.md)的版本

## 直接服务器返回创建负载平衡 VM 端点

对于 ILB，您必须首先创建内部负载平衡器。 这是在下面的脚本中。 

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. 对于**ILB**，应分配一个静态 IP 地址。 首先，检查当前的 VNet 配置，通过运行以下命令︰

        (Get-AzureVNetConfig).XMLConfiguration

1. 请注意包含承载副本的虚拟机的子网的**子网**名称。 **$SubnetName**参数在脚本中使用此文件。 

1. 然后记下**VirtualNetworkSite**的名称和开始**AddressPrefix**包含承载副本的虚拟机的子网。 通过将两个值传递给**测试 AzureStaticVNetIP**命令，检查**可用**来寻找可用的 IP 地址。 例如，如果名为*MyVNet*并且有 VNet *172.16.0.128*在启动时启动的子网掩码地址范围，以下命令将列出可用的地址︰

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. 选择一个可用的地址和下面的脚本的**$ILBStaticIP**参数中使用它。 

3. 将下面的 PowerShell 脚本复制到文本编辑器并设置变量的值，以适应您的环境 （请注意，已提供一些参数的默认值）。 请注意，使用好友小组的现有部署无法添加 ILB。 ILB 要求的详细信息，请参阅[内部负载平衡器](../load-balancer/load-balancer-internal-overview.md)。 此外，如果可用性组跨越 Azure 的区域，您必须一次运行脚本中每个数据中心，云服务和驻留在该数据中心中的节点。

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value
        
        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP
        
        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM 
        }

1. 一旦设置变量，复制到 Azure PowerShell 会话文本编辑器，若要运行该脚本。 如果仍然显示该提示 >>，键入回车键再次确保该脚本开始运行。请注意 

>[AZURE.NOTE] Azure 管理门户不到目前为止，支持内部负载平衡器，因此您将看不到 ILB 或在门户中的终结点。 但是，如果负载平衡器运行在其上**获取 AzureEndpoint**返回内部 IP 地址。 否则，它返回 null。

## 验证已安装 KB2854082，是否需要

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## 在可用性组节点中打开防火墙端口

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## 创建可用性组侦听器

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. 对于 ILB，必须使用前面创建内部负载平衡器 (ILB) 的 IP 地址。 使用下面的脚本以获得继续此 IP 地址。

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. 在一个虚拟机，将下面 PowerShell 脚本复制到文本编辑器和变量设置为前面提到的值。

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name 
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)
        
        Import-Module FailoverClusters
        
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code. 
        
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

1. 一旦设置了这些变量、 打开提升权限的 Windows PowerShell 窗口，然后将该脚本复制从文本编辑器并粘贴到 Azure PowerShell 会话来运行它。 如果仍然显示该提示 >>，键入回车键再次确保该脚本开始运行。 

2. 在每个虚拟机重复此步骤。 该脚本使用云服务的 IP 地址配置的 IP 地址资源，如探测端口设置其他参数。 在 IP 地址资源联机时，它可以再响应探测器端口上轮询从之前在本教程中创建的负载平衡的端点。

## 使联机的侦听器

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## 跟进项目

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## 测试可用性组侦听器 （在相同的 VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## 下一步行动

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]


