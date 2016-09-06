---
ms.openlocfilehash: 50c151395e74d1f58697b8c99c2787946853c3f9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="B2B 邮件跟踪" 
   description="本主题介绍了 B2B 处理的跟踪" 
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
   ms.date="07/01/2015"
   ms.author="rajram"/>


# B2B 邮件跟踪

## B2B 的跟踪信息
B2B 通信涉及贸易合作伙伴之间所处理的消息。 关系被指两个贸易伙伴之间的协议。 建立通信后然后需要有一种监视如果像预期的那样发生通信的方法。 作为 Azure 应用程序服务的一部分使 B2B API 的应用程序的一部分，我们已经启用了跟踪数据并通过 Azure 门户也出现相同。 

## AS2
一旦您已创建 AS2 API 的应用程序，然后浏览到该实例的实例，然后转到跟踪部分。 本文所述人将不能查看所有 AS2 跟踪信息，并且还通过筛选器刀片出来后对其进行筛选。

![][1]  

## EDIFACT
一旦您创建了 EDIFACT API 的应用程序，然后浏览到该实例的实例，然后转到跟踪部分。 此处所人都不能查看跟踪信息的所有 EDIFACT 和还通过出来后的筛选器进行筛选。
另外一个将能够查看交换数据、 分组级别的数据和事务组级别的数据到视图中的一个步骤。 

如果作为 EDIFACT 协议相关联的贸易合作伙伴管理 API 应用程序中的一部分创建的批次的批处理部分将列出所有这些批次。 一个能够单步执行批处理来构成活动消息 （如果有的话） 的邮件，也在过去已经完成的批次的信息，请参阅。

![][2]      

## X12
一旦您已经创建 X12 API 应用程序然后浏览到该实例，然后转到跟踪部件的实例。 本文所述人将不能查看跟踪信息的所有 X12 和还通过出来后的筛选器进行筛选。
另外一个将能够查看交换数据、 分组级别的数据和事务组级别的数据到视图中的一个步骤。 

如果已创建批 X12 的一部分协议相关联的贸易合作伙伴管理 API 应用程序那么批处理部分将列出所有这些批次。 一个能够单步执行批处理来构成活动消息 （如果有的话） 的邮件，也在过去已经完成的批次的信息，请参阅。 

X12 和 EDIFACT 具有类似跟踪视图。 

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.jpg
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.jpg 

测试
