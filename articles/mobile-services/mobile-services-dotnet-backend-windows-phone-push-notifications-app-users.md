---
ms.openlocfilehash: c60b7e00102349a9fbeb08a7777a12b95e631103
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对经过身份验证的用户发送推式通知"
    description="了解如何向特定发送推式通知"
    services="mobile-services,notification-hubs"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags 
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="glenga"/>

# 对经过身份验证的用户发送推式通知

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

本主题演示如何向任何已注册的设备上经过身份验证的用户发送推式通知。 与前面的[外推式通知到您的应用程序]教程，本教程更改您的移动服务，需要对用户进行身份验证，客户端可以使用推式通知的通知中心注册之前。 注册也修改添加标记基于所分配的用户 id。 最后，更新的服务器代码，以将通知发送到所有注册到身份验证的用户而不是只。

本教程支持 Windows Phone 8.0 和 Windows Phone 8.1 Silverlight 的应用程序。 Windows Phone 8.1 存储应用程序，请参阅[Windows 应用商店版本的主题](mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md)。

##先决条件

在开始本教程之前，您必须已经完成这些移动服务指南︰

+ [添加到您的应用程序的身份验证]<br/>应做事项列表示例应用程序中添加登录要求。

+ [将推式通知添加到您的应用程序]<br/>通过使用通知集线器配置任务列表示例应用程序用于推式通知。

完成这两个教程后，可以防止注册为从您的移动服务的推式通知进行未经身份验证的用户。

##<a name="register"></a>更新服务要求身份验证注册

[AZURE.INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)]

##<a name="update-app"></a>更新应用程序之前注册登录

[AZURE.INCLUDE [mobile-services-windows-phone-push-notifications-app-users](../../includes/mobile-services-windows-phone-push-notifications-app-users.md)]


##<a name="test"></a>测试应用程序

[AZURE.INCLUDE [mobile-services-windows-test-push-users](../../includes/mobile-services-windows-test-push-users.md)]

<!-- Anchors. -->
[更新服务登记要求身份验证]: #register
[更新应用程序之前注册登录]: #update-app
[测试应用程序]: #test
[下一步行动]:#next-steps


<!-- URLs. -->
[添加到您的应用程序的身份验证]: mobile-services-dotnet-backend-windows-phone-get-started-users.md
[将推式通知添加到您的应用程序]: mobile-services-dotnet-backend-windows-phone-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/

测试
