---
ms.openlocfilehash: 098c209b8d04683e59d6457a4f1ce3005e69ddb2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="保护虚拟机从一个 VMM 站点到另一个使用 PowerShell 和 Microsoft Azure 资源管理器"
    description="两个内部 VMM 站点和 Azure 使用 PowerShell 和 Azure 资源管理器之间的保护来实现自动化。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/26/2015"
    ms.author="raynew"/>
    

#  VMM 到 VMM 使用 PowerShell 和 Azure 资源管理器的站点的站点


## 概述

Azure 站点恢复通过协调复制、 故障切换和恢复大量的部署方案中的虚拟机将影响您的业务连续性和灾难恢复 (BCDR) 战略。 有关部署方案的完整列表，请参阅[Azure 站点恢复概述](site-recovery-overview.md)。

Azure PowerShell 是提供 cmdlet 来管理通过 Windows PowerShell Azure 的模块。 它可以使用两种类型的 Azure 配置文件模块，或者在 Azure 资源管理器 (ARM) 模块的模块。 

本文介绍如何使用 Windows PowerShell® 与 ARM 部署 Azure 站点恢复配置和协调两个 VMM 站点之间的虚拟机保护。 在 VMM 中的主站点的私有云位于 Hyper-V 主机服务器上运行的虚拟机将复制和故障切换到辅助 VMM 站点使用 Hyper-V 副本。

不需要成为一 PowerShell 专家使用这篇文章，但它 does 假定您了解的基本概念，如模块、 cmdlet 和会话。 有关 Windows PowerShell 的详细信息，请参阅[Windows PowerShell 入门知识](http://technet.microsoft.com/library/hh857337.aspx)。
- 了解有关[使用 Azure PowerShell 使用 Azure 资源管理器](powershell-azure-resource-manager.md)详细信息。

文章包括先决条件的情况下，并向您显示如何设置 Azure PowerShell 使用站点恢复、 创建站点恢复存储库、 VMM 服务器上安装 Azure 站点恢复服务提供商并注册存储库、 配置将应用到所有受保护的虚拟机，然后再启用对这些虚拟机保护的 VMM 群的保护设置。 通过测试故障转移，以确保一切按预期的方式工作，您将完成。


## 在开始之前

请确保您有这些系统必备组件︰

- 您将需要[Microsoft Azure](http://azure.microsoft.com/)帐户。 您将需要[Microsoft Azure](http://azure.microsoft.com/)帐户。 您可以[免费试用版](pricing/free-trial/)开始。 此外，您可以阅读有关[定价 Azure 站点恢复管理器](http://azure.microsoft.com/pricing/details/site-recovery/)。
- 您将需要运行在 System Center 2012 R2 上的主要和辅助站点中的 VMM 服务器。
- 每个 VMM 服务器应具有包含至少一个云︰
    - 一个或多个 VMM 主机组。
    - 一个或多个 Hyper-V 主机服务器或群集中每个主机组。
    - 一个或多个源 Hyper-V 服务器上的虚拟机。
    - 了解更多有关设置 VMM 云︰

        - [什么是私有云与 System Center 2012 R2 VMM 中的新增功能](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/MDC-B357#fbid=dfgvHAmYryA)和[VMM 2012](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html)中的群。
        - [配置 VMM 云结构](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
        - [创建私有云在 VMM](https://technet.microsoft.com/library/jj860425.aspx)和[演练︰ 使用 System Center 2012 SP1 VMM 创建私有云](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx)。
- 一个或多个 Hyper-V 服务器至少运行 Windows Server 2012 与 Hyper-V 角色与最新的更新安装。 在 VMM 群中必须包含的服务器或群集。
- 如果您运行 Hyper-V 群集注意在该群集中介不是自动创建的如果您有一个静态 IP 地址基于群集。 您将需要手动配置该群集中介。 有关说明，请参阅[配置 Hyper-V 复制代理程序](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)。

    - 将所需的 Azure PowerShell。 请确保您运行的 Azure PowerShell 版本 0.9.6 或更高版本。 了解[如何安装和配置 Azure PowerShell](powershell-install-configure.md)。 
    - 安装 Azure PowerShell 安装自定义的控制台。 通过此控制台或从任何其他主机程序，如 Windows PowerShell ISE，您可以运行 PowerShell 命令。

## 步骤 1︰ 设置 PowerShell


1. 打开 PowerShell 控制台并运行以下命令来切换到 ARM 模块︰

    `Switch-AzureMode AzureResourceManager`

3. 现在，运行以下命令以添加您 Azure PowerShell 会话的帐户。 该 cmdlet 将提示您输入您的帐户的登录凭据。

    `Add-AzureAccount` 

    请注意，是否你代表承租人使用的 CSP 合作伙伴需要时添加 Azure 帐户指定为承租人的客户︰ 

    `Add-AzureAccount-Tenant "customer"`

5. 帐户可以有多个预订，因此您将需要将相关联的订阅，您想要使用的帐户。

    `Select-AzureSubscription -SubscriptionName $SubscriptionName`

6. 如果您第一次在订阅中使用站点恢复 cmdlet，您需要注册 Azure 站点恢复提供程序︰

    `Register-AzureProvider -ProviderNamespace Microsoft.SiteRecovery`

## 步骤 2︰ 设置站点恢复存储库

1. 如果目前还没有得到您将需要创建一个运行[新建 AzureSiteRecoveryVault](https://msdn.microsoft.com/library/azure/dn954225.aspx) cmdlet 的站点恢复保险存储︰

    `New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
    $vault = Get-AzureSiteRecoveryVault -Name $VaultName;`

## 第 3 步︰ 生成存储库的注册密钥

1. 运行[Get AzureSiteRecoveryVaultSettingsFile](https://msdn.microsoft.com/library/azure/dn850404.aspx) cmdlet 以获得存储库的注册密钥。 此键以在存储库中注册 VMM 服务器，您需要︰

    `$VaultSettingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;`

## 步骤 4︰ 安装 Azure 站点恢复服务提供商

1. 从站点恢复存储库中的快速启动页下载提供商的安装文件。
2. 在 VMM 中的每个服务器上创建文件夹使用以下命令︰

    `pushd C:\ASR\`

3. 运行以下命令，将文件解压已下载提供商︰

    `AzureSiteRecoveryProvider.exe /x:. /q`

4. 通过运行安装的提供程序︰

    `.\SetupDr.exe /i`
    `$installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
                $isNotInstalled = $true;
                if(Test-Path $installationRegPath)
                {
                                $isNotInstalled = $false;
                }
    }While($isNotInstalled)`

5. 等待安装过程完成，然后在存储库中注册服务器︰

    `$BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice`

## 步骤 5︰ 设置的存储库上下文

1. 运行 Get AzureSiteRecoveryVault cmdlet 以确保在特定的保险存储下运行所有命令︰

    `$Vault = Get-AzureSiteRecoveryVault -ResouceGroupName $ResourceGroupName | where { $_.Name -eq $VaultName}`

2. 下载的存储库设置︰ 

    `$VaultSettingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Vault $Vault -Path $OutputPathForSettingsFile`

3. 为了确保这些 cmdlet 运行下运行该存储库︰

    `Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSetingsFile.FilePath`

## 步骤 3︰ 配置云保护设置

VMM 服务器在存储库中注册后可以配置云保护设置将应用于位于群注册的 VMM 服务器上的 Hyper-V 主机上的所有虚拟机。

1. 在主要和次要云彩的电子仓库创建容器︰

    `$PrimaryContainer = Get-AzureSiteRecoveryProtectionContainer -FriendlyName  $PrimaryCloudName`
    `$$RecoveryContainer = Get-AzureSiteRecoveryProtectionContainer -FriendlyName  $RecoveryCloudName`

2. 配置保护设置将应用于群︰

    `New-AzureSiteRecoveryProtectionProfile -Name $ProtectionProfileName -ReplicationProvider HyperVReplica -ReplicationMethod Online -ReplicationFrequencyInSeconds 30 -RecoveryPoints 1 -ApplicationConsistentSnapshotFrequencyInHours 0 -ReplicationPort 8083 -Authentication Kerberos -AllowReplicaDeletion`

3. 启动作业要云保护设置相关联的云容器︰

    `Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $ProtectionProfile -PrimaryProtectionContainer $PrimaryContainer -RecoveryProtectionContainer $RecoveryContainer`


## 步骤 4︰ 启用虚拟机保护

启用对 VMM 群中的虚拟机的保护︰

1. 获得您想要保护的虚拟机︰

    `$VM = Get-AzureSiteRecoveryProtectionEntity -ProtectionContainer $PrimaryContainer -FriendlyName $VMName `

2. 启用虚拟机的保护︰

    `Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $VM -Protection Enable`


## 步骤 5︰ 运行测试故障转移

1.  选择要进行故障转移虚拟的机︰

    `$VM = Get-AzureSiteRecoveryProtectionEntity -ProtectionContainer $PrimaryContainer -FriendlyName  $VMName`

2. 运行测试故障切换作业︰

    `$ currentJob = Start-AzureSiteRecoveryTestFailoverJob -ProtectionEntity $VM -Direction PrimaryToRecovery`

3. 检查故障转移 VM 中显示，辅助站点，并完成故障转移︰

    `Resume-AzureSiteRecoveryJob -Id $currentJob.Name`


## 下一步行动

有关的问题和意见，在这种情况下请访问[站点恢复论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr/)
