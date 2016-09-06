---
ms.openlocfilehash: c71e906620249a56ba59a43578baf637b8ddf2f6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册 Azure Active Directory 验证 |Microsoft Azure" 
    description="了解如何注册 Azure Active Directory 中移动服务应用程序的身份验证。" 
    authors="wesmc7777" 
    services="mobile-services" 
    documentationCenter="" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="06/15/2015" 
    ms.author="wesmc"/>

# 注册您的应用程序使用 Azure 活动目录帐户登录

[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

##概述

本主题演示如何注册您的应用程序，以便能够作为身份验证提供程序使用 Azure Active Directory，您的移动服务。 

##注册您的应用程序

>[AZURE.NOTE] 本主题中介绍的步骤用于[将添加到您的移动服务应用程序的身份验证](../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md)教程与您想要与您的应用程序使用[导向服务登录操作](http://msdn.microsoft.com/library/azure/dn283952.aspx)时使用。 或者，如果您的应用程序[客户端定向登录操作](http://msdn.microsoft.com/library/azure/jj710106.aspx)的必要条件的 Azure Active Directory 和.NET 后端移动服务应与[身份验证与活动目录身份验证库单一登录应用程序](mobile-services-windows-store-dotnet-adal-sso-authentication.md)教程开始。

1. 登录到[Azure 管理门户]、 导航到您的移动服务，单击**标识**选项卡，然后向下滚动到**Azure 的活动目录**标识提供程序部分中和复制那里显示的**应用程序的 URL** 。

    ![AAD 的移动服务应用程序 URL](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-copy-app-url-waad-auth.png)

2. 导航到**活动目录**管理门户中单击目录，然后**域**，记下您的目录的默认域。

3. 单击**应用程序** > **添加** > **添加我的公司正在开发的应用程序**。

4. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**Web 应用程序和/或 Web API**类型。 

    ![命名您 AAD 的应用程序](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-add-app-wizard-1-waad-auth.png)

5. 在**登录 URL**框中，将粘贴来自您的移动服务的应用程序 URL 值。 在**应用程序 ID URI**框中，输入相同的唯一值，然后单击继续。
 
    ![设置 AAD 的应用程序属性](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-add-app-wizard-2-waad-auth.png)

6. 添加了应用程序后，单击**配置**选项卡并复制该应用程序**的客户端 ID** 。

    >[AZURE.NOTE]对于.NET 后端移动服务，还必须编辑在**单一登录**您的移动服务路径，_登录 aad_附加 url 下的**回复 URL**值。 例如，  `https://todolist.azure-mobile.net/signin-aad`

7. 返回到您的移动服务的**标识**选项卡和 azure 的活动目录标识提供程序中粘贴复制的**客户机 ID**值。
 
    ![](./media/mobile-services-how-to-register-active-directory-authentication/mobile-services-clientid-pasted-waad-auth.png)

8.  在**允许承租人**列表中，键入在其中注册应用程序的目录的域 (如`contoso.onmicrosoft.com`)，然后单击**保存**。    

现在您可以为您的应用程序中的身份验证使用 Azure Active Directory。 

<!-- Anchors. -->

<!-- Images. -->


<!-- URLs. -->
[Azure 的管理门户]: https://manage.windowsazure.com/

 
测试
