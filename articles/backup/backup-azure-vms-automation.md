---
ms.openlocfilehash: 1365028542fcf847a3f6fc7a084cba362f1a06d5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="部署和管理使用 PowerShell 的 Azure Vm 备份 |Microsoft Azure"
    description="了解如何部署和管理使用 PowerShell 的 Azure 备份"
    services="backup"
    documentationCenter=""
    authors="aashishr"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2015"
    ms.author="aashishr"/>


# 部署和使用 PowerShell 的 Azure Vm 的管理备份
这篇文章演示了如何使用 Azure PowerShell Azure IaaS 虚拟机的备份和恢复。

## 概念
在 Azure 备份文档将得到[引入到 Azure IaaS VM 备份](backup-azure-vms-introduction.md)。 它涵盖有关为什么您应该先备份您的 VM、 先决条件以及限制要素。

为了有效地使用 PowerShell，有必要了解层次结构的对象，并从何处开始。

![对象层次结构](./media/backup-azure-vms-automation/object-hierarchy.png)

2 最重要流是启用保护虚拟机，并从恢复点恢复数据。 本文的重点是帮助您熟练地使用 PowerShell commandlets，以便这两个方案。


## 安装和注册
若要开始，请切换到*AzureResourceManager*模式通过使用**开关 AzureMode** commandlet 启用 Azure 备份 commandlets:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

以下安装和注册任务可以使用 PowerShell 自动化︰

- 创建备份存储库
- 注册 Azure 备份服务的虚拟机

### 创建备份存储库

> [AZURE.WARNING] 对于第一次使用 Azure 备份的客户，您需要注册 Azure 备份提供程序用于您的订购。 这可以通过运行下面的命令︰ 登记 AzureProvider ProviderNamespace"Microsoft.Backup"

您可以创建新的备份存储库使用**New AzureRMBackupVault** commandlet。 备份存储库是 ARM 的资源，因此您需要将其放在资源组中。 在提升的 Azure PowerShell 控制台中，运行以下命令︰

```
PS C:\> New-AzureRMResourceGroup –Name “test-rg” –Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

您可以备份的所有存储库的列表中给定的订阅使用**Get AzureRMBackupVault** commandlet。

> [AZURE.NOTE] 很方便到变量中存储的备份存储库对象。 作为许多 Azure 备份 commandlets 的输入需要存储库对象。


### 注册虚拟机
使用 Azure 备份配置备份的第一步是 Azure 备份的存储库中注册您的计算机或虚拟机。 **注册 AzureRMBackupContainer** commandlet 采用 Azure IaaS 虚拟机的输入的信息并注册指定的存储库。 注册操作将 Azure 的虚拟机与备份存储库相关联，并跟踪虚拟机备份周期。

Azure 备份服务中注册您的虚拟机创建一个顶级容器对象。 一个容器通常包含多个项，可以进行备份，但在虚拟机的情况下将只有一个备份项的容器。

```
PS C:\> $registerjob = Register-AzureRMBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## 备份 Azure 的虚拟机

### 创建保护策略
不强制要求创建新的保护策略来开始您的虚拟机的备份。 保险存储带有默认策略，可以用来快速启用保护，和右边的详细信息与以后再编辑。 可以使用**Get AzureRMBackupProtectionPolicy** commandlet 获取存储库中的可用策略的列表︰

```
PS C:\> Get-AzureRMBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

备份策略是与至少一种保留策略相关联。 保留策略定义了多长时间恢复点保持与 Azure 的备份。 **新 AzureRMBackupRetentionPolicy** commandlet 创建 PowerShell 对象包含保留策略信息。 这些保留策略对象将用作输入到*新建 AzureRMBackupProtectionPolicy* commandlet，或直接以*启用 AzureRMBackupProtection* commandlet。

备份策略定义了某一项的备份时间和频率完成。 **新 AzureRMBackupProtectionPolicy** commandlet 创建 PowerShell 对象包含备份的策略信息。 作为*启用 AzureRMBackupProtection* commandlet 的输入使用备份的策略。

```
PS C:\> $Daily = New-AzureRMBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRMBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy ($Daily) -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### 启用保护
启用保护涉及两个对象的项目和政策，并且两者都必须属于相同的存储库。 该策略与项关联后，备份工作流将进入已定义的日程安排。

```
PS C:\> Get-AzureRMBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRMBackupItem | Enable-AzureRMBackupProtection -Policy $newpolicy
```

### 初始备份
对于后续的备份操作的项完全初始复制和增量复制将负责备份时间表。 但是，如果您想要强制在特定时间或甚至立即发生的初始备份使用**备份 AzureRMBackupItem** commandlet:

```
PS C:\> $container = Get-AzureRMBackupContainer -Vault $backupvault -type AzureVM -name "testvm"
PS C:\> $backupjob = Get-AzureRMBackupItem -Container $container | Backup-AzureRMBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

### 监视备份作业
在 Azure 备份最长时间运行操作是作为作业来模拟。 这便于跟踪进度，无需保留 Azure 的门户打开任何时候。

若要获取正在工作的最新状态，使用**Get AzureRMBackupJob** commandlet。

```
PS C:\> $joblist = Get-AzureRMBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

而不是轮询完成-这是不必要的额外代码-这些作业是使用**等待 AzureRMBackupJob** commandlet 更简单。 脚本中使用时，commandlet 将暂停执行，直至作业完成或到达指定的超时时间值。

```
PS C:\> Wait-AzureRMBackupJob -Job $joblist[0] -Timeout 43200
```


## 还原 Azure 的虚拟机

若要还原备份数据，您需要确定备份项和持有的时间点数据恢复点。 此信息提供给恢复 AzureRMBackupItem commandlet，以启动数据从保险存储到客户的帐户恢复。

### 选择虚拟机

若要获取标识合适的备份项的 PowerShell 对象，您需要开始从该存储库中的容器和对象层次结构中下一路做。 若要选择表示该 VM 的容器，使用**Get AzureRMBackupContainer** commandlet 和于**Get AzureRMBackupItem** commandlet 管道的。

```
PS C:\> $backupitem = Get-AzureBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRMBackupItem
```

### 选择恢复点

现在可以列出所有使用**Get AzureRMBackupRecoveryPoint** commandlet 中，备份项的恢复点，然后选择要恢复的恢复点。 通常用户在列表中选取最近的*AppConsistent*点。

```
PS C:\> $rp =  Get-AzureRMBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

### 恢复磁盘

没有完成通过 Azure 的门户和 Azure PowerShell 的还原操作有重要区别。 使用 PowerShell，还原操作停止在从恢复点恢复以及磁盘配置信息。 它不会创建一个虚拟机。

> [AZURE.WARNING] 还原-AzureRMBackupItem 不会创建一个虚拟机。 它只将磁盘还原到指定的存储帐户。 这不是您将体验到 Azure 门户中相同的行为。

```
PS C:\> $restorejob = Restore-AzureRMBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

您可以使用**Get AzureRMBackupJobDetails** commandlet 完成的还原作业后还原操作的详细信息。 *ErrorDetails*属性会重新生成虚拟机所需的信息。

```
PS C:\> $restorejob = Get-AzureRMBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRMBackupJobDetails -Job $restorejob
```

### 生成虚拟机

可以完成构建 VM 从已还原的磁盘，使用较旧的 Azure ServiceManager PowerShell commandlets 新的 Azure ResourceManager 模板，或甚至使用 Azure 的门户。 在一个快速的示例，我们将演示如何实现使用 Azure ServiceManager commandlets。

```
 $properties  = $details.Properties

 $storageAccountName = $properties["TargetStorageAccountName"]
 $containerName = $properties["TargetContainerName"]
 $blobName = $properties["TargetBlobName"]

 Switch-AzureMode AzureServiceManagement

 $keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
 $storageAccountKey = $keys.Primary
 $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


 $destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
 Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


 $obj = [xml](Get-Content $destination_path)
 $pvr = $obj.PersistentVMRole
 $os = $pvr.OSVirtualHardDisk
 $dds = $pvr.DataVirtualHardDisks
 $osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
 $vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

 if (!($dds -eq $null))
 {
     foreach($d in $dds.DataVirtualHardDisk)
     {
         $lun = 0;
         if(!($d.Lun -eq $null))
         {
             $lun = $d.Lun
         }
         $name = "panbhadataDisk" + $lun
     Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
     $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

有关如何生成还原磁盘从一个虚拟机的详细信息，阅读有关以下 commandlets:

- [添加 AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
- [新 AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
- [新 AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

测试
