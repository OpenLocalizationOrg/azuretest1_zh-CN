---
ms.openlocfilehash: 0836ec0ef603d1f0f09a71eb6a1aa2fd366fa790
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="如何在 iOS 上使用服务 API"
    description="最新的 iOS SDK-如何在 iOS 上使用服务 API"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="piyushjo" />


#如何在 iOS 上使用服务 API

本文档是对文档的某个加载项如何在 iOS 集成服务︰ 它提供了有关如何使用服务 API 报告您的应用程序统计信息的深度详细信息中。

请记住，如果您只需要合作报告应用程序的会话、 活动、 系统崩溃和技术信息，则最简单的方法是使所有您的自定义`UIViewController`对象继承自的对应`EngagementViewController`类。

如果您想要做更多，例如，如果您需要报表应用程序特定事件、 错误和作业，或如果您要以不同的方式报告应用程序的活动，不是在一个实现`EngagementViewController`类，则需要使用服务 API。

服务 API 提供的`EngagementAgent`类。 此类的实例可以通过调用来检索`[EngagementAgent shared]`静态方法 (请注意，`EngagementAgent`返回的对象是一个单一实例)。

所有 API 调用前,`EngagementAgent`必须通过调用方法来初始化对象 `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##交往概念

下列部件优化常见的[移动合作概念](mobile-engagement-concepts.md)，为 iOS 平台。

### `Session`  和  `Activity`

*活动*是通常与一个屏幕的应用程序，也就是说在*活动*启动时屏幕显示和屏幕关闭时停止︰ 使用集成服务 SDK 时，这种情况`EngagementViewController`类。

但*活动*还能受手动使用服务 API。 这样就可以拆分给的屏幕中若干子部分，以获取更多详细信息 （例如对已知频率和多长时间在此屏幕内部使用对话） 此屏幕的用法。

##报告的活动

### 用户启动一个新的活动

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

您需要调用`startActivity()`每次用户活动的更改。 第一次调用此函数启动一个新的用户会话。

### 用户结束他当前活动

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] 您**从不**应该自行调用此函数，除非在要拆分到多个会话的应用程序的一种用途︰ 对此函数的调用将结束当前会话立即，因此，对的后续调用`startActivity()`将启动新的会话。 当应用程序关闭时，SDK 自动调用此函数。

##报告事件

### 会话事件

会话事件通常用来报告他的会话期间由用户执行的操作。

**而无需额外数据的示例︰**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**包含额外数据的示例︰**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### 独立事件

会话事件与独立事件可以使用会话的上下文之外。

**示例：**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##报告错误

### 会话错误

会话错误通常用来报告他的会话过程中影响用户的错误。

**示例：**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### 独立的错误

与会话错误可以会话的上下文之外使用独立的错误。

**示例：**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##报告作业

**示例：**

假设您要报告您的登录过程的持续时间︰

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### 在作业过程中报告错误

可以与正在运行的作业，而不是正相关的当前用户会话相关的错误。

**示例：**

假设您想在登录过程中报告错误︰

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### 在作业过程中的事件

可以与正在运行的作业，而不是正相关的当前用户会话相关的事件。

**示例：**

假设我们有一个社交网络，并且我们使用报告作业的总时间，在此期间用户连接到服务器。 用户可以接收来自他朋友的邮件，这是一个作业事件。

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##额外的参数

可以将任意数据附加到事件、 错误、 活动和作业。

此数据可能结构化，它会使用 iOS 的 NSDictionary 类。

请注意，额外内容可以包含`arrays(NSArray, NSMutableArray)`， `numbers(NSNumber class)`， `strings(NSString, NSMutableString)`， `urls(NSURL)`，`data(NSData, NSMutableData)`或其他`NSDictionary`实例。

> [AZURE.NOTE] 额外的参数是在 JSON 序列化。 如果您想要通过不同的对象，比前面的描述，必须在类中实现下面的方法︰
>
             -(NSString*)JSONRepresentation;
>
> 此方法应返回对象的 JSON 的表示。

### 示例

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### 限制

#### 密钥

在每个键`NSDictionary`必须匹配以下正则表达式︰

`^[a-zA-Z][a-zA-Z_0-9]*`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

显示额外内容被限制为**1024年**个字符，每个调用 （一次编码在 JSON 服务代理）。

在前面的示例中，发送到服务器的 JSON 是 58 个字符︰

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##报告应用程序信息

您可以手动报告跟踪信息 （或任何其他应用程序特定信息） 使用`sendAppInfo:`函数。

请注意，可以将这些信息发送增量︰ 只给定键的最新值将保存对某个给定设备。

如事件其他方案`NSDictionary`类用于抽象应用程序的信息，请注意数组或子字典会认为是简单的字符串 （使用 JSON 序列化）。

**示例：**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### 限制

#### 密钥

在每个键`NSDictionary`必须匹配以下正则表达式︰

`^[a-zA-Z][a-zA-Z_0-9]*`

这意味着至少一个字母后, 跟字母、 数字或下划线，必须从开始键 (\_)。

#### 大小

应用程序信息被限制为**1024年**个字符，每个调用 （一次编码在 JSON 服务代理）。

在前面的示例中，发送到服务器的 JSON 是 44 个字符︰

    {"birthdate":"1983-12-07","gender":"female"}

测试
