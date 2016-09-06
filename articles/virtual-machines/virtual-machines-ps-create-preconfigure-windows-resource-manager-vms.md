---
ms.openlocfilehash: 821a694ffc36c5deeb7789151253ec3fdb339cdb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建并预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机"
    description="了解如何使用 Azure PowerShell 创建和预配置 Windows 和 Azure 中基于资源管理器的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2015"
    ms.author="kathydav"/>

# 创建并预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机

下列步骤显示了如何构建一套 Azure PowerShell 的资源管理器模式，创建预配置的基于 Windows Azure 虚拟机的命令。 此构建基块过程可用于快速地创建一个新的基于 Windows 的虚拟机的命令集和扩展现有的部署。 您还可以使用它来创建多个快速构建自定义的开发人员测试或专业的 IT 环境的命令集。

这些步骤创建 Azure PowerShell 命令集的一种填写空白的方法。 如果您是新手 PowerShell 或只是想知道若要成功配置为指定的值，此方法非常有用。 如果您是高级的用户 PowerShell，可以取得命令并替换您自己的变量的值 （以"$"开头的行）

[AZURE.INCLUDE [resource-manager-pointer-to-service-management](../../includes/resource-manager-pointer-to-service-management.md)]

- [使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)

## 步骤 1︰ 安装 Azure PowerShell

您还必须具有 Azure PowerShell 0.9.0 版或更高版本。 如果您不安装并配置 Azure PowerShell，请单击[此处](../powershell-install-configure.md)的说明。

您可以检查已安装使用此命令在 Azure PowerShell 提示符下的 Azure PowerShell 的版本。

    Get-Module azure | format-table version

下面是一个示例。

    Version
    -------
    0.9.0

如果您没有 0.9.0 版或更高版本，必须删除使用的程序和功能的控制面板的 Azure PowerShell，然后安装最新版本。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md) 。

## 步骤 2︰ 设置您的订购

首先，运行 Azure PowerShell 提示。

接下来，通过在 Azure PowerShell 提示符下运行以下命令来设置 Azure 订购。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $subscr="<subscription name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current

此命令显示中可以正确订阅名称。

    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName

接下来，切换到资源管理器模式的 Azure PowerShell。

    Switch-AzureMode AzureResourceManager

## 步骤 3︰ 创建所需的资源

基于资源管理器的虚拟机需要资源组。 如果需要请使用这些命令创建新的虚拟机的新资源组。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $rgName="<resource group name>"
    $locName="<location name, such as West US>"
    New-AzureResourceGroup -Name $rgName -Location $locName

要确定一个唯一的资源组名称，请使用以下命令列出您现有的资源组。

    Get-AzureResourceGroup | Sort ResourceGroupName | Select ResourceGroupName

要列出的 Azure 的位置，您可以在其中创建基于资源管理器的虚拟机，请使用这些命令。

    $loc=Get-AzureLocation | where { $_.Name –eq "Microsoft.Compute/virtualMachines" }
    $loc.Locations

基于资源管理器的虚拟机需要基于资源管理器的存储帐户。 如果需要请使用这些命令创建新的存储帐户为您的新虚拟机。

    $rgName="<resource group name>"
    $locName="<location name, such as West US>"
    $saName="<storage account name>"
    $saType="<storage account type, specify one: Standard_LRS, Standard_GRS, Standard_RAGRS, or Premium_LRS>"
    New-AzureStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

您必须选择一个全局唯一名称为您的存储帐户中只包含小写字母和数字。 可以使用此命令可以列出现有的存储帐户。

    Get-AzureStorageAccount | Sort Name | Select Name

若要测试是否选择的存储帐户名称的全局唯一性，需要 PowerShell 的 Azure 服务管理模式下运行**测试 AzureName**命令。 使用这些命令。

    Switch-AzureMode AzureServiceManagement
    Test-AzureName -Storage <Proposed storage account name>

如果测试 AzureName 命令显示"False"，则您建议的名称是唯一的。  您就能确定一个唯一的名称，Azure PowerShell 重新切换到资源管理器模式下使用此命令。

    Switch-AzureMode AzureResourceManager

基于资源管理器的虚拟机可以使用一个公用域名称标签，它可以包含字母、 数字和连字符。 在字段中的第一个和最后一个字符必须是字母或数字。  

若要测试是否是全局唯一的选择的域的名称标签，请使用这些命令。

    $domName="<domain name label to test>"
    $loc="<short name of an Azure location, for example, for West US, the short name is westus>"
    Test-AzureDnsAvailability -DomainQualifiedName $domName -Location $loc

如果 DNSNameAvailability 为"True"，则建议的名称是全局唯一的。

>[AZURE.NOTE] 在 Azure PowerShell 的版本早于版本 0.9.5，测试 AzureDnsAvailability cmdlet 被任命为 Get AzureCheckDnsAvailability。 如果您正在使用版本 0.9.4 或更早版本，替换为测试 AzureDnsAvailability Get AzureCheckDnsAvailability 在上面显示的命令。  

基于资源管理器的虚拟机可将基于资源管理器的可用性设置。 如果需要创建新的可用性，使用这些命令新的虚拟机设置。

    $avName="<availability set name>"
    $rgName="<resource group name>"
    $locName="<location name, such as West US>"
    New-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName -Location $locName

使用此命令可以列出现有的可用性设置。

    Get-AzureAvailabilitySet –ResourceGroupName $rgName | Sort Name | Select Name

基于资源管理器的虚拟机可以配置 NAT 允许从 Internet 传入的通信量和放在负载平衡组的入站规则。 在这两种情况下，您必须指定一个负载平衡器和其他设置。 有关详细信息，请参阅[创建负载平衡器使用 Azure 资源管理器](../load-balancer/load-balancer-arm-powershell.md)。

基于资源管理器的虚拟机需要基于资源管理器的虚拟网络。 如果需要请使用新的虚拟机的至少一个子网创建新的基于资源管理器的虚拟网络。 下面是使用名为 frontendSubnet 和 backendSubnet 的两个子网的新虚拟网络的一个示例。

    $rgName="LOBServers"
    $locName="West US"
    $frontendSubnet=New-AzureVirtualNetworkSubnetConfig -Name frontendSubnet -AddressPrefix 10.0.1.0/24
    $backendSubnet=New-AzureVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix 10.0.2.0/24
    New-AzurevirtualNetwork -Name TestNet -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $frontendSubnet,$backendSubnet

使用这些命令可以列出现有的虚拟网络。

    $rgName="<resource group name>"
    Get-AzureVirtualNetwork -ResourceGroupName $rgName | Sort Name | Select Name

## 第 4 步︰ 生成命令集

打开文本编辑器中选择或 PowerShell 集成脚本环境 (ISE) 的一个新实例并复制以下行来启动命令集。 指定此新的虚拟机的资源组，Azure 的位置和存储帐户的名称。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    Switch-AzureMode AzureResourceManager
    $rgName="<resource group name>"
    $locName="<Azure location, such as West US>"
    $saName="<storage account name>"

在虚拟的网络中，必须指定基于资源管理器的虚拟网络名称和一个子网的索引号。  使用这些命令可以列出虚拟网络的子网。

    $rgName="<resource group name>"
    $vnetName="<virtual network name>"
    Get-AzureVirtualNetwork -Name $vnetName -ResourceGroupName $rgName | Select Subnets

子索引是在此命令中，它们进行编号依次从左到右和从 0 开始显示的子网数。

为使此示例︰

    PS C:\> Get-AzureVirtualNetwork -Name TestNet -ResourceGroupName LOBServers | Select Subnets

    Subnets
    -------
    {frontendSubnet, backendSubnet}

FrontendSubnet 的子索引为 0，backendSubnet 的子索引为 1。

将这些行复制到命令集，指定一个现有的虚拟网络名称和虚拟机的子网的索引。

    $vnetName="<name of an existing virtual network>"
    $subnetIndex=<index of the subnet on which to create the NIC for the virtual machine>
    $vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName

接下来，您将创建一个网络接口卡 (NIC)。 将下列选项之一复制到命令集并填写所需的信息。

### 选项 1︰ 指定的 NIC 名称并分配一个公用 IP 地址

将以下行复制到命令集和指定的 nic。

    $nicName="<name of the NIC of the VM>"
    $pip = New-AzurePublicIpAddress -Name $nicName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[$subnetIndex].Id -PublicIpAddressId $pip.Id

### 选项 2︰ 指定 NIC 名称和一个 DNS 域的名称标签

将以下行复制到命令集，并指定 NIC 和域全局唯一名称标签的名称。 在 Azure PowerShell 的服务管理模式中创建虚拟机，Azure 将自动完成这些步骤。

    $nicName="<name of the NIC of the VM>"
    $domName="<domain name label>"
    $pip = New-AzurePublicIpAddress -Name $nicName -ResourceGroupName $rgName -DomainNameLabel $domName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[$subnetIndex].Id -PublicIpAddressId $pip.Id

### 选项 3︰ 指定 NIC 名称并分配一个静态的、 专用的 IP 地址

将以下行复制到命令集和指定的 nic。

    $nicName="<name of the NIC of the VM>"
    $staticIP="<available static IP address on the subnet>"
    $pip = New-AzurePublicIpAddress -Name $nicName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[$subnetIndex].Id -PublicIpAddressId $pip.Id -PrivateIpAddress $staticIP

### 第 4 种选择︰ 指定 NIC 名称和入站 NAT 规则的负载平衡器实例。

若要创建一个 NIC 并将其添加到负载平衡器实例站 NAT 规则，您需要︰

- 以前创建的负载平衡器实例具有入站的 NAT 规则的通信转发到虚拟机的名称。
- 要为 NIC 分配的负载平衡器实例后端地址池的索引号
- 要为 NIC 分配的入站 NAT 规则索引号

有关如何创建负载平衡器实例具有入站 NAT 规则的信息，请参阅[创建负载平衡器使用 Azure 资源管理器](../load-balancer/load-balancer-arm-powershell.md)。

将这些行复制到命令集，并指定所需的名称和索引号。

    $nicName="<name of the NIC of the VM>"
    $lbName="<name of the load balancer instance>"
    $bePoolIndex=<index of the back end pool, starting at 0>
    $natRuleIndex=<index of the inbound NAT rule, starting at 0>
    $lb=Get-AzureLoadBalancer -Name $lbName -ResourceGroupName $rgName
    $nic=New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -Subnet $vnet.Subnets[$subnetIndex].Id -LoadBalancerBackendAddressPool $lb.BackendAddressPools[$bePoolIndex] -LoadBalancerInboundNatRule $lb.InboundNatRules[$natRuleIndex]

$NicName 的字符串必须是唯一的资源组。 最佳做法是将在字符串中，如"LOB07 NIC"虚拟机的名称。

### 选项 5︰ 指定 NIC 名称和负载平衡集的负载平衡器实例。

要创建一个 NIC 并将其添加到负载平衡集的负载平衡器实例，您需要︰

- 有一种规则的负载平衡通信以前创建负载平衡器实例的名称。
- 要为 NIC 分配的负载平衡器实例后端地址池的索引号

有关如何创建负载平衡器实例具有负载平衡通讯的规则的信息，请参阅[创建负载平衡器使用 Azure 资源管理器](../load-balancer/load-balancer-arm-powershell.md)。

将这些行复制到命令集，并指定所需的名称和索引号。

    $nicName="<name of the NIC of the VM>"
    $lbName="<name of the load balancer instance>"
    $bePoolIndex=<index of the back end pool, starting at 0>
    $lb=Get-AzureLoadBalancer -Name $lbName -ResourceGroupName $rgName
    $nic=New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -Subnet $vnet.Subnets[$subnetIndex].Id -LoadBalancerBackendAddressPool $lb.BackendAddressPools[$bePoolIndex]

接下来，创建本地 VM 对象，或者将其添加到的可用性设置。 将以下两个选项之一复制到命令集并填写名称、 大小和可用性组名称。

选项 1︰ 指定虚拟机的名称和大小。

    $vmName="<VM name>"
    $vmSize="<VM size string>"
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize

若要确定选项 1 的 VM 大小字符串的可能值，请使用这些命令。

    $locName="<Azure location of your resource group>"
    Get-AzureVMSize -Location $locName | Select Name

选项 2︰ 指定虚拟机的名称和大小，并将其添加到的可用性设置。

    $vmName="<VM name>"
    $vmSize="<VM size string>"
    $avName="<availability set name>"
    $avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id

若要确定对选项 2 的 VM 大小字符串的可能值，请使用这些命令。

    $rgName="<resource group name>"
    $avName="<availability set name>"
    Get-AzureVMSize -ResourceGroupName $rgName -AvailabilitySetName $avName | Select Name

> [AZURE.NOTE] 当前使用资源管理器中，您只能添加虚拟机对在创建过程中设置的可用性。

若要将其他数据磁盘添加到虚拟机，复制这些命令行设置，并指定磁盘设置。

    $diskSize=<size of the disk in GB>
    $diskLabel="<the label on the disk>"
    $diskName="<name identifier for the disk in Azure storage, such as 21050529-DISK02>"
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + $diskName  + ".vhd"
    Add-AzureVMDataDisk -VM $vm -Name $diskLabel -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty

接下来，您需要确定出版商、 优惠，以及虚拟机映像的 SKU。 下面是常用的、 基于 Windows 的图像表。

|发布服务器名称 | 提供名称 | SKU 名称
|:---------------------------------|:-------------------------------------------|:---------------------------------|
|MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 |
|MicrosoftWindowsServer | WindowsServer | 2012 数据中心 |
|MicrosoftWindowsServer | WindowsServer | 2012 R2 数据中心 |
|MicrosoftDynamicsNAV | DynamicsNAV | 2015 |
|MicrosoftSharePoint | MicrosoftSharePointServer | 2013 |
|MicrosoftSQLServer | SQL2014 WS2012R2 | 企业-优化-的-数据仓库 |
|MicrosoftSQLServer | SQL2014 WS2012R2 | 企业-优化-的-OLTP |
|MicrosoftWindowsServerEssentials | WindowsServerEssentials | WindowsServerEssentials |
|MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | 2012R2 |

如果未列出所需的虚拟机映像，请按照说明[此处](resource-groups-vm-searching.md#powershell)确定发布服务器、 优惠和 SKU 名称。

将这些命令复制到命令集并填写发布服务器、 优惠和 SKU 名称。

    $pubName="<Image publisher name>"
    $offerName="<Image offer name>"
    $skuName="<Image SKU name>"
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName $pubName -Offer $offerName -Skus $skuName -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id

最后，将这些命令复制到命令集并填充为 VM 操作系统所在磁盘的名称标识符。

    $diskName="<name identifier for the disk in Azure storage, such as OSDisk>"
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

## 步骤 5︰ 运行命令集

查看您在您的文本编辑器或 PowerShell ISE 包含多个块的步骤 4 中的命令生成的 Azure PowerShell 命令集。 确保您指定了所有所需的变量，并且它们具有正确的值。 另外请确保您已删除所有 < 和 > 字符。

如果您有您的命令的文本编辑器中，将命令集复制到剪贴板上，然后右键单击打开的 Azure PowerShell 提示符。 这将作为一系列 PowerShell 命令发出命令集并创建 Azure 的虚拟机。 或者，运行该命令集从 Azure PowerShell ISE。

如果您将创建此虚拟机或一个类似，您可以保存该命令设置为 PowerShell 脚本文件 (*.ps1)。

## 示例

我需要 PowerShell 命令设置为程序创建基于 web 的业务线的工作负载的其他虚拟机︰

- 放置在现有的 LOBServers 资源组
- 使用 Windows Server 2012 R2 数据中心图像
- LOB07 的名称并且在现有的 WEB_AS 可用性设置
- 在现有的 AZDatacenter 虚拟网络的前端子网 （子网索引为 0） 具有一个公共 IP 地址与网卡
- 具有 200 gb 的其他数据磁盘

下面是相应的 Azure PowerShell 命令集用来创建此虚拟机，根据在步骤 4 中所描述的过程。

    # Switch to the Resource Manager mode
    Switch-AzureMode AzureResourceManager

    # Set values for existing resource group and storage account names
    $rgName="LOBServers"
    $locName="West US"
    $saName="contosolobserverssa"

    # Set the existing virtual network and subnet index
    $vnetName="AZDatacenter"
    $subnetIndex=0
    $vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName

    # Create the NIC
    $nicName="LOB07-NIC"
    $domName="contoso-vm-lob07"
    $pip=New-AzurePublicIpAddress -Name $nicName -ResourceGroupName $rgName -DomainNameLabel $domName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[$subnetIndex].Id -PublicIpAddressId $pip.Id

    # Specify the name, size, and existing availability set
    $vmName="LOB07"
    $vmSize="Standard_A3"
    $avName="WEB_AS"
    $avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id

    # Add a 200 GB additional data disk
    $diskSize=200
    $diskLabel="APPStorage"
    $diskName="21050529-DISK02"
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + $diskName  + ".vhd"
    Add-AzureVMDataDisk -VM $vm -Name $diskLabel -DiskSizeInGB $diskSize -VhdUri $vhdURI -CreateOption empty

    # Specify the image and local administrator account, and then add the NIC
    $pubName="MicrosoftWindowsServer"
    $offerName="WindowsServer"
    $skuName="2012-R2-Datacenter"
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName $pubName -Offer $offerName -Skus $skuName -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id

    # Specify the OS disk name and create the VM
    $diskName="OSDisk"
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + $diskName  + ".vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

## 其他资源

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](../resource-group-overview.md)

[部署和管理使用资源管理器模板和 PowerShell Azure 虚拟机](virtual-machines-deploy-rmtemplates-powershell.md)

[使用资源管理器模板和 PowerShell 创建 Windows 虚拟机](virtual-machines-create-windows-powershell-resource-manager-template-simple)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)
