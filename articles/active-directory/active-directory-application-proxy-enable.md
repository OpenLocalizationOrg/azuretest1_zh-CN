---
ms.openlocfilehash: 4099e544c4a9c3e1041b0dadb14ded38c33174c2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="启用 AD Azure 应用程序代理"
    description="介绍如何使用 Azure AD 应用程序代理获取启动并运行。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2015"
    ms.author="rkarlin"/>

# 启用 AD Azure 应用程序代理
> [AZURE.NOTE] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](https://msdn.microsoft.com/library/azure/dn532272.aspx)。

Microsoft Azure 应用程序代理广告，可以发布应用程序，如 SharePoint 站点、 Outlook Web Access 和基于 IIS 的应用程序，您的专用网络内部和网络以外的用户提供安全访问。 员工可以登录到您的应用程序，从家庭，在他们自己的设备上，并通过此基于云的代理服务器进行身份验证

这一节将指导您完成在 Azure 的广告，使 Microsoft Azure 云目录广告应用程序代理在您的专用网络，通过 Microsoft Azure AD 租户订阅注册连接器上安装应用程序代理服务器连接器。

##应用程序代理系统必备组件
您可以启用和使用应用程序代理服务器服务之前，您需要︰

- Microsoft Azure 管理员帐户。 如果不这样做有一个，您可以获取一个此处。
- 运行 Windows Server 2012 R2 或 Windows 8.1 或更高版本可以在其上安装应用程序代理服务器连接器服务器。 服务器必须能够将 HTTPS 请求发送到云中的应用程序代理服务，它必须具有 HTTPS 连接到您想要发布的应用程序。 
- 如果防火墙放置在该路径，请确保防火墙处于打开状态，以允许应用程序代理到 HTTPS (TCP) 请求来自连接器。 路由组连接器使用这些端口以及属于高级别域的子域︰ msappproxy.net。 请确保打开**所有****出站**通信流到以下端口︰

端口号 | 说明
--- | ---
80 | 若要启用出站 HTTP 通信的安全验证。
443 | 若要启用用户身份验证 Azure 广告 （仅对连接器注册过程必需的）
10100-10120 | 若要启用 LOB HTTP 响应发送回代理
9352、 5671 | 若要启用朝 Azure 服务侦听传入的请求的接口之间的通信。
9350 | 可选项。 若要启用传入请求的更好的性能。
8080 | 若要启用连接器引导自陷和启用连接器自动更新
9090 | 若要启用连接器注册 （仅对连接器注册过程必需的）
9091 | 若要启用连接器信任证书自动续订
 
如果您的防火墙会强制执行根据来自用户的通信，打开这些端口通信来自 Windows 服务，作为网络服务运行。 另外，请确保为 NT Authority\System 启用端口 8080。


##步骤 1︰ 启用在 Azure AD 的应用程序代理
1. 在 Azure 管理门户管理员身份登录。
2. 转到活动目录并选择要在其中启用应用程序代理的目录。
3. 单击配置，向下滚动到应用程序代理服务器切换到启用此目录中启用应用程序代理服务。

    ![启用应用程序代理](http://i.imgur.com/87woFzq.png) <p>
4. 单击下载现在在屏幕的底部。 这将下载页面。 阅读并接受许可协议条款并单击下载保存的应用程序代理服务器连接器的 Windows 安装程序文件 (.exe)。 

##步骤 2︰ 安装和注册该连接器
1. 您准备好的服务器上运行 AADApplicationProxyConnectorInstaller.exe （请参见应用程序代理系统必备组件）。
2. 按照向导中的说明进行安装。
3. 在安装过程中将提示您注册该连接器与活动应用程序代理帐户。
<p>提供了 Azure AD 全局管理员凭据。
-确保管理人员注册连接器是位于同一目录中启用应用程序代理服务，例如如果 contoso.com 租户域，则管理员应该 admin@contoso.com 或域上的任何其他别名。 并且您是 Azure AD 租户的全局管理员。 全局管理员租户可能不同于您的 Microsoft Azure 凭据。
-如果 IE 增强的安全配置设置为在服务器上要安装 Azure AD 接口，则可能会阻止注册屏幕。 如果发生这种情况，请按照以允许访问该错误消息中的说明进行操作。 请确保 Internet Explorer 增强的安全性已关闭。
-如果连接器注册不成功，请参阅故障诊断应用程序代理。

4. 安装完成后，两个新的服务添加到您的服务器，如下所示。 这些是连接器服务，从而使连接，并自动的更新服务，它定期检查连接器的新版本，并根据需要更新该连接器。 单击安装窗口中的完成以完成安装 ![应用程序代理服务器连接器服务](http://i.imgur.com/zsVJKOz.png) <p>
5. 现在您可以为应用程序代理的发布应用程序。

如果您想要卸载连接线，在卸载连接器服务和更新服务之后，请确保重新启动计算机才能完全删除该服务。
<p>为了获得高可用性，您必须部署至少一个额外的连接器。 若要部署的其他连接器，请重复上面的步骤 2 和 3。 每个连接器都必须单独注册。



## 其他资源

* [作为一个组织注册 azure](..sign-up-organization.md)
* [Azure 的标识](..fundamentals-identity.md)

测试
