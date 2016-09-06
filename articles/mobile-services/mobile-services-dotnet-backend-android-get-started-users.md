---
ms.openlocfilehash: 603fbbb18f8d3ab102fe617434f242f31b33a66d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="开始使用身份验证 (Android) |Microsoft Azure" 
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft Windows 应用商店应用程序的用户进行身份验证。" 
    services="mobile-services" 
    documentationCenter="android" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="06/13/2015" 
    ms.author="ricksal"/>

# 添加到您的移动服务 Android 应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 摘要

本主题演示如何在 Azure 移动服务从您的应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。

本教程将引导您完成基本的步骤，以使您的应用程序中的身份验证。


## 先决条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

##<a name="register"></a>注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)] 

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)] 

3. 打开时完成[移动服务入门]教程创建的项目。 

4. 从**运行**菜单，然后单击**运行应用程序**以启动该应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 

     这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

[AZURE.INCLUDE [mobile-services-android-authenticate-app](../../includes/mobile-services-android-authenticate-app.md)]

## <a name="cache-tokens"></a>缓存在客户端的身份验证令牌

[AZURE.INCLUDE [mobile-services-android-authenticate-app-with-token](../../includes/mobile-services-android-authenticate-app-with-token.md)] 

## <a name="refresh-tokens"></a>令牌缓存刷新

[AZURE.INCLUDE [mobile-services-android-authenticate-app-refresh-token](../../includes/mobile-services-android-authenticate-app-refresh-token.md)] 

##<a name="next-steps"></a>下一步行动

在下一步的教程中，[服务端的移动服务用户授权，][授权用户提供的脚本]，将采取基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。 


<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[存储在客户端的身份验证令牌]: #cache-tokens
[刷新已到期的令牌]: #refresh-tokens
[下一步行动]:#next-steps

<!-- URLs. -->
[开始使用移动服务]: mobile-services-dotnet-backend-android-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-android-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-android-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-android-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-dotnet-backend-android-authorize-users-in-scripts.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: /develop/mobile/how-to-guides/work-with-net-client-library
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md
 
测试
