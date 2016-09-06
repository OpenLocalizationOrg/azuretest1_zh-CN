---
ms.openlocfilehash: 6d3f90b45ac0f35e3cef809e292e9ce2c414bbc0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何配置应用程序服务应用程序的 Microsoft 客户身份验证"
    description="了解如何配置应用程序服务应用程序的 Microsoft 客户身份验证。"
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

# 如何配置应用程序以使用 Microsoft 帐户登录

[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何配置 Azure 移动应用程序使用 Microsoft 帐户作为身份验证提供程序。

## <a name="register"> </a>使用 Microsoft 帐户注册应用程序

1. 登录到[Azure 管理门户]，然后导航到您的移动应用程序。

2. 单击**设置**，**用户身份验证**，然后单击**Microsoft 帐户**。 将复制的**重定向 URL**。 您将使用此 Microsoft 帐户配置新的应用程序。

3. 导航到[我的应用程序]页在 Microsoft 客户开发人员中心中，并使用您的 Microsoft 帐户，登录，如果需要。

4. 单击**创建的应用程序**，然后键入**应用程序的名称**，并单击**我接受**。

5. 单击**设置 API**。 选择**是****移动或桌面客户端应用程序**。 在**重定向 URL**字段中，输入先前复制的**重定向 URL** 。 这是移动应用程序网关附有_/signin-microsoft_。 例如， `https://contosogateway.azurewebsites.net/signin-microsoft`。 请确保您正在使用 HTTPS 方案。 在输入后的重定向 URL，单击**保存**。

    ![][0]

    >[AZURE.NOTE]现有的 Microsoft 客户应用程序注册，您可能必须先启用**增强重定向的安全性**。

4. 单击**应用程序设置**并记下**客户机 ID**和**客户端密钥**的值。

    > [AZURE.NOTE] 客户端密钥是重要的安全凭据。 不要与任何人共享客户端密钥或将其分发到客户端应用程序内部。


## <a name="secrets"> </a>将 Microsoft 帐户信息添加到您的移动应用程序

1. 回在[Azure 管理门户]Microsoft 帐户设置刀片式服务器为您的移动应用程序上，粘贴在以前获取的客户机 ID 和客户端密钥值。 然后单击**保存**。

    ![][1]

现在您可以使用 Microsoft 帐户进行身份验证，您的应用程序中。

## <a name="related-content"> </a>相关的内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Authenticate your app with Live Connect Single Sign-On: [Windows](windows-liveconnect) -->



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication-preview/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication-preview/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure 的管理门户]: https://portal.azure.com/
 
测试
