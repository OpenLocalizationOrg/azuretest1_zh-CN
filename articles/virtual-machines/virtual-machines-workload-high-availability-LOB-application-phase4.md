---
ms.openlocfilehash: 746a4d8f81baf3ae8615e0960b61344c8bb8b4cf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="业务线应用程序阶段 4 |Microsoft Azure" 
    description="创建 web 服务器和负载您业务线应用程序对它们在阶段 4 中在 Azure 中的业务应用程序的行的行。" 
    documentationCenter=""
    services="virtual-machines" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="josephd"/>

# 业务应用程序工作负荷阶段 4 行︰ web 服务器配置

在部署在 Azure 的基础结构服务的业务应用程序的高可用性线条的此阶段中，您构建 web 服务器并加载您的业务应用程序在其上的行。

这一阶段到[第五阶段](virtual-machines-workload-high-availability-LOB-application-phase5.md)搬迁之前，必须完成。 请参阅[部署高可用性线条在 Azure 中的业务应用程序的](virtual-machines-workload-high-availability-LOB-application-overview.md)所有阶段。

## 在 Azure 创建 web 服务器的虚拟机

有两个 web 服务器虚拟机可以部署 ASP.NET 应用程序或旧可以通过 Internet Information Services (IIS) 8 Windows Server 2012 R2 中承载的应用程序。

首先，您配置内部负载平衡这样的 Azure 分散到业务线应用程序在两个 web 服务器之间平均客户端通信量。 这要求您指定内部负载平衡组成的名称和其 IP 地址，从配给 Azure 虚拟网络的子网地址空间分配的实例。 若要确定您选择的用于内部负载平衡器的 IP 地址是否可用，请在 Azure PowerShell 提示符下使用这些命令。 指定变量的值并删除 < 和 > 字符。

    Switch-AzureMode AzureServiceManagement
    $vnet="<Table V – Item 1 – Value column>"
    $testIP="<a chosen IP address from the subnet address space, Table S - Item 2 – Subnet address space column>"
    Test-AzureStaticVNetIP –VNetName $vnet –IPAddress $testIP

如果测试 AzureStaticVNetIP 命令显示中的**IsAvailable**字段为**True**时，您可以使用的 IP 地址。

切换回 PowerShell 的资源管理器模式，此命令。

    Switch-AzureMode AzureResourceManager

接下来，填充变量中，然后运行下面的命令集︰

    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $vnetName="<Table V – Item 1 – Value column>"
    $privIP="<available IP address on the subnet>"
    $vnet=Get-AzureVirtualNetwork -Name $vnetName -ResourceGroupName $rgName

    $frontendIP=New-AzureLoadBalancerFrontendIpConfig -Name WebServers-LBFE -PrivateIPAddress $privIP -SubnetId $vnet.Subnets[1].Id
    $beAddressPool=New-AzureLoadBalancerBackendAddressPoolConfig -Name WebServers-LBBE

    # This example assumes unsecured (HTTP-based) web traffic to the web servers.
    $healthProbe=New-AzureLoadBalancerProbeConfig -Name WebServersProbe -Protocol "TCP" -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    $lbrule=New-AzureLoadBalancerRuleConfig -Name "WebTraffic" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "TCP" -FrontendPort 80 -BackendPort 80
    New-AzureLoadBalancer -ResourceGroupName $rgName -Name "WebServersInAzure" -Location $locName -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe -FrontendIpConfiguration $frontendIP

接下来，添加到您的组织内部 DNS 基础结构，可以解决业务线应用程序 （如 lobapp.corp.contoso.com) 的完全限定的域名的 DNS 地址记录的 IP 地址分配给内部负载平衡器 （在前面的 Azure PowerShell 命令块 $privIP 的值）。

接下来，使用下面的块的 PowerShell 命令创建两个 web 服务器中的虚拟机。 请注意，此 PowerShell 命令设置使用下表中的值︰

- 表 M，为您的虚拟机
- 表 V，为虚拟网络设置
- 表 S，为您的子网
- 表 ST，为您的存储帐户
- 表 A，以您的可用性设置

回想起在[阶段 2](virtual-machines-workload-high-availability-LOB-application-phase2.md)和表 V、 S、 ST 和在[第一阶段](virtual-machines-workload-high-availability-LOB-application-phase1.md)的一个定义表 M。

当您提供所有适当的值时，在 Azure PowerShell 提示符下运行产生的块。

    # Set up subscription and key variables
    $subscr="<name of the Azure subscription>"
    Set-AzureSubscription -SubscriptionName $subscr
    Switch-AzureMode AzureResourceManager
    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $webLB=Get-AzureLoadBalancer -ResourceGroupName $rgName -Name "WebServersInAzure"   
    
    # Use the standard storage account
    $saName="<Table ST – Item 2 – Storage account name column>"

    $vnetName="<Table V – Item 1 – Value column>"
    $beSubnetName="<Table S - Item 2 - Name column>"
    $avName="<Table A – Item 3 – Availability set name column>"
    $vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $backendSubnet=Get-AzureVirtualNetworkSubnetConfig -Name $beSubnetName -VirtualNetwork $vnet
    
    # Create the first web server virtual machine
    $vmName="<Table M – Item 6 - Virtual machine name column>"
    $vmSize="<Table M – Item 6 - Minimum size column>"
    $nic=New-AzureNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0]
    $avSet=Get-AzureAvailabilitySet -Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first web server." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the second web server virtual machine
    $vmName="<Table M – Item 7 - Virtual machine name column>"
    $vmSize="<Table M – Item 7 - Minimum size column>"
    $nic=New-AzureNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0]
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second second SQL Server computer." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

使用您所选的远程桌面客户端，并创建到每个 web 服务器虚拟机的远程桌面连接。 使用内联网 DNS 或计算机名称和本地管理员帐户的凭据。

接下来，为每个 web 服务器的虚拟机，将它们加入到合适的 Active Directory 域与在 Windows PowerShell 提示符下这些命令。

    $domName="<Active Directory domain name to join, such as corp.contoso.com>"
    Add-Computer -DomainName $domName
    Restart-Computer

请注意，您必须输入**添加计算机**命令后提供域帐户凭据。

在重新启动后，连接到它们使用具有本地管理员权限的帐户。

接下来，每台 web 服务器都安装和配置 IIS。

1. 运行服务器管理器，然后单击**添加角色和功能**。
2. 之前在您开始页面，单击**下一步**。
3. 在选择安装类型页中，单击**下一步**。
4. 在选择目的服务器页上单击**下一步**。
5. 在服务器角色页中，单击**Web 服务器 (IIS)**的**角色**的列表。
6. 出现提示时，单击**添加功能**，然后单击**下一步**。
7. 在选择功能页面上单击**下一步**。
8. 在 Web 服务器 (IIS) 页上，单击**下一步**。
9. 在选择角色服务页上，选择或清除 LOB 应用程序所需的服务的复选框，然后单击**下一步**。
10.在确认安装选项页上，单击**安装**。

## 部署您的业务应用程序的 web 服务器的虚拟机上的行

从内部网络上的计算机︰

1.  添加您业务线应用程序的两个 web 服务器的文件。
2.  在 SQL Server 群集上创建您业务线应用程序的数据库。
3.  测试到您行的业务应用程序和它的功能的访问。

此图是源于这一阶段成功完成的配置。

![](./media/virtual-machines-workload-high-availability-LOB-application-phase4/workload-lobapp-phase4.png)

## 下一步

要继续进行此工作负载的配置，请转到[阶段 5︰ 创建可用性组和应用程序数据库中添加](virtual-machines-workload-high-availability-LOB-application-phase5.md)。

## 其他资源

[部署在 Azure 中的业务应用程序的高可用性线条](virtual-machines-workload-high-availability-LOB-application-overview.md)

[行的业务应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)

[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ SharePoint Server 2013 场](virtual-machines-workload-intranet-sharepoint-farm.md)
