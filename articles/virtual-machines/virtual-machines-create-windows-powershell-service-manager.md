---
ms.openlocfilehash: 3c4273b45e3951b2ab40d56b4efb3360b3ccdb0a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="创建和管理 Windows 虚拟机中使用 Azure PowerShell 的服务管理。"
    description="使用 Azure PowerShell 快速服务管理在创建新的基于 Windows 的虚拟机并执行管理功能。"
    services="virtual-machines"
    documentationCenter=""
    authors="KBDAzure"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/09/2015"
    ms.author="kathydav"/>

# 创建和使用 Azure PowerShell 管理服务管理中的基于 Windows 的虚拟机

本文介绍如何创建和使用 Azure PowerShell 管理服务管理中的基于 Windows Azure 的虚拟机。

[AZURE.INCLUDE [service-management-pointer-to-resource-manager](../../includes/service-management-pointer-to-resource-manager.md)]

- [部署和管理虚拟机使用 Azure 资源管理器模板和 PowerShell](virtual-machines-deploy-rmtemplates-powershell.md)

## 设置 Azure PowerShell

如果您已经安装了 Azure PowerShell，您必须具有 Azure PowerShell 版本 0.8.0 或更高版本。 您可以检查 Azure PowerShell Azure PowerShell 命令提示符处使用以下命令安装的版本︰

    Get-Module azure | format-table version

如果您没有这样做，使用中[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)说明在您的本地计算机上安装 Azure PowerShell。 然后，打开 Azure PowerShell 命令提示符。

首先，您必须登录到 Azure 使用以下命令︰

    Add-AzureAccount

Microsoft Azure 登录对话框中指定您 Azure 帐户和密码的电子邮件地址。

接下来，如果您有多个 Azure 的订阅，您需要设置 Azure 订购。 若要查看您当前订阅的列表，请运行以下命令︰

    Get-AzureSubscription | sort SubscriptionName | Select SubscriptionName

现在，替换引号，包括一切 < 和 > 字符，替换为正确的订阅名称和运行这些命令︰

    $subscrName="<subscription name>"
    Select-AzureSubscription -SubscriptionName $subscrName –Current

## 创建虚拟机

首先，您需要存储帐户。 您可以通过使用此命令显示您当前的存储帐户列表︰

    Get-AzureStorageAccount | sort Label | Select Label

如果您没有一个，创建新的存储帐户。 您必须选择一个唯一的名称仅包含小写字母和数字。 可以使用以下命令对存储帐户名称的唯一性进行测试︰

    Test-AzureName -Storage <Proposed Storage account name>

如果该命令返回"False"，则您建议的名称是唯一的。

您将需要创建存储帐户时指定的 Azure 数据中心的位置。 要获取的 Azure 数据中心的列表，请运行以下命令︰

    Get-AzureLocation | sort Name | Select Name

现在，创建并使用下面的命令来设置存储帐户。 填充的存储帐户的名称和替换引号，包括一切 < 和 > 字符。

    $stAccount="<chosen Storage account name>"
    $locName="<Azure location>"
    New-AzureStorageAccount -StorageAccountName $stAccount -Location $locName
    Set-AzureStorageAccount -StorageAccountName $stAccount
    Set-AzureSubscription -SubscriptionName $subscrName -CurrentStorageAccountName $stAccount

接下来，您需要一种云服务。 如果您没有现有的云服务，则必须创建一个。 您必须选择一个唯一的名称，其中包含字母、 数字和连字符。 在字段中的第一个和最后一个字符必须是字母或数字。

例如，您可以命名 TestCS-*UniqueSequence*， *UniqueSequence*处于组织的缩写。 例如，如果您的组织名为乖宝贝玩具公司，可以命名 TestCS Tailspin 云服务。

可以通过使用此 Azure PowerShell 命令的名称的唯一性进行测试︰

    Test-AzureName -Service <Proposed cloud service name>

如果该命令返回"False"，则您建议的名称是唯一的。 通过使用这些命令来创建云服务︰

    $csName="<cloud service name>"
    $locName="<Azure location>"
    New-AzureService -Service $csName -Location $locName

接下来，将这套 Azure PowerShell 命令复制到记事本之类的文本编辑器︰

    $vmName="<machine name>"
    $csName="<cloud service name>"
    $locName="<Azure location>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm=New-AzureVMConfig -Name $vmName -InstanceSize Medium -ImageName $image
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
    New-AzureVM –ServiceName $csName –Location $locName -VMs $vm

在文本编辑器中，填入虚拟机、 云服务名称和位置。

最后，将命令集复制到剪贴板上，然后右键单击打开的 Azure PowerShell 命令提示符。 这将作为一系列 Azure PowerShell 命令发出命令集，提示您输入的用户名和密码的本地管理员帐户，并创建将 Azure 的虚拟机。
这里是一种什么运行命令集如下所示︰

    PS C:\> $vmName="PSTest"
    PS C:\> $csName=" TestCS-Tailspin"
    PS C:\> $locName="West US"
    PS C:\> $image=Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    VERBOSE: 3:01:17 PM - Begin Operation: Get-AzureVMImage
    VERBOSE: 3:01:22 PM - Completed Operation: Get-AzureVMImage
    VERBOSE: 3:01:22 PM - Begin Operation: Get-AzureVMImage
    VERBOSE: 3:01:23 PM - Completed Operation: Get-AzureVMImage
    PS C:\> $vm=New-AzureVMConfig -Name $vmName -InstanceSize Medium -ImageName $image
    PS C:\> $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    PS C:\> $vm | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.
    GetNetworkCredential().Password


    AvailabilitySetName               :
    ConfigurationSets                 : PSTest,Microsoft.WindowsAzure.Commands.ServiceManagement.Model.NetworkConfigurationSet}
    DataVirtualHardDisks              : {}
    Label                             : PSTest
    OSVirtualHardDisk                 : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.OSVirtualHardDisk
    RoleName                          : PSTest
    RoleSize                          : Medium
    RoleType                          : PersistentVMRole
    WinRMCertificate                  :
    X509Certificates                  : {}
    NoExportPrivateKey                : False
    NoRDPEndpoint                     : False
    NoSSHEndpoint                     : False
    DefaultWinRmCertificateThumbprint :
    ProvisionGuestAgent               : True
    ResourceExtensionReferences       : {BGInfo}
    DataVirtualHardDisksToBeDeleted   :
    VMImageInput                      :

    PS C:\> New-AzureVM -ServiceName $csName -Location $locName -VMs $vm
    VERBOSE: 3:01:46 PM - Begin Operation: New-AzureVM - Create Deployment with VM PSTest
    VERBOSE: 3:02:49 PM - Completed Operation: New-AzureVM - Create Deployment with VM PSTest

    OperationDescription                    OperationId                            OperationStatus
    --------------------                    -----------                            --------------
    New-AzureVM                             8072cbd1-4abe-9278-9de2-8826b56e9221   Succeeded

## 有关虚拟机的显示信息
这是您将经常使用的基本任务。 使用它来获取 VM 的信息、 在 VM 上执行任务或得到的输出存储在一个变量中。

要获取有关 VM 的信息，请运行以下命令，替换在引号中，包括一切 < 和 > 字符︰

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

若要将输出存储在 $vm 变量中，请运行︰

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## 登录到基于 Windows 的虚拟机

运行以下命令︰

    $svcName="<cloud service name>"
    $vmName="<virtual machine name>"
    $localPath="<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile=$localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

>[AZURE.NOTE] 从**获取 AzureVM**命令显示可以虚拟机和云服务名称。

## 停止虚拟机

运行以下命令︰

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT] **StayProvisioned**参数用于保持云服务的虚拟 IP (VIP)，它是最后一个 VM 中的云服务的情况下。 如果您使用此参数，您将仍然货款用于 VM。

## 启动虚拟机

运行以下命令︰

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## 附加数据磁盘
此任务需要几个步骤。 首先，您可以使用**添加 AzureDataDisk** cmdlet 将磁盘添加到 $vm 对象。 然后您可以使用更新 AzureVM cmdlet 更新虚拟机的配置。

您将需要决定是否要附加新磁盘，或在表单中包含的数据。 为新磁盘命令创建.vhd 文件，并将其附加在相同的命令。

若要添加新的磁盘，请运行以下命令︰

    Add-AzureDataDisk -CreateNew -DiskSizeInGB <disk size> -DiskLabel "<label name>" -LUN <LUN number> -VM $vm | Update-AzureVM

若要附加现有数据磁盘，请运行以下命令︰

    Add-AzureDataDisk -Import -DiskName "<existing disk name>" -LUN <LUN number> | Update-AzureVM

若要从现有.vhd 文件 blob 存储在附加数据磁盘，请运行以下命令︰

    $diskLoc="https://mystorage.blob.core.windows.net/mycontainer/" + "<existing disk name>" + ".vhd"
    Add-AzureDataDisk -ImportFrom -MediaLocation  $diskLoc -DiskLabel "<label name>" -LUN <LUN number> | Update-AzureVM

## 其他资源

[使用资源管理器和 Azure PowerShell 创建 Windows 虚拟机](virtual-machines-create-windows-powershell-resource-manager.md)

[使用资源管理器模板和 Azure PowerShell 创建 Windows 虚拟机](virtual-machines-create-windows-powershell-resource-manager-template-simple.md)

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)

[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)
