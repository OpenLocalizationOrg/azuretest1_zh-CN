---
ms.openlocfilehash: 97a2d4f8b8a6ff32ef2921aa9b2fd14c87deafb9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
4. 导航到 API 应用程序网关刀片式服务器。

    ![单击网关](./media/app-service-api-gateway-config-auth/gateway.png)

7. 在**网关**刀片式服务器，单击**设置**，然后单击**标识**。

    ![单击设置](./media/app-service-api-gateway-config-auth/clicksettingsingateway.png)

    ![单击标识](./media/app-service-api-gateway-config-auth/clickidentity.png)

    从**标识**刀片式服务器中，您可以导航到不同的刀片，用于配置身份验证使用 Azure Active Directory 和其他几个提供程序。

    ![刀片式服务器标识](./media/app-service-api-gateway-config-auth/identityblade.png)
  
3. 选择所需的标识提供程序，然后按照相应文章使用该提供程序配置 API 应用程序中的步骤操作。 这些文章面向那些使用移动应用程序，但这些过程都是相同的 API 的应用程序。 某些过程要求您使用 [Azure 门户]。 

 - [Microsoft 帐户](../articles/app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication-preview.md)
 - [Facebook 登录](../articles/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication-preview.md)
 - [Twitter 登录](../articles/app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication-preview.md)
 - [Google 登录](../articles/app-service-mobile/app-service-mobile-how-to-configure-google-authentication-preview.md)
 - [Azure 的活动目录](../articles/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication-preview.md)

作为示例，下面的屏幕快照显示什么您应该看到 [Azure 门户] 页面和 [Azure 预览门户] 刀片式服务器中设置 Azure Active Directory 验证之后。

在 Azure 预览门户中， **Azure Active Directory**刀片式服务器已从 Azure 的门户，Azure 活动目录选项卡中创建的应用程序**的客户端 ID**并**允许承租人**有您 Azure Active Directory 租户 (例如，"contoso.onmicrosoft.com")。

![Azure Active Directory 刀片式服务器](./media/app-service-api-gateway-config-auth/tdinaadblade.png)

在 Azure 的门户中， **Azure 活动目录**选项卡中创建的应用程序的**配置**选项卡已从**Azure Active Directory**刀片式服务器在 Azure 预览门户**登录 URL**、**应用程序 ID URI**，并**回复 URL** 。

![](./media/app-service-api-gateway-config-auth/oldportal1.png)

![](./media/app-service-api-gateway-config-auth/oldportal2.png)

![](./media/app-service-api-gateway-config-auth/oldportal3.png)

![](./media/app-service-api-gateway-config-auth/oldportal4.png)

(回复 URL 在图像中显示相同的 URL 两次，一次用`http:`和一次用`https:`。)

test111111
