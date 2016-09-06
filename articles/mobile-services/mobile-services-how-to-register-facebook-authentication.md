---
ms.openlocfilehash: d32ebf9638f0751198e8128aff969794846783f7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册 Facebook 的身份验证 |Microsoft Azure" 
    description="了解如何在您的移动服务 Azure 应用程序中使用 Facebook 的身份验证。" 
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
    ms.date="06/27/2015" 
    ms.author="glenga"/>

# 移动服务中注册 Facebook 的身份验证的应用程序

[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

本主题演示如何注册您的应用程序能够使用 Facebook 来使用 Azure 移动服务进行身份验证。 

>[AZURE.NOTE] 本教程是关于[Azure 移动服务]，一个解决方案来帮助您生成可扩展的移动应用程序的任何平台。 移动服务使您可以轻松地同步数据，验证用户身份，并发送推式通知。 此页支持<a href="http://azure.microsoft.com/documentation/articles/mobile-services-ios-get-started-users/">开始使用身份验证</a>教程演示如何登录到您的应用程序的用户。 如果这是您的第一个体验移动服务，请填写本教程<a href="http://azure.microsoft.com/documentation/articles/mobile-services-ios-get-started/">开始使用移动服务</a>。
    
若要完成此主题中的过程，您必须有一个经验证的电子邮件地址和移动电话号码的 Facebook 帐户。 要创建新的 Facebook 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268285" target="_blank">facebook.com</a>。

1. 定位到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268286" target="_blank">Facebook 开发</a>网站，登录与您的 Facebook 帐户凭据。

2. （可选）如果尚未注册，单击**我的应用程序**然后单击**注册开发人员为**接受此策略，请按照注册步骤。 

3. 单击**我的应用程序** > **添加新应用程序** > **高级的设置**。

4. 键入一个唯一的**显示名称**为您的应用程序、 选择**页面的应用程序****类别**，然后单击**创建的应用程序 ID**并完成安全练习。 

    这将创建一个新的 Facebook 应用程序 id。

5. 单击**设置****应用程序域**中键入您的移动服务的域、 输入可选的**联系人的电子邮件**，单击**添加平台**，选择**网站**。

    ![][3]

6. 在**网站 URL**中键入您的移动服务的 URL，然后单击**保存更改**。

7. 单击**显示**，提供您的密码，如果请求，然后记下**应用程序 ID**和**应用程序密码**的值。 

    ![][5]
    &nbsp;
    
    >[AZURE.IMPORTANT] 应用程序密码是重要的安全凭据。 不要与任何人共享该密钥或将其分发您的应用程序使用。
    &nbsp;

8. 单击**高级**选项卡，键入附加路径_/login/facebook_中**有效 OAuth 重定向 Uri**，您移动服务的 URL，然后单击**保存更改**。 
    &nbsp;

     >[AZURE.NOTE] 为.NET 后端移动服务发布到 Azure 使用 Visual Studio，重定向 URL 是您的移动服务路径_登录 facebook_上附加 URL 您的移动服务作为.NET 服务，如`https://todolist.azure-mobile.net/signin-facebook`。  
       

9. 单击**状态和查看** > **是**以启用一般公众对于您的应用程序。

    注册新的应用程序时使用的 Facebook 帐户是应用程序的管理员，并有权访问该应用程序以管理员的身份。 此步骤授予一般公共访问权限，以便应用程序可以通过使用其他的 Facebook 帐户进行身份验证。 


现在您可以通过提供的应用程序 ID 和应用程序密码值到移动服务为您的应用程序中的身份验证使用 Facebook 登录。  

<!-- Anchors. -->

<!-- Images. -->
[3]: ./media/mobile-services-how-to-register-facebook-authentication/mobile-services-facebook-configure-app.png
[5]: ./media/mobile-services-how-to-register-facebook-authentication/mobile-services-facebook-completed.png

<!-- URLs. -->
[Facebook 开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure 的管理门户]: https://manage.windowsazure.com/
[Azure 的移动服务]: http://azure.microsoft.com/services/mobile-services/
 
测试
