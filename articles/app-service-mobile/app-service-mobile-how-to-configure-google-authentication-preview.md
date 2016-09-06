---
ms.openlocfilehash: c0937856396db8ab4d5493b038273cde1f934131
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何配置 Google 应用程序服务应用程序的身份验证"
    description="了解如何配置 Google 应用程序服务应用程序的身份验证。"
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
    ms.date="08/28/2015"
    ms.author="mahender"/>

# 如何配置应用程序以使用 Google 登录

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何配置移动应用程序作为身份验证提供程序使用 Google。

若要完成此主题中的过程，您必须具有一个已验证电子邮件地址的 Google 帐户。 若要创建一个新的 Google 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>。

1. 登录到[Azure 管理门户]，然后导航到您的移动应用程序。 复制您的**URL**。 您将使用您的 Google 应用程序稍后使用此。
 
2. 单击**设置**，**用户身份验证**，然后单击**Google**。 复制**重定向 URI**。 您将使用此配置您的 Google 应用程序。

3. 导航到[Google 的 api](http://go.microsoft.com/fwlink/p/?LinkId=268303)的网站，登录与您的 Google 帐户凭据，单击**创建项目**，提供**项目名称**，然后单击**创建**。

4. 在左侧的导航栏中， **API 和身份验证**，请单击，然后单击下**社会 Api**的**Google + API** > **启用 API**。

5. 单击**API 并验证** > **凭据** > **OAuth 同意屏幕**，然后选择您的**电子邮件地址**，输入一个**产品名称**，并单击**保存**。

6. 在**凭据**选项卡，单击**添加凭据** > **OAuth 2.0 客户机 ID**，然后选择**Web 应用程序**。

7. 粘贴先前复制**授权 JavaScript 起源**，到移动应用程序**的 URL** ，然后粘贴先前复制到**授权重定向 URI****重定向 URI** 。 重定向 URI 是附加的路径， _/signin-google_的移动应用程序网关。 例如， `https://contosogateway.azurewebsites.net/signin-google`。 请确保您正在使用 HTTPS 方案。 然后单击**创建**。

8. 在下一个屏幕上，记下客户机 ID 和客户端密钥的值。

    > [AZURE.IMPORTANT] 客户端密钥是重要的安全凭据。 不要与任何人共享该密钥或将其分发到客户端应用程序内部。

9. 回在[Azure 管理门户]，为移动应用程序，Google 设置刀片上粘贴中的客户机 ID 和客户端以前获取的机密值。 然后单击**保存**。

     ![][1]

现在您可以为您的应用程序中的身份验证使用 Google。

## <a name="related-content"> </a>相关的内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication-preview/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication-preview/mobile-app-google-settings.png

<!-- URLs. -->

[Google 的 api]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure 的管理门户]: https://portal.azure.com/
 

测试
