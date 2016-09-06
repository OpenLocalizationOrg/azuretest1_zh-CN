---
ms.openlocfilehash: 2efdd72419270150a1410442e7eca0f7fc41c090
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何配置 Twitter 应用程序服务应用程序的身份验证"
    description="了解如何配置 Twitter 的应用程序服务应用程序的身份验证。"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="07/27/2015"
    ms.author="mahender"/>

# 如何配置应用程序以使用 Twitter 的登录

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何配置 Azure 移动应用程序作为身份验证提供程序使用 Twitter。

若要完成此主题中的过程，您必须具有一个已验证电子邮件地址的 Twitter 帐户。 要创建新的 Twitter 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>。

## <a name="register"> </a>与 Twitter 注册应用程序


1. 登录到[Azure 管理门户]，然后导航到您的移动应用程序。 复制您的**URL**。 您将使用此配置您 Twitter 的应用程序。

2. 单击**设置****用户身份验证**，然后单击**使用 Twitter**。 复制该**回调 URL**。 您将使用此配置您 Twitter 的应用程序。

3. 定位到[使用 Twitter 开发]网站，登录使用 Twitter 帐户凭据，然后单击**创建新的应用程序**。

4. 键入**名称**和**说明**的新的应用程序。 在您的**移动应用程序 URL**的**网站**值中粘贴。 然后，**回调 URL**，将粘贴先前复制**回调 URL** 。 这是您的移动应用程序网关后面有一个路径， _/signin-twitter_。 例如， `https://contosogateway.azurewebsites.net/signin-twitter`。 请确保您正在使用 HTTPS 方案。

    ![][0]

3.  在页面的底部，读取和接受的条款。 然后单击**创建您的 Twitter 应用程序**。 这将注册该应用程序显示应用程序的详细信息。

4. 单击**设置**选项卡，检查**允许登录 Twitter 可用于此应用程序**，然后单击**更新设置**。

5. 选择**密钥和访问令牌**的选项卡。 记下的值的**使用者密钥 （API 密钥）**和**使用者密码 （API 机密）**。

    > [AZURE.NOTE] 使用者秘密是重要的安全凭据。 不要与任何人共享该密钥或将其分发您的应用程序使用。


## <a name="secrets"> </a>将 Twitter 信息添加到您的移动应用程序

1. 在[Azure 管理门户]上 Twitter 设置刀片式服务器为您的移动应用程序，重新粘贴以前获取的 API 键和 API 密钥值。 然后单击**保存**。

    ![][1]

现在您可以为您的应用程序中的身份验证使用 Twitter。

## <a name="related-content"> </a>相关的内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication-preview/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication-preview/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter 的开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure 的管理门户]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-dotnet-backend-xamarin-ios-get-started-users-preview.md
 
测试
