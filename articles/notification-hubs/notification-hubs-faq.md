---
ms.openlocfilehash: da18ced92c1284063f5ae3bf3713ecc7e18456cb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="Azure 通知集线器的常见问题 (Faq)"
    description="在设计/实施解决方案通知集线器上的常见问题解答"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="wesmc"
    manager="dwrede"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/18/2015" 
    ms.author="wesmc" />

#Azure 通知集线器的常见问题 (Faq)

##常规
###1.什么是通知集线器的定价？
通知集线器提供三个层次的*自由*，*基本*在 & *标准*层。 更多详细描述了这里-[通知集线器定价]。 定价收取订阅级别，因此无论多少的命名空间或通知集线器有根据的数目。
开发目的，与不 SLA 保证提供免费的层。 基本和标准层提供生产使用率仅为标准层启用下列主要功能︰

- *广播*的基本层限制通知中心至 3 K （适用于 > 5 设备） 的标记的数量。 如果您有访问群体大小大于 3 K 则必须移到标准层。
- *丰富的遥测*的基本层不允许导出遥测或登记数据。 如果您需要将导出遥测数据以供脱机查看和分析的能力则必须移到标准层。
- *多租户*-如果您要创建使用通知的集线器可以支持多个承租人，则必须考虑到标准层移动的移动应用程序。 这使您可以在通知中心命名空间级别为应用程序设置推送通知服务 （企业型） 凭据，然后将分隔承租人向他们提供此公共命名空间下的单独集线器。 这样便于维护，同时为确保非跨租户重叠每个租户将 SA 密钥来发送和接收通知的通知集线器从隔离。

###2.什么是 SLA？
基本和标准的通知中心层，我们保证，至少 99.9%的情况下，正确地配置的应用程序将能够发送通知或执行方面通知中心部署基本或标准的通知中心层内的注册管理操作。 若要了解有关我们的 SLA 的详细信息，请访问 SLA 页面-[通知集线器 SLA]。 请注意由于通知集线器取决于外部平台提供商能够将通知传递到设备没有任何 SLA 保证对平台通知服务与设备之间的链路。

###3.哪些客户正在使用通知集线器？
我们有大量的客户使用通知集线器，具有以下几个值得注意的︰

* 利益集团的 Sochi 2014 – 100 条、 3 + 万设备 150 多万个通知在 2 周内派。 [-Sochi CaseStudy]
* Skanska- [CaseStudy-Skanska]
* 西雅图时间- [CaseStudy-西雅图时间]
* Mural.ly- [CaseStudy-Mural.ly]
* 7Digital- [CaseStudy-7Digital]
* Bing 应用程序 – 多万个设备，发送通知的 3 万/天

##设计与开发
###1.您是否支持哪些服务端平台？
我们提供 Sdk 和示例.NET、 Java、 PHP、 Python，Node.js，以便应用程序的后端可以设置通知集线器到通信使用任何这些平台。 通知集线器 Api 基于 REST 接口以便您可以选择这些直接交谈。 详细信息详细描述了这里- [NH 的 REST Api]

###2.支持哪些设备平台？
我们支持发送通知到苹果 iOS，Android、 Windows 通用和 Windows Phone，Kindle，Android 中国 （通过 Baidu)、 Xamarin （iOS 和 Android）、 镶边的应用程序平台。 逐个步骤获取入门的教程，对于这些平台可以在这里- [NH-获取启动教程]

###3.是否支持 SMS/电子邮件/网站通知？
通知集线器主要用于将通知发送到移动应用程序使用上面列出的平台。 我们不提供发送电子邮件或 SMS，但是可以通过使用 Azure 移动服务发送本机推式通知的通知集线器以及集成第三方平台提供这些功能的能力。 例如 本教程讨论如何发送 SMS 通知使用 Azure 移动服务-我们也不会提供在浏览器[的移动服务发送 SMS]开箱即用的推式通知。 客户可以选择使用那么 SignalR 实现这。 我们还提供了教程，说明如何将工作在 Google Chrome 浏览器 Chrome 应用程序向服务器发送推式通知。 看到这-[铬应用程序教程]

###4.什么是 Azure 移动服务和 Azure 通知集线器之间的关系，当使用什么？
如果您有现有的移动应用程序端，并且只想添加发送推式通知的功能，则必须使用 Azure 通知集线器。 如果您想要安装程序的从零开始您移动应用程序后的端您应该考虑使用 Azure 移动服务。 Azure 的移动服务自动设置作为您能够很容易地从移动应用程序的后端发送推式通知的通知中心。 Azure 移动服务定价的通知中心包括基本费用，仅支付时要考虑的因素包括推进。 更多详细描述了这里的[移动服务定价]

###5.可支持多少设备？
在基本和标准层-我们不会强制任何限制的活动可以接收通知的设备的数量。 有关详细信息，请参阅︰[通知集线器定价]

###6.多少推式通知可以发送？
客户正在使用 Azure 通知集线器发送推式通知，每日的数以百万计。 不需要任何额外扩展通知集线器。 我们会自动扩大根据流经系统的通知数。 请注意，定价 does 获取受影响取决于正在处理推式通知。

###7.如何长时间到达我的设备的通知的？
Azure 通知集线器是能够处理的至少在 1 分钟内，在正常使用情况下传入的负载是非常一致的不是 spikey 在本质发送 100 万个。 此速率会因数字标记，传入发送等性质。在此期间，我们将能够计算每个基于注册的标记标记表达式的各自推式通知服务的平台和路由消息的目标。 下文是推送通知服务 （企业型） 将通知发送到该设备的责任。 PNSs 不保证任何 SLA 但是通常绝大多数的邮件将被传送到设备在几分钟 （< 10 分钟） 内从它们被发送到我们的平台的时间的通知传递。 可能有几个离群值可能需要更长的时间。 Azure 通知集线器也有一个策略，以除去根本无法在 30 分钟内送达企业型的任何通知。 此延迟可能会由于多种原因，最常因为企业型调节您的应用程序。

###8.是否有任何延迟保证？
由外部平台特定的推式通知的特点推送通知服务，没有任何延迟保证。 通常情况下，不要在几分钟内传送通知的大部分。

###9.什么是我们需要考虑到哪些设计解决方案与命名空间和通知集线器的考虑因素？
*移动的应用程序环境︰*应该有一个通知中心，每个每个环境的移动应用程序。 在多租户方案中的每个租户应有单独集线器。
如在发送通知时，这可能会导致问题，永远不会必须共享相同的通知中心测试和生产环境之间。 例如苹果提供沙盒和生产推动终结点与每个单独凭据。 如果集线器是使用苹果沙盒证书最初配置，然后重新配置为使用苹果生产证书，旧设备标记将会使用新的证书无效并导致失败的推进。 最好以单独的生产和测试环境，对不同的环境中使用不同的集线器。

*企业型凭据︰*如果移动应用程序的平台 （如苹果或 Google 等） 的开发人员门户中注册然后获得应用程序标识符和安全令牌的应用程序的后端需要提供对平台的推式通知服务，以便能够向设备发送推式通知。 这些可以为在窗体中的证书 （例如用于苹果 iOS 或 Windows Phone） 或安全密钥 (Google Android，Windows) 等需要配置通知集线器中的安全令牌。 这通常在通知中心级别完成，但也可以在命名空间级别在多租户方案。

*命名空间︰*命名空间还可以用于部署分组。  它还可以用于表示所有承租人在多租户的情况下的相同应用程序的所有通知集线器。

*地理分布︰*地理分布并不总是关键时推式通知。 很值得注意，各种推式通知服务 （如 APNS 等 GCM） 最终将推式通知传递到设备的就不会均匀地分布。 但是如果必须为全球使用的应用程序然后可以创建多个集线器在不同的命名空间在世界各地的不同 Azure 地区利用通知集线器服务的可用性。 请注意，所以这不真的建议，如果真的需要必须只完成，这样会增加尤其是登记的管理成本。

###10.我们办登记后端应用程序或设备的直接？
必须执行前创建注册客户端身份验证或标记，必须创建或修改应用程序的后端基于某些应用程序逻辑时，登记后端应用程序很有用。 更多指导位于此处的[后端注册指导] & [后端注册指南-2]

###11.什么是安全模型？
Azure 通知集线器使用共享访问签名 (SAS) 基于的安全模型。 根命名空间级别或精细通知集线器级别，可以使用 SAS 标记。 这些 SAS 标记可以用来设置不同的授权规则如发送消息的权限、 侦听通知权限等。详细信息详细描述了这里- [NH 安全模型]

###12.如何处理敏感负载的通知中？
所有通知平台推送通知服务 （企业型） 都传送到这些设备。 当发件人发送通知到 Azure 通知集线器然后我们处理，并将通知传递到各自的企业型。 所有连接的发企业型到 Azure 通知集线器，然后再都使用 HTTPS。 Azure 通知集线器不以任何方式记录消息的负载。
但是发送敏感负载的建议其中发件人的消息标识符的 ping 通知将发送到设备不敏感负载的情况下，当在该设备上的应用程序接收此负载，然后，它调用安全应用程序端 API 直接提取消息详细信息的安全将推的模式。 教程以实现模式是此处的[NH-安全推教程]

##操作
###1.什么是灾难恢复 (DR) 故事的？
在我们结束 （通知中心名称、 连接字符串等），我们提供元数据灾难恢复。 在灾难恢复、 登记数据是将会丢失，因此，必须要拿出一个解决方案来重新填充该新集线器到其中的一个。

- *步骤 1* -在另一个域控制器中创建辅助的通知中心。 您可以创建此即时灾难事件时或您可以创建一个从 get 转。 无论更因为 NH 资源调配是一个快速的过程，按几秒钟。 有一个从一开始将还可使您从灾难恢复的事件影响我们的管理能力，因此建议。

- *步骤 2* -Hydrate 次通知中心从主通知中心登记。 建议不要尝试维护登记在两个集线器，然后尝试使它们保持同步动态如登记即将，如由于登记过期的企业型的固有性质也没有起作用的。 通知集线器清理它们我们收到企业型反馈有关过期或无效的注册。  

推荐方法是使用应用程序的后端的两种︰

- 维护所设置的登记在其结尾处以便它可以执行大容量插入到对于灾难恢复，或第二通知中心

- 获取从主要作为备份中心登记的正则表达式的转储，然后没有批量插入辅助 NH。

(此处所述登记标准层中可用的导出/导入功能︰[登记导出/导入])

如果您没有后端然后当在设备上启动该应用程序然后它们要做中辅助 NH 的新注册并最终具有辅助 NH 将所有活动设备注册但不足将是当应用程序尚未开启的设备将不会收到通知的时间段。

###2.是否有任何审核日志功能？
所有通知集线器管理操作都转到 Azure 管理门户中公开的操作日志中。

##监控和故障诊断
###1.哪些故障排除功能是否可用？
Azure 通知集线器提供几种功能来执行常见故障排除，特别是在周围拖放通知最常见的情况。 请参阅详细信息在此疑难解答白皮书- [NH-故障排除]

###2.何种遥测功能是否可用？
Azure 通知集线器启用 Azure 管理门户中查看遥测数据。 可用的度量值的详细信息可以在这里- [NH 的衡量标准]。
请注意，成功通知仅意味着，通知被发送到外部推送通知服务 （例如 APNS 为苹果、 Google 等的 GCM） 则最多包含传递到设备通知企业型和企业型不公开给我们这些度量标准。  
它还提供导出以编程方式 （在标准层） 的遥测数据的能力。 此示例以供详细信息- [NH 的度量标准的示例]，请参阅

[定价的通知集线器]: http://azure.microsoft.com/pricing/details/notification-hubs/
[通知集线器 SLA]: http://azure.microsoft.com/support/legal/sla/
[-Sochi CaseStudy]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[-Skanska CaseStudy]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy-西雅图时间]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[-Mural.ly CaseStudy]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[-7Digital CaseStudy]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH 的 REST Api]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH-快速入门教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[发送短消息的移动服务]: http://azure.microsoft.com/documentation/articles/partner-twilio-mobile-services-how-to-use-voice-sms/
[铬色应用程序教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[移动服务定价]: http://azure.microsoft.com/pricing/details/mobile-services/
[后端注册指南]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[后端注册指南-2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH 安全模型]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH-安全推教程]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH-疑难解答]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH 的度量标准]: https://msdn.microsoft.com/library/dn458822.aspx
[NH-衡量标准示例]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[登记导出/导入]: https://msdn.microsoft.com/library/dn790624.aspx
