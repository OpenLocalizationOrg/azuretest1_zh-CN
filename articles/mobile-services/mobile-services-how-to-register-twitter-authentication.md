---
ms.openlocfilehash: 62ea7b8cd32b14f8d54507e62da4acf9bdb0eba9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册 Twitter 身份验证 |Microsoft Azure" 
    description="了解如何使用 Twitter 身份验证与 Azure 移动服务应用程序。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/08/2015" 
    ms.author="glenga"/>

#移动服务中注册 Twitter 登录应用程序

[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

本主题演示如何注册您的应用程序能够使用 Twitter 与 Azure 移动服务进行身份验证。

>[AZURE.NOTE] 本教程是关于[Azure 移动服务](http://azure.microsoft.com/services/mobile-services/)，一个解决方案来帮助您生成可扩展的移动应用程序的任何平台。 移动服务使您可以轻松地同步数据，验证用户身份，并发送推式通知。 此页支持<a href="http://azure.microsoft.com/documentation/articles/mobile-services-ios-get-started-users/">开始使用身份验证</a>教程演示如何登录到您的应用程序的用户。 如果这是您的第一个体验移动服务，请填写本教程<a href="http://azure.microsoft.com/documentation/articles/mobile-services-ios-get-started/">开始使用移动服务</a>。

若要完成此主题中的过程，您必须具有一个已验证电子邮件地址的 Twitter 帐户。 要创建新的 Twitter 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>。

1. 定位到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268300" target="_blank">使用 Twitter 开发</a>网站，登录使用 Twitter 帐户凭据，然后单击**创建新的应用程序**。

    ![][1]

2. 键入**名称**和**说明**，以及**网站**值对于您的应用程序，然后键入您的移动服务路径_/login/twitter_ **回调 URL**中附加 URL。

    >[AZURE.NOTE]对于.NET 后端移动服务通过使用 Visual Studio 发布到 Azure，重定向 URL 是附加路径_登录 twitter_与您移动服务的 URL。 在此示例中，我们的移动服务者必须回调 URL ```https://todolist.azure-mobile.net/signin-twitter```。

    ![][2]

3.  在页面的底部，读取和接受的条款，然后单击**创建 Twitter 应用程序**。 

    这将注册该应用程序显示应用程序的详细信息。

6. 单击**密钥和访问令牌**在您的应用程序的仪表板选项卡并记下**用户密钥**和**使用者密码**的值。 

    > [AZURE.NOTE] 使用者秘密是重要的安全凭据。 不要与任何人共享该密钥或将其分发您的应用程序使用。

7. 单击**设置**选项卡，向下滚动并请确保选中了**允许登录 Twitter 可用于此应用程序**复选框，然后单击**更新设置**。

现在您就可以使用 Twitter 登录进行身份验证，在您的应用程序通过提供移动服务的使用者密钥和使用者密钥值。

<!-- Anchors. -->

<!-- Images. -->
[1]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-developers.png
[2]: ./media/mobile-services-how-to-register-twitter-authentication/mobile-services-twitter-register-app1.png

<!-- URLs. -->

[Twitter 的开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet/

[Azure 的管理门户]: https://manage.windowsazure.com/
 
测试
