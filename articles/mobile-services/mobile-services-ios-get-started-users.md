---
ms.openlocfilehash: e0c3eadb4f47f18a913a1834c9ce43077a686417
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

# 添加到现有应用程序的身份验证

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

在本教程中，您将添加身份验证[移动服务快速入门教程]使用支持的标识提供程序。

我们建议您首先完成[移动服务快速入门教程]。 或者，只需下载的快速启动 iOS 项目︰ 单击 Azure 门户网站 >**移动服务**> 移动服务 > 上左上角的云号 > **iOS** > **创建新的 iOS 应用程序** > **下载并运行您的应用程序** > **目标 C** > **下载**。 请务必单击**创建 TodoItem 表**，再单击**下载**，如果您还没有创建了表。

##<a name="register"></a>注册应用程序进行身份验证

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>限制数据已经过身份验证的用户权限

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

##<a name="add-authentication"></a>添加到应用程序的身份验证

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../../includes/mobile-services-ios-authenticate-app.md)]

##<a name="store-authentication"></a>在应用程序中存储身份验证令牌

[AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../../includes/mobile-services-ios-authenticate-app-with-token.md)]

## <a name="next-steps"></a>下一步行动

接下来，学习[如何使用用户 ID 值来筛选返回的数据](mobile-services-javascript-backend-service-side-authorization.md)。

<!-- Anchors. -->
[注册您的应用程序进行身份验证和配置移动服务]: #register
[限制为经过身份验证的用户的表权限]: #permissions
[向应用程序添加验证]: #add-authentication
[下一步行动]:#next-steps
[在您的应用程序中存储身份验证令牌]:#store-authentication

<!-- Images. -->




[4]: ./media/mobile-services-ios-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-ios-get-started-users/mobile-service-uri.png







[13]: ./media/mobile-services-ios-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-ios-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png


<!-- URLs. -->
[服务端授权的移动服务用户]: mobile-services-javascript-backend-service-side-authorization.md
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[为使用实时连接的 Windows 应用商店应用程序的单一登录]: /develop/mobile/tutorials/single-sign-on-windows-8-dotnet
[移动服务快速入门教程]: /develop/mobile/tutorials/get-started-ios
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-ios
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-ios
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-ios
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-ios

[Azure 的管理门户]: https://manage.windowsazure.com/
测试t
