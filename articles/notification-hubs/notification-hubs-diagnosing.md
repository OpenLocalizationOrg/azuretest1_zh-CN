---
ms.openlocfilehash: fa7f7f87120a496ca86be7dcc81eaddb258904c4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 通知集线器的诊断指南" 
    description="如何诊断常见问题 Azure 通知集线器的指导原则。" 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="wesmc7777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/14/2015" 
    ms.author="wesmc"/>

#Azure 通知集线器的诊断指南

##概述

我们听到从 Azure 通知集线器客户最常见的问题之一是如何找出为什么他们看不到其应用程序的后端发送的通知出现在客户端设备的位置和原因而放弃通知以及如何解决此问题。 在本文中我们将经过设备通知可能会被丢弃或无法得到的各种原因。 我们还将通过在其中您可以分析并找出根本原因的方法。 

首先，就必须懂得如何 Azure 通知集线器推送通知到的设备。
![][0]

在典型发送通知流，消息从**后端应用程序**发送到**Azure 通知中心 (NH)**这反过来会做一些处理，在考虑到已配置的标记和标记表达式，以确定"靶心"亦即所有的登记需要接收推式通知的所有登记。 这些登记可以跨越任何或所有我们支持平台的 iOS 中，Google，窗口，Windows Phone，Kindle 和 Baidu 为中国 Android。 NH 然后推送通知，目标建立后，分割成多个批次的登记，到设备平台特定**推送通知服务 （企业型）** -例如 APNS 为苹果、 Google 等的 GCM。NH 与各自的企业型根据在 Azure 门户配置通知中心页面上设置的凭据进行身份验证。 企业型然后转发到相应的**客户机设备**的通知。 这是建议提供推式通知和注意，企业型平台和设备之间发生的通知传递的最后 leg 方法的平台。 因此，我们有四个主要的组件的*客户端*、*应用程序的后端*， *Azure 通知集线器 (NH)*和*推送通知服务 （企业型）*以及其中的任何可能会导致中断的通知。 提供[通知集线器概述]了这种体系结构中的更多详细信息。

未传递通知在初始测试/组配过程中可能会出现阶段这可能表示配置问题或可能在生产环境中所有或部分的通知可能变得越来越下沉，表示一些更深层次的应用程序或消息传递模式问题。 在部分，下面我们将介绍各种丢弃的通知方案从通用到极少数种类，其中有一些您可能会发现明显和一些其他人不太多。 

##Azure 通知集线器错误配置 

Azure 通知集线器需要自行开发的应用程序能够成功地将通知发送给各自的企业型的上下文中进行身份验证。 这可由开发人员创建具有各自平台 （Google、 苹果、 Windows 等） 的开发者帐户和再注册他们他们从哪里获得凭据，这需要在 Azure 门户下通知集线器配置节中配置的应用程序。 如果通过不进行任何的通知，第一步应该是确保通知中心将它们匹配其平台特定开发人员帐户下创建的应用程序中配置了正确的凭据。 您会发现我们[正在获取启动教程]以按部就班的方式，转到这一过程很有用。 以下是一些常见配置错误︰

1. **常规**
 
    ） 请确保您通知中心名称 （无需键入错误） 都是相同︰

    - 您要注册客户端， 
    - 您要从后端，发送通知  
    - 企业型凭据的配置位置和 
    - 已配置客户端和后端的 SAS 凭据。 
        
    b） 请确保您正在使用正确的 SAS 配置字符串在客户端和应用程序的后端。 根据经验，必须使用**DefaultListenSharedAccessSignature**客户端和**DefaultFullSharedAccessSignature**上对应用程序的后端 （其中就有权将能够将通知发送到 NH）

2. **苹果推送通知服务 (APNS) 配置**
 
    必须维护两个不同的集线器的一个用于生产，另一个用于测试用途。 这意味着上载要使用在沙盒环境中单独集线器的证书和证书要生产到单独集线器。 不要尝试将不同类型的证书传到同一个集线器，它可能引起推进通知失败。 如果找到自己在无意中已在此上载不同类型的证书到同一个集线器的位置，建议删除该集线器并重新开始。 如果由于某种原因，您不能删除然后最起码的中心，必须删除所有现有的登记从集线器。 

3. **Google 云消息 (GCM) 配置** 

    ） 请确保您使"Google 云消息为 Android"云项目下。 
    
    ![][2]
    
    b） 使确保获取凭据的 NH 将使用与 GCM 进行身份验证时创建"服务器密钥"。 
    
    ![][3]
    
    c） 请确保您已配置"项目 ID"是从仪表板可以获得一个完全数字实体的客户机︰
    
    ![][1]

##应用程序问题

1) **标记表达式 / 标记**

如果使用标记或标记表达式来细分受众，都可能在发送通知时，没有发现基于发送调用中指定的标记/标记表达式没有目标。 它是最适合用来查看您的登记以确保标签相匹配时发送通知，然后验证只能从这些登记使用客户端通知回执。 例如 如果所有已完成您的登记与 NH 说标记"政治"，而发送带有标记的通知"运动"，则它将不会发送到任何设备。 复杂的用例可能涉及标记表达式，您只能注册"标记 A"或者"标记 B"，但同时发送通知，您的目标"标记和与标记 B"。 在下面的自我诊断的提示部分，有的方法，在其中您可以查看您的登记以及他们的标记。 

2) **模板的问题**

如果您使用的模板，请确保您正在追随在[模板指南]所述的指导原则。 

3) **无效的注册**

NH 假设通知中心的配置正确，并且导致的需要向其发送通知的有效目标查找正确使用任何标记/标记表达式，触发并行-将消息发送到一组登记的每个批次的几个处理批处理。 

> [AZURE.NOTE] 我们进行并行处理，因为我们并不能保证将传递通知的顺序。 

现在 Azure 通知中心专为"最在的一次"消息传递模型。 这意味着我们尝试重复数据消除，这样没有通知到设备传送一次。 要确保这一点我们登记一下，并确保实际发送到企业型之前只有一封发送每个设备标识符。 每个批处理发送到企业型，它又是接受并验证登记，则可能企业型检测到一个或多个批处理中的登记错误到 Azure NH 将返回一个错误并停止处理从而完全除去该批处理。 这是与 APNS 使用 TCP 流协议尤其如此。 由于我们在大多数进行优化后交付，它是要注意的是由于我们不知道肯定企业型断整批没有此失败的批次不能重试或部分。 企业型并未但是告诉 Azure NH 注册导致失败并根据反馈我们删除该注册从我们的数据库。 这意味着它是可能，一个注册批处理或它的一个子集可能不会收到通知但是因为我们清除坏在注册时，尝试发送，则在下一次更好地成功发送。 随着目标设备的数量比例的增长 （一些我们的客户将通知发送到数以百万计的设备），各处放于奇数批毫无中接收通知，但是，如果发送几个通知，并有一些企业型错误则可能看到所有或大多数未获取收到的通知的设备的整体百分比太大差异。 如果反复出现这种情况您必须标识任何错误登记并删除它们。 因为它们是最常见的原因的被丢弃的通知，明确必须删除任何现有格式的登记。 如果这是一个测试环境，可能还直接删除所有登记，因为应用程序在设备上打开时将重试并重新注册通知集线器确保所有登记有规定创建将都有效的。 

##企业型问题

各自的企业型接收到通知消息后则其负责将通知传递到该设备。 Azure 通知集线器不在这里的图片和时，或将通知发送到该设备有无控制。 由于平台通知服务相当强健，通知往往从企业型几秒后到达设备。 如果限制但是企业型然后 Azure 通知集线器适用于策略指数后退，如果企业型保持 30 分钟到达然后我们制定策略过期并永久删除这些邮件。 

如果企业型尝试提供通知，但该设备处于脱机状态，通知有限的一段时间，企业型的存储，并且当它变为可用时发送到设备。 存储为一个特定的应用程序只能有一个最新通知。 如果设备处于脱机状态时发送了多个通知，每个新的通知使放弃事先通知。 这种现象让只最新通知的称为合并中 APNS 的通知和折叠在 GCM （它使用折叠键）。 如果设备长时间保持离线，将放弃它所存储的任何通知。 源- [APNS 指导] & [GCM 指南]

使用 Azure 通知集线器-可以传递合并键通过 HTTP 标头使用泛型`SendNotification`API (例如.NET SDK- `SendNotificationAsync`) 也接受 HTTP 标头的刀路原样各自企业型。 

##自我诊断的提示

这里我们将讨论各种途径来诊断和根会导致通知中心问题︰

###验证凭据

1. **企业型开发人员门户**

    验证它们各自企业型开发人员门户 （APNS，GCM，WNS 等） 使用我们[正在获取启动教程]。

2. **Azure 的管理门户**

    请转至配置选项卡查看并与那些从企业型开发人员门户获得匹配的凭据。 

    ![][4]

###验证注册

1. **Visual Studio**

    如果您使用 Visual Studio 开发您可以连接到 Microsoft Azure 并查看和管理大量的 Azure 服务，其中包括从"服务器资源管理器"通知中心。 这是主要用于开发/测试环境。 

    ![][9]

    您可以查看和管理网络集线器的平台上，本机可以很好地进行分类的所有登记或模板注册、 任何标记、 企业型标识符、 注册 id 和过期日期。 您还可以编辑动态-这是说如果要编辑任何标记注册。 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio 功能来编辑登记只应在开发/测试与有限的登记过程。 如果那里出现需要批量修复您的登记，请考虑使用的导出/导入注册功能介绍-[导出/导入登记]（只适用于标准层）

2. **服务总线资源管理器**

    许多客户使用资源管理器介绍- [ServiceBus 浏览器]用于查看和管理他们的通知中心的 ServiceBus。 它是开源项目可从 code.microsoft.com- [ServiceBus 资源管理器中的代码]

###验证邮件通知

1. **Azure 门户**

    您可以转到"调试"选项卡上，将测试通知发送给您的客户端，而无任何服务后端向上运行。 

    ![][7]

2. **Visual Studio**

    您还可以从 Visual Studio comforts 发送测试通知︰

    ![][10]

    您可以阅读更多的 Visual Studio 的通知中心 Azure 资源管理器功能在这里的 
    
    - [与服务器资源管理器概述]
    - [与服务器资源管理器中博客文章-1]
    - [与服务器资源管理器中博客文章-2]

###调试失败的通知/审查通知结果

**EnableTestSend 属性**

当您发送的通知通过通知集线器时，最初它只是获取排队等候的 NH 进行处理，以找出所有目标，然后最终 NH 发送到企业型。 这意味着，当您使用 REST API 或任何客户端 SDK 时，发送调用成功返回仅表示，该消息具有已成功排队向通知中心。 它不能提供深入 NH 最终获得将消息发送到企业型发生了什么变化。 如果您通知未到达客户端设备，则可能当 NH 尝试将邮件传递给企业型，如有效载荷大小超出了所允许的企业型的最大错误或 NH 中配置的凭据是无效等。要深入了解企业型错误，我们引入了一个名为[EnableTestSend 的功能]属性。 此属性从门户或 Visual Studio 的客户端发送测试消息时，将自动启用，因此，您可以查看详细的调试信息。 可以使用此通过采用即用现在的.NET sdk 示例 Api 并将被添加到所有客户端 Sdk 最终。 若要使用此 REST 调用，只需追加查询字符串参数的发送调用结尾如称为"测试" 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*示例 (.NET SDK)*
 
假设您使用.NET SDK 将发送本机 toast 通知︰

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State` 将状态只是`Enqueued`结尾的执行没有任何深入了解您推到出了什么问题。 现在，您可以使用`EnableTestSend`初始化时的布尔值属性`NotificationHubClient`，可以获取有关企业型出错时发送通知的详细的状态。 这里的发送调用将需要更多时间返回，因为它仅返回后 NH 已传递到企业型来确定结果的通知。 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*输出示例*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
此消息表明或者无效凭据配置通知中心或在中心登记的问题和建议应删除此登记，并让客户端重新发送邮件之前创建它。 
 
> [AZURE.NOTE] 请注意，此属性的用法被很大程度遏制，因此必须仅使用这中仅有少数登记的开发/测试环境。 我们只向 10 个设备调试的通知。 我们还必须处理调试发送为每分钟 10 的限制。 

###查看遥测 

1. **使用 Azure 的门户网站**

    Azure 门户使您能够快速浏览所有的活动通知中心。 
    
    a） 从"仪表板"选项卡中，您可以查看汇总的视图登记、 通知以及每个平台错误。 
    
    ![][5]
    
    b） 您还可以从"监视器"选项卡中时尝试将通知发送到企业型 NH 返回任何企业型特定错误特别是在更深层次了解添加许多其他平台的具体标准。 
    
    ![][6]
    
    c） 您应着手检查**传入消息**，**注册操作****成功通知**，然后进入每平台选项卡以查看具体的错误企业型。 
    
    d） 如果有通知中心使用的身份验证设置配置不正确，您将看到企业型身份验证错误。 这是很好的指示，检查企业型凭据。 

2) **以编程方式访问**

这里的更多详细信息 

- [遥测编程访问]
- [遥测访问 Api 的示例通过] 

> [AZURE.NOTE] 几个遥测相关功能**导出/导入登记**，如**通过 Api 的遥测访问**等仅有标准层中。 如果您试图使用这些功能，如果您是在自由或基本层然后将直接从 REST Api 使用它们时使用 SDK 和 HTTP 403 （禁止） 时收到此影响异常消息。 请确保您已经移动到标准层通过 Azure 管理门户。  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[通知集线器概述]: notification-hubs-overview.md
[入门教程]: notification-hubs-windows-store-dotnet-get-started.md
[模板的指南]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS 指南]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM 指南]: http://developer.android.com/google/gcm/adv.html
[导出/导入登记]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus 资源管理器]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus 资源管理器中的代码]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[与服务器资源管理器概述]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[与服务器资源管理器中博客文章-1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[与服务器资源管理器中博客文章-2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend 功能]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[遥测编程访问]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[遥测访问 Api 的示例通过]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 