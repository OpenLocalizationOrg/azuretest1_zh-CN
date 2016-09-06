---
ms.openlocfilehash: eb4ee57259f145e485f254797ac15a1d8f6bc307
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="自动修补为 SQL Server 在 Azure 的虚拟机中"
   description="对于 SQL Server 运行的虚拟机在 Azure 解释自动修补功能。"
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
   ms.date="08/05/2015"
   ms.author="jroth" />

# 自动修补为 SQL Server 在 Azure 的虚拟机中

自动修补修补建立维护窗口的 Azure 虚拟机运行 SQL Server 2012年或 2014年。 只能在此维护窗口期间安装自动的更新。 对于 SQL Server，这可确保系统更新和任何关联的重启在最佳数据库时出现。 它取决于 SQL Server IaaS 代理。

>[AZURE.NOTE] 自动修补修补依赖于 SQL Server IaaS 代理。 若要安装和配置该代理，您必须目标虚拟机上运行的 Azure VM 代理。 较新的虚拟机库图像具有默认情况下，启用此选项，但 Azure VM 代理可能会找不到现有的虚拟机上。 如果您使用的虚拟机映像，您还需要安装 SQL Server IaaS 代理。 有关详细信息，请参见[VM 代理和扩展](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/)。

## 配置自动修补门户

您可以使用[Azure 预览门户](http://go.microsoft.com/fwlink/?LinkID=525040&clcid=0x409)配置自动修补时创建新的 SQL Server 虚拟机。 下面的屏幕快照显示在**可选配置**这些选项 | **SQL 自动修补**。

![SQL Azure 门户中修补的自动](./media/virtual-machines-sql-server-automated-patching/IC778484.jpg)

对于现有的 SQL Server 2012年或 2014年虚拟机，虚拟机属性的**配置**部分中选择的**自动修补程序**设置。 在**自动修补程序**窗口中，可以启用该功能，设置维护计划和开始小时，然后选择维护窗口期间。 下面的屏幕快照所示。

![自动修补在 Azure 门户的配置](./media/virtual-machines-sql-server-automated-patching/IC792132.jpg)

>[AZURE.NOTE] 当首次启用自动修补时，Azure 上配置 SQL Server IaaS 代理在后台。 在此期间，门户不会显示已配置的自动修补。 等待代理安装，配置几分钟。 之后，门户将反映新的设置。

## 配置使用 PowerShell 的自动修补

您可以使用 PowerShell 配置自动修补。

在下面的示例中，使用 PowerShell 现有的 SQL Server 虚拟机上配置自动修补。 **新 AzureVMSqlServerAutoPatchingConfig**命令用于配置自动更新为新的维护窗口。

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"
    
    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

基于此示例下, 表描述了目标 Azure VM 上的实际效果︰

|参数|Effect|
|---|---|
|**星期**|每个星期四安装修补程序。|
|**MaintenanceWindowStartingHour**|上午 11:00 开始更新。|
|**MaintenanceWindowsDuration**|在 120 分钟内必须安装修补程序。 基于开始时间，他们必须完成由下午 1:00。|
|**PatchCategory**|此参数的唯一可能设置为"重要信息"。|

它可能需要几分钟时间来安装和配置 SQL Server IaaS 代理。

要禁用自动修补程序，运行该脚本而无需新建 AzureVMSqlServerAutoPatchingConfig-启用参数。 与安装，可能需要几分钟，要禁用自动修补程序。

## 禁用和卸载 SQL Server IaaS 代理

如果您想要禁用的自动备份和修补修补的 SQL Server IaaS 代理，请使用下面的命令︰

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -Disable | Update-AzureVM

若要卸载 SQL Server IaaS 代理，请使用下面的语法︰

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension –Uninstall | Update-AzureVM

也可以卸载使用**删除 AzureVMSqlServerExtension**命令扩展︰

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Remove-AzureVMSqlServerExtension | Update-AzureVM

## 兼容性

以下的产品都与 SQL Server IaaS 代理功能的自动修补程序︰

- Windows Server 2012

- Windows Server 2012 R2

- SQL Server 2012

- SQL Server 2014

## 下一步行动

SQL Server 在 Azure 中的虚拟机相关的功能是[自动备份 SQL Server 在 Azure 的虚拟机](virtual-machines-sql-server-automated-backup.md)。

查看其他[运行 SQL Server 在 Azure 的虚拟机的资源](virtual-machines-sql-server-infrastructure-services.md)。
