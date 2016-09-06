---
ms.openlocfilehash: 0ea271dd6182463d28f26bd31587c377c1b8cf15
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 Facebook 连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置的 Facebook 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/23/2015"
   ms.author="andalmia"/>


# 开始使用 Facebook 连接器并将其添加到您的逻辑应用程序
连接到要发布的消息，或发布照片的 Facebook 帐户。 可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。 

与 Facebook 连接器，您可以︰

- 使用触发器检索"新公告的用户的时间线"在页面的新帖子"。 检索新文章时，它会触发新的流程实例并传递到流中进行处理的请求中收到的数据。
- 使用"发布公告"、"发布的照片"等让您的操作。 这些操作获取回响应，并使其可用的操作中使用的流程。

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 Facebook 连接器。 

## 触发器和操作

触发器 | 操作
--- | ---
<ul><li>用户时间线上的新投递</li><li>在页面上的新投递</li></ul> | <ul><li>发布文章</li><li>发布照片</li></ul>



## 为您的逻辑应用程序创建的 Facebook 连接器
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "Facebook 连接器"搜索、 选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性︰  
    ![][1]
4.  选择**创建**。


## 在您的应用程序逻辑中使用 Facebook 连接器
一旦创建 API 的应用程序，为您的逻辑应用程序现在为触发器/操作使用 Facebook 连接器。 要执行此操作，您需要︰

1.  逻辑应用程序中打开**触发器和操作**来打开逻辑应用程序设计器和配置您的流︰  
    ![][3]
2.  Facebook 连接器会在库中列出︰  
    ![][4]
3. 选择要在设计器中自动添加的 Facebook 连接器。 选择**授权**、 输入您的凭据，并选择**允许**:  
    ![][5]
    ![][6]
    ![][7]
    ![][8]
4.  选择某个触发器︰  
    ![][9]

您现在可以使用从 Facebook 触发器在其他操作中检索到的公告。 在下面流，只要用户的 Facebook 的时间线，一篇文章已过帐相同的帖子将会 Tweeted 使用 Twitter 用户的时间轴中︰  
    ![][10]

类似的方法可以使用 Facebook 的连接器操作创建流。 下面流将检索新 Yammer 组上发布的消息，并发布同一管理用户的 Facebook 页面上︰  
    ![][11]

> [AZURE.TIP] 若要获取 Facebook 页面 ID 或 Yammer 组 ID，请查找 URL 中的数字代码。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-facebook/img1.png
[2]: ./media/app-service-logic-connector-facebook/img2.png
[3]: ./media/app-service-logic-connector-facebook/img3.png
[4]: ./media/app-service-logic-connector-facebook/img4.png
[5]: ./media/app-service-logic-connector-facebook/img5.png
[6]: ./media/app-service-logic-connector-facebook/img6.png
[7]: ./media/app-service-logic-connector-facebook/img7.png
[8]: ./media/app-service-logic-connector-facebook/img8.png
[9]: ./media/app-service-logic-connector-facebook/img9.png
[10]: ./media/app-service-logic-connector-facebook/img10.png
[11]: ./media/app-service-logic-connector-facebook/img11.png

测试
