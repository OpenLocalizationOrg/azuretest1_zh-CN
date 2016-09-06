---
ms.openlocfilehash: ca5d3624d6b5c5512e447d2ca94bb781291a1592
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="通知集线器重大新闻教程-iOS"
    description="了解如何使用 Azure 服务总线通知集线器将重大新闻通知发送到 iOS 设备。"
    services="notification-hubs"
    documentationCenter="ios"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

# 使用通知集线器发送的新闻

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##概述

本主题演示如何使用 Azure 通知集线器广播通知 iOS 应用程序的重大新闻。 完成后，将能够注册的重大新闻类别感兴趣，并在收到这些类别只推式通知。 这种情况下是许多应用程序的常见模式通知需要发送到以前已声明它们，例如 RSS 阅读器、 音乐风扇等应用程序感兴趣的用户组。

通过包含一个或多个_标记_，通知中心中创建登记时启用了广播的方案。 当通知被发送到一个标记时，所有已注册标记的设备将接收到通告。 标记是简单的字符串，因为它们不需要预先设置。 有关标记的详细信息，请参阅[通知集线器指南]。


##先决条件

本主题基于[入门通知集线器][入门]在创建应用程序。 之前开始学习本教程，您必须已完成[通知集线器开始][入门的]。

##向应用程序添加类别选择

第一步是将 UI 元素添加到您现有的演示图板，使用户能够选择要注册的类别。 由用户选择的类别存储在该设备中。 当应用程序启动时，设备注册中创建与所选的类别通知网络集线器作为标记。

1. 在您的 MainStoryboard_iPhone.storyboard 对象库中添加以下组件︰
    + 带有"重大新闻"文本的标签
    + "世界"、"政治"、"商务"、"技术"、"科学"、"运动"与类别文本标签
    + 六个开关，每个类别，其中一个设置每个开关**状态**，默认情况下处于**关闭状态**。
    + 一个按钮标记为"订阅"

    演示图板应该如下所示︰

    ![][3]

2. 在助手编辑器中创建的所有开关插座和人称它们为"WorldSwitch""PoliticsSwitch"、"BusinessSwitch"、"TechnologySwitch"、"ScienceSwitch"、"SportsSwitch"


3. 创建一个称为"订阅"按钮的动作。 您 ViewController.h 应包含下列信息︰

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. 创建一个名为的新**Cocoa 接触类** `Notifications`。 在文件 Notifications.h 的接口部分复制下面的代码︰

        @property NSData* deviceToken;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. 将下面的导入指令添加到 Notifications.m 中︰

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. 将下面的代码复制文件 Notifications.m 的实现部分，并替换`<hub name>`和`<connection string with listen access>`您通知中心名称与之前获得的*DefaultListenSharedAccessSignature*的连接字符串的占位符。

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
            SBNotificationHub* hub = [[SBNotificationHub alloc]
                                        initWithConnectionString:@"<connection string with listen access>"
                                        notificationHubPath:@"<hub name>"];

            [hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];
        }



    此类使用本地存储区来存储和检索此设备将接收的消息的类别。 此外，它包含一个方法来注册这些类别。

    > [AZURE.NOTE] 分布式客户端应用程序使用的凭据不是通常的安全，因为您只应该与您的客户端应用程序分发听访问的键。 倾听 access 将启用不能修改您的应用程序的通知，但现有登记注册并不能发送通知。 完全访问键用于发送通知和更改现有登记的受保护的后端服务。

7. 在 AppDelegate.h 文件中，添加导入语句为 Notifications.h 并添加通知类的实例的属性︰

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;

    这在 AppDelegate 中创建通知类的单一实例。

8. 在 AppDelegate.m 在**didFinishLaunchingWithOptions**方法中，添加代码以初始化方法的开头的通知实例︰

        self.notifications = [[Notifications alloc] init];

    初始化通知单一实例。


9. 在 AppDelegate.m 在**didRegisterForRemoteNotificationsWithDeviceToken**方法中，将方法中的代码替换以下代码以将设备标记传递给通知类。 通知类将执行注册与类别的通知。 如果用户更改类别选择，我们称之为`subscribeWithCategories`在**订阅**按钮来更新这些响应的方法。

    > [AZURE.NOTE] 因为设备标记分配由苹果推送通知服务 (APNS) 可以在任何时间的机会，您应注册通知经常以避免通知失败。 本示例注册通知每个应用程序启动的时间。 为经常运行的应用程序，不止一次，每天可以可能跳过注册以节省带宽，如果以前的注册起已过了不少于一天。

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    请注意，此时应该有没有其他代码在**didRegisterForRemoteNotificationsWithDeviceToken**方法中。

10. 下列方法应该已经是出现在 AppDelegate.m 的完成[通知集线器入门][的入门]教程。  如果没有，请添加它们。

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    此方法会通过显示简单的**UIAlert**运行应用程序时接收到通知。

11. 在 ViewController.m，AppDelegate.h 为添加导入语句，将以下代码复制到**订阅**XCode 生成方法。 此代码将更新通知注册使用用户已选择在用户界面中的新类别标记。

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [self MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    此方法创建类别**NSMutableArray**和**通知**类用于存储本地存储区中的列表并通知中心注册相应的标记。 更改类别，注册新类别重新创建。

12. 在 ViewController.m 中，添加下面的代码在**viewDidLoad**方法中设置的用户界面，基于以前保存的类别。


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(BreakingNewsAppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



应用程序现在可以注册通知中心，每次启动应用程序时使用的设备本地存储区中存储一组类别。  用户可以更改的类别在运行时选择并单击要更新的设备注册的**订阅**方法。 接下来，您将更新该应用程序在应用程序自身中直接发送重大新闻通知。


##发送通知

通常由后端服务将发送通知，但是对于本教程我们会更新我们发送通知代码，以便我们可以直接从应用程序发送重大新闻通知。 为此我们将更新`SendNotificationRESTAPI`[入门通知集线器][的入门]教程中，我们定义的方法。


1. 在 ViewController.m 更新`SendNotificationRESTAPI`方法作为后面，这样只要花平台通知服务`pns`参数，并且参数类别的标记。

        - (void)SendNotificationRESTAPI:(NSString*)pns Category:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNS notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Windows Notification format of the notification message
            if ([pns isEqualToString:@"wns"])
            {
                json = [NSString stringWithFormat:@"<?xml version=\"1.0\" encoding=\"utf-8\"?>"
                                                   "<toast>"
                                                   "<visual><binding template=\"ToastText01\">"
                                                   "<text id=\"1\">Breaking %@ News : %@</text>"
                                                   "</binding>"
                                                   "</visual>"
                                                   "</toast>",
                        categoryTag, self.notificationMessage.text];

                // Signify windows notification format
                [request setValue:@"windows" forHTTPHeaderField:@"ServiceBusNotification-Format"];

                // XML Content-Type
                [request setValue:@"application/xml" forHTTPHeaderField:@"Content-Type"];

                // Set X-WNS-TYPE header
                [request setValue:@"wns/toast" forHTTPHeaderField:@"X-WNS-Type"];
            }

            // Google Cloud Messaging Notification format of the notification message
            if ([pns isEqualToString:@"gcm"])
            {
                json = [NSString stringWithFormat:@"{\"data\":{\"message\":\"Breaking %@ News : %@\"}}",
                        categoryTag, self.notificationMessage.text];
                // Signify gcm notification format
                [request setValue:@"gcm" forHTTPHeaderField:@"ServiceBusNotification-Format"];

                // JSON Content-Type
                [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
            }

            // Apple Notification format of the notification message
            if ([pns isEqualToString:@"apns"])
            {
                json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"Breaking %@ News : %@\"}}",
                        categoryTag, self.notificationMessage.text];
                // Signify apple notification format
                [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

                // JSON Content-Type
                [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
            }

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
                       {
                       NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                           if (error || httpResponse.statusCode != 200)
                           {
                               NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                           }
                           if (data != NULL)
                           {
                               //xmlParser = [[NSXMLParser alloc] initWithData:data];
                               //[xmlParser setDelegate:self];
                               //[xmlParser parse];
                           }
                       }];
            [dataTask resume];
        }



2. ViewController.m 在更新**发送通知**操作，如下面的代码所示。 这样它将发送单独使用每个标记的通知，并将发送到多个平台。



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:@"wns" Category:category];
                [self SendNotificationRESTAPI:@"gcm" Category:category];
                [self SendNotificationRESTAPI:@"apns" Category:category];
            }
        }



3. 重新生成项目，请确保您有没有生成错误。


##运行应用程序并生成通知

1. 按运行按钮以生成项目并启动应用程序。 选择一些重大新闻选项，订阅，然后按**订阅**按钮。 您会看到一个对话框，指示已订阅通知。

    ![][1]

    当您选择**订阅**时，应用程序转换标记所选的类别及其从通知中心请求新的设备注册为选定的标记。

2. 输入消息的新闻作为发送，然后按**发送通知**按钮

    ![][2]


3. 重要资讯订阅每个设备将接收重大新闻通知只发送。



## 下一步行动

在本教程中我们学习了如何按类别广播的新闻。 完成下面的教程，突出显示其他高级的通知集线器方案之一，请考虑︰

+ **[使用通知集线器以本地化的新闻广播]**

    了解如何展开重大新闻应用程序启用本地化的发送通知。

+ **[通知用户使用通知集线器]**

    了解如何对特定的已验证身份用户推式通知。 这是一个不错的解决方案，只能向特定用户发送通知。



<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[如何︰ 服务总线通知集线器 (iOS 应用程序)]: http://msdn.microsoft.com/library/jj927168.aspx
[使用通知集线器以本地化的新闻广播]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[移动服务]: /develop/mobile/tutorials/get-started
[通知用户使用通知集线器]: notification-hubs-aspnet-backend-ios-notify-users.md
[Azure 的管理门户]: https://manage.windowsazure.com/
[通知集线器指南]: http://msdn.microsoft.com/library/dn530749.aspx
[如何为 iOS 的集线器通知]: http://msdn.microsoft.com/library/jj927168.aspx
[入门-]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
