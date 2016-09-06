---
ms.openlocfilehash: ce56bdfbfced11da68ae4e6acdd2a8079708b50d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 Azure 服务总线连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 Azure 服务总线接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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


# 开始使用 Azure 服务总线连接器并将其添加到您的逻辑应用程序 
连接到 Azure 服务总线从队列和订阅接收消息并将消息发送到队列和主题。 逻辑应用程序中使用连接器作为"工作流"的一部分。 

## 触发器和操作
触发器是发生的事件。 例如，在更新订单时或当新客户添加。 对策是触发器的结果。 例如，当订单或新消息放入队列中，发送警报或消息。  

Azure 服务总线连接器可以用作触发器或 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。

Azure 服务总线连接器具有以下触发器和操作可用︰

触发器 | 操作
--- | ---
可用的消息 | 发送消息

## 创建 Azure 服务总线连接器
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "Azure 服务总线连接器"搜索，请选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性︰  
    ![][1]

4. 输入以下程序包设置︰

    名称 | 说明
--- | ---
连接字符串 | Azure 服务总线连接字符串。 例如，输入︰ *Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName = [名称];SharedAccessKey = [项]*。
实体名称 | 输入队列或主题的名称。
订阅名 | 输入预订从其接收邮件的名称。

5. 单击**创建**。

## 在您的应用程序逻辑中使用服务总线连接器
连接器创建之后，您现在可以作为触发器或逻辑应用程序的操作使用 Azure 服务总线连接器。 若要此操作︰

1.  创建新的逻辑应用程序，然后选择 Azure 服务总线连接器都有同一个资源组︰  
    ![][2]

2.  打开"触发器和操作"来打开逻辑应用程序设计器和配置您的工作流︰  
    ![][3]

3. Azure 服务总线连接器将出现在"API 此资源组中的应用程序"一节中上右手边的库︰  
    ![][4]

4. 您可以通过单击"Azure 服务总线连接器"Azure 服务总线连接器放到编辑器中。

5.  您现在可以在工作流中使用 Azure 服务总线连接器。 您可以使用从流中的其他操作 Azure 服务总线触发器 （"消息可用"） 中检索的消息︰  
    ![][5]  

    ![][6]

您还可以使用 Azure 服务总线"发送消息"操作︰  
![][7]  

![][8]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-azureservicebus/img1.PNG
[2]: ./media/app-service-logic-connector-azureservicebus/img2.PNG
[3]: ./media/app-service-logic-connector-azureservicebus/img3.png
[4]: ./media/app-service-logic-connector-azureservicebus/img4.PNG
[5]: ./media/app-service-logic-connector-azureservicebus/img5.PNG
[6]: ./media/app-service-logic-connector-azureservicebus/img6.PNG
[7]: ./media/app-service-logic-connector-azureservicebus/img7.PNG
[8]: ./media/app-service-logic-connector-azureservicebus/img8.PNG

测试
