---
ms.openlocfilehash: c2cb446012f2de67cf5e8132848236ae7e384ce1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用身份验证 (Android) |Microsoft Azure"
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft Android 应用程序的用户进行身份验证。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="ricksal"/>

# 添加到您的移动服务 Android 应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 摘要

本主题演示如何在 Azure 移动服务从您的应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。

> [AZURE.VIDEO android-getting-started-with-authentication-in-windows-azure-mobile-services]

本教程将引导您完成基本的步骤，以使您的应用程序中的身份验证。


##先决条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## 注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## 限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

1. 在 Android Studio 中，打开您完成本教程[开始使用移动服务]时创建的项目。

2. 从**运行**菜单，然后单击**运行应用程序**;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。

     这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

## 向应用程序添加验证

[AZURE.INCLUDE [mobile-services-android-authenticate-app](../../includes/mobile-services-android-authenticate-app.md)]

## <a name="cache-tokens"></a>缓存在客户端的身份验证令牌

[AZURE.INCLUDE [mobile-services-android-authenticate-app-with-token](../../includes/mobile-services-android-authenticate-app-with-token.md)]

## <a name="refresh-tokens"></a>令牌缓存刷新

[AZURE.INCLUDE [mobile-services-android-authenticate-app-refresh-token](../../includes/mobile-services-android-authenticate-app-refresh-token.md)]



## <a name="next-steps"></a>下一步行动

在下一步的教程中，[有脚本的授权用户]，将采用基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[存储在客户端的身份验证令牌]: #cache-tokens
[刷新已到期的令牌]: #refresh-tokens
[下一步行动]:#next-steps

<!-- Images. -->




[4]: ./media/mobile-services-android-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-android-get-started-users/mobile-service-uri.png







[13]: ./media/mobile-services-android-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-android-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-android-get-started-users/mobile-portal-change-table-perms.png


<!-- URLs. -->

[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[为使用实时连接的 Windows 应用商店应用程序的单一登录]: /develop/mobile/tutorials/single-sign-on-windows-8-dotnet
[开始使用移动服务]: /develop/mobile/tutorials/get-started-android
[添加到现有应用程序的移动服务]: /develop/mobile/tutorials/get-started-with-data-android
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-android
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-android
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-android

[Azure 的管理门户]: https://manage.windowsazure.com/

测试
