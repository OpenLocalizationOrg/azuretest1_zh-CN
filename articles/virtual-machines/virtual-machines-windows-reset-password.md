---
ms.openlocfilehash: 0f7379e3686cc481e57d93c112c74f13de4421a9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何重置密码或 Windows 虚拟机的远程桌面服务"
    description="快速重置本地管理员密码或 Windows Azure 预览门户或 PowerShell 命令使用的虚拟机的远程桌面服务。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2015"
    ms.author="dkshir"/>

# 如何重置密码或 Windows 虚拟机的远程桌面服务

如果您无法连接到 Windows 虚拟机由于忘记的密码或远程桌面服务配置问题，使用 Azure 预览门户或 VMAccess 扩展名重置本地管理员密码或重置远程桌面服务配置。

> [AZURE.NOTE] 本文不适用于 Azure 资源管理器中创建的虚拟机。

## 预览门户

要重置[预览门户](https://portal.azure.com)中的远程桌面服务，请单击**浏览所有** > **虚拟机 （经典）** > *您的 Windows 虚拟机* > **重置远程访问**。 显示以下页面。


![](./media/virtual-machines-windows-reset-password/Portal-RDP-Reset-Windows.png)

若要重置的名称和[预览门户](https://portal.azure.com)中的本地管理员帐户的密码，请单击**浏览所有** > **虚拟机 （经典）** > *您的 Windows 虚拟机* > **所有设置** > **重设密码**。 显示以下页面。

![](./media/virtual-machines-windows-reset-password/Portal-PW-Reset-Windows.png)


## VMAccess 扩展 PowerShell

在开始之前，您将需要以下︰

- Azure PowerShell 模块、 版本 0.8.5 或更高版本。 您可以检查已安装使用的 Azure PowerShell 的版本**获取模块 azure | 表格格式版本**命令。 有关说明和指向最新版本的链接，请参阅[如何安装和配置 Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320552&clcid=0x409)。
- 新的本地管理员帐户密码。 如果您想重置远程桌面服务配置，您不需要这样。
- 虚拟机代理。

VMAccess 扩展不需要进行安装才能使用它。 只要在虚拟机上安装了 VM 代理，使用**一组 AzureVMExtension** cmdlet 的 Azure PowerShell 命令运行时扩展时自动加载。

首先，请验证已安装了 VM 代理。 添加的云服务名称和虚拟机的名称，然后管理员级 Azure PowerShell 命令提示符处运行以下命令。 引号，包括的所有内容替换 < 和 > 字符。

    $csName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $csName -Name $vmName
    write-host $vm.VM.ProvisionGuestAgent

如果您不知道的云服务和运行**Get AzureVM**在您的当前订购中显示该信息的所有虚拟机的虚拟机名称。

如果**写入主机**命令显示**True**，安装了 VM 代理。 如果显示**False**，请参阅的说明和链接到下载的[VM 代理和扩展-第 2 部分](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409)的 Azure 的博客张贴内容中。

如果使用 Azure 门户创建虚拟机，可以运行以下附加命令。

    $vm.GetInstance().ProvisionGuestAgent = $true

此命令将防止"规定访客代理必须启用虚拟机对象上设置 IaaS VM 访问扩展之前"错误时运行**一组 AzureVMExtension**命令在以下各节中。

现在，您可以执行以下任务︰

- 重置本地管理员帐户密码。
- 远程桌面服务配置重置。

## 重置本地管理员帐户密码

添加当前的本地管理员帐户名和新密码，然后再运行下列命令。

    $cred=Get-Credential –Message "Type the name of the current local administrator account and the new password."
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

- 如果您键入一个不同的名称，比当前帐户，VMAccess 扩展名重命名本地管理员帐户，将密码赋给该帐户，并发出远程桌面注销。
- 如果禁用本地管理员帐户，则 VMAccess 扩展启用它。

这些命令也重置远程桌面服务配置。

## 重置远程桌面服务配置

要重置远程桌面服务配置，请运行下面的命令。

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

VMAccess 扩展虚拟机上运行这两个命令︰

- **netsh 由防火墙规则组设置"远程桌面"新启用的 = = 是**

    此命令使允许传入的远程桌面通信，使用 TCP 端口 3389 的内置 Windows 防火墙组。

- **集 ItemProperty-HKLM:\System\CurrentControlSet\Control\Terminal 服务器路径的命名为"fDenyTSConnections"的值为 0**

    此命令将 fDenyTSConnections 注册表值设置为 0 时，启用远程桌面连接。

如果这没有解决您的远程桌面访问问题，运行[诊断软件包 Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)。

1.  在诊断包中，请单击**Microsoft Azure IaaS (Windows) 诊断包**来创建新的诊断会话。
2.  在**的下列问题，您是否遇到与 Azure VM？**页上，选择了**到 Azure VM （需要重新启动） 的 RDP 连接**问题。

有关详细信息，请参阅[Microsoft Azure IaaS (Windows) 诊断包](http://support.microsoft.com/kb/2976864)的知识库文章。

如果您不能运行 Azure IaaS (Windows) 诊断程序包或运行它未解决问题，请参阅[解决远程桌面连接到基于 Windows Azure 的虚拟机](virtual-machines-troubleshoot-remote-desktop-connections.md)。


## 其他资源

[Azure VM 扩展和功能](http://msdn.microsoft.com/library/azure/dn606311.aspx)

[连接到使用 RDP 或 SSH Azure 的虚拟机](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[疑难解答远程桌面连接到基于 Windows Azure 的虚拟机](virtual-machines-troubleshoot-remote-desktop-connections.md)
