---
ms.openlocfilehash: 78383bd727dbc570eb2ecb40ae9667f2a98e86cd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="AS2 连接器消息存档 |Microsoft Azure 应用程序服务" 
   description="如何存档或存储 AS2 连接器 Azure 应用程序服务中的邮件" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2015"
   ms.author="rajram"/>


# AS2 连接器消息的存档概述

**AS2 连接器**公开存档的邮件的能力。 存档是包设置**Azure Blob 容器**中存储消息。 

存档的邮件和确认 (MDNs) 的两个点表现︰

1. **接收/解码触发器**︰ 一旦 API 的应用程序实例接收消息都必须存档 
2. **编码/发送操作**︰ 编码的消息都必须存档，毕竟处理已完成，只是之前将其发送到该伙伴 

## 如何︰ 检索存档消息的 URL

浏览到 AS2 连接器 API 的应用程序实例，然后单击跟踪。 通过使用筛选器参数的跟踪信息缩小。 一旦您的邮件视图中，单击要查看其详细的视图。 邮件存档的 URL 将显示在此详细视图中︰  
![][1]  

## 如何︰ 检索已存档的邮件

使用 URL 检索上面从 Azure Blob 存储中检索已存档的邮件。


<!--Image references-->
[1]: ./media/app-service-logic-archive-as2-messages/Tracking.jpg
 

测试
