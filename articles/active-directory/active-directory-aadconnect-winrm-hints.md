---
ms.openlocfilehash: c730d8e5a49bce0512ee4e3e619cff9b25cff9ea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure AD 连接的 Windows 远程管理的提示" 
    description="对于使用 AD FS 的 azure AD 连接 Windows 远程管理的提示。" 
    services="active-directory" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2015" 
    ms.author="billmath"/>

# Azure AD 连接的 Windows 远程管理的提示


当使用 Azure AD 连接部署 Active Directory 联合身份验证服务或 Web 应用程序代理，请检查下面提示的要求，以确保连接和配置将会成功︰ 

- 如果目标服务器加入域，请确保已启用 Windows 远程管理 
    * 在提升 PSH 命令窗口中，使用命令"启用 PSRemoting – 强制" 

- 如果目标服务器为非域加入 WAP 机器，有几个额外要求 
    - 目标计算机 （WAP 机器） 上:" 

- 确保 winrm (Windows 远程管理 / WS 管理) 通过服务管理单元中运行服务 

- 在提升 PSH 命令窗口中，使用命令"启用 PSRemoting – 强制" 
    - 在其运行该向导的计算机上 （如果目标机器是非常非域加入或不受信任的域）︰ 

- 在提升 PSH 命令窗口中，使用命令"集项 WSMan:\localhost\Client\TrustedHosts – 值<DMZServerFQDN>-强制-Concatenate" 
    - 在服务器管理器中︰
        - 添加到计算机池 DMZ WAP 主机 (服务器管理器-> 管理-> 添加服务器...使用 DNS 选项卡) 
        - 服务器管理器所有服务器选项卡︰ 右键单击 WAP 服务器并选择管理另存为...，WAP 机输入凭据设置本地 （不是域） 
        - 若要验证远程 PSH 连接，在服务器管理器的所有服务器选项卡中︰ 右键单击 WAP 服务器并选择 Windows PowerShell。  远程会话 PSH 应打开，以确保可以建立远程 PowerShell 会话。 

**其他资源**


* [在 Azure AD 连接的帐户和权限的详细信息](active-directory-aadconnect-account-summary.md)
* [Azure AD 连接的自定义安装](active-directory-aadconnect-get-started-custom.md)
* [在 MSDN 上的 azure AD 连接](active-directory-aadconnect.md) 

测试
