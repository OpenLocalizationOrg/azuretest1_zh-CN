---
ms.openlocfilehash: b444b8db7792bcb82b36fa808be46c78df3228d6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="添加到现有 Azure 移动服务的应用程序 (iOS) 的身份验证 |Microsoft Azure"
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft iOS 应用程序的用户进行身份验证。"
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="07/01/2015"
    ms.author="krisragh"/>

# 添加到现有的 Azure 的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

在本教程中，您将添加身份验证到使用支持的标识提供程序快速启动项目。 本教程基于[移动服务快速入门教程]，您必须首先完成它。

##<a name="register"></a>注册用于身份验证的应用程序并配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

在 Xcode，打开该项目。 按**运行**按钮以启动该应用程序。 验证应用程序启动之后引发的异常状态代码 401 （未经授权）。 这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

##<a name="add-authentication"></a>添加到应用程序的身份验证

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../../includes/mobile-services-ios-authenticate-app.md)]

##<a name="store-authentication"></a>在应用程序中存储身份验证令牌

[AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../../includes/mobile-services-ios-authenticate-app-with-token.md)]

##<a name="next-steps"></a>下一步行动

在下一步的教程中，[服务端的移动服务用户的授权]，您将用户的用户 ID 值来筛选返回的数据。

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps
[在您的应用程序中存储身份验证令牌]:#store-authentication

<!-- URLs. -->
[服务端授权的移动服务用户]: mobile-services-dotnet-backend-service-side-authorization.md
[移动服务快速入门教程]: mobile-services-dotnet-backend-ios-get-started.md
[有关数据入门]: mobile-services-dotnet-backend-ios-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-ios-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-ios-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-dotnet-backend-ios-authorize-users-in-scripts.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: /develop/mobile/how-to-guides/work-with-net-client-library
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md

测试
