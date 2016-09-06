---
ms.openlocfilehash: ac724cb4d545c6ca535e56505aeffeafa138f241
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="开始使用 Azure 通知集线器 |Microsoft Azure"
    description="在本教程中，您将学习如何使用 Azure 通知集线器到 iOS 应用程序推送通知。"
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
    ms.topic="hero-article" 
    ms.date="09/02/2015"
    ms.author="wesmc"/>

# 开始使用通知集线器

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概述

本教程展示如何使用 Azure 通知集线器到 iOS 应用程序发送推式通知。 您将创建空的 iOS 应用程序通过使用苹果推送通知服务 (APNs) 接收推式通知。 完成后，您将能够使用通知中心广播运行您的应用程序的所有设备的推式通知。

本教程演示中使用通知集线器的简单广播的方案。

##先决条件

本教程要求如下︰

+ [移动服务 iOS SDK]
+ [6 Xcode][安装 Xcode]
+ IOS 8 （或更高版本） 能力的设备
+ iOS 开发商计划成员资格

   > [AZURE.NOTE] 推式通知的配置要求，因此必须部署，而不是 iOS 模拟器测试在 iOS 功能的设备 （iPhone 或 iPad） 推式通知。

学完本教程是为 iOS 应用程序的所有其他通知集线器教程的先决条件。

> [AZURE.NOTE] 若要完成本教程，您必须为活动 Azure 帐户。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 有关详细信息，请参阅[Azure 免费试用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started)。

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##配置通知中心

本节引导您完成创建和配置新的通知中心通过创建推式证书。 如果您想要使用您已创建一个通知集线器，则可以跳过步骤 2 – 5。


1. 在钥匙链访问中，右键单击您创建的**证书**类别中新推证书。 单击**导出**来命名该文件，选择**.p12**格式，，然后单击**保存**。

    ![][1]

    请记下的文件的名称和导出的证书的位置。

    >[AZURE.NOTE] 本教程中创建的 QuickStart.p12 文件。 您的文件的名称和位置可能不同。

2. 登录到[Azure 的门户网站]，然后单击**+ 新建**在屏幕的底部。

3. 单击**服务应用程序**，单击**服务总线**，**通知集线器**中，请单击，然后单击**快速创建**。

    ![][2]

4. 通知中心为键入一个名称，选择您需要的区域，然后单击**创建新的通知中心**。

    ![][3]

5. 单击您刚创建 (通常为***通知中心名称*-ns**) 要打开的仪表板的名称空间。

    ![][4]

6. 单击**通知集线器**选项卡的顶部，然后单击您刚刚创建的通知中心。

    ![][5]

7. 单击在顶部，**配置**选项卡，然后单击上载证书指纹的苹果通知设置中的**上载**按钮。 然后选择**.p12**导出证书的更早版本，并且该证书的密码。 请确保选择您是否要使用**生产**（如果您想要购买您的应用程序存储区中的用户发送推式通知） 或**沙盒**（在开发过程中） 推送服务。

    ![][6]

8. 单击顶部的**仪表板**选项卡，然后单击**视图连接字符串**。 请注意两个连接字符串。

    ![][7]

通知中心现已配置为使用 APNs，并且必须发送通知并注册您的应用程序的连接字符串。

##将您的应用程序连接到通知中心

1. 在 Xcode，创建一个新的 iOS 项目并选择**单个视图应用程序**模板。

    ![][8]

2. 当设置新项目的选项，请确保使用相同的**产品名称**和**组织标识符**时以前苹果开发人员门户上设置的绑定 ID 使用。

    ![][11]

3. 下**的目标**，请单击项目名称，单击**生成设置**选项卡展开**代码签名标识**，然后**调试**，请在下设置您的代码签名标识。 切换成不同**级别**从**基本****所有**，并将**提供配置文件**设置为您先前创建的设置配置文件。

    如果您看不到在 Xcode 创建新设置配置文件，请尝试刷新您签名标识的配置文件。 菜单栏上单击**Xcode** **首选项**单击**帐户**选项卡，单击**查看详细信息**按钮，单击签名的标识，和，然后单击右下角的刷新按钮。

    ![][9]

4. 1.2.4 版本[移动服务 iOS SDK]下载并解压缩文件。 Xcode 中, 右击项目并单击 Xcode 项目中添加**WindowsAzureMessaging.framework**文件夹**将文件添加到**选项。 **复制项目，如果需要**请选择，然后单击**添加**。

    ![][10]

5. 打开您的 AppDelegate.h 文件中添加下面的导入指令︰

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. 在 AppDelegate.m 文件中，添加以下代码以`didFinishLaunchingWithOptions`方法，根据您的版本的 iOS。 此代码将向 APNs 注册您的设备句柄︰

    对于 iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    8 iOS 版本︰

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


7. 在同一文件中，添加下列方法和*中心名称*和前面提到的*DefaultListenSharedAccessSignature*替换字符串文本占位符。 此代码使设备标记通知集线器，以便通知中心可以发送通知︰

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<Enter your listen connection string>"
                                        notificationHubPath:@"<Enter your hub name>"];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


8. 在同一文件中，添加下面的方法来显示**UIAlert** ，如果应用程序处于活动状态时接收到通知︰


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

8. 生成并运行该应用程序在您的设备，以验证不存在任何故障。

## 发送通知


您可以测试您的应用程序中通过在 Azure 门户中的调试选项卡上通知中心，通过发送通知接收通知，如下面的屏幕中所示。
![][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

![][31]

1. 在 Xcode，打开 Main.storyboard 和从要允许用户在应用程序中发送推式通知的对象库中添加以下 UI 组件︰

    - 无标签文本标签。 它将用于在发送通知中报告错误。 **Lines**属性应设置为**0** ，这样，它将自动地大小限制为右和左的页边距和视图的顶部。
    - 带有**占位符**文本的文本字段设置为**输入通知消息**。 约束字段标签的下方，如下所示。 作为出口代理设置视图控制器。
    - **发送通知**的标题为一个按钮约束正下方的文本字段和水平中心。

    该视图应如下所示︰

    ![][32]


2. 打开 ViewController.h 文件，然后添加以下`#import`和`#define`语句。 您实际*DefaultFullSharedAccessSignature*连接字符串和*中心名称*替换占位符字符串文本。


        #import <CommonCrypto/CommonHMAC.h>

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
        #define HUBNAME @"<Enter the name of your hub>"


3. 添加的标签和文本字段的插座连接您的视图，并更新您`interface`定义，以支持`UITextFieldDelegate`， `NSXMLParserDelegate`。 添加如下所示来帮助支持 REST API 调用和响应分析的三个属性声明。

    您的 ViewController.h 文件应如下所示︰

        #import <UIKit/UIKit.h>
        #import <CommonCrypto/CommonHMAC.h>

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
        #define HUBNAME @"<Enter the name of your hub>"

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end


4. 打开 ViewController.m 并添加下面的代码来分析您的*DefaultFullSharedAccessSignature*连接字符串。 如在[REST API 参考](http://msdn.microsoft.com/library/azure/dn495627.aspx)中所述，此分析的信息将用于生成的**授权**请求标头的 SaS 标记。

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

5. 在 ViewController.m，更新`viewDidLoad`方法来分析连接字符串视图加载时。 此外将添加如下所示的实用程序方法。  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





6. 在 ViewController.m 中，添加以下代码以生成 SaS 授权令牌，将获得**授权**，在标题中提到[REST API 参考](http://msdn.microsoft.com/library/azure/dn495627.aspx)。

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sr=%@&sig=%@&se=%qu&skn=%@",
                    targetUri, signature, expires, HubSasKeyName];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


7. Ctrl + 从**发送通知**按钮拖到 ViewController.m 添加执行 REST API 的**向下触控**事件的操作通过使用下面的代码的调用。

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

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
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


8. 在 ViewController.m 中，添加下面的委托方法，以支持关闭键盘的文本字段。 按住 Ctrl 并从文本字段拖到视图控制器图标界面设计器设置视图控制器与插座委托中。

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


9. 在 ViewController.m 中，添加下面的委托方法，以支持分析响应，方法使用`NSXMLParser`。

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



10. 生成项目并验证没有任何错误。



苹果[本地推送通知编程指南]中，您可以找到所有可能通知有效载荷。



##测试您的应用程序

要测试 iOS 上的推式通知，必须将应用程序部署到设备。 您无法通过使用 iOS 模拟器发送苹果推式通知。

1. 运行应用程序并确认注册成功，然后按**确定**。

    ![][33]

2. 触摸输入一条通知消息的文本字段内。 键盘或发送通知邮件视图中的**发送通知**按钮上，然后按**发送**按钮。

    ![][34]

3. 通知被发送到所有已注册要接收通知的设备。

    ![][35]

如果您有任何问题或建议为所有读者提高本教程，请留言我们 Disqus 下面。


##下一步行动

在此简单示例中，您所有的 iOS 设备到广播通知。 为了瞄准特定的用户，是指本教程[使用通知集线器到推式通知给用户]。 如果您想要段由有兴趣的集团用户，您可以阅读[使用通知集线器发送的新闻]。 了解更多关于如何使用[通知集线器指南]中的通知集线器。



<!-- Images. -->

[1]: ./media/notification-hubs-ios-get-started/notification-hubs-export-cert-p12.png
[2]: ./media/notification-hubs-ios-get-started/notification-hubs-create-from-portal.png
[3]: ./media/notification-hubs-ios-get-started/notification-hubs-create-from-portal2.png
[4]: ./media/notification-hubs-ios-get-started/notification-hubs-select-from-portal.png
[5]: ./media/notification-hubs-ios-get-started/notification-hubs-select-from-portal2.png
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-connection-strings.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-debug-hub-ios.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[移动服务 iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[提交应用程序页]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[对于 Windows live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[开始使用移动服务]: /develop/mobile/tutorials/get-started-ios
[Azure 门户]: https://manage.windowsazure.com/
[通知集线器指南]: http://msdn.microsoft.com/library/jj927170.aspx
[安装 Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS 资源调配门户]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[开始使用推式通知中移动服务]: ../mobile-services-javascript-backend-ios-get-started-push.md
[使用推式通知给用户通知集线器]: notification-hubs-aspnet-backend-ios-notify-users.md
[使用通知集线器发送的新闻]: notification-hubs-ios-send-breaking-news.md

[本地和推送通知编程指南]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
