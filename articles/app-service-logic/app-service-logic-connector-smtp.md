---
ms.openlocfilehash: f3d908651e336088ec6893ddf0b25ed645e5d960
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用 SMTP 连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置 SMTP 连接器或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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


# 开始使用 SMTP 连接器，并将其添加到您的逻辑应用程序
连接到 SMTP 服务器并发送电子邮件，包括带有附件的电子邮件。 SMTP 连接器"发送电子邮件"操作，您可以将电子邮件发送到指定的电子邮件地址。

可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据，作为工作流的一部分。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 SMTP 连接器。 


## 触发器和操作
*触发器*是发生的事件。 例如，在更新订单时或当新客户添加。 *操作*是触发器的结果。 例如，当顺序更新或添加新客户时，给新客户发送电子邮件。

SMTP 连接器可以用作 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 目前，没有为该连接器的触发器。

SMTP 连接器具有以下触发器和操作可用︰

触发器 | 操作
--- | ---
无 | 发送电子邮件


## 创建 SMTP 连接器
连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. 选择**API 的应用程序**和搜索"SMTP 连接器"。
3. **创建**连接器。
4. 输入的名称、 应用程序服务计划，以及其他属性。
5. 输入以下程序包设置︰

    名称 | 是否必需 |  说明
    --- | --- | ---
    用户名称 | 是 | 输入可连接到 SMTP 服务器的用户名。
    Password | 是 | 输入的用户名和密码。
    服务器地址 | 是 | 输入 SMTP 服务器名称或 IP 地址。
    服务器端口 | 是 | 输入 SMTP 服务器的端口号。
    启用 SSL | 否 | 输入*true*通过安全的 SSL/TLS 通道使用 SMTP。

6. 选择**创建**。

## 在您的应用程序逻辑中使用 SMTP 连接器
连接器创建之后，为您的逻辑应用程序现在为一个操作使用 SMTP 连接器。 若要此操作︰

1.  创建新的逻辑应用程序︰  
![][2]
2.  打开**触发器和操作**来打开逻辑应用程序设计器和配置您的工作流︰  
![][3]
3.  在 SMTP 连接器上右手边的库中的"API 此资源组中的应用程序"一节中列出。 选择它︰  
![][4]
4.  选择要自动将其添加到工作流设计器的 SMTP 连接器。

现在可以配置 SMTP 连接器，以便在工作流中使用。 选择**发送电子邮件**的操作和配置输入的属性︰

    属性 | 说明
    --- | ---
    目的 | 请输入收件人的电子邮件地址。 分隔多个电子邮件地址之间用分号 （;）。 例如，输入︰ *recipient1@domain.com;recipient2@domain.com*。
    抄送 | 输入抄送收件人的电子邮件地址。 分隔多个电子邮件地址之间用分号 （;）。 例如，输入︰ *recipient1@domain.com;recipient2@domain.com*。
    Subject | 输入的电子邮件的主题。
    Body | 输入的电子邮件的正文。
    为 HTML | 当此属性设置为 true 时，正文的内容以 html 格式发送。
    密件抄送 | 输入密件抄送收件人的电子邮件地址。 分隔多个电子邮件地址之间用分号 （;）。 例如，输入︰ *recipient1@domain.com;recipient2@domain.com*。
    Importance | 输入的电子邮件的重要性。 选项是普通、 低和高。
    Attachments | 随电子邮件一起发送的附件。 它包含以下字段︰ <ul><li>内容 （字符串）</li><li>内容传输编码 （枚举） （"无"|"") base64</li><li>内容类型 （字符串）</li><li>内容 ID （字符串）</li><li>文件名称 （字符串）</li></ul>

![][5]  
![][6]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-smtp/img1.PNG
[2]: ./media/app-service-logic-connector-smtp/img2.PNG
[3]: ./media/app-service-logic-connector-smtp/img3.png
[4]: ./media/app-service-logic-connector-smtp/img4.PNG
[5]: ./media/app-service-logic-connector-smtp/img5.PNG
[6]: ./media/app-service-logic-connector-smtp/img6.PNG

测试
