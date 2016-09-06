---
ms.openlocfilehash: c05a66ff0acf2fa7819d9dae5e17c0272edd73af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 HTTP 侦听程序和连接器 |Microsoft Azure 应用程序服务 "
   description="如何创建和配置 HTTP 侦听器和 HTTP 操作接口或 API 应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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
   ms.author="prkumar"/>


# 开始使用 HTTP 侦听程序和 HTTP 操作并将其添加到您的逻辑应用程序
直接连接到 HTTP 资源侦听 HTTP 请求并配置 HTTP web 请求。 有某些情况下，您可能需要使用直接的 HTTP 连接，包括︰

1.  若要开发支持 web 或移动用户交互前端应用程序的逻辑。
2.  若要获取并处理来自 web 服务的数据，没有框中接口的针脚。
3.  执行已公开为 web 服务，但不是能用作 API 的应用程序的操作。

对于这些情况，有两个选项︰

1. **HTTP 侦听程序**︰ 此连接器用作触发器并侦听 HTTP 请求配置终结点。 配置终结点上收到一个调用时，它会触发新的流程实例并传递到流中进行处理的请求中收到的数据。 它还可以配置为自动启动，或使您可以构建基于数据流执行响应并发送到调用方的响应流的响应传入的请求。
2. **HTTP 操作**︰ 这允许您在 internet 上配置到任何可用的终结点的 web 请求、 获取返回的响应，并使其可用于其他操作中使用的流程。

可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 HTTP 连接器。 

## 创建您的逻辑应用程序的 HTTP 侦听器
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. 搜索"HTTP"，选择 HTTP 侦听程序，然后选择**创建**。
3.  按如下所述配置 HTTP 侦听程序︰  
![][1]

4.  安装程序包设置时，您将看到下列侦听器是否应自动响应，或者要求在发送选项显式响应。 将此值设置为**False**可发送自己的响应︰  
![][2]

5.  单击**确定**以创建。
6.  API 的应用程序实例创建之后，打开要配置安全设置。 HTTP 侦听程序目前支持基本身份验证。 您可以配置此 HTTP 侦听程序打开时使用安全选项︰  
![][3]
  
    **已知问题**  *安全的设置显示"无"作为默认值，但是它未定义。您必须更改该设置为基本并无保存它以确保正确配置了 HTTP 侦听器之前回到。*

7. 最后，将 API 的应用程序的安全设置设置为公共 （匿名），以允许外部客户机访问终结点。 此设置可在下"的所有设置 > 应用程序设置"HTTP 侦听程序 API 应用程序︰
![][10]

完成后，您现在可以创建一个逻辑应用程序要使用 HTTP 侦听程序。

## 在您的应用程序逻辑中使用 HTTP 侦听程序
一旦创建 API 的应用程序，为您的逻辑应用程序现在为触发器使用 HTTP 侦听程序。 要执行此操作，您需要︰

4.  创建新的逻辑应用程序。
5.  打开"触发器和操作"来打开逻辑应用程序设计器和配置您的流。 在库列出 HTTP 侦听程序。 请选择它。
6.  现在可以根据需要设置的 HTTP 方法和相对 URL 需要监听器触发流程︰  
![][4]  
![][5]

7.  要获取完整的 URI，请双击 HTTP 侦听程序以查看它的配置和 API 的应用程序的"主机"中复制的 URL:  
![][6]
8.  现在可以使用数据接收 HTTP 请求流中的其他操作中，如下所示︰  
![][7]  
![][8]
9.  最后，要发送响应，请添加另一个 HTTP 侦听程序并选择发送的 HTTP 响应操作。 请求 ID 设置为从 HTTP 侦听程序，获得了申请 Id 和填充响应正文和您想要重新返回的 HTTP 状态︰  
![][9]

## 使用 HTTP 操作
HTTP 操作逻辑应用程序本身支持，不需要 API 的应用程序创建第一个能够使用它。 您可以逻辑应用程序中任何位置插入 HTTP 操作并选择 URI、 标头和正文调用。
HTTP 操作支持的客户端安全方面的多个选项。 若要使用它们，请查看文章[下面](http://aka.ms/logicapphttpauth)。

HTTP 操作的输出是页眉和正文中，它可以用于进一步下行类似于如何使用其他操作和连接器的输出流中。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png

测试
