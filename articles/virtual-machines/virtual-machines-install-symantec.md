---
ms.openlocfilehash: b81933c637ae92e1b4099c047b911040001ca32b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何安装和配置在 Azure VM 赛门铁克端点保护"
    description="介绍了安装和配置的赛门铁克端点保护安全扩展 Azure 中一个新的或现有的虚拟机上"
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
    ms.date="07/14/2015"
    ms.author="dkshir"/>

# 如何安装和配置在 Azure VM 赛门铁克端点保护

本文介绍了如何安装和配置新的或现有虚拟机 (VM) 运行 Windows Server 上的赛门铁克端点保护客户端。 这是完整的客户端，包括病毒和间谍软件保护、 防火墙和入侵防护等服务。 请注意，这篇文章是指使用传统部署模型创建的虚拟机。

客户端通过使用 VM 代理安装作为安全扩展插件。 在新的虚拟机，将安装代理和客户端终结点。 在现有虚拟机没有代理，您需要下载并安装代理程序第一次。 本文介绍了两种情况。

如果有现有订阅来自赛门铁克的内部解决方案，可用于保护您 Azure 的虚拟机。 如果你不客户尚未，您可以注册试用订阅。 有关此解决方案的详细信息，请参阅[微软 Azure 平台上的赛门铁克端点保护][Symantec]。 此页还包含指向许可信息，如果您已经是赛门铁克客户安装客户端的说明。

## 在新的虚拟机上安装赛门铁克端点保护

[Azure 管理门户][门户]允许您安装虚拟机代理和赛门铁克安全扩展时使用**剪辑库中的**选项来创建虚拟机。 使用这种方法是简便的方法，从赛门铁克添加保护，如果您正在创建单个虚拟机。

此**剪辑库中的**选项将打开一个向导，可帮助您设置虚拟机。 在向导的最后一页用于安装虚拟机代理和赛门铁克安全扩展。

一般的说明，请参阅[创建一个虚拟机运行 Windows 服务器][创建]。 当收到的最后一页上的向导:

1.  在 VM 代理**安装虚拟机代理**应已签。

2.  安全扩展插件，在选中**赛门铁克端点保护**。


    ![安装虚拟机代理和端点保护客户端](./media/virtual-machines-install-symantec/InstallVMAgentandSymantec.png)

3.  单击底部的页后，可以创建虚拟机的复选标记。

## 现有的虚拟机上安装赛门铁克端点保护

在开始之前，您将需要以下︰

- Azure PowerShell 模块、 版本 0.8.2 或更高版本，您的工作计算机上。 您可以检查已安装使用的 Azure PowerShell 的版本**获取模块 azure | 表格格式版本**命令。 有关说明和指向最新版本的链接，请参阅[如何安装和配置 Azure PowerShell][PS]。 请确保登录到 Azure 订购。

- 运行在 Azure 虚拟机的虚拟机代理。

首先，验证已将虚拟机上安装了 VM 代理。 填写的云服务名称和虚拟机的名称，然后管理员级 Azure PowerShell 命令提示符处运行以下命令。 引号，包括的所有内容替换 < 和 > 字符。

> [AZURE.TIP] 如果您不知道的云服务和虚拟机名称，运行**Get AzureVM**列出的在您当前的订阅中的所有虚拟机的名称。

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

如果**写入主机**命令显示**True**，安装了 VM 代理。 如果显示**False**，请参阅的说明和链接到 Azure 博客中下载发布[代理]的[虚拟机代理和扩展-第 2 部分]。

如果虚拟机安装代理后，运行以下命令以安装赛门铁克端点保护代理。

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

若要验证已安装赛门铁克安全扩展，并且是最新的︰

1.  登录到虚拟机。 有关说明，请参阅[如何登录到虚拟机运行 Windows 服务器][登录]。
2.  Windows Server 2008 R2，请单击**开始 > 赛门铁克端点保护**。 对于 Windows Server 2012 或 Windows Server 2012 R2，从开始屏幕中，键入**赛门铁克**，，然后单击**赛门铁克端点保护**。
3.  从**状态赛门铁克端点保护**窗口的**状态**选项卡上，应用更新或如果需要重新启动。

## 其他资源

[如何登录到运行 Windows 服务器的虚拟机][登录]

[Azure VM 扩展和功能][分机]


<!--Link references-->
[Symantec]: http://go.microsoft.com/fwlink/p/?LinkId=403942

[门户]: http://manage.windowsazure.com

[创建]: virtual-machines-windows-tutorial.md

[PS]: ../powershell-install-configure.md

[代理]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[登录]: virtual-machines-log-on-windows-server.md

[分机]: http://go.microsoft.com/fwlink/p/?linkid=390493
