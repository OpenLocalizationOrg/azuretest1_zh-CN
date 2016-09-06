---
ms.openlocfilehash: 25acbb7181e9b824a94a3347450f45bc00263911
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="业务线应用程序第二阶段 |Microsoft Azure" 
    description="创建和配置两个 Active Directory 复制副本域控制器在 Azure 中的业务应用程序的行的第二阶段。" 
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

# 行的业务应用程序工作负载第 2 阶段︰ 配置域控制器

在这一阶段的部署中 Azure 的基础结构服务的业务应用程序的高可用性线条，您将配置 Azure 虚拟网络中的两个副本的域控制器，以便客户端 web 请求的 web 资源都可以在 Azure 的虚拟网络，本地身份验证而不是发送到您的内部网络连接在该身份验证通迅流。 

您必须先完成这一阶段之前将移到[第 3 阶段](virtual-machines-workload-high-availability-LOB-application-phase3.md)。 请参阅[部署高可用性行的业务应用程序在 Azure 中](virtual-machines-workload-high-availability-LOB-application-overview.md)所有阶段。

## 在 Azure 创建域控制器虚拟机

首先，您需要填写表格 M 的**虚拟机名称**列中，根据需要在**最小大小**列中修改虚拟机大小。  

项目 | 虚拟机的名称 | 插图 | 最小大小 
--- | --- | --- | --- 
1。 | ___ （DC1 的示例第一个域控制器) | Windows Server 2012 R2 数据中心 | Standard_D1
2。 | ___ （第二个域控制器，DC2 的示例） | Windows Server 2012 R2 数据中心 | Standard_D1
3。 | ___ （主数据库服务器、 SQL1 的示例） | Microsoft SQL Server 2014年企业 – Windows Server 2012 R2 |   Standard_DS4
4。 | ___ （辅助数据库服务器、 SQL2 的示例） | Microsoft SQL Server 2014年企业 – Windows Server 2012 R2 |     Standard_DS4
5。 | ___ （多数为群集，示例 MN1 见证节点） | Windows Server 2012 R2 数据中心 | Standard_D1
6。 | ___ （第一台 web 服务器的示例 WEB1） | Windows Server 2012 R2 数据中心 | Standard_D3
7。 | ___ （第二台 web 服务器，WEB2 示例） | Windows Server 2012 R2 数据中心 | Standard_D3

**M — 虚拟机的高可用性业务线应用程序在 Azure 表**

虚拟机大小的完整列表，请参阅[虚拟机和 Azure 的云服务规模](https://msdn.microsoft.com/library/azure/dn197896.aspx)。

使用下面的块的 Azure PowerShell 命令创建的两个域控制器虚拟机。 为指定的值的变量，删除 < 和 > 字符。 请注意，设置此 PowerShell 命令使用以下值︰

- 表 M，为您的虚拟机
- 表 V，为虚拟网络设置
- 表 S，为您的子网
- 表 ST，为您的存储帐户
- 表 A，以您的可用性设置

记得您定义表 V、 S、 ST 和一个在[第 1 阶段︰ 配置 Azure](virtual-machines-workload-high-availability-LOB-application-phase1.md)。

当您提供所有适当的值时，在 Azure PowerShell 提示符下运行产生的块。

    # Set up subscription and key variables
    $subscr="<name of the Azure subscription>"
    Set-AzureSubscription -SubscriptionName $subscr
    Switch-AzureMode AzureResourceManager
    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $saName="<Table ST – Item 2 – Storage account name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    $avName="<Table A – Item 1 – Availability set name column>"
    
    # Create the first domain controller
    $vmName="<Table M – Item 1 - Virtual machine name column>"
    $vmSize="<Table M – Item 1 - Minimum size column>"
    $staticIP="<Table V – Item 6 - Value column>"
    $vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
    $avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for AD DS data in GB>
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
    Add-AzureVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first domain controller." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the second domain controller
    $vmName="<Table M – Item 2 - Virtual machine name column>"
    $vmSize="<Table M – Item 2 - Minimum size column>"
    $staticIP="<Table V – Item 7 - Value column>"
    $vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
    $avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for AD DS data in GB>
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
    Add-AzureVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second domain controller." 
    $vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

## 将第一个域控制器配置

使用您所选的远程桌面客户端，并创建与第一个域控制器虚拟机的远程桌面连接。 使用内联网 DNS 或计算机名称和本地管理员帐户的凭据。

接下来，您需要添加额外的数据磁盘的第一个域控制器。

### <a id="datadisk"></a>若要初始化一个空的磁盘

1.  在服务器管理器的左窗格中，单击**文件和存储服务**，然后单击**磁盘**。
2.  在内容窗格中的**磁盘**组中，单击磁盘**2** （设置为**未知****分区**）。
3.  单击**任务**，然后单击**新建卷**。
4.  之前在您开始新建卷向导页，单击**下一步**。
5.  在选择服务器和磁盘页面上，单击**磁盘 2**，然后单击**下一步**。 出现提示时，单击确定。
6.  在指定卷页的大小，单击**下一步**。
7.  在分配给驱动器号或文件夹页面上，单击**下一步**。
8.  在选择文件系统设置页上，单击**下一步**。
9.  在确认选项页中，单击**创建**。
10. 完成后，单击**关闭**。

接下来，测试的第一个域控制器连接到组织网络上的位置。

### <a id="testconn"></a>若要测试连接

1.  从桌面，打开 Windows PowerShell 提示符。
2.  使用**ping**命令来 ping 名称和 IP 地址，您的组织的网络上的资源。

此过程确保该 DNS 名称解析工作正常 （即使用内部 DNS 服务器正确配置该虚拟机），并可以跨部署虚拟网络进出时发送的数据包。 如果这个基本的测试失败，请与您的 IT 部门来解决 DNS 名称解析和数据包传递问题。

接下来，第一个域控制器的 Windows PowerShell 命令提示符下，运行下面的命令︰

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

系统将提示您提供域管理员帐户的凭据。 计算机将重新启动。

## 配置第二个域控制器

使用您所选的远程桌面客户端，并创建到第二个域控制器虚拟机的远程桌面连接。 使用内联网 DNS 或计算机名称和本地管理员帐户的凭据。

接下来，您需要添加额外的数据磁盘到另一个域控制器。 [初始化一个空的磁盘过程](#datadisk)，请参阅。

接下来，第二个域控制器上的 Windows PowerShell 提示符下，运行下面的命令︰

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

系统将提示您提供域管理员帐户的凭据。 计算机将重新启动。

接下来，您需要更新的 DNS 服务器的虚拟网络，以便该 Azure 将虚拟机分配了两个新的域控制器用作其 DNS 服务器的 IP 地址。 请注意，此过程使用来自表 V （用于虚拟网络设置） 和表 （适用于您的虚拟机） 的 M 值。

1.  在[Azure 预览门户](https://portal.azure.com/)的左窗格中，单击**浏览所有 > 虚拟网络**，然后单击虚拟网络 （表 – 1 项 – V 值列） 的名称。
2.  在虚拟网络的窗格中，单击**所有设置**。
3.  在**设置**窗格上，单击**DNS 服务器**。
4.  在**DNS 服务器**窗格中，键入以下命令︰
    - **主 DNS**服务器︰ 表 V – 项目 6 – 值列
    - **辅助 DNS**服务器︰ 表 V – 项目 7 – 值列
5.  在 Azure 预览门户的左窗格中，单击**浏览所有 > 虚拟机**。
6.  在**虚拟机的窗格中**，单击第一个域控制器 （表 M – 项目 1-虚拟机名称列） 的名称。
7.  在虚拟机窗格中，单击**重新启动**。
8.  启动后的第一个域控制器，请单击**虚拟机**窗格 （表 M – 项目 2-虚拟机名称列） 上第二个域控制器的名称。
9.  在虚拟机窗格中，单击**重新启动**。 等待，直到第二个域控制器已启动。

请注意，我们重新启动两个域控制器，以便它们不已配置为 DNS 服务器的内部 DNS 服务器。 因为它们是两个 DNS 服务器本身，它们自动与内部 DNS 服务器作为 DNS 转发器时配置它们被提升为域控制器。

接下来，我们需要创建一个 Active Directory 复制站点以确保 Azure 的虚拟网络中的服务器使用本地域控制器。 登录到该域的主域控制器的域管理员帐户和管理员级 Windows PowerShell 提示符下运行以下命令︰

    $vnet="<Table V – Item 1 – Value column>"
    $vnetSpace="<Table V – Item 5 – Value column>"
    New-ADReplicationSite -Name $vnet 
    New-ADReplicationSubnet –Name $vnetSpace –Site $vnet

接下来，添加一个用户帐户来管理 SQL Server 虚拟机。

    New-ADUser -SamAccountName sqlservice -AccountPassword (read-host "Set user password" -assecurestring) -name "sqlservice" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

此关系图显示因成功完成这一阶段，与占位符计算机名称的配置。

![](./media/virtual-machines-workload-high-availability-LOB-application-phase2/workload-lobapp-phase2.png)

## 下一步

要继续进行此工作负载的配置，请转到[阶段 3︰ 配置 SQL Server 基础架构](virtual-machines-workload-high-availability-LOB-application-phase3.md)。

## 其他资源

[部署在 Azure 中的业务应用程序的高可用性线条](virtual-machines-workload-high-availability-LOB-application-overview.md)

[行的业务应用程序的体系结构蓝图](http://msdn.microsoft.com/dn630664)

[设置基于 web 的 LOB 应用程序中用于测试的混合云](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[Azure 的基础结构服务实施指南](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Azure 的基础结构服务工作负荷︰ SharePoint Server 2013 场](virtual-machines-workload-intranet-sharepoint-farm.md)
