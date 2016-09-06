---
ms.openlocfilehash: c13207426b000ba7a434184058b6a3af927860dd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 Twilio 接口 |Microsoft Azure 应用程序服务"
   description="如何创建和配置的 Twilio 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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


# 开始使用 Twilio 连接器并将其添加到您的逻辑应用程序
连接到您的 Twilio 帐户来发送和接收短信。 此外可以检索电话号码和使用数据。 可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 Twilio 连接器。 

## 为您的逻辑应用程序创建一个 Twilio 连接器 ##
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. 搜索"Twilio 连接器"，选中它，然后选择**创建**。
3. 将 Twilio 连接器配置，如下所示︰  
![][1]  
    - **位置**-选择您想要进行部署的连接器的地理位置
    - **预订**-选择您想要在中创建此连接器的订阅
    - **资源组**的选择或创建资源组连接器所在的位置
    - **Web 托管计划**-选择或创建 web 宿主计划
    - **定价层**-选择连接器定价层
    - **名称**-一个 Twilio 连接器的名称为
    - **程序包设置**
        - **帐户的 SID**的帐户的唯一标识符。 为您的帐户的帐户的 SID 可从<https://www.twilio.com/user/account/settings>
        - **身份验证令牌**的授权与该帐户相关联的标记。 为您的帐户授权标记可从<https://www.twilio.com/user/account/settings>


4.  在上的单击创建。 创建一个新的 Twilio 连接器。
5.  API 的应用程序实例创建之后，您可以创建逻辑应用程序使用 Twilio 连接器。

## 在您的应用程序逻辑中使用 Twilio 连接器 ##
一旦创建 API 的应用程序，为您的逻辑应用程序现在为一个操作使用 Twilio 连接器。 要执行此操作，您需要︰

1.  创建新的逻辑应用程序，然后选择具有 Twilio 连接器的同一个资源组︰  
![][2]
2.  打开"触发器和操作"来打开逻辑应用程序设计器和配置您的流︰  
![][3]
3.  Twilio 连接器将出现在"API 此资源组中的应用程序"一节中上右手边的库︰  
![][4]
4. 您可以通过单击"Twilio"连接器上的 Twilio 连接器 API 的应用程序放到编辑器。

5.  现在，您可以使用 Twilio 连接器流中。 可以使用"发送消息"操作流程中，发送一条消息。 "发送消息"操作的输入的属性按如下配置︰
    - **从电话号码**-输入您想要发送的消息的类型启用 Twilio 的电话号码。 仅电话号码或从 Twilio 购买的短代码将使用此连接器。
    - **对电话号码**-目标电话号码。 的格式可接受的是: + 跟国家代码，再拨电话号码。 例如，+16175551212。 如果省略 + Twilio 将使用您在开始数字输入国家/地区代码。
    - **文本**-您想要发送的消息的文本。

    ![][5]  
    ![][6]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-twilio/img1.PNG
[2]: ./media/app-service-logic-connector-twilio/img2.PNG
[3]: ./media/app-service-logic-connector-twilio/img3.png
[4]: ./media/app-service-logic-connector-twilio/img4.png
[5]: ./media/app-service-logic-connector-twilio/img5.PNG
[6]: ./media/app-service-logic-connector-twilio/img6.PNG

测试
