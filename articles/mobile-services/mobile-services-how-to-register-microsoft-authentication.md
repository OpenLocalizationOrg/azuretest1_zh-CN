---
ms.openlocfilehash: 916dd476e144ae0e92f84346a49fedf3733e3905
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册为 Microsoft 身份验证 |Microsoft Azure" 
    description="了解如何注册 Microsoft 在 Azure 移动服务应用程序的身份验证。" 
    authors="ggailey777" 
    services="mobile-services" 
    documentationCenter="Mobile" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/08/2015" 
    ms.author="glenga"/>

# 注册您的应用程序使用 Microsoft 帐户进行身份验证

[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

## 概述 

本主题演示如何注册您的移动应用程序能使用 Microsoft 帐户作为标识提供程序使用 Azure 移动服务。 导向服务身份验证和客户端定向的身份验证使用实时 SDK 应用相同的步骤。

##注册您的 Windows 应用商店应用程序在 Windows 开发人员中心

首先必须使用 Windows 开发人员中心注册 Windows 应用商店应用程序。 

>[AZURE.NOTE]Windows Phone 8、 Windows Phone 8.1 Silverlight 和非 Windows 应用程序可以跳过此部分。

1. 如果尚未注册您的应用程序，导航到 Windows 应用商店应用程序的开发人员中心在[提交应用程序页]，使用您的 Microsoft 帐户，登录，然后单击**应用程序名称**。

    ![](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-submit-win8-app.png)

2. 选择**创建新的应用程序通过保留一个唯一的名称**并单击**继续**，然后键入一个名称为您的**应用程序名称**的应用程序，请单击**保留应用程序名称**，然后**保存**。

    ![](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-win8-app-name.png)

    这将创建新的 Windows 应用商店注册为您的应用程序。

3. 在 Visual Studio 中，打开您完成本教程[开始使用移动服务](mobile-services-dotnet-backend-windows-store-dotnet-get-started.md)时创建的项目。

4. 在解决方案资源管理器中，右击 Windows 应用商店应用程序项目，单击**存储**，然后单击**将相关联的应用程序与存储...**。 

    ![](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-store-association.png)

    此时将显示向导**将应用程序与 Windows 应用商店**。

5. 在向导中，单击**登录**，然后再使用您的 Microsoft 帐户登录，选择在步骤 2 中，您保留的应用程序名称，然后单击**下一步** > **相关联**。

    将所需的 Windows 应用商店注册信息添加到应用程序清单。   

6. （可选）通用的 Windows 应用程序中，请重复步骤 4 和 5 为 Windows Phone 存储项目。 

6. 在新应用程序的 Windows 开发人员中心页面，单击**服务** > **推式通知**。 

7. 在**推式通知**页上，单击**Windows 推送通知服务 (WNS) 和 Microsoft Azure 移动服务**下的**Live 服务网站**。

这将显示 Microsoft 帐户页为您的应用程序。

## 配置您的 Microsoft 帐户注册并连接到移动服务

在本部分中的第一步仅适用于 Windows Phone 8、 Windows Phone 8.1 Silverlight，而非 Windows 商店应用程序。 为这些应用程序，您还可以忽略包安全标识符 (SID)，该功能仅适用于 Windows 应用商店应用程序。 

1. 对于非 Windows 商店应用程序中，导航到<a href="http://go.microsoft.com/fwlink/p/?LinkId=262039" target="_blank">我的应用程序</a>（如果需要） 在 Microsoft 客户开发中心，请与您的 Microsoft 帐户登录页上，单击**创建的应用程序**，然后键入**应用程序的名称**，单击**我接受**。

    这保留您的应用程序名称与 Microsoft 帐户，并显示 Microsoft 帐户页为您的应用程序。

2. 在为您的应用程序 Microsoft 帐户页，单击**API 设置**，选择启用**移动或桌面客户端应用程序**，作为**目标域**设置移动服务 URL，提供的值`https://<mobile_service>.azure-mobile.net/`在**重定向的 URL**，然后单击**保存**。

     >[AZURE.NOTE]为.NET 后端移动服务发布到 Azure 使用 Visual Studio，重定向 URL 是附加路径_登录 microsoft_与您移动服务的 URL 您的移动服务作为.NET 服务，如`https://todolist.azure-mobile.net/signin-microsoft`。 

    ![Microsoft 帐户 API 设置](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-win8-app-push-auth-2.png)

    **根域**应填写自动。

4. 单击**应用程序设置**并记下的**客户机 ID**、**客户的机密**和**包 SID**值。 
    
    ![Microsoft 帐户的应用程序设置](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-win8-app-push-auth.png)
    
    
    > [AZURE.NOTE] 客户端密钥是重要的安全凭据。 不要与任何人共享客户端密钥或将其分发您的应用程序使用。 只有 Windows 应用商店应用程序登记将看到一个包 SID 字段。

4. 在[Azure 管理门户]中，单击移动服务的**标识**选项卡，输入客户机 ID、 客户的机密和打包 SID 获得从标识提供程序，然后单击**保存**。 

    ![移动服务标识选项卡](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-identity-tab.png)
    
    >[AZURE.NOTE]您不需要为 Windows Phone 8、 Windows Phone 商店 8.1 Silverlight 或非 Windows 应用程序提供一个包 SID 值。
    
现在将您的移动服务和您的应用程序配置为使用 Microsoft 客户。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039

[Azure 的管理门户]: https://manage.windowsazure.com/
 
测试
