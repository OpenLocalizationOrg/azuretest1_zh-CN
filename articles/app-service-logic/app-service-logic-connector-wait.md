---
ms.openlocfilehash: 1c467f2bb98c072c6b8020717a04cd8cc37c8548
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在应用程序逻辑中使用等待连接器 |Microsoft Azure 应用程序服务" 
   description="如何创建和配置等待接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajeshramabathiran" 
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

# 开始使用等待连接器并将其添加到您的逻辑应用程序
等待连接器可以延迟指定的持续时间或指定时间的事件直至其执行的应用程序。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加等待连接器。 逻辑应用程序中使用时，它可以用于延迟执行。

## 使用等待连接器
若要使用等待连接器，您需要首先创建等待连接器 API 的应用程序的实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择等待连接器 API 的应用程序。

## 逻辑应用程序设计器图面中使用等待连接器
等待连接器可以用作某项操作。 它没有任何触发器。

### 操作
- 从右窗格中单击等待连接器︰  
![操作列表][1]
- 等待连接器支持两种操作︰ 
    - 延迟
    - 延迟到
     
- 选择*延迟*︰  
![延迟输入][2]
- 为该操作提供输入，并对其进行配置︰  
![配置操作][3]

参数|类型|参数的说明
---|---|---
以分钟为单位的工期|integer|以分钟为单位的延迟时间


## 用做更多您的连接器
创建连接器时，可以将其添加到使用逻辑应用程序的业务流程中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视 API 的应用程序和连接器](../app-service-api/app-service-api-manage-in-portal.md)。

<!--References -->
[1]: ./media/app-service-logic-wait/ListOfActions.PNG
[2]: ./media/app-service-logic-wait/DelayInput.PNG
[3]: ./media/app-service-logic-wait/ActionConfigured.PNG

测试
