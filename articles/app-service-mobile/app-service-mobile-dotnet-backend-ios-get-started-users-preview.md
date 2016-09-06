---
ms.openlocfilehash: 7a22a449acc915cdb7717016eb1a2b800c7965f0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 iOS Azure 移动应用程序上添加身份验证"
    description="了解如何使用 Azure 移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft iOS 应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="krisragh" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/27/2015"
    ms.author="krisragh"/>

# iOS 使用 Azure 移动应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

在本教程中，您将添加身份验证到[iOS 快速启动]项目使用支持的标识提供程序。 本教程基于[iOS 快速启动]教程，您必须首先完成。 如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。  

##<a name="create-gateway"></a>创建应用程序的服务网关

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-gateway-preview](../../includes/app-service-mobile-dotnet-backend-create-gateway-preview.md)] 

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

在 Xcode，按**运行**启动该应用程序。 因为应用程序尝试访问后端作为未经身份验证的用户，但_TodoItem_表现在要求身份验证，则将引发异常。

##<a name="add-authentication"></a>添加到应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS 快速入门]: app-service-mobile-dotnet-backend-ios-get-started-preview.md

[Azure 的管理门户]: https://portal.azure.com
 

测试
