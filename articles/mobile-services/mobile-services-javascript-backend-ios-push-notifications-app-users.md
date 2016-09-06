---
ms.openlocfilehash: 987bb6d67423cde9c4d96c786d1a18623c4a2acf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="对经过身份验证的用户发送推式通知"
    description="了解如何向特定用户发送推式通知"
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

[AZURE.INCLUDE [mobile-services-javascript-backend-push-notifications-app-users](../../includes/mobile-services-javascript-backend-push-notifications-app-users.md)]

更换`insert`函数使用以下代码，然后单击**保存**。 此插入脚本使用用户 ID 标记发送到 iOS 应用程序的所有注册的推式通知，从登录的用户︰

```
// Get the ID of the logged-in user.
var userId = user.userId;

function insert(item, user, request) {
    request.execute();
    setTimeout(function() {
        push.apns.send(userId, {
            alert: "Alert: " + item.text,
            payload: {
                "Hey, a new item arrived: '" + item.text + "'"
            }
        });
    }, 2500);
}
```

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
[开始使用身份验证]: mobile-services-ios-get-started-users.md
[开始使用推式通知]: mobile-services-javascript-backend-ios-get-started-push.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务.NET 帮助概念参考]: mobile-services-ios-how-to-use-client-library.md

测试
