---
ms.openlocfilehash: 5bcf2e9d9227e527d4431fc47ee577144ebf715d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="注册为使用移动服务的推式通知的当前用户 |Microsoft Azure" 
    description="了解如何申请在 Azure 通知集线器与 iOS 应用程序推送通知登记注册执行通过 Azure 移动服务时。" 
    services="notification-hubs" 
    documentationCenter="ios" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="yuaxu"/>

# 通过使用移动服务注册为推式通知的当前用户

> [AZURE.SELECTOR]
[Windows 商店 C#](notification-hubs-windows-store-mobile-services-register-user-push-notifications.md)
[Io](notification-hubs-ios-mobile-services-register-user-push-notifications.md)

本主题演示如何申请使用 Azure 通知集线器的推送通知注册登记执行的 Azure 移动服务时。 本主题将扩展本教程[采用通知中枢通知用户]。 您必须已经完成所需的步骤在该教程，以创建已验证身份的移动服务。 通知用户方案的详细信息，请参阅[使用通知集线器通知用户]。  

1. 在 Xcode，完成必备教程[开始使用身份验证]，您创建的项目中打开 QSTodoService.h 文件并添加下面的**deviceToken**属性︰

        @property (nonatomic) NSData* deviceToken;

    此属性存储的设备的标记。

2. 在 QSTodoService.m 文件中，添加下面的**getDeviceTokenInHex**方法︰

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

    此方法将该设备标记转换为十六进制的字符串值。

3. 在 QSAppDelegate.m 文件中，对**didFinishLaunchingWithOptions**方法中添加以下代码行︰

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    这样，在您的应用程序中的推式通知。

4.  在 QSAppDelegate.m 文件中，添加下列方法︰

            - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
                [QSTodoService defaultService].deviceToken = deviceToken;
            }

    这将更新的**deviceToken**属性。

    > [AZURE.NOTE] 在这种情况下，不应有任何其他代码在此方法中。 如果您已经对[有关通知集线器入门](/manage/services/notification-hubs/get-started-notification-hubs-ios/"%20target="_blank")教程完成添加了**registerNativeWithDeviceToken**方法的调用，必须注释出或移除该调用。

5.  （可选）在 QSAppDelegate.m 文件中，添加下面的处理程序方法︰

            - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
                NSLog(@"%@", userInfo);
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      [[userInfo objectForKey:@"aps"] valueForKey:@"alert"] delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            }

    当您的应用程序在运行时接收通知，此方法在 UI 中显示一条警报。

6. 在 QSTodoListViewController.m 文件中，添加**registerForNotificationsWithBackEnd**方法︰

            - (void)registerForNotificationsWithBackEnd {
                NSString* json = [NSString  stringWithFormat:@"{\"platform\":\"ios\", \"deviceToken\":\"%@\"}", [self.todoService getDeviceTokenInHex] ];

                [self.todoService.client invokeAPI:@"register_notifications" data:[json dataUsingEncoding:NSUTF8StringEncoding] HTTPMethod:@"POST" parameters:nil headers:nil completion:^(id result, NSHTTPURLResponse *response, NSError *error) {
                    if (error != nil) {
                        NSLog(@"Registration failed: %@", error);
                    } else {
                        // display UIAlert with successful login
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    }
                }];
            }

    此方法构造包含设备标记的 json 负载。 然后，它会在您的移动服务注册通知调用自定义的 API。 此方法创建推式通知的设备标记并发送，以及设备类型、 到创建通知集线器中注册的自定义 API 方法。 在[采用通知中枢通知用户]定义了此自定义的 API。

7.  最后，在**viewDidAppear**方法中，这对新的**registerForNotificationsWithBackEnd**方法后添加用户成功通过验证，如以下示例所示︰

            - (void)viewDidAppear:(BOOL)animated
            {
                MSClient *client = self.todoService.client;

                if (client.currentUser != nil) {
                    return;
                }

                [client loginWithProvider:@"microsoftaccount" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                    [self registerForNotificationsWithBackEnd];
                }];
            }

    > [AZURE.NOTE] 这将确保注册请求每次加载页。 在您的应用程序，您可能只想要定期以确保注册为当前此登记。
    
现在，客户端应用程序已被更新，[通知用户使用通知集线器]返回，并更新移动服务通过使用通知集线器发送通知。

<!-- Anchors. -->

<!-- Images. -->


<!-- URLs. -->
[通知用户使用通知集线器]: /manage/services/notification-hubs/notify-users
[开始使用身份验证]: /develop/mobile/tutorials/get-started-with-users-ios/

[Azure 的管理门户]: https://manage.windowsazure.com/
[开始使用通知集线器]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
 
