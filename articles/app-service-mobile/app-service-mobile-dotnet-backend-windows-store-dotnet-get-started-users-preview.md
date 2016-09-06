---
ms.openlocfilehash: ec324321b24da05178776ddee3bc25fcee4b3841
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="将身份验证添加到您的 Windows 运行时 8.1 通用应用程序 |Azure 的移动应用程序"
    description="了解如何使用 Azure 应用程序服务移动应用程序使用不同的标识提供程序，包括 Windows 应用程序的用户身份验证︰ AAD、 Google、 Facebook、 Twitter，以及 Microsoft。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="mattchenderson" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/22/2015"
    ms.author="glenga"/>

# 添加到您的 Windows 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

本主题演示如何从客户端应用程序的应用程序服务移动应用程序的用户进行身份验证。 在本教程中，您将添加身份验证到快速入门项目使用标识提供程序所支持的应用程序服务。 后被成功通过身份验证和授权的移动应用程序，将显示用户 ID 值。

本教程基于移动应用程序快速入门。 您必须先完成本教程[开始使用您的移动应用程序]。 http://acom-sandbox.azurewebsites.net/documentation/articles/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview/?rnd=1

##<a name="create-gateway"></a>创建应用程序的服务网关

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-gateway-preview](../../includes/app-service-mobile-dotnet-backend-create-gateway-preview.md)] 

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4.在 Visual Studio 中，客户端应用程序项目中打开共享的 App.xaml.cs 项目文件并确保**MobileServiceClient**实例被配置为使用两个后端移动应用程序的 URL 和网关。

&nbsp;&nbsp;5.与一个 Windows 应用程序项目设置为启动项目，请按 F5 键以运行该应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。

&nbsp;&nbsp;这是因为该应用程序试图访问您的移动应用程序代码作为未经身份验证的用户，但*TodoItem*表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从您的应用程序服务请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

[AZURE.INCLUDE [app-service-mobile-windows-universal-dotnet-authenticate-app](../../includes/app-service-mobile-windows-universal-dotnet-authenticate-app.md)]


##<a name="tokens"></a>在客户端上存储的身份验证令牌

[AZURE.INCLUDE [app-service-mobile-windows-store-dotnet-authenticate-app-with-token](../../includes/app-service-mobile-windows-store-dotnet-authenticate-app-with-token.md)]

##下一步行动


<!-- URLs. -->
[开始使用您的移动应用程序]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md
 

测试
