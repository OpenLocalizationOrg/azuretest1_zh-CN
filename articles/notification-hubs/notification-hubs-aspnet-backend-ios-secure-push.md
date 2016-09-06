---
ms.openlocfilehash: dabb77e98a92a8f24e015e4231046d0270dd440e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="安全的 azure 通知集线器推"
    description="了解如何从 Azure 发送到 iOS 应用程序的安全的推式通知。 在 OBJECTIVE-C 和 C# 中编写的代码样本。"
    documentationCenter="ios"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="wesmc"/>

#安全的 azure 通知集线器推

> [AZURE.SELECTOR]
- [Windows 世界](notification-hubs-aspnet-backend-windows-dotnet-secure-push.md)
- [iOS](notification-hubs-aspnet-backend-ios-secure-push.md)
- [Android](notification-hubs-aspnet-backend-android-secure-push.md)


##概述

在 Microsoft Azure 支持推送通知使您可以访问一个易于使用、 多平台和向外扩展的推的基础结构，极大地简化了使用者和企业应用程序的移动平台的推式通知的实现。

法规或安全约束，有时应用程序可能需要在无法通过标准推式通知基础架构传输通知中包括的内容。 本教程介绍了如何通过发送敏感信息通过安全、 经身份验证的客户端设备和应用程序的后端之间连接来获得相同的体验。

在较高级别流程如下所示︰

1. 应用程序后端︰
    - 在后端数据库中的存储安全有效负载。
    - 此通知的 ID 将发送到设备 （安全信息不会发送）。
2. 该应用程序在设备上，在接收到此通知时︰
    - 设备联系人后端请求安全有效负载。
    - 应用程序可以显示为通知设备上的负载。

值得注意的是，上述流中 （并在本教程中），我们假定用户登录后，设备将在本地存储中中, 存储身份验证令牌。 这可以保证完全无缝的体验，因为该设备可以检索通知使用此令牌的安全有效负载。 如果您的应用程序不会存储在设备上，身份验证令牌，或这些标记可以到期，设备应用程序中的，在接收到此通知时应显示一般通知提示用户启动该应用程序。 应用程序对用户进行身份验证，然后显示通知负载。

本安全推教程演示如何安全地发送推式通知。 本教程基于**通知用户**教程，所以应完成的步骤，该教程中第一次。

> [AZURE.NOTE] 本教程假设您已经创建并配置通知中心[入门通知集线器 (iOS)](notification-hubs-ios-get-started.md)中所述。

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## 修改 iOS 项目

现在，您可以修改您的应用程序后端发送只是*id*的通知，您必须更改您的 iOS 应用程序来处理该通知以及回拨您后端来检索要显示安全消息。

为实现这一目标，我们必须编写逻辑来检索从后端应用程序的安全的内容。

1. 在**AppDelegate.m**，请确保从后端发送静默通知以便处理通知 id 的应用程序寄存器。 在 didFinishLaunchingWithOptions 中添加**UIRemoteNotificationTypeNewsstandContentAvailability**选项︰

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. 在**AppDelegate.m**您将添加实现部分最下面的声明带有︰

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. 然后将下面的代码替换该占位符添加在实现部分`{back-end endpoint}`与以前获得您后端的终结点︰

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. 现在我们必须处理传入通知并使用上面的方法来检索要显示的内容。 首先，我们必须启用您的 iOS 应用程序来接收推式通知时在后台运行。 **XCode**，在选择在左边的面板中，您的应用程序项目，然后单击主应用程序目标从中央窗格中**目标**部分中。

5. 然后单击您的**功能**选项卡顶部的中央窗格中，检查**远程通知**复选框。

    ![][IOS1]


6. 在**AppDelegate.m**中添加下面的方法来处理推式通知︰

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    请注意，最好由后端处理缺少身份验证标头属性或拒绝的情况。 这种情况下的特定处理大部分依赖于您的目标用户体验。 一个选项是显示一个通知，通知一般提示用户进行身份验证以检索实际的通知。

## 运行应用程序

要运行该应用程序，请执行以下操作︰

1. XCode，（推送通知不起在模拟器中） 的物理的 iOS 设备上运行应用程序。

2. 在 iOS 应用程序用户界面中输入用户名和密码。 这些可以是任何字符串，但它们必须是相同的值。

3. 在 iOS 应用程序用户界面中，单击**登录**。 然后单击**发送推**。 您应该看到在通知中心中显示的安全通知。

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
