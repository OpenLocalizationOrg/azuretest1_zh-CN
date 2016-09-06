---
ms.openlocfilehash: af1826f6b2e308447f804110dade6af081013c95
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="添加到应用程序 (iOS) 推送通知 |JavaScript 后端"
    description="了解如何使用 Azure 移动服务于 iOS 应用程序发送推式通知。"
    services="mobile-services,notification-hubs"
    documentationCenter="ios"
    manager="dwrede"
    editor=""
    authors="krisragh"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="krisragh"/>

# 添加到 iOS 应用程序和后端 JavaScript 的推送通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主题演示如何将推式通知添加到[快速启动项目](mobile-services-ios-get-started.md)中，以便您的移动服务发送推式通知每当插入记录。 您必须完成[移动服务入门]第一次。

> [AZURE.NOTE] [IOS 模拟器不支持推式通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html)，因此您必须使用物理的 iOS 设备。 您还需要注册付费[苹果开发商计划的成员资格](https://developer.apple.com/programs/ios/)。

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


## <a id="configure"></a>配置 Azure 发送推式通知

[AZURE.INCLUDE [Configure Push Notifications in Azure Mobile Services](../../includes/mobile-services-apns-configure-push.md)]

## <a id="update-scripts"></a>更新后端脚本发送推式通知

* 在管理门户中，单击**数据**选项卡，然后单击**TodoItem**。 在**TodoItem**，单击**脚本**选项卡，然后选择**插入**。 这将显示插入发生**TodoItem**表中时将调用该函数。

* 用下面的代码替换该插入函数，然后单击**保存**。  这将注册一个新的插入脚本，使用[apns 对象]来插入请求中提供的设备发送推式通知 （插入的文本）。 此脚本发送通知，使您的延迟时间来关闭应用程序以接收推式通知。


```
        function insert(item, user, request) {
            request.execute();
            // Set timeout to delay the notification, to provide time for the
            // app to be closed on the device to demonstrate push notifications
            setTimeout(function() {
                push.apns.send(null, {
                    alert: "Alert: " + item.text,
                    payload: {
                        inAppMessage: "Hey, a new item arrived: '" + item.text + "'"
                    }
                });
            }, 2500);
        }
```

[AZURE.INCLUDE [Add Push Notifications to App](../../includes/add-push-notifications-to-app.md)]

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]


<!-- Anchors. -->


<!-- Images. -->
[5]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step5.png
[6]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step6.png
[7]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step7.png

[9]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step9.png
[10]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step10.png
[17]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/mobile-services-ios-get-started-push/mobile-services-selection.png
[19]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios.png
[20]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/mobile-services-ios-get-started-push/mobile-portal-data-tables.png
[22]: ./media/mobile-services-ios-get-started-push/mobile-insert-script-push2.png
[23]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push1-ios.png
[24]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push2-ios.png
[25]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-02.png
[103]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-03.png
[104]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-04.png
[105]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-05.png
[106]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-06.png
[107]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-07.png
[108]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-08.png

[110]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-10.png
[111]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-11.png
[112]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-12.png
[113]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-13.png
[114]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-14.png
[115]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-15.png
[116]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-16.png
[117]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-17.png

<!-- URLs.   -->
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS 资源调配门户]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[苹果推送通知服务]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[开始使用移动服务]: mobile-services-ios-get-started.md
[有关数据入门]: mobile-services-ios-get-started-data.md
[开始使用身份验证]: mobile-services-ios-get-started-users.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[apns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=272333

[移动服务服务器脚本引用]: http://go.microsoft.com/fwlink/?LinkId=262293

[对经过身份验证的用户发送推式通知]: mobile-services-javascript-backend-ios-push-notifications-app-users.md
[什么是通知集线器？]: ../notification-hubs-overview.md
[将广播的通知发送到订阅服务器]: ../notification-hubs-ios-send-breaking-news.md
[将基于模板的通知发送到订阅服务器]: ../notification-hubs-ios-send-localized-breaking-news.md
[移动服务目标-C 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md

测试
