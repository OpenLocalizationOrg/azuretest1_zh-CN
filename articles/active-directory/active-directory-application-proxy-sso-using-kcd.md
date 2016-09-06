---
ms.openlocfilehash: a953929206954a699e33a9fdc0824d8d0d3ab46f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="SSO 对 Prem IWA KCD 使用应用程序代理的应用程序"
    description="介绍如何使用 Azure AD 应用程序代理获取启动并运行。"
    services="active-directory"
    documentationCenter=""
    authors="rkarlin"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="rkarlin"/>



# 在 prem IWA 应用程序与应用程序代理使用 KCD SSO


您可以启用单一登录 (SSO) 应用程序使用集成 Windows 身份验证 (IWA) 通过在 Active Directory 中为应用程序代理服务器连接器权限来模拟用户并发送和接收标记代表他们。

> [AZURE.IMPORTANT] 应用程序代理服务器是仅当您升级到特优或 Azure Active directory 的基本版才可用的功能。 有关详细信息，请参见[Azure Active Directory 版本](active-directory-editions.md)。


## 在网络图中

![Microsoft AAD 身份验证流程关系图](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

当用户尝试访问使用 IWA prem 上应用程序时，此关系图解释流。 一般的流程是︰

1. 用户输入的 URL 来访问 prem 上应用程序到应用程序代理。
2. 应用程序代理将请求重定向到 Azure AD 身份验证服务，以对。 此时，Azure AD 应用任何可用的身份验证和授权策略，例如多因素身份验证。 如果用户通过验证，Azure 广告创建标记并将其发送给用户。
3. 用户将该标记传递给应用程序代理。
4. 应用程序代理服务器验证该令牌并检索用户主体名称 (UPN)，然后通过双重身份验证的安全通道的接口发送请求、 UPN 和服务主体名称 (SPN)。
5. 该连接器执行上 prem 与 Kerberos 约束委派 (KCD) 协商广告，模拟用户以获取 Kerberos 令牌给应用程序。
6. 活动目录发送到连接器应用程序的 Kerberos 令牌。
7. 连接器原始请求发送到应用程序服务器上，使用 Kerberos 令牌来自广告。
8. 应用程序将响应发送到连接器，然后返回到该应用程序代理服务并最后给用户。

### 先决条件

1. 请确保您的应用程序，如 SharePoint Web 应用程序，都设置为使用集成 Windows 身份验证。 有关详细信息，请参阅[启用 Kerberos 身份验证的支持](https://technet.microsoft.com/library/dd759186.aspx)，或 SharePoint 请参阅[SharePoint 2013 在 Kerberos 身份验证计划](https://technet.microsoft.com/library/ee806870.aspx)。
2. 创建您的应用程序的服务主体名称。
3. 请确保运行连接器的服务器，并运行该应用程序要发布的服务器加入域，属于同一个域。 在加入域的详细信息，请参阅[连接到域的计算机](https://technet.microsoft.com/library/dd807102.aspx)。


## 活动目录配置

Active Directory 配置各不相同，具体取决于您的应用程序代理服务器连接器和已发布的服务器是否位于同一域中。

### 连接器和发布的服务器位于同一域中

在活动目录中，请转到**工具** > **的用户和计算机**。 选择运行连接器的服务器。 右键单击并选择**属性** > **委派**。 选择**信任此计算机来委派指定的服务**，并**指此帐户可以提出委派的凭据的服务**，在添加应用程序服务器的服务主体名称 (SPN) 标识的值。 这使应用程序代理服务器连接器列表中定义的应用程序针对 AD 中模拟用户。

![连接器 SVR 属性窗口屏幕快照](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

### 连接器和发布不同域中的服务器

1. 有关先决条件 KCD 使用跨域的列表，请参阅[跨域 Kerberos 约束委派](https://technet.microsoft.com/library/hh831477.aspx)。
2. 在 Windows 2012 R2，使用`principalsallowedtodelegateto`连接器服务器以启用发布的服务器所在的连接器服务器委派应用程序代理上的属性`sharepointserviceaccount`和代理服务器是`connectormachineaccount`。

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount` 可以是 SP 机器帐户或服务帐户运行 SPS 应用程序池。


## Azure 的入口配置

1. 发布您的应用程序，根据所述[应用程序代理的发布应用程序](active-directory-application-proxy-publish.md)的说明。 请确保选择**Azure Active Directory**作为**预身份验证方法**。
2. 您的应用程序出现在应用程序列表后，选择它并单击**配置**。
3. 在**属性**，将**内部身份验证方法**设置为**集成 Windows 身份验证**。

![高级应用程序配置](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)

4. 输入应用程序服务器的**内部应用程序 SPN** 。 在此示例中，我们已发布的应用程序中的 SPN 是 http/lob.contoso.com。

>[AZURE.IMPORTANT] 在 Azure Active Directory Upn 必须与您内部部署 Active Directory 中为预身份验证工作 Upn 相同。 请确保将 Azure Active Directory 与内部活动目录进行同步。

| | |
| --- | --- |
| 内部身份验证方法 | 如果您使用预身份验证的 Azure 的广告，您可以设置一个内部身份验证方法，使用户享受到此应用程序的单点登录 (SSO)。 <br><br> 如果您的应用程序使用 IWA 并可以配置 Kerberos 约束委派 (KCD) 以启用此应用程序的 SSO，请选中**集成 Windows 身份验证**(IWA)。 必须使用 KCD 配置使用 IWA 的应用程序，否则应用程序代理服务器将不能将这些应用程序发布。 <br><br> 如果您的应用程序不使用 IWA，选择**None** 。 |
| 内部应用程序 SPN | 这是内部应用程序的服务主体名称 (SPN)，prem 在 Azure 广告中进行配置。 SPN 是应用程序代理服务器连接器用于读取应用程序使用 KCD Kerberos 令牌。 |

<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg

测试
