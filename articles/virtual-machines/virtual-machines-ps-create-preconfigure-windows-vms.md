---
ms.openlocfilehash: bb74055704a193443f966846efa9ea76aaa35ec7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机"
    description="了解如何使用 Azure PowerShell 创建和预配置的基于 Windows Azure 中的虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2015"
    ms.author="kathydav"/>

# 使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机

> [AZURE.SELECTOR]
- [Azure 预览门户](virtual-machines-windows-tutorial.md)
- [Azure 门户](virtual-machines-windows-tutorial-classic-portal.md)
- [PowerShell︰ 资源管理器部署](virtual-machines-deploy-rmtemplates-powershell.md)
- [PowerShell︰ 经典部署](virtual-machines-ps-create-preconfigure-windows-vms.md)

下列步骤显示了如何自定义一套 Azure PowerShell 命令创建和预配置的基于 Windows Azure 虚拟机使用构建基块的方法。 快速创建一个新的基于 Windows 的虚拟机的命令集和扩大现有部署或创建多个快速构建自定义的开发人员测试或专业的 IT 环境的命令集，您可以使用此过程。

这些步骤创建 Azure PowerShell 命令集的一种填写空白的方法。 如果您是新手 PowerShell 或只是想知道若要成功配置为指定的值，此方法非常有用。 高级的 PowerShell 用户可以执行命令并替换自己的变量的值 （以"$"开头的行）。

要配置基于 Linux 的虚拟机的配套主题，请参阅[使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)。

[AZURE.INCLUDE [service-management-pointer-to-resource-manager](../../includes/service-management-pointer-to-resource-manager.md)]

- [创建并预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)

## 步骤 1︰ 安装 Azure PowerShell

如果您没有这样做，使用中[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)说明在您的本地计算机上安装 Azure PowerShell。 然后，打开 Azure PowerShell 命令提示符。

## 步骤 2︰ 设置您的订购和存储帐户

通过在 Azure PowerShell 命令提示符下运行以下命令来设置 Azure 订购、 存储帐户。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

从 SubscriptionName 属性的**Get AzureSubscription**命令的输出可以正确订阅名称。 运行**选择 AzureSubscription**命令之后，可以从标签属性的**Get AzureStorageAccount**命令的输出中获取正确的存储帐户名。

## 步骤 3︰ 确定 ImageFamily

接下来，您需要确定到 Azure 的虚拟机，您想要创建相对应的特定图像的 ImageFamily 或标签值。 下面是一些示例 Azure 管理门户中的库中。

![](./media/virtual-machines-ps-create-preconfigure-windows-vms/PSPreconfigWindowsVMs_1.png)

您可以使用此命令的可用 ImageFamily 值的列表。

    Get-AzureVMImage | select ImageFamily -Unique

基于 Windows 的计算机上的 ImageFamily 值的示例如下︰

- Windows Server 2012 R2 数据中心
- Windows Server 2008 R2 SP1
- Windows 服务器技术预览
- SQL Server 2012 SP1 企业上 Windows Server 2012

如果您找到您正在寻找的图像，打开您所选择或 PowerShell 集成脚本环境 (ISE) 的文本编辑器的新实例。 将以下复制到新的文本文件或 PowerShell ISE，替换 ImageFamily 值。

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

在某些情况下，图像名称是 Label 属性而不是 ImageFamily 值。 如果您没有找到您正在寻找使用 ImageFamily 属性的图像，使用此命令其标签属性列出映像。

    Get-AzureVMImage | select Label -Unique

如果找到正确的图像，使用此命令，请打开您所选择或 PowerShell ISE 的文本编辑器的新实例。 将以下复制到新的文本文件或 PowerShell ISE，替换标签值。

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## 第 4 步︰ 生成命令集

生成的命令设置通过将组适当的下面的块复制到新的文本文件或 ISE 中变量的值填写并删除其余 < 和 > 字符。 看到这两个[示例](#examples)的最终结果以了解这篇文章的末尾。

首先通过选择以下两个命令块 （必填） 之一来设置您的命令。

选项 1︰ 指定的虚拟计算机的名称和大小。

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

选项 2︰ 指定名称、 大小和可用性组名称。

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

D、 DS 或 G 系列的虚拟机的 InstanceSize 值，请参阅[虚拟机和 Azure 的云服务规模](https://msdn.microsoft.com/library/azure/dn197896.aspx)。

或者，对于独立 Windows 计算机，指定本地管理员帐户和密码。

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

 选择强密码。 要检查其强度，请查看[密码检查器︰ 使用强密码](https://www.microsoft.com/security/pc-security/password-checker.aspx)。

（可选） 若要添加到现有的 Active Directory 域的 Windows 计算机，指定为本地管理员帐户和密码、 域，以及名称和域帐户的密码。

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

基于 Windows 的虚拟机器的其他预配置选项，请参阅**Windows**和**WindowsDomain**参数的语法设置中[添加 AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx)。

（可选） 将虚拟机分配一个特定的 IP 地址，称为静态 DIP。

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

您可以验证特定的 IP 地址是可用的︰

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

（可选） 将虚拟机分配到 Azure 的虚拟网络中的特定子网。

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

还可以将单个数据磁盘添加到虚拟机。

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

对于 Active Directory 域控制器设置为"无"的 $hcaching。

（可选） 添加到外部通信为现有的负载平衡设置的虚拟机。

    $port="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, udp>"
    $probeport=<TCP or UDP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $prot -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

最后，选择一种用于创建虚拟机这些必需的命令块。

选项 1︰ 在现有的云服务中创建虚拟机。

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

云服务的短名称是云服务在 Azure 管理门户或 Azure 预览门户网站中的资源组列表中的列表中显示的名称。

选项 2: 现有的云服务和虚拟网络中创建虚拟机。

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## 步骤 5︰ 运行命令集

查看您在您的文本编辑器或 PowerShell ISE 包含多个块的步骤 4 中的命令生成的 Azure PowerShell 命令集。 确保您指定了所有所需的变量，并且它们具有正确的值。 另外请确保您已删除所有 < 和 > 字符。

如果您使用的文本编辑器，将命令集复制到剪贴板上，然后右键单击打开的 Azure PowerShell 命令提示符。 这将作为一系列 PowerShell 命令发出命令集并创建 Azure 的虚拟机。 或者，运行命令设置使用 PowerShell ise。

如果您将创建此虚拟机或一个类似，您可以︰

- 保存该命令设置为 PowerShell 脚本文件 (*.ps1)。
- 保存在 Azure 管理门户的**自动化**部分设置 Azure 自动化 runbook 为该命令。

## <a id="examples"></a>示例

下面是使用上述步骤以生成创建基于 Windows Azure 的虚拟机的 Azure PowerShell 命令集的两个示例。

### 示例 1

我需要 PowerShell 命令设置为程序创建初始的 Active Directory 域控制器虚拟机︰

- 使用 Windows Server 2012 R2 数据中心图像。
- 有 AZDC1 的名称。
- 是独立的计算机。
- 有 20 GB 的其他数据磁盘。
- 有静态 IP 地址 192.168.244.4。
- 位于 AZDatacenter 的虚拟网络的后端的子网中。
- 位于 TailspinToys Azure 的云服务。

下面是相应的 Azure PowerShell 命令设置为创建该虚拟机，以便于阅读每个块之间的空行。

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### 示例 2

我需要 PowerShell 命令设置为创建虚拟机的业务线的服务器︰

- 使用 Windows Server 2012 R2 数据中心图像。
- 有 LOB1 的名称。
- 是 corp.contoso.com 域中的成员。
- 具有 200 gb 的其他数据磁盘。
- 是 AZDatacenter 虚拟网络的前端子网中。
- 位于 TailspinToys Azure 的云服务。

下面是相应的 Azure PowerShell 命令集用来创建此虚拟机。

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.GetNetworkCredential().Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## 其他资源

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[Azure 的虚拟机的常见问题解答](http://msdn.microsoft.com/library/azure/dn683781.aspx)

[Azure 的虚拟机的概述](http://msdn.microsoft.com/library/azure/jj156143.aspx)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)

[使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机](virtual-machines-ps-create-preconfigure-linux-vms.md)

[创建并预配置 Windows 资源管理器和 Azure PowerShell 的虚拟机](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)
