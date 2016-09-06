---
ms.openlocfilehash: 8807cf7403cfe4983c1d670b1d08dd74a2ec7f60
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="访问面板简介"
    description="学习如何使用各种版本的访问权限面板中 （Web 浏览器，Android 应用程序，iPhone 和 iPad 应用程序） 来访问分配给您的 SaaS 应用程序。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2015"
    ms.author="markusvi"/>


# 访问面板简介


访问权限面板是基于 web 的门户网站，允许最终用户使用 Azure Active Directory 以查看和启动基于云的应用程序中到已被授予访问 Azure AD 管理员的组织帐户。 如果您是最终用户使用 Azure Active Directory 版本，您还可以利用自助服务组管理功能，通过访问权限面板。 <br>
访问面板是分开 Azure 管理门户网站，并且不需要用户能够订阅了 Azure。 


![访问面板][1] 


访问权限面板允许用户编辑其配置文件设置，包括的功能的一些︰

- 更改与您组织的帐户关联的密码

- 编辑密码重置设置

- 编辑多因素身份验证相关的联系人和首选项设置 （对于那些需要使用其管理员的帐户）

- 查看帐户详细信息，如您的用户 ID，备用电子邮件、 手机和办公室电话号码

- 查看和发布基于云的应用程序到您已被授予访问 Azure AD 管理员。 从最终用户的角度来看访问权限面板有关的详细信息，请参阅[使用访问权限面板](https://msdn.microsoft.com/library/azure/dn756411.aspx)。

- 自我管理组。 更特别的是，您可以创建和管理安全组并请求在 Azure AD 安全组成员身份。 有关详细信息，请参阅[自助服务组合管理 Azure AD 中的用户](active-directory-accessmanagement-self-service-group-management.md)和[管理您的组](active-directory-manage-groups.md)。 




## 访问接入面板


用户通过访问下面的 URL 的 web 浏览器中访问访问权限面板︰ <br> 
**http://myapps.microsoft.com**

如果必须配置为登录页面的自定义标签，则可以加载默认情况下此品牌，通过附加到 URL 的末尾您所在组织的域︰ <br> 
**http://myapps.microsoft.com/contosobuild.com**

在这种情况下，目录 Azure 管理门户中的域选项卡下配置任何活动或已验证域名可用，如下面的屏幕快照中所示。


![Wingtip toys][2]  


该 URL 必须将分发给所有用户将登录到应用程序集成在一起 Azure 的广告。
 




## 身份验证

为达到访问权限面板中，用户必须进行身份验证使用 Azure AD 中的组织帐户。 <br>
用户可以到 Azure AD 直接验证。 <br>
或者，如果组织配置联合身份验证使用 ADFS 或其他技术，可以通过 Windows 服务器的 Active Directory 身份验证的用户。

如果用户已预订的 Azure 或 Office 365，一直在 Azure 管理门户或 Office 365 提供应用程序使用，然后他们将会看到应用程序列表而无需重新登录。 未经过身份验证的用户将被提示在 Azure 广告为其帐户使用的用户名和密码登录。 如果组织已配置联合身份验证，然后键入用户名就足够了。

身份验证后，用户将能够与已经集成了目录管理员应用程序交互。 若要了解如何使用 Azure AD 集成的应用程序，请参阅[应用程序访问权限和单一登录使用 Azure 活动目录是什么？](active-directory-appssoaccess-whatis.md)。
 




## Web 浏览器要求

至少，接入面板 JavaScript 和 CSS 启用要求浏览器支持。 为了使用户能够使用基于密码的 SSO 应用程序登录，访问控制面板扩展名必须安装在用户的浏览器中。 当用户选择的应用程序配置为基于密码的 SSO，自动下载此访问控制面板扩展名。

目前，访问控制面板扩展为可用于 Internet Explorer 8 及更高版本、 镶边和 Firefox 浏览器。





## 移动应用程序支持

若要能够登录到 iOS 和 Android 设备上的基于密码的 SSO 应用程序，用户应该安装我的应用程序发布 Azure Active Directory 团队的移动应用程序。





### 我对 Android 的应用程序


我对 Android 的应用程序今天在[Google 播放存储](https://play.google.com/store/apps/details?id=com.microsoft.myapps)中可用和 Android 设备运行 Android 版本 4.1 部，支持。


![我的应用程序][3]   






### 我的 iPhone 和 iPad 的应用程序


我为 iOS 的应用程序和任何 iPhone 或 iPad 运行 iOS 版本 7 部，支持，是目前在苹果应用程序商店。


![应用程序配置文件][4]    




> [AZURE.NOTE] Azure 的广告 （包括销售队伍、 Google 应用程序、 收存箱、 框、 Concur、 工作日、 Office 365 和 70 多个其他人），与支持联合身份验证的应用程序可以登录在任何设备上几乎任何 web 浏览器不需要插件或移动应用程序。 在[https://myapps.microsoft.com](https://myapps.microsoft.com/)的访问面板体验的其余部分也不需要我的应用程序移动应用程序在移动设备上使用。
 


 

## 用于测试的最终用户体验的提示

如果您已登录到 Azure 管理门户网站目录中使用的帐户是 Azure 管理员，您都会自动签名到访问权限面板为您当前的管理员帐户。 在这种情况下，您可以查看已分配给该帐户的所有应用程序。

**若要测试作为另一个用户帐户︰**

1. 单击右上角的 Azure 门户或盖板，用户菜单并选择"**签出**"。 这会对您签名从 Azure 的广告。

2. 转到**http://myapps.microsoft.com**在访问权限面板。

3. 在签名页中，输入您想要测试的目录中的帐户的用户名和密码。
 
## 启动应用程序

有几种类型的应用程序可以显示在访问权限面板上。
 
### Office 365 应用程序

如果组织使用 Office 365 提供应用程序和用户授权它们，Office 365 提供应用程序将显示在用户访问权面板上。

当用户单击 Office 365 提供应用程序的应用程序图块上时，它们是被重定向到该应用程序，自动登录。

### 配置了基于联盟的 SSO 的 Microsoft 和第三方应用程序

这是应用程序管理员 Azure 管理门户的活动目录部分中添加单个登录模式设置为"*Azure AD 单一登录*"。 如果它们已被显式授予访问该应用程序由管理员，用户将只能看到这些应用程序。

当用户单击这些应用程序之一的应用程序图块上时，它们是被重定向到该应用程序，自动登录。

### 基于密码的 SSO 无标识资源调配

这些是管理员具有 Azure 管理门户的活动目录部分中添加单个登录模式设置为*基于密码的单一登录*应用程序。 <br> 目录中的所有用户都将都看到在此模式下配置的所有应用程序。

用户单击某个应用程序，应用程序图块的第一次他们将被提示安装密码 SSO 插件 Internet Explorer 或镶边，可能需要重新启动 web 浏览器。 当用户返回到访问权限面板，并再次单击应用程序图块上时，它们将提示输入用户名和密码的应用程序。 一旦输入用户名和密码，将安全地存储在 Azure 的广告和链接到他们的帐户在 Azure 的广告，这些凭据并接入面板将自动用户登录到该应用程序使用这些凭据。

下一次用户单击应用程序图块，它们都会自动签名插入应用程序而无需输入凭据再次和无需重新安装密码 SSO 插件。

如果目标第三方应用程序中更改了用户的凭据，该用户必须同时更新其凭据存储在 Azure 的广告。 更新凭据，用户必须选择该图标在右下角应用程序磁贴，并选择"更新凭据"以重新输入用户名和密码，该应用程序。

### 基于密码的身份资源调配的 SSO

它们标识资源调配以及单一登录模式设置为"*密码的基于单一登录*"Azure 管理门户的活动目录部分中添加具有管理员应用程序。

用户单击某个应用程序，应用程序图块的第一次他们将被提示安装密码 SSO 插件 Internet Explorer 或镶边，可能需要重新启动 web 浏览器。 当他们返回到访问权限面板并再次单击应用程序图块上时，它们都会自动签名给应用程序。

某些应用程序可能需要的用户更改他们的密码中的第一个迹象。 如果目标第三方应用程序中更改了用户的凭据，该用户必须同时更新其凭据存储在 Azure 的广告。 更新凭据，用户必须选择该图标在右下角应用程序磁贴，并选择"更新凭据"以重新输入用户名和密码，该应用程序。

### 与现有的 SSO 解决方案的应用程序

在配置单一登录应用程序时，Azure 管理门户网站提供的"现有单一登录"第三种选择。 此选项只允许管理员创建一个链接到应用程序，并将其放置在所选用户的访问面板。 例如，如果没有被配置为使用活动目录联合身份验证服务 2.0 的用户进行身份验证的应用程序，管理员可以使用"现有单一登录"选项在访问权限面板上创建指向它的链接。 当用户访问该链接时，它们在使用活动目录联合身份验证服务 2.0，或任何现有的单一登录解决方案提供的应用程序进行身份验证。


[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/ic767166.png
[2]: ./media/active-directory-saas-access-panel-introduction/ic767167.png
[3]: ./media/active-directory-saas-access-panel-introduction/ic767168.png
[4]: ./media/active-directory-saas-access-panel-introduction/ic767169.png

测试
