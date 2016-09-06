---
ms.openlocfilehash: a2711fc9c091d4d9829ed479aabd694027602013
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="如何配置应用程序服务应用程序的 Azure Active Directory 身份验证" 
    description="了解如何配置应用程序服务应用程序的 Azure Active Directory 身份验证。" 
    authors="mattchenderson" 
    services="app-service\mobile" 
    documentationCenter="" 
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

# 如何配置应用程序以使用 Azure Active Directory 登录

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何配置作为身份验证提供程序使用 Azure Active Directory Azure 应用程序服务。 

## <a name="register"> </a>Azure Active Directory 中注册应用程序

1. 登录到[预览 Azure 管理门户网站]，然后导航到您的移动应用程序。

2. 在**设置****用户身份验证**，请单击，然后单击**Azure Active Directory**。 复制**应用程序 URL**并**回复 URL**列出。 您将使用这些更高。 请确保**应用程序的 URL**和**回复 URL**使用 HTTPS 方案。

    ![][1]

3. 登录到[Azure 管理门户]并定位到**活动目录**。

    ![][2] 

4. 选择您的目录，然后选择**应用程序**选项卡的顶部。 若要创建新的应用程序注册的底部，单击**添加**。 

5. 单击**添加我的公司正在开发的应用程序**。

6. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**Web 应用程序和/或 Web API**类型。 然后单击以继续。

7. 在**符号在 URL**中，粘贴来自 Active Directory 标识提供程序设置您的移动应用程序的应用程序 ID。 在**应用程序 ID URI**框中输入此相同的资源标识符。 然后单击以继续。

8. 该应用程序添加后，单击**配置**选项卡。 编辑**回复 URL**下**单一登录**为先前复制的移动应用程序回复 URL。 它应附有_/signin-aad_的移动应用程序网关。 例如， `https://contosogateway.azurewebsites.net/signin-aad`。 请确保您正在使用 HTTPS 方案。

    ![][3]

9. 单击**保存**。 然后复制该应用程序**的客户端 ID** 。

## <a name="secrets"> </a>将 Azure Active Directory 信息添加到您的移动应用程序

1. 为您的移动应用程序返回预览管理门户和**Azure Active Directory**设置刀片式服务器。 粘贴在 Azure Active Directory 身份标识提供程序的**客户端 ID**设置。
  
2. 在**允许承租人**列表中，您需要添加已注册的应用程序 (如 contoso.onmicrosoft.com) 的目录的域。 您可以通过单击 Azure Active Directory 租户上的**域**选项卡中找到默认域的名称。 向**允许承租人**列表中添加您的域名，然后单击**保存**。  

现在您可以为您的应用程序中的身份验证使用 Azure Active Directory。 

## <a name="related-content"> </a>相关的内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

移动应用程序使用 Azure Active Directory 单一登录的用户进行身份验证︰ [iOS][ios adal]

<!-- Images. -->

[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication-preview/mobile-app-aad-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication-preview/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication-preview/app-service-aad-app-configure.png

<!-- URLs. -->

[预览 Azure 的管理门户]: https://portal.azure.com/
[Azure 的管理门户]: https://manage.windowsazure.com/
[ios adal]: ../app-service-mobile-dotnet-backend-xamarin-ios-aad-sso-preview.md
 
测试
