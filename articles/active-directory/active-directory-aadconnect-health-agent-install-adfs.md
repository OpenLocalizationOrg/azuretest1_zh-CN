---
ms.openlocfilehash: 91717d6e2794e497e498b7f34e08a66bb97ac06e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure AD 连接健康 AD FS 代理安装 |Microsoft Azure" 
    description="这是描述安装 Active Directory 联合身份验证服务 (AD FS) 代理 Azure AD 连接健康页。" 
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
    ms.date="08/14/2015" 
    ms.author="billmath"/>






# Azure AD 连接 AD fs 健康代理安装

本文档将指导您完成安装和配置您的服务器的 AD fs 的 Azure AD 连接健康代理。 

>[AZURE.NOTE]请记住，您看到 Azure AD 连接健康的实例中的任何数据之前，您将需要在目标服务器上安装 Azure AD 连接健康代理。  请务必完成要求[这里](active-directory-aadconnect-health.md#requirements)之前安装代理。  您可以下载代理[此处](http://go.microsoft.com/fwlink/?LinkID=518973)。


## 代理安装
要启动代理安装，请双击您下载的.exe 文件。 在第一个屏幕上，单击安装。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install1.png)

一旦完成安装，请单击立即配置。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install2.png)

这将启动跟将执行注册 AzureADConnectHealthADFSAgent 一些 PowerShell 命令提示符。 系统将提示您登录到 Azure。 继续操作并登录。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install3.png)


登录后，将继续 PowerShell。 它完成后，您可以关闭 PowerShell 和配置已完成。

在这种情况下，应自动启动服务，代理会立即监视和数据收集。  请注意，您将看到警告 PowerShell 窗口中的，如果没有满足所有必备了前面各节中所述。 请务必完成要求[这里](active-directory-aadconnect-health.md#requirements)之前安装代理。 下面的屏幕快照下面是这些错误的一个示例。

![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install4.png)

若要验证已安装代理，请打开服务并查找以下。 如果您完成了配置，则应该运行这些服务。 否则，他们不会启动直到配置完成。

- Azure AD 连接健康 AD FS 诊断服务
- Azure AD 连接健康 AD FS 见解服务
- Azure AD 连接健康 AD FS 监视服务
 
![验证 Azure AD 连接的运行状况](./media/active-directory-aadconnect-health-requirements/install5.png)


## 在 Windows Server 2008 R2 服务器上的代理安装

对于 Windows Server 2008 R2 服务器执行以下操作︰

1. 请确保该服务器正在运行 Service Pack 1 或更高。
1. 关闭代理安装的 IE esc 键︰
1. 在每个服务器之前安装 AD 健康代理安装 Windows PowerShell 4.0。  若要安装 Windows PowerShell 4.0:
 - 安装[Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779)使用下面的链接下载脱机安装程序。
 - 安装 PowerShell ISE （从 Windows 功能）
 - 安装[Windows 管理框架 4.0。](https://www.microsoft.com/download/details.aspx?id=40855)
 - 安装 Internet Explorer 版本 10 或更高的服务器上。 这是健康服务要求您使用 Azure 管理员凭据进行身份验证。
1. 在 Windows Server 2008 R2 上安装 Windows PowerShell 4.0 的其他信息，请参阅 wiki 文章[下面](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx)。

## 相关的链接

* [Azure AD 连接的运行状况](active-directory-aadconnect-health.md)
* [Azure AD 连接健康操作](active-directory-aadconnect-health-operations.md)
* [与 AD FS 使用 Azure AD 连接运行状况](active-directory-aadconnect-health-adfs.md)
* [Azure AD 连接健康常见问题解答](active-directory-aadconnect-health-faq.md)

测试
