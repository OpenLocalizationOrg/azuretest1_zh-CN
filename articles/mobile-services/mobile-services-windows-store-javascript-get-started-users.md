---
ms.openlocfilehash: 3af8833d82f1633f4705be4d19e4ca472ee64d47
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用身份验证 (JavaScript) |Microsoft Azure"
    description="了解如何使用移动服务通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft Windows 商店 JavaScript 应用程序的用户进行身份验证。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 添加到您的移动服务应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users-legacy](../../includes/mobile-services-selector-get-started-users-legacy.md)]

本主题演示如何在 Azure 移动服务从您的应用程序的用户进行身份验证。  在本教程中，您添加到快速入门项目使用标识提供程序支持移动服务的身份验证。 后被成功通过身份验证和授权的移动服务，将显示用户 ID 值。  

本教程将引导您完成以下基本步骤，以便您的应用程序中的身份验证︰

1. [注册您的应用程序进行身份验证和配置移动服务]
2. [限制为经过身份验证的用户的表权限]
3. [向应用程序添加验证]
4. [存储在客户端的身份验证令牌]

本教程基于移动服务快速入门。 此外第一次必须完成本教程[开始使用移动服务]。

>[AZURE.NOTE]本教程演示如何移动服务使用不同的身份提供程序管理的身份验证流程。 此方法可以很容易地配置，并且支持多个提供程序。 要改为使用客户端管理身份验证实时连接并提供单一登录体验 Windows Phone 应用程序中的，请参阅[为 Windows 应用商店应用程序通过使用实时连接的单一登录]的主题。 通过使用客户端管理身份验证，您的应用程序有权访问其他用户标识提供程序维护的数据。 通过服务器脚本中调用**user.getIdentities()**函数，可以通过移动服务中获得相同的用户数据。 有关详细信息，请参阅[此公告](http://go.microsoft.com/fwlink/p/?LinkId=506605)。

##<a name="register"></a> 注册您的应用程序进行身份验证和配置移动服务

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

<ol start="5">
<li><p>（可选）执行中的步骤<a href="/documentation/articles/mobile-services-how-to-register-store-app-package-microsoft-authentication/">注册 Microsoft 身份验证的 Windows 应用商店应用程序软件包</a>。</p>


    <p>请注意此步骤是可选的因为它只适用于 Microsoft 帐户的登录提供程序。 使用移动服务注册您的 Windows 应用商店应用程序软件包信息时，客户端就能够重复使用单一登录方式进行登录体验 Microsoft 帐户登录凭据。 如果不这样做，每当调用登录时，将显示 Microsoft 帐户登录用户登录提示。 当您计划使用 Microsoft 客户身份标识提供程序，请完成此步骤。</p>
</li>
</ol>
现在将您的移动服务和应用程序配置为使用您选择的身份验证提供程序。

##<a name="permissions"></a> 限制对已验证身份的用户的权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

<ol start="3">
<li><p>在 Visual Studio 2012 Express 针对 Windows 8，打开项目时您完成本教程创建<a href="/develop/mobile/tutorials/get-started/">开始使用移动服务</a>。</p></li>
<li><p>按 F5 键以运行此快速入门基于的应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。</p>

    <p>这是因为该应用程序试图访问移动服务作为未经身份验证的用户，但<em>TodoItem</em>表现在要求进行身份验证。</p></li>
</ol>

接下来，您将更新应用程序验证用户身份之前从移动服务中请求的资源。

##<a name="add-authentication"></a> 向应用程序添加验证

[AZURE.INCLUDE [mobile-services-windows-store-javascript-authenticate-app](../../includes/mobile-services-windows-store-javascript-authenticate-app.md)]

##<a name="tokens"></a>在客户端上存储的授权标记

[AZURE.INCLUDE [mobile-services-windows-store-javascript-authenticate-app-with-token](../../includes/mobile-services-windows-store-javascript-authenticate-app-with-token.md)]

## <a name="next-steps"> </a>下一步行动

在下一步的教程中，[服务端的移动服务用户授权，][授权用户提供的脚本]，将采取基于身份验证的用户的移动服务提供的用户 ID 值，并使用它来筛选移动服务返回的数据。


<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[存储在客户端的身份验证令牌]: #tokens
[下一步行动]:#next-steps


<!-- URLs. -->
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[为使用实时连接的 Windows 应用商店应用程序的单一登录]: mobile-services-windows-store-javascript-single-sign-on.md
[开始使用移动服务]: ../mobile-services-windows-store-get-started.md
[有关数据入门]: mobile-services-windows-store-javascript-get-started-data.md
[开始使用身份验证]: mobile-services-windows-store-javascript-get-started-users.md
[开始使用推式通知]: mobile-services-javascript-backend-windows-store-javascript-get-started-push.md
[授权用户使用的脚本]: ../mobile-services-windows-store-javascript-authorize-users-in-scripts.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[注册为 Microsoft 身份验证 Windows 应用商店应用程序软件包]: /develop/mobile/how-to-guides/register-windows-store-app-package
