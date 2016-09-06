---
ms.openlocfilehash: 781c18ecfce43ce801148e3d1cc9de5df5834505
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="BizTalk XPath 提取程序"
   description="BizTalk XPath 提取程序"
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

#BizTalk XPath 提取程序

BizTalk XPath 提取连接器有助于应用程序查找并从基于给定的 XPath 的 XML 内容中提取数据。

##使用 BizTalk Xpath 提取程序
1. 若要使用 BizTalk Xpath 提取器，您需要首先创建的 BizTalk Xpath 提取器 API 的应用程序实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 BizTalk Xpath 提取器 API 的应用程序。

    >[AZURE.NOTE] 没有与 BizTalk Xpath 提取程序相关的配置设置。
2. [创建新的逻辑应用程序]。 打开"触发器和操作"创建逻辑应用程序打开逻辑应用程序设计器来配置您的流中。
3. 在设计器中，右窗格中列出了 API 应用程序可用于生成与您的流。 找到"BizTalk 抽风机 XPath"。 选择此将添加到您的流的 Xpath 提取器，并将设置它的实例。
2. 一旦设置，设计器会显示了与 BizTalk XPath 提取器 API 的应用程序关联的操作。

![BizTalk XPath 提取器选择操作][1]

3. 选择"提取使用 XPath"

"提取使用 XPath"计算在给定的输入 Xml 输入的 xpath 表达式。

![BizTalk XPath 提取器输入][2]

参数|类型|参数的说明
---|---|---
XPath|string|在 xml 的查询路径。
输入的 Xml|string|输入的 Xml 内容。

该操作将输出返回作为字符串的结果。 结果包含在 Xml 的查询路径的值。

<!-- References -->
[1]: ./media/app-service-logic-xpath-extract/ChooseAction.PNG
[2]: ./media/app-service-logic-xpath-extract/ConfigureInput.PNG

<!-- Links -->
[创建新的逻辑应用程序]: app-service-logic-create-a-logic-app.md

测试
