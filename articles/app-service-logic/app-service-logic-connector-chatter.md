---
ms.openlocfilehash: adec15b5ce73fe293da92a705e9a8e287b909603
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="蜡笔画连接器用逻辑应用程序 |Microsoft Azure 应用程序服务"
   description="如何创建和配置联网接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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
   ms.author="sameerch"/>


# 开始使用噪音连接器并将其添加到您的逻辑应用程序 
连接到蜡笔画和发布一条消息或搜索源。 例如，您可以搜索重复数据源并找到某个对象后，您可以过帐该重复消息到销售组。

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加蜡笔画连接器。 

## 触发器和操作

触发器启动基于特定的事件，如新的蜡笔画消息的到达一个新实例。 对策是结果，像后接收新的蜡笔画消息，然后到另一个重复组或其他社交媒体网站，如 Facebook 或者 Twitter 发布消息。

联网的连接器可以用作触发器或 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 蜡笔画连接器具有以下触发器和操作可用︰

触发器 | 操作
--- | ---
新邮件 | <ul><li>开机自检信息</li><li>搜索</li></ul>


## 为您的逻辑应用程序创建的蜡笔画连接器
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "联网接口"中搜索、 选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性︰  
    ![][1]  
    - **位置**-选择您想要进行部署的连接器的地理位置
    - **预订**-选择您想要在中创建此连接器的订阅
    - **资源组**的选择或创建资源组连接器所在的位置
    - **Web 托管计划**-选择或创建 web 宿主计划
    - **定价层**-选择连接器定价层
    - **名称**-一个联网连接器的名称为

4. 选择**创建**。


## 在您的应用程序逻辑中使用蜡笔画连接器
创建 API 应用程序后，在您的逻辑应用程序现在为触发器或操作使用蜡笔画连接器。 若要此操作︰

1. 逻辑应用程序中打开**触发器和操作**来打开逻辑应用程序设计器和配置您的流。

2. 在库列出蜡笔画连接器︰  
    ![][4]
3. 选择要在设计器中自动添加的蜡笔画连接器。 选择**授权**、 输入您的凭据，并选择**允许**:  
    ![][5]
    ![][6]
    ![][7]

现在，您可以使用蜡笔画连接器流中。 您可以使用从蜡笔画触发器 （"新信息"） 在其他操作流中检索到的新消息。 蜡笔画触发器的输入的属性按如下配置︰

**组 ID**的输入应从中检索新邮件的组的 ID。 如果没有提供组 ID，然后从用户的数据源检索新邮件︰  
    ![][8]
    ![][9]


以类似的方式可用于重复操作流程中选择"发送消息"操作发布一条消息。 "开机自检消息"操作的输入的属性按如下配置︰  
    - **消息文本**-要发送的消息的文本内容    - **组 ID** — 指定新邮件应过帐到其中的组的 ID。 如果没有提供组 ID，消息将会发送用户的源。
    -   **文件名**的文件的名称与此邮件附加    -   **内容数据**的附件的内容数据    -   **内容类型**的内容类型的附件的    -   **内容传输编码**的内容传输编码的附件 ("无"|"base64")     -   **提到**的要将标记此邮件中的用户名数组    -    **Hashtags** -hashtags 数组沿消息发送  

![][10]
![][11]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-chatter/img1.PNG
[2]: ./media/app-service-logic-connector-chatter/img2.PNG
[3]: ./media/app-service-logic-connector-chatter/img3.png
[4]: ./media/app-service-logic-connector-chatter/img4.png
[5]: ./media/app-service-logic-connector-chatter/img5.PNG
[6]: ./media/app-service-logic-connector-chatter/img6.PNG
[7]: ./media/app-service-logic-connector-chatter/img7.PNG
[8]: ./media/app-service-logic-connector-chatter/img8.PNG
[9]: ./media/app-service-logic-connector-chatter/img9.PNG
[10]: ./media/app-service-logic-connector-chatter/img10.PNG
[11]: ./media/app-service-logic-connector-chatter/img11.PNG

测试
