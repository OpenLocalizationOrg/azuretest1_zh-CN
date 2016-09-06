---
ms.openlocfilehash: 4077c822630b0e7c77e0c10d7d8762a6e3134eea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何安装和配置为在 Azure VM 服务趋势微深层安全 |Microsoft Azure"
    description="本文介绍如何安装和配置一个 Azure 中的虚拟机上的 Trend Micro 安全。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/15/2015"
    ms.author="dkshir"/>


# 如何安装和配置为在 Azure VM 服务趋势微深层安全

本文介绍了如何安装和配置为在新的或现有虚拟机 (VM) 运行 Windows Server 服务趋势微深层安全。 作为一种服务的深层安全包括反恶意软件保护、 防火墙、 入侵防御系统，和完整性监视。 请注意，这篇文章是指使用传统部署模型创建的虚拟机。

作为通过 VM 代理安全扩展插件安装客户端。 在新的虚拟机，将安装 VM 代理和深层安全代理。 在没有虚拟机代理现有虚拟机，您需要下载并安装它第一次。 本文介绍了两种情况。

如果有现有订阅 Trend Micro 从内部解决方案，可用于帮助保护您 Azure 的虚拟机。 如果你不客户尚未，您可以注册试用订阅。 有关此解决方案的详细信息，请参阅 Trend Micro 博客文章[Microsoft Azure VM 代理扩展为深层安全](http://go.microsoft.com/fwlink/p/?LinkId=403945)。

## 新的虚拟机上安装深度的安全代理程序

[Azure 门户](http://manage.windowsazure.com)允许您安装虚拟机代理和 Trend Micro 安全扩展时使用**剪辑库中的**选项来创建虚拟机。 使用这种方法是简便的方法，如果您正在创建单个虚拟机从 Trend Micro 增加保护。

此**剪辑库中的**选项将打开一个向导，可帮助您设置虚拟机。 在向导的最后一页用于安装虚拟机代理和 Trend Micro 安全扩展。 一般的说明，请参阅[创建虚拟机运行 Windows Azure 门户中](virtual-machines-windows-tutorial-classic-portal.md)。 时至最后一页上的向导，请执行以下操作︰

1.  在**VM 代理**，请检查**安装虚拟机代理**。

2.  **安全扩展插件**下, 检查**趋势微深层安全代理**。

    ![安装虚拟机代理和深层安全代理程序](./media/virtual-machines-install-trend/InstallVMAgentandTrend.png)

3.  单击复选标记以创建该虚拟机。

## 现有的虚拟机上安装深度的安全代理程序

若要执行此操作，您需要︰

- Azure PowerShell 模块、 版本 0.8.2 或更高版本，安装到本地计算机上。 您可以检查已安装使用的 Azure PowerShell 的版本**获取模块 azure | 表格格式版本**命令。 有关说明和指向最新版本的链接，请参阅[如何安装和配置 Azure PowerShell](../install-configure-powershell.md)。

- 目标虚拟机上安装了 VM 代理。

首先，请验证已安装了 VM 代理。 填写的云服务名称和虚拟机的名称，然后管理员级 Azure PowerShell 命令提示符处运行以下命令。 引号，包括的所有内容替换 < 和 > 字符。

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

如果您不知道的云服务和运行**Get AzureVM**在您的当前订购中显示该信息的所有虚拟机的虚拟机名称。

**写主机**命令返回**True**，如果安装了 VM 代理。 如果它返回**False**，请参阅的说明和链接到 Azure 博客中下载后[VM 代理和扩展-第 2 部分](http://go.microsoft.com/fwlink/p/?LinkId=403947)。

如果虚拟机安装代理后，运行以下命令。

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## 下一步行动

它采用代理开始运行安装时的几分钟时间。 在此之后，您需要激活深层安全虚拟机上，以便它可以管理由深层安全管理器。 请参阅下列附加说明︰

- 有关此解决方案中， [Instant-On 为 Microsoft Azure 的云安全](http://go.microsoft.com/fwlink/?LinkId=404101)趋势的文章
- [Windows PowerShell 的示例脚本](http://go.microsoft.com/fwlink/?LinkId=404100)来配置虚拟机
- 示例的[说明](http://go.microsoft.com/fwlink/?LinkId=404099)

## 其他资源

[如何登录到运行 Windows 服务器的虚拟机]

[Azure VM 扩展和功能]


<!--Link references-->
[如何登录到运行 Windows 服务器的虚拟机]: virtual-machines-log-on-windows-server.md
[Azure VM 扩展和功能]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
