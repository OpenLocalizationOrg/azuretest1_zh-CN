---
ms.openlocfilehash: 69a7b24b6e87bfbeb83f652271aab65487f349da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="管理您的虚拟机使用 Azure PowerShell |Microsoft Azure"
   description="了解可用于管理您的虚拟机在自动执行任务的命令。"
   services="virtual-machines"
   documentationCenter="windows"
   authors="singhkay"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="kasing"/>

# 通过使用 Azure PowerShell 管理您的虚拟机

不要每天来管理您的虚拟机的许多任务可以自动使用 Azure PowerShell cmdlet。 本文提供了示例命令更简单的任务，以及链接到显示更复杂的任务命令的文章。

>[AZURE.NOTE] 如果您还没有安装并配置了 Azure PowerShell，则您可以说明[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)文章中。

## 如何使用示例命令
您将需要用适合于您的环境的文本替换某些命令中的文字。 < 和 > 符号指示您需要替换的文本。 替换文本，请移除的符号，但在引号将保留在原处。

## 获取虚拟机
这是您将经常使用的基本任务。 使用它来获取 VM 的信息、 在 VM 上执行任务或得到的输出存储在一个变量中。

要获取有关 VM 的信息，请运行以下命令，替换在引号中，包括一切 < 和 > 字符︰

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

若要将输出存储在 $vm 变量中，请运行︰

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## 登录到基于 Windows 的虚拟机

运行以下命令︰

>[AZURE.NOTE] 从**获取 AzureVM**命令显示可以虚拟机和云服务名称。
>
    $svcName="<cloud service name>"
    $vmName="<virtual machine name>"
    $localPath="<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile=$localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## 停止虚拟机

运行以下命令︰

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT] 此参数用于保持云服务的虚拟 IP (VIP)，它是最后一个 VM 中的云服务的情况下。 <br><br> 如果您使用 StayProvisioned 参数，您将仍然货款用于 VM。

## 启动虚拟机

运行以下命令︰

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## 附加数据磁盘
此任务需要几个步骤。 首先，使用 *** 添加-AzureDataDisk *** cmdlet 将磁盘添加到 $vm 对象。 然后，您可以使用**更新 AzureVM** cmdlet 以更新虚拟机的配置。

您将需要决定是否要附加新磁盘，或在表单中包含的数据。 为新磁盘命令创建.vhd 文件，并将其附加。

若要添加新的磁盘，请运行以下命令︰

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM <$vm> `
              | Update-AzureVM

若要附加现有数据磁盘，请运行以下命令︰

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> `
              | Update-AzureVM

若要从现有.vhd 文件 blob 存储在附加数据磁盘，请运行以下命令︰

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> `
              | Update-AzureVM

## 创建基于 Windows 的 VM

在 Azure 创建新的基于 Windows 的虚拟机，使用中的说明[使用 Azure PowerShell 创建和预配置的基于 Windows 的虚拟机](virtual-machines-ps-create-preconfigure-windows-vms.md)。 此主题的步骤您通过设置命令 Azure PowerShell 创建创建可以预先配置的基于 Windows 的虚拟机︰

- 使用 Active Directory 域成员身份。
- 与其他磁盘。
- 为现有的负载平衡的成员集。
- 使用静态 IP 地址。
