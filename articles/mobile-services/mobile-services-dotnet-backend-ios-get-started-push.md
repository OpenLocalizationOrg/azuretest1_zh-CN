---
ms.openlocfilehash: 7803910efa00861e68d6d8d90005a4b427cdd247
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="添加到应用程序 (iOS) 推送通知 |.NET 后端"
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


# 添加到 iOS 应用程序和.NET 后端推送通知

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

本主题演示如何将推式通知添加到[快速启动项目](mobile-services-dotnet-backend-ios-get-started.md)中，以便您的移动服务发送推式通知每当插入记录。 您必须完成[移动服务入门]第一次。

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a id="configure"></a>配置 Azure 发送推式通知

[AZURE.INCLUDE [Configure Push Notifications in Azure Mobile Services](../../includes/mobile-services-apns-configure-push.md)]

##<a id="update-server"></a>更新后端代码发送推式通知

* 打开 Visual Studio 项目 >**控制器**文件夹 > **TodoItemController.cs** > 方法`PostTodoItem`。 用以下内容替换该方法。 插入一个 todo 项时，此代码发送推式通知用项文本。 如果没有出现错误，该代码将添加通过门户网站的日志部分可以查看错误日志项。


```
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            ApplePushMessage message = new ApplePushMessage(item.Text, System.TimeSpan.FromHours(1));

            try
            {
                var result = await Services.Push.SendAsync(message);
                Services.Log.Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                Services.Log.Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }
```

##<a name="publish-the-service"></a>发布到 Azure 的移动服务

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

[AZURE.INCLUDE [Add Push Notifications to App](../../includes/add-push-notifications-to-app.md)]

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

<!-- Anchors.  -->
[生成证书签名请求]: #certificates
[注册您的应用程序并启用推式通知]: #register
[创建应用程序设置的配置文件]: #profile
[配置移动服务]: #configure
[更新脚本发送推式通知]: #update-scripts
[向应用程序添加推式通知]: #add-push
[插入数据接收通知]: #test
[测试针对手机信息发布服务应用程序]: #test-app
[下一步行动]:#next-steps
[下载本地服务]: #download-the-service-locally
[测试移动服务]: #test-the-service
[发布到 Azure 的移动服务]: #publish-mobile-service

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

<!-- URLs. -->
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS 资源调配门户]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[移动服务 iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[苹果推送通知服务]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[开始使用移动服务]: mobile-services-dotnet-backend-ios-get-started.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[apns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=272333

[有关数据入门]: mobile-services-dotnet-backend-ios-get-started-data.md
[开始使用身份验证]: mobile-services-dotnet-backend-ios-get-started-users.md
[对经过身份验证的用户发送推式通知]: mobile-services-dotnet-backend-ios-push-notifications-app-users.md
[移动服务目标-C 帮助概念参考]: mobile-services-windows-dotnet-how-to-use-client-library.md
[什么是通知集线器？]: ../notification-hubs-overview.md
[将广播的通知发送到订阅服务器]: ../notification-hubs-ios-send-breaking-news.md
[将基于模板的通知发送到订阅服务器]: ../notification-hubs-ios-send-localized-breaking-news.md

测试
