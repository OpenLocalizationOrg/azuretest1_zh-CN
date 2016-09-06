---
ms.openlocfilehash: ce25913aed1279d449147690cff086cb5061bca8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="经过身份验证的用户 （.NET 后端） 发送推式通知"
    description="了解如何向特定发送推式通知"
    services="mobile-services,notification-hubs"
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

# 对经过身份验证的用户发送推式通知

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

在本主题中，您将学习如何向身份验证的用户在 Io 上发送推式通知。 在开始学习本教程之前, 完成[开始使用身份验证]和[使用推式通知开始]第一次。

在本教程中，您需要用户首先进行身份验证，使用推式通知的通知中心注册并更新服务器脚本将这些通知发送给只有已通过身份验证的用户。

##<a name="register"></a>更新服务要求注册的身份验证

[AZURE.INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)]

##<a name="update-app"></a>登录注册之前更新应用程序

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-login](../../includes/mobile-services-ios-push-notifications-app-users-login.md)]

##<a name="test"></a>测试应用程序

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-test-app](../../includes/mobile-services-ios-push-notifications-app-users-test-app.md)]

<!-- Anchors. -->
[更新服务登记要求身份验证]: #register
[更新应用程序之前注册登录]: #update-app
[测试应用程序]: #test
[下一步行动]:#next-steps


<!-- URLs. -->
[开始使用身份验证]: mobile-services-dotnet-backend-ios-get-started-users.md
[开始使用推式通知]: mobile-services-dotnet-backend-ios-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: /develop/mobile/how-to-guides/work-with-net-client-library

测试
