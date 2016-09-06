---
ms.openlocfilehash: d9a177b2d06e295c096dc768dd4fe96524c60a5a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="应用程序访问权限和单一登录使用 Azure 活动目录是什么？ |Microsoft Azure"
    description="使用 Azure Active Directory 以启用单一登录到所有的 SaaS 和 web 应用程序所需的业务。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2015"
    ms.author="asmalser-msft"/>

#应用程序访问权限和单一登录使用 Azure 活动目录是什么？

单一登录意味着可以访问所有的应用程序和资源所需做的业务，仅当使用单个用户帐户登录。 登录之后，您可以访问所有需要而无需进行身份验证的应用程序 （例如键入的密码） 第二次。

许多组织依赖于软件 Office 365、 框和销售队伍等服务 (SaaS) 应用程序的最终用户生产效率。 从历史上看，IT 人员需要单独创建和更新每个 SaaS 应用程序中的用户帐户，用户需要记住每个 SaaS 应用程序的密码。

Azure Active Directory 扩展了内部部署 Active Directory 入云，从而使用户能够使用其主要组织帐户不只登录到加入域的设备以及公司的资源，但所有的 web 和 SaaS 应用程序还需要完成作业。

因此不仅用户无需管理多个组的用户名和密码，他们可以自动设置或取消设置访问权限的应用程序基于自身组织的组成员，以及他们的状态作为员工。 Azure 的 Active Directory 介绍安全和存取治理控制使您可以集中管理跨 SaaS 应用程序的用户的访问权限。

Azure 广告允许方便地集成到许多当今的流行 SaaS 应用程序;它提供身份和访问管理，并使用户能够直接，单一登录应用程序或发现并从 Office 365 或 Azure 广告接入面板等门户启动它们。

集成的体系结构由以下四个主要构造块组成︰

* 单一登录使用户能够访问他们在 Azure AD 中根据其组织帐户的 SaaS 应用程序。 单一登录是一种使用户对应用程序使用单个组织帐户进行身份验证。

* 用户资源调配允许用户供应和消除资源调配到目标中，SaaS 基于 Windows 服务器的 Active Directory 和/或 Azure 的广告中所做的更改。 提供的帐户是一种使用户得到授权才能使用应用程序之后他们已经通过单一登录身份验证。

* 在 Azure 管理门户中的集中应用程序访问管理委派应用程序访问决策的制定和批准，再到组织中的任何人的能力使单个点的 SaaS 应用程序的访问和管理，

* 统一的报告和 Azure AD 中监视用户活动

##如何 does 单一登录使用 Azure Active Directory 工作吗？

当用户"登陆"到应用程序时，它们经过身份验证过程它们是需要证明他们是谁他们所说。 而不使用单一登录，这通常是通过输入密码将存储在该应用程序，并且用户必须知道此密码。

Azure 的广告支持三种不同方法登录到应用程序︰

*   **联合 Single Sign-on**使应用程序重定向到 Azure AD 用户身份验证而不提示输入自己的密码。 支持此功能的应用程序支持协议如 SAML 2.0 中，WS 联合身份验证或 OpenID 连接，并且是最丰富的单一登录模式。

*   **基于密码的单一登录**启用安全应用程序密码存储和使用的 web 浏览器扩展或移动应用程序的重播。 但这使管理员能够管理密码，并利用现有登录过程中提供的应用程序，不要求用户知道密码。

*   **现有单一登录**使 Azure 的广告，利用任何现有单一登录已设置为该应用程序，但使这些应用程序可以链接到 Office 365 或 Azure AD 访问面板入口，并且还可以在 Azure AD 时存在启动应用程序中的其他报告。

一旦用户具有与应用程序进行身份验证，还需要有在告诉应用程序的应用程序提供一个帐户记录其中那里权限和访问级别是在应用程序内。 此帐户记录的供应可以是自动进行，也会发生手动管理员用户提供单点登录访问之前。

 这些单一登录模式和资源调配，下面的详细信息。

###联盟单一登录

联盟单一登录启用登录，您的组织中的用户能够自动登录到一个第三方的 SaaS 应用程序通过 Azure 使用 Azure AD 中的用户帐户信息的广告。

在这种情况下，当您已经已记录到 Azure 的广告，并且想要访问的由第三方 SaaS 应用程序中，控制资源联盟不用要重新通过身份验证的用户。

Azure 广告联盟单一登录与支持 SAML 2.0 中，WS 联合身份验证的应用程序可以支持或 OpenID 连接协议。

另请参见︰[管理联盟单一登录证书](active-directory-sso-certs.md)

###基于密码的单一登录

基于密码的单一登录配置使您的组织中的用户能够自动登录到一个第三方的 SaaS 应用程序通过使用第三方的 SaaS 应用程序的用户帐户信息的 Azure 广告。 当启用此功能时，Azure 的广告收集并安全地存储用户帐户信息和相关的密码。

Azure 的广告可以为任何基于云的应用程序具有一个基于 HTML 的登录页上支持基于密码的单一登录。 通过使用自定义浏览器插件，AAD 自动化过程通过安全地从目录中检索应用程序的凭据，例如用户名和密码的用户登录和应用程序的登录页面代表用户输入这些凭据。 有两个用例︰

1.  **管理员可以管理凭据**-管理员可以创建和管理应用程序的凭据，并将这些凭据分配给用户或组需要访问该应用程序。 在这些情况下，最终用户不需要知道凭据，但仍然获得了只需通过单击它，其接入面板或通过提供的链接到应用程序的单一登录访问。 这样，同时为最终用户，因此，他们不需要记住或管理应用程序特定的密码的管理员，以及方便使用的凭据生命周期管理。 凭据是从最终用户混淆期间自动注册过程;但是，它们是由用户使用 web 调试工具，技术上可发现，用户和管理员一样由用户直接出示的凭据，应遵循相同的安全策略。 提供很多的用户，如社会媒体或共享应用程序的文档共享的帐户访问权限时，管理员提供的凭据是非常有用的。

2.  **用户管理的凭据**— 管理员可以分配应用程序到最终用户或组，并允许最终用户输入时访问应用程序第一次在他们访问面板直接他们自己的凭据。 这将创建为最终用户，因此，它们不需要它们访问该应用程序每次都不断地输入特定于应用程序的密码提供了便利。 本使用案例还可以用作管理管理凭据，因此，管理员可以设置新应用程序的凭据在将来的日期而无需更改应用程序的访问体验到最终用户的踏脚石。

在这两种情况下，凭据存储在目录中，加密状态并自动签入过程中只通过 HTTPS 传递。 在使用基于密码的单一登录，Azure 广告提供方便标识访问管理解决方案，都不能够支持的联合身份验证协议的应用程序。

基于密码的 SSO 依赖浏览器扩展来安全地从 Azure AD 中检索的应用程序和用户的特定信息，并将其应用到服务。 大多数第三方的 SaaS 应用程序所支持的 Azure 的广告支持此功能。

对于基于密码的 SSO，可以为最终用户的浏览器︰

- Internet Explorer 8、 9 和 10-Windows 7 或更高版本
- 在 Windows 7 上铬-或更高版本，和 MacOS X 或更高版本
- Firefox 26.0 或以后 — Windows XP SP2 或更高版本，以及 Mac OS X 10.6 或更高版本

**注意︰**基于密码的 SSO 扩展将成为可用的 Windows 10 中的边缘时浏览器扩展成为支持边缘。

###现有单一登录

在配置单一登录应用程序时，Azure 管理门户网站提供的"现有单一登录"第三种选择。 此选项只允许管理员创建一个链接到应用程序，并将其放置在所选用户的访问面板。

例如，如果没有被配置为使用活动目录联合身份验证服务 2.0 的用户进行身份验证的应用程序，管理员可以使用"现有单一登录"选项在访问权限面板上创建指向它的链接。 当用户访问该链接时，它们在使用活动目录联合身份验证服务 2.0，或任何现有的单一登录解决方案提供的应用程序进行身份验证。

###用户资源调配

对于选择的应用程序，Azure 的广告使自动化的用户供应和消除资源调配的第三方 SaaS 应用程序中从 Azure 管理门户上，使用您的 Windows 服务器的 Active Directory 或 Azure AD 身份信息中的帐户。 当用户在这些应用程序之一的 Azure AD 中给定权限时，可以自动创建帐户 （调配） 在 SaaS 应用程序的目标。

当用户被删除或在 Azure AD 更改他们的信息时，这些更改将还反映在 SaaS 应用程序中。 这意味着，配置自动的身份生命周期管理使管理员能够控制和自动化的资源调配和消除资源调配的 SaaS 应用程序提供。 在 Azure 的广告，用户供应启用此身份生命周期管理的自动化。

要了解详细信息，请参阅[自动化用户供应和 Deprovisioning 对 SaaS 应用程序](active-directory-saas-app-provisioning.md)

##学习如何使用 Azure AD 应用程序库

准备好开始了吗？ 部署单一登录之间 Azure 的 AD 和 SaaS 应用程序，您的组织使用，请遵循以下准则。

###使用 Azure AD 应用程序库

[Azure 活动目录应用程序库](http://azure.microsoft.com/marketplace/active-directory/all/)提供已知支持单一登录使用 Azure 活动目录的一种形式的应用程序的列表。

![][1]

以下是一些提示通过它们支持哪些功能来查找应用程序︰

*   Azure 的广告支持自动资源调配和消除资源调配的[Azure 活动目录应用程序库](http://azure.microsoft.com/marketplace/active-directory/all/)中的所有"主要"应用程序。

*   单一登录使用 SAML，WS 联合身份验证等协议联合联盟特别支持的应用程序列表或 OpenID 连接可以找到[这里](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx)。

一旦找到您的应用程序，您可以开始通过按照循序渐进的说明显示在应用程序库和 Azure 管理门户中启用单一登录。

###不在库中的应用程序？

如果在 Azure AD 应用程序库中找不到您的应用程序，则可以使用这些选项︰

*   **添加您使用未列出的应用程序**-使用 Azure 管理门户中应用程序库中的自定义类别来连接您的组织使用列出的应用程序。 您可以添加支持 SAML 2.0 为联合应用程序中，任何应用程序或任何应用程序具有基于 HTML 的登录页面作为密码 SSO 应用程序。 有关更多详细信息，请参阅[添加您自己的应用程序](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)上的这篇文章。


*   **添加您自己正在开发的应用程序**-如果您自己开发应用程序，按照实施联盟单一登录的 Azure 广告开发人员文档中的指南或资源调配使用 Azure 广告图形 API。 有关详细信息，请参阅以下资源︰
  * [Azure AD 身份验证方案](active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)

*   **请求的应用程序集成**的应用程序需要使用[Azure 的广告反馈论坛](http://feedback.azure.com/forums/169401-azure-active-directory)请求支持。

###使用 Azure 的管理门户

在 Azure 管理门户中，可以使用 Active Directory 扩展应用单一登录配置。 作为第一步，您需要从门户中的 Active Directory 部分选择一个目录︰

![][2]

管理第三方的 SaaS 应用程序，您可以切换到所选目录的应用程序选项卡。 此视图使管理员可以︰

* 从 Azure AD 库以及正在开发的应用程序中添加新的应用程序
* 删除集成的应用程序
* 管理他们已经具有集成的应用程序

典型的管理任务，为第三方的 SaaS 应用程序包括︰

* 启用单一登录使用 Azure 广告，使用 SSO 的密码，或如果适用于目标 SaaS，联合 SSO
* （可选） 使资源调配的资源调配和消除资源调配 （身份生命周期管理） 的用户的用户
* 对于应用程序中启用用户供应，选择哪些用户有权访问该应用程序

用于库支持联盟单一登录的应用程序，配置通常要求您提供如证书和元数据的其他配置设置来创建联合的信任关系的第三方应用程序和 Azure 的广告。 配置向导可指导您完成的详细信息，并提供可以方便地访问 SaaS 应用程序特定的数据和指令。

对于库支持自动资源调配的用户的应用程序，这需要您为 Azure AD 权限来管理您的帐户在 SaaS 应用程序中。 至少，您需要提供给目标应用程序通过身份验证时，应使用凭据 Azure 的广告。 是否需要提供的其他配置设置，取决于应用程序的要求。

##部署 Azure AD 集成到用户应用程序

Azure 的广告提供了多种应用程序部署到您的组织中的最终用户可自定义方法︰

* Azure 广告接入面板
* Office 365 应用程序启动器
* 直接登录到联合应用程序
* 联合的、 基于密码的深层链接或现有应用程序

选择您的组织中部署的方法是您根据自己的判断。

###Azure 广告接入面板

在 https://myapps.microsoft.com 的访问面板是基于 web 的门户网站，允许最终用户使用 Azure Active Directory 以查看和启动基于云的应用程序中到已被授予访问 Azure AD 管理员的组织帐户。 如果您是最终用户使用[Azure 活动目录特优](http://azure.microsoft.com/pricing/details/active-directory/)，您还可以利用自助服务组管理功能，通过访问权限面板。

![][3]

访问面板从 Azure 管理门户独立并不要求用户具有 Azure 订阅或 Office 365 订阅。

Azure 广告接入面板的详细信息，请参阅[简介访问权限面板](active-directory-saas-access-panel-introduction.md)。

###Office 365 应用程序启动器

对于已经部署了 Office 365 的组织，Azure 广告通过向用户分配的应用程序也会出现在 https://portal.office.com/myapps Office 365 门户中。 这使得很容易和方便用户的组织，而无需使用第二个门户网站，启动其应用程序中，使用 Office 365 的组织建议使用的应用程序启动解决方案。

![][4]

有关 Office 365 提供应用程序启动程序的详细信息，请参阅[让你出现在 Office 365 提供应用程序启动程序中的应用](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher)。

###直接登录到联合应用程序

最支持 SAML 2.0、 WS 联合身份验证或 OpenID 的联合的应用连接还支持用户启动应用程序，然后再获取签名通过 Azure 广告通过自动重定向或单击一个链接上的登录能力。 这就是服务提供商-登录，启动和 Azure AD 应用程序库中的联合的应用最支持此 （请参阅文档链接从应用程序的单一登录配置向导在 Azure 管理门户中的详细信息）。

![][5]

###用于联合的、 基于密码的或现有的应用程序直接登录链接

Azure 的广告还支持直接登录链接到单个应用程序支持的基于密码的单一登录、 现有单一登录，以及任何形式的联盟单一登录。

这些链接是在特定应用程序的过程中发送用户通过 Azure 广告符号，而不需要用户启动它们从 Azure 广告接入面板或 Office 365 的专门编制的 Url。 这些单一的登录 Url 中可包含任何预先集成的应用程序的仪表板选项卡下 Active Directory Azure 的管理门户，明如下面的屏幕快照中所示。

![][6]

这些链接可以复制并粘贴任何地方您想要提供所选应用程序的登录的链接。 这可能是在一封电子邮件，或为用户应用程序访问的设置的任何自定义的基于 web 的门户。 以下是 Azure 广告直接单个登录 URL 的 Twitter 的一个示例︰

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

类似于组织特定 Url 的访问面板，您可以进一步自定义此 URL 通过 myapps.microsoft.com 域后添加一个活动或已验证域，以便于您的目录。 这可以确保任何组织品牌立即上加载登录页而无需先输入他们的用户 ID 的用户︰

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

当授权的用户单击其中一个特定于应用程序的链接时，他们第一次看到 （假定它们尚未登录），其组织登录页和登录后将被重定向到其应用程序而无需首先停止接入面板在。 如果用户没有访问该应用程序，如基于密码的单一登录浏览器扩展，系统必备链接将提示用户安装缺少的扩展名。 链接 URL 也保持不变，如果应用程序的单一登录配置发生了变化。

这些链接使用相同的访问控制机制作为接入面板和 Office 365，这些用户或组已分配到 Azure 管理门户中的应用程序将能够成功通过身份验证。 但是，任何未获授权的用户将看到一个消息，解释它们未被授予访问权限，并给出了加载访问面板以查看可用的应用程序，它们无权访问的链接。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png

测试
