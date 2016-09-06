---
ms.openlocfilehash: 51f257474dd21494f324429088dcddb9154070b5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机"
    description="了解如何使用 Azure PowerShell 创建和预配置的基于 Linux 的 Azure 中的虚拟机。"
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
    ms.date="07/09/2015"
    ms.author="kathydav"/>

# 使用 Azure PowerShell 创建和预配置的基于 Linux 的虚拟机

> [AZURE.SELECTOR]
- [Azure CLI](virtual-machines-linux-tutorial.md)
- [PowerShell](virtual-machines-ps-create-preconfigure-linux-vms.md)

下列步骤显示了如何自定义一套 Azure PowerShell 命令创建和预配置的基于 Linux 的 Azure 中的虚拟机服务管理使用构建基块的方法。 快速创建一个新的基于 Linux 的虚拟机的命令集和扩大现有部署或创建多个快速构建自定义的开发人员测试或专业的 IT 环境的命令集，您可以使用此过程。

这些步骤创建 Azure PowerShell 命令集的一种填写空白的方法。 如果您是新手 Azure PowerShell 或只是想知道若要成功配置为指定的值，此方法非常有用。 Azure PowerShell 的高级的用户可以执行命令并替换自己的变量的值 （以"$"开头的行）。

要配置的基于 Windows 的虚拟机的配套主题，请参阅[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)。

## 步骤 1︰ 安装 Azure PowerShell

如果您没有这样做，使用中[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)说明在您的本地计算机上安装 Azure PowerShell。 然后，打开 Azure PowerShell 命令提示符。

## 步骤 2︰ 设置您的订购和存储帐户

通过在 Azure PowerShell 命令提示符下运行以下命令来设置 Azure 订购、 存储帐户。 引号，包括的所有内容替换 < 和 > 字符，使用正确的名称。

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

从**SubscriptionName**属性的**Get AzureSubscription**命令的输出可以正确订阅名称。 之后**选择 AzureSubscription**命令，可以从**标签**属性的**Get AzureStorageAccount**命令的输出中获取正确的存储帐户名。 您还可以存储在文本文件中供将来使用这些命令。

## 步骤 3︰ 确定 ImageFamily

接下来，您需要确定到 Azure 的虚拟机，您想要创建相对应的特定图像的 ImageFamily 值。 您可以使用以下命令的可用 ImageFamily 值的列表。

    Get-AzureVMImage | select ImageFamily -Unique

下面是基于 Linux 的计算机上的 ImageFamily 值的一些示例︰

- Ubuntu 12.10 服务器
- CoreOS 字母
- 在 SUSE Linux 企业服务器 12

打开所选的文本编辑器的新实例或实例的 PowerShell 集成脚本环境 (ISE)。 将以下复制到新的文本文件或 PowerShell ISE，替换 ImageFamily 值。

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## 第 4 步︰ 生成命令集

生成的命令设置之一的命令块下面的设置复制到新的文本文件或 PowerShell ISE 中变量的值填写并删除其余 < 和 > 字符。 看到这两个[示例](#examples)的最终结果以了解这篇文章的末尾。

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

使用以下命令指定初始的 Linux 用户名称和密码 （必需的）。 选择强密码。 要检查其强度，请查看[密码检查器︰ 使用强密码](https://www.microsoft.com/security/pc-security/password-checker.aspx)。

    $cred=Get-Credential -Message "Type the name and password of the initial Linux account."
    $vm1 | Add-AzureProvisioningConfig -Linux -LinuxUser $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

（可选） 指定 SSH 密钥对已部署一套预订中。

    $vm1 | Add-AzureProvisioningConfig -Linux -SSHKeyPairs "<SSH key pairs>"

有关详细信息，请参阅[如何使用 SSH 使用 Linux 在 Azure 上](virtual-machines-linux-use-ssh-key.md)。

（可选） 订阅中指定已部署的 SSH 公用密钥的列表。

    $vm1 | Add-AzureProvisioningConfig -Linux - SSHPublicKeys "<SSH public keys>"

基于 linux * 的虚拟机的附加预配置选项，请参阅设置中[添加 AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) **Linux**参数的语法。

（可选） 将虚拟机分配一个特定的 IP 地址，称为静态 DIP。

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

您可以验证特定的 IP 地址是用下面的命令可用。

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

（可选） 将虚拟机分配到 Azure 的虚拟网络中的特定子网。

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

还可以将单个数据磁盘添加到虚拟机。

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

（可选） 添加到外部通信为现有的负载平衡设置的虚拟机。

    $prot="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, udp>"
    $probeport=<TCP or UDP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $prot -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

最后，通过选择以下命令块 （必填） 之一启动虚拟机创建过程。

选项 1︰ 在现有的云服务中创建虚拟机。

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

云服务的短名称是 Azure 云服务 Azure 门户中或在 Azure 预览门户的资源组列表中的列表中显示的名称。

选项 2: 现有的云服务和虚拟网络中创建虚拟机。

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## 步骤 5︰ 运行命令集

查看您在您的文本编辑器或 PowerShell ISE 包含多个块的步骤 4 中的命令生成的 Azure PowerShell 命令集。 确保您指定了所有所需的变量，并且它们具有正确的值。 另外请确保您已删除所有 < 和 > 字符。

如果您使用的文本编辑器，将命令集复制到剪贴板上，然后右键单击打开的 Azure PowerShell 命令提示符。 这将作为一系列 PowerShell 命令发出命令集并创建 Azure 的虚拟机。 或者，运行命令设置使用 PowerShell ise。

如果您在错误订阅创建虚拟机，存储帐户、 云服务、 可用性组、 虚拟网络或子网，删除虚拟机、 更正命令块语法，然后再运行更正后的命令集。

创建虚拟机后，请参阅[如何登录到运行 Linux 的虚拟机](virtual-machines-linux-how-to-log-on.md)。

如果您将创建此虚拟机或一个类似，您可以︰

- 保存该命令设置为 PowerShell 脚本文件 (*.ps1)
- 保存在 Azure 门户的**自动化**部分设置 Azure 自动化 runbook 为此命令

## <a id="examples"></a>示例

以下是两个示例使用前面的步骤生成在 Azure 创建基于 Linux 的虚拟机的 Azure PowerShell 命令集。

### 示例 1

我需要 PowerShell 命令设置为程序创建初始 MySQL 服务器的 Linux 虚拟机︰

- 使用 Ubuntu 服务器 12.10 图像。
- 有 AZMYSQL1 的名称。
- 有 500 GB 的其他数据磁盘。
- 有 192.168.244.4 的静态 IP 地址。
- 位于 AZDatacenter 的虚拟网络的后端的子网中。
- 位于 TailspinToys Azure 的云服务。

下面是相应的 Azure PowerShell 命令设置为创建该虚拟机，以便于阅读每个块之间的空行。

    $family="Ubuntu Server 12.10"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vmname="AZMYSQL1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the initial Linux account."
    $vm1 | Add-AzureProvisioningConfig -Linux -LinuxUser $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=500
    $disklabel="MySQLData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### 示例 2

我需要 PowerShell 命令设置为所创建的 Apache 服务器的 Linux 虚拟机︰

- 使用 SUSE Linux 企业服务器 12 图像。
- 有 LOB1 的名称。
- 有 50GB 的其他数据磁盘。
- 是标准的 web 通讯 LOBServers 负载平衡器组的成员。
- 是 AZDatacenter 虚拟网络的前端子网中。
- 位于 TailspinToys Azure 的云服务。

下面是相应的 Azure PowerShell 命令集用来创建此虚拟机。

    $family="SUSE Linux Enterprise Server 12"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vmname="LOB1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the initial Linux account."
    $vm1 | Add-AzureProvisioningConfig -Linux -LinuxUser $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=50
    $disklabel="LOBApp"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $prot="tcp"
    $localport=80
    $pubport=80
    $endpointname="LOB1"
    $lbsetname="LOBServers"
    $probeprotocol="tcp"
    $probeport=80
    $probepath="/"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $prot -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## 其他资源

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[Azure 的虚拟机的常见问题解答](http://msdn.microsoft.com/library/azure/dn683781.aspx)

[Azure 的虚拟机的概述](http://msdn.microsoft.com/library/azure/jj156143.aspx)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)

[如何登录到运行 Linux 的虚拟机](virtual-machines-linux-how-to-log-on.md)

[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)
