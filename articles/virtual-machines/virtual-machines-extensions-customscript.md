---
ms.openlocfilehash: 7b3e28f75e01ad2c34284aa5248d1ae5bf1c6aed
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在 Windows 上的自定义脚本扩展 |Microsoft Azure"
   description="在 Windows 上使用自定义脚本扩展自动化 Azure 的虚拟机配置任务 "
   services="virtual-machines"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# 窗口的自定义脚本扩展

这篇文章概括介绍在 Windows 上使用自定义脚本扩展使用 Azure PowerShell cmdlet。


虚拟机 (VM) 扩展是由 Microsoft 构建和受信任的第三方发行商来扩展虚拟机的功能。 VM 扩展的概述，请参阅<a href="https://msdn.microsoft.com/library/azure/dn606311.aspx" target="_blank">Azure VM 扩展和功能</a>。

## 自定义脚本扩展概述

自定义脚本扩展 Windows 允许您在无需登录到其中的远程 VM 上运行 PowerShell 脚本。 设置 VM 或 VM 的生命周期期间的任何时间，而无需打开 VM 的任何其他端口后，可以运行脚本。 最常见的自定义脚本扩展用例包括运行、 安装和后它提供在虚拟机上配置额外的软件。

### 运行自定义脚本扩展名的先修课程

1. 安装 Azure PowerShell cmdlet 版本 0.8.0 或更高版本的<a href="http://azure.microsoft.com/downloads" target="_blank">在此处</a>。
2. 如果将现有的虚拟机上运行的脚本，请确保虚拟机代理启用在 VM 中，如果不按照这<a href="https://msdn.microsoft.com/library/azure/dn832621.aspx" target="_blank">文章</a>安装一个。
3. 上载要到 Azure 存储在虚拟机上运行的脚本。 这些脚本可以是单个容器或多个存储容器。
4. 输入脚本中，通过扩展启动时，启动其他脚本的方式，应编写脚本。

## 自定义脚本扩展方案

### 将文件上载到默认的容器

如果您有您的脚本在您的订阅的默认帐户的存储容器中，下面的示例演示如何在虚拟机上运行它们。 容器是上载到脚本的位置。 可以使用验证默认的存储帐户**Get AzureSubscription – 默认**命令。

下面的示例创建一个新的虚拟机，但可以在现有的 VM 以及运行相同的背景情况。

    # create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script Extension to the VM. The container name refer to the storage container which contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### 将文件上载到非默认存储容器

此方案演示如何使用非默认存储在相同的订阅或其他订阅中的上载脚本和文件。 这里我们将使用现有虚拟机，但可以完成相同的操作时创建新的虚拟机。

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### 将跨不同的存储帐户脚本上传到多个容器

  如果跨多个容器存储的脚本文件，运行脚本，您必须提供完整的 SAS URL 文件。

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### 从门户网站中添加自定义脚本扩展

浏览到虚拟机中<a href="https://portal.azure.com/ " target="_blank">Azure 预览门户网站</a>，通过指定要运行的脚本文件添加该扩展名。

  ![][5]


### 正在卸载自定义脚本扩展

可以从虚拟机使用下面的命令卸载自定义脚本扩展。

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### 使用模板的自定义脚本扩展

若要了解有关使用的模板的自定义脚本扩展名，请参阅文档[此处](virtual-machines-extensions-customscript -with template.md)。

<!--Image references-->
[5]: ./media/virtual-machines-extensions-customscript/addcse.png
