---
ms.openlocfilehash: 1a55ef703af5122fa2b84929a9acb66efc693caf
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 Yammer 连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 Yammer 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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


# 逻辑应用程序中使用 Yammer 连接器 #
连接到 Yammer 和开机自检信息操作和触发器，以检索新邮件。

可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。

## 为您的逻辑应用程序创建 Yammer 连接器 ##
若要使用 Yammer 连接器，您需要首先创建 Yammer 连接器 API 的应用程序的实例。 进行这种，如下所示︰

1. 在 Azure 的 startboard 中，选择**市场**。
2. 搜索"Yammer 连接器"，选中它，然后选择**创建**。
3. 对 Yammer 连接器进行如下配置︰  
![][1]

    - **位置**-选择您想要进行部署的连接器的地理位置
    - **预订**-选择您想要在中创建此连接器的订阅
    - **资源组**的选择或创建资源组连接器所在的位置
    - **应用程序服务计划**的选择，或创建 web 宿主计划
    - **定价层**-选择连接器定价层
    - **名称**-一个 Yammer 连接器的名称为

4. 在上的单击创建。 创建新的 Yammer 连接器。
5. API 的应用程序实例创建之后，您可以创建逻辑 Yammer 连接器使用的应用程序。

## 在您的应用程序逻辑中使用 Yammer 连接器 ##
一旦创建 API 的应用程序，为您的逻辑应用程序现在作为触发器/操作使用 Yammer 连接器。 要执行此操作，您需要︰

1.  创建新的逻辑应用程序︰  
![][2]

2.  打开"触发器和操作"来打开逻辑应用程序设计器和配置您的流︰  
![][3]

3.  Yammer 连接器将出现在"API 此资源组中的应用程序"一节中上右手边的库︰  
![][4]

4. 您可以通过单击"Yammer 连接器"上 Yammer 连接器 API 应用程序放到编辑器。 单击授权按钮。 提供您 Yammer 的凭据。 请单击"允许":  
![][5]  
![][6]  
![][7]

现在，您可以使用 Yammer 连接器流中。

## 为触发器使用 Yammer 连接器

您可以使用从 Yammer 触发器 （"新信息"） 在其他操作流中检索到的新消息。 对 Yammer 触发器的输入的属性进行如下配置︰

- **组 ID** -应从中检索新邮件的组的 ID。 如果没有提供组 ID，将从以下源检索到消息。 可以从在 Yammer 组 URL 检索组 ID。
        
    示例︰ 在以下 URL 组 ID 是"5453203":  
https://www.yammer.com/microsoft.com/#/threads/inGroup?type=in_group&feedId=5453203  
![][8]  
![][9]

## Yammer 连接器用于发布一条消息

逻辑应用程序中，还可以使用 Yammer 连接器为一个操作。 首先，指定为您的逻辑应用程序的触发器或检查手动运行此逻辑 （如下所示）。 添加 yammer 连接器、 适当授权并选择"发布消息"操作。 "开机自检消息"操作的输入的属性按如下配置︰

- **消息文本**-要发送的消息的文本内容
- **组 ID** — 指定新邮件应过帐到其中的组的 ID。 如果组 ID 未提供消息将会发送到所有公司送。 可以从在 Yammer 组 URL 检索组 ID。  

    示例︰ 在以下 URL 组 ID 是"5453203":  
https://www.yammer.com/microsoft.com/#/threads/inGroup?type=in_group&feedId=5453203
-   **标记的用户**-用户需要标记邮件中的网络名称组成的数组︰  
![][10]  
![][11]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-yammer/img1.PNG
[2]: ./media/app-service-logic-connector-yammer/img2.PNG
[3]: ./media/app-service-logic-connector-yammer/img3.png
[4]: ./media/app-service-logic-connector-yammer/img4.png
[5]: ./media/app-service-logic-connector-yammer/img5.PNG
[6]: ./media/app-service-logic-connector-yammer/img6.PNG
[7]: ./media/app-service-logic-connector-yammer/img7.png
[8]: ./media/app-service-logic-connector-yammer/img8.PNG
[9]: ./media/app-service-logic-connector-yammer/img9.PNG
[10]: ./media/app-service-logic-connector-yammer/img10.PNG
[11]: ./media/app-service-logic-connector-yammer/img11.PNG

测试
