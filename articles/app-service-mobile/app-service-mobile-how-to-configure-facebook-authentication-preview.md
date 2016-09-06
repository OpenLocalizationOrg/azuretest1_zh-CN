---
ms.openlocfilehash: a69e27c1f765ffc61edd6e4fcdf81a07bb16485b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何配置 Facebook 应用程序服务应用程序的身份验证"
    description="了解如何配置 Facebook 应用程序服务应用程序的身份验证。"
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

# 如何配置应用程序以使用 Facebook 登录

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何配置 Azure 移动应用程序，使用 Facebook 作为身份验证提供程序。

若要完成此主题中的过程，您必须有一个经验证的电子邮件地址和移动电话号码的 Facebook 帐户。 若要创建新的 Facebook 帐户，请转到[facebook.com]。

## <a name="register"> </a>使用 Facebook 注册应用程序

1. 登录到[Azure 管理门户]，然后导航到您的移动应用程序。 复制您的**URL**。 您将使用此配置您的 Facebook 应用程序。
 
2. 单击**设置**、**用户身份验证**，以及**Facebook**。 然后从 Facebook 刀片式服务器复制**重定向 URI** 。 将与您的 Facebook 应用程序中使用此数据。
 
3. 在另一个浏览器窗口中，导航到[Facebook 开发]网站和登录使用 Facebook 的帐户凭据。

4. （可选）如果尚未注册，请单击**应用程序**然后单击**注册开发者**接受此策略，按照注册步骤。

5. 单击**我的应用程序**，然后单击**添加新的应用程序**。

6. 选择与您的平台**网站**。 选择您的应用程序的唯一名称，然后单击**创建新的 Facebook 应用程序 ID**。

7. 选择您的应用程序，请从下拉列表中的一个类别。 然后，单击**创建的应用程序 ID**。

8. 在下一页上，在右上角选择**跳过的快速启动**。 这将为您的应用程序的开发人员仪表板。

9. 在**应用程序机密**字段中，单击**显示**，提供您的密码，如果请求，然后记下**应用程序 ID**和**应用程序密码**的值。 您将设置这些移动应用程序的 Facebook 的身份验证设置刀片式服务器上。

    > [AZURE.NOTE] **安全说明**
应用程序的机密是在重要的安全凭据。 不要与任何人共享该密钥或将其分发到客户端应用程序内部。

10. 在左侧的导航栏上，单击**设置**。 在**应用程序域**中，键入您的移动应用程序的**URL** ，并输入**联系人的电子邮件**。 

    ![][0]

11. 如果您看不到下面的网站部分，请单击**添加平台**并选择**网站**。 在**网站 URL**字段中，输入您的移动应用程序的**URL** ，然后单击**保存更改**。

12. 单击**高级**选项卡并添加您先复制到**有效 OAuth 重定向 Uri**的移动应用程序**重定向 URI** 。 然后单击**保存更改**。 您重定向 URI 是附加的路径， _/signin-facebook_网关移动应用程序的 URL。 例如， `https://contosogateway.azurewebsites.net/signin-facebook`。 请确保您正在使用 HTTPS 方案。

13. 用于将应用程序注册的 Facebook 帐户是应用程序的管理员。 在这种情况下，只有管理员可以登录到此应用程序。 要验证其他 Facebook 帐户，单击左侧的导航主板内置中的**状态与回顾**。 然后单击**是**以启用常规公共访问权限。


## <a name="secrets"> </a>将 Facebook 信息添加到您的移动应用程序


12. 重新设置[Azure 管理门户]，并再次导航到 Facebook 设置刀片式服务器为您的移动应用程序。 粘贴的应用程序 ID 和应用程序密码以前获取的值。 然后单击**保存**。

    ![][1]

现在您可以为您的应用程序中的身份验证使用 Facebook。

## <a name="related-content"> </a>相关的内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication-preview/app-service-facebook-dashboard.png
[1]: ./media/app-service-mobile-how-to-configure-facebook-authentication-preview/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook 开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[开始使用身份验证]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure 的管理门户]: https://portal.azure.com/
 
测试
