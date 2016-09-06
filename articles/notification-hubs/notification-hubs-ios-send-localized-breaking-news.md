---
ms.openlocfilehash: 27cbc235bce7a71509eb338e8e687f6f8eb474db
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通知集线器本地化重大新闻教程 iOS"
    description="了解如何使用 Azure 服务总线通知集线器发送本地化的重大新闻通知 (iOS)。"
    services="notification-hubs"
    documentationCenter="ios"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>
# 使用通知集线器将本地化的新闻发送到 iOS 设备

> [AZURE.SELECTOR]
- [Windows 应用商店 C#](notification-hubs-windows-store-dotnet-send-localized-breaking-news)
- [iOS](notification-hubs-ios-send-localized-breaking-news)


##概述

本主题演示如何使用**模板**功能的 Azure 通知集线器广播已本地化的语言和设备的重大新闻通知。 在本教程中开始[使用通知集线器发送新闻]中创建的 Windows 应用商店应用程序。 完成后，您将能够为您感兴趣的类别注册，指定用来接收通知，并接收该语言只推式通知的所选类别的语言。


有这种情况下的两个部分︰

- iOS 应用程序允许客户端设备指定一种语言，以及订阅不同重大新闻类别;

- 后端上广播通知，使用 Azure 通知集线器**标记**和**模板**feautres。



##先决条件

您必须已完成[使用通知集线器发送新闻]教程和具有的代码可用，因为本教程建立直接在该代码的基础。

您还需要 Visual Studio 2012。



##模板概念

[使用通知集线器发送的新闻]在您生成**标记**用于订阅通知不同的新闻类别的应用程序。
许多应用程序，但是，多个市场为目标，并且需要本地化。 这意味着，通知本身的内容必须本地化和传送到正确的设备组。
本主题中，我们将显示如何使用通知集线器的**模板**功能可以方便地实现本地化的重大新闻通知。

注意︰ 将本地化的通知发送的一种方法是创建多个版本的每个标记。 例如，若要支持英语、 法语和普通话，需要在三个不同的标记世界新闻:"world_en"，"world_fr"，和"world_ch"。 我们不得不将世界新闻的本地化的版本发送到其中每个标记。 本主题中我们使用模板来避免大量标记和发送多个消息的要求。

在高级别上，模板是一种方法来指定特定的设备应如何接收通知。 该模板指定确切有效负载格式通过引用应用程序后端发送的消息的一部分的属性。 在我们的例子中，我们将发送包含所有受支持的语言的区域设置无关消息︰

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

然后，我们将确保设备注册与正确的属性引用的模板。 例如，想要注册为法语新闻 iOS 应用程序将记录如下︰

    {
        aps:{
            alert: "$(News_French)"
        }
    }

模板是非常强大的功能，您可以了解更多有关我们[通知集线器指南]文章中。 模板表达式语言引用位于我们[How To︰ 服务总线通知集线器 (iOS 应用程序)]。

##应用程序用户界面

现在，我们将修改[使用通知集线器发送的新闻]将发送使用模板的本地化的新闻的主题中创建的新新闻应用程序。


在您 MainStoryboard_iPhone.storyboard 添加分段控件与我们支持三种语言︰ 英语、 法语和普通话。

![][13]

请确保为您 ViewController.h 中添加 IBOutlet，如下所示︰

![][14]

##建立 iOS 应用程序

为了适应您的客户端应用程序进行本地化的消息，您必须替换模板注册*本机*登记 （即您的登记指定模板）。

1. 您 Notification.h 中将*retrieveLocale*方法中，添加和修改存储订阅的方法，如下所示︰

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    在您的 Notification.m，通过添加区域设置参数并将其存储在用户的默认值修改*storeCategoriesAndSubscribe*方法︰

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    然后修改*订阅*方法可以包括区域设置︰

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"newsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    请注意，我们现在使用的方式方法*registerTemplateWithDeviceToken*中，而不*registerNativeWithDeviceToken*。 我们注册模板时我们必须提供 json 模板和模板的名称 （在我们的应用程序可能需要注册不同的模板）。 我们想要确保接收这些新闻 notifciations，则请确保作为标记，注册您的类别。

    最后，添加一个方法以检索来自用户默认设置的区域设置︰

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

3. 现在，我们修改我们的通知类，我们必须确保我们的 ViewController 使新 UISegmentControl 的使用。 *ViewDidLoad*方法，以确保以显示当前所选的区域设置中添加以下行︰

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    然后，在您*订阅*的方法，变为*storeCategoriesAndSubscribe*调用以下︰

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

4. 最后，您必须更新您 AppDelegate.m 中的*didRegisterForRemoteNotificationsWithDeviceToken*方法，以便您的应用程序启动时可以正确地刷新您的注册。 更改*订阅*的通知使用以下方法调用︰

        NSSet* categories = [notifications retrieveCategories];
        int locale = [notifications retrieveLocale];
        [notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##从后端发送本地化的通知

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]


## 下一步行动

使用模板的详细信息，请参阅︰

- [通知用户采用通知中枢︰ ASP.NET]
- [通知用户采用通知中枢︰ 移动服务]
- [通知集线器指南]

模板表达式语言引用是在[帮助为 iOS 的集线器通知]。






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[如何︰ 服务总线通知集线器 (iOS 应用程序)]: http://msdn.microsoft.com/library/jj927168.aspx
[使用通知集线器发送的新闻]: /manage/services/notification-hubs/breaking-news-ios
[移动服务]: /develop/mobile/tutorials/get-started
[通知用户采用通知中枢︰ ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[通知用户采用通知中枢︰ 移动服务]: /manage/services/notification-hubs/notify-users
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[开始使用移动服务]: /develop/mobile/tutorials/get-started/#create-new-service
[有关数据入门]: /develop/mobile/tutorials/get-started-with-data-ios
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-ios
[开始使用推式通知]: /develop/mobile/tutorials/get-started-with-push-ios
[推式通知给应用程序用户]: /develop/mobile/tutorials/push-notifications-to-users-ios
[授权用户使用的脚本]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript 和 HTML]: ../get-started-with-push-js.md

[Azure 的管理门户]: https://manage.windowsazure.com/
[移动服务的 Windows 开发者预览版注册步骤]: ../mobile-services-windows-developer-preview-registration.md
[wns 对象]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[如何为 iOS 的集线器通知]: http://msdn.microsoft.com/library/jj927168.aspx
