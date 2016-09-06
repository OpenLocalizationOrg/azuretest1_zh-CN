---
ms.openlocfilehash: cd322564905e9e9ab75e2811c388365c533bdeea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册为 Google 身份验证 |Microsoft Azure" 
    description="了解如何注册您的应用程序来使用 Google 使用 Azure 移动服务进行身份验证。" 
    services="mobile-services" 
    documentationCenter="android" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/27/2015" 
    ms.author="glenga"/>

# 移动服务中注册为 Google 登录应用程序

[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

本主题演示如何注册您的应用程序能够使用 Google 使用 Azure 移动服务进行身份验证。

>[AZURE.NOTE] 本教程是关于[Azure 移动服务](http://azure.microsoft.com/services/mobile-services/)，一个解决方案来帮助您生成可扩展的移动应用程序的任何平台。 移动服务使您可以轻松地同步数据，验证用户身份，并发送推式通知。 此页支持[身份验证开始](mobile-services-ios-get-started-users.md)教程，说明如何为您的应用程序的用户提供登录。 
<br/>如果这是您的第一个体验移动服务，请填写[移动服务入门](mobile-services-ios-get-started.md)教程。

若要完成此主题中的过程，您必须具有一个已验证电子邮件地址的 Google 帐户。 若要创建一个新的 Google 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>。

3. 导航到[Google 的 api](http://go.microsoft.com/fwlink/p/?LinkId=268303)的网站，登录与您的 Google 帐户凭据，单击**创建项目**，提供**项目名称**，然后单击**创建**。

4. 在左侧的导航栏中， **API 和身份验证**，请单击，然后单击下**社会 Api**的**Google + API** > **启用 API**。

5. 单击**API 并验证** > **凭据** > **OAuth 同意屏幕**，然后选择您的**电子邮件地址**，输入一个**产品名称**，并单击**保存**。

6. 在**凭据**选项卡，单击**添加凭据** > **OAuth 2.0 客户机 ID**，然后选择**Web 应用程序**。

7. 键入您的移动服务 URL**授权 JavaScript 起源**，**授权将重定向 URI**中生成的 URL 替换您附加的路径的移动服务的 URL `/login/google`，然后单击**创建的客户端 ID**。

    >[AZURE.NOTE] 为.NET 后端移动服务发布到 Azure 使用 Visual Studio，重定向 URL 是附有路径_登录 google_移动服务的 URL 您的移动服务作为.NET 服务，如`https://todolist.azure-mobile.net/signin-google`。 
    &nbsp;
    
8. 在下一个屏幕上，记下客户机 ID 和客户端密钥的值。

    > [AZURE.IMPORTANT] 客户端密钥是重要的安全凭据。 不要与任何人共享该密钥或将其分发到客户端应用程序内部。

现在您可以配置您的移动服务 Google 登录进行身份验证，您的应用程序中使用。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

[Google 的 api]: http://go.microsoft.com/fwlink/p/?LinkId=268303
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet/

[Azure 的管理门户]: https://manage.windowsazure.com/
 

测试
