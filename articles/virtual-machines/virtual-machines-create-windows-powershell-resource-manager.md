---
ms.openlocfilehash: e0960c7e4d2d1549e4594559670893faf00e2edc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用 Azure 资源管理器和 PowerShell 创建 Windows 虚拟机"
    description="使用 Azure PowerShell 的资源管理模式，可以轻松地创建新的 Windows 虚拟机。"
    services="virtual-machines"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2015"
    ms.author="davidmu"/>

# 使用 Azure 资源管理器和 PowerShell 创建 Windows 虚拟机

本主题描述如何快速创建基于 Windows Azure 的虚拟机使用 Azure 资源管理器和 PowerShell。

[AZURE.INCLUDE [resource-manager-pointer-to-service-management](../../includes/resource-manager-pointer-to-service-management.md)]

- [创建 Windows 虚拟机使用 PowerShell 和 Azure 服务管理](virtual-machines-create-windows-powershell-service-manager.md)

## 创建 Windows 虚拟机

如果您已经安装了 Azure PowerShell，您必须具有 Azure PowerShell 0.9.0 版或更高版本。 您可以检查已安装使用此命令在 Azure PowerShell 命令提示符的 Azure PowerShell 的版本。

    Get-Module azure | format-table version

如果您没有这样做，或需要更新版本的 Azure PowerShell 安装，使用中[如何安装和配置 Azure PowerShell](install-configure-powershell.md)说明在您的本地计算机上安装 Azure PowerShell。 然后，打开 Azure PowerShell 命令提示符。

首先，您必须登录到 Azure 使用此命令。

    Add-AzureAccount

Microsoft Azure 登录对话框中指定您 Azure 帐户和密码的电子邮件地址。

接下来，如果您有多个 Azure 的订阅，您需要设置 Azure 订购。 若要查看您当前订阅的列表，请运行此命令。

    Get-AzureSubscription | sort SubscriptionName | Select SubscriptionName

现在，替换引号，包括一切 < 和 > 字符，替换为正确的订阅名称和运行这些命令。

    $subscrName="<subscription name>"
    Select-AzureSubscription -SubscriptionName $subscrName –Current

接下来，您需要创建存储帐户。 您必须选择一个唯一的名称仅包含小写字母和数字。 您可以测试与该命令的存储帐户名称的唯一性。

    Test-AzureName -Storage <Proposed storage account name>

如果该命令返回"False"，则您建议的名称是唯一的。

您将需要指定 Azure 数据中心的位置。 要获取的 Azure 数据中心的列表，请运行以下命令。

    Get-AzureLocation | sort Name | Select Name

接下来，您需要切换到资源管理器中的 Azure PowerShell 的模式。 运行此命令。

    Switch-AzureMode AzureResourceManager

现在，将下面的 PowerShell 命令块复制到文本编辑器中。 填写您选择的存储帐户和位置，替换引号，包括一切 < 和 > 字符。

    $stName="<chosen storage account name>"
    $locName="<chosen Azure location name>"
    $rgName="TestRG"
    New-AzureResourceGroup -Name $rgName -Location $locName
    $storageAcc=New-AzureStorageAccount -ResourceGroupName $rgName -Name $stName -Type "Standard_GRS" -Location $locName
    $singleSubnet=New-AzureVirtualNetworkSubnetConfig -Name singleSubnet -AddressPrefix 10.0.0.0/24
    $vnet=New-AzurevirtualNetwork -Name TestNet -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    $pip = New-AzurePublicIpAddress -Name TestPIP -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic = New-AzureNetworkInterface -Name TestNIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    $vm = New-AzureVMConfig -VMName WindowsVM -VMSize "Standard_A1"
    $vm = Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName MyWindowsVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

最后，将上述命令集复制到剪贴板上，然后右键单击打开的 Azure PowerShell 命令提示符。 这将发出命令根据一系列 PowerShell 命令提示您输入的用户名和密码的本地管理员帐户，并创建您 Azure 的虚拟机设置。

这是一种您可能会看到︰

    PS C:\> $stName="contosost"
    PS C:\> $locName="West US"
    PS C:\> $rgName="TestRG"
    PS C:\> New-AzureResourceGroup -Name $rgName -Location $locName
    VERBOSE: 12:45:15 PM - Created resource group 'TestRG' in location 'westus'


    ResourceGroupName : TestRG
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *

    ResourceId        : /subscriptions/fd92919d-eeca-4f5b-840a-e45c6770d92e/resourceGroups/TestRG


    PS C:\> $storageAcc=New-AzureStorageAccount -ResourceGroupName $rgName -Name $stName -Type "Standard_GRS" -Location $locName
    PS C:\> $singleSubnet=New-AzureVirtualNetworkSubnetConfig -Name singleSubnet -AddressPrefix 10.0.0.0/24
    PS C:\> $vnet=New-AzurevirtualNetwork -Name TestNet3 -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    PS C:\> $pip = New-AzurePublicIpAddress -Name TestNIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    PS C:\> $nic = New-AzureNetworkInterface -Name TestNIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    PS C:\> $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    PS C:\> $vm = New-AzureVMConfig -VMName WindowsVM -VMSize "Standard_A1"
    PS C:\> $vm = Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName MyWindowsVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    PS C:\> $vm = Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    PS C:\> $vm = Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
    PS C:\> $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/MyWindowsVMosDisk.vhd"
    PS C:\> $vm = Set-AzureVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri -CreateOption fromImage
    PS C:\> New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm


    EndTime             : 4/28/2015 1:00:05 PM -07:00
    Error               :
    Output              :
    StartTime           : 4/28/2015 12:52:52 PM -07:00
    Status              : Succeeded
    TrackingOperationId : 45035a90-ea12-4e1e-87e7-2a5e9ed12c93
    RequestId           : 98c7b4fb-b26e-4a58-b17a-b0983d896aae
    StatusCode          : OK

## 其他资源

[Azure 计算、 网络和存储提供商在 Azure 资源管理器](virtual-machines-azurerm-versus-azuresm.md)

[Azure 的资源管理器概述](resource-group-overview.md)

[使用资源管理器模板和 PowerShell 创建 Windows 虚拟机](virtual-machines-create-windows-powershell-resource-manager-template-simple.md)

[创建 Windows 虚拟机使用 PowerShell 和 Azure 服务管理](virtual-machines-create-windows-powershell-service-manager.md)

[虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)

[如何安装和配置 Azure PowerShell](install-configure-powershell.md)
