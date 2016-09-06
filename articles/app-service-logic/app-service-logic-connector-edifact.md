---
ms.openlocfilehash: 1c191871cda75132958a2ae73fcab67fed8160dc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在应用程序逻辑中使用 BizTalk Edifact 连接器 |Microsoft Azure 应用程序服务" 
   description="如何创建和配置 BizTalk Edifact 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它" 
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

# 开始使用 BizTalk Edifact 连接器并将其添加到您的逻辑应用程序  
使用 Edifact 服务来接收和发送邮件，通过在企业对企业通讯 Edifact 协议。 Edifact 通常又称为为 ASC Edifact 或鉴定标准委员会 Edifact 和各行业广泛使用。

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 BizTalk Edifact 连接器。 

## 先决条件
- TPM API 的应用程序︰ 创建之前 Edifact 连接器，您需要创建一个[BizTalk 贸易合作伙伴管理连接器][1]。
- SQL Azure 数据库︰ 每个 B2B API 应用程序需要自己 Azure 的 SQL 数据库。
- Azure 服务总线︰ 这是可选的并且仅在批处理中使用。

## 使用 Edifact 连接器
若要使用 Edifact 连接器，您需要首先创建 AS2 连接器 API 的应用程序的实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 AS2 连接器 API 的应用程序。

## 配置 Edifact 连接器
贸易伙伴是实体参与 B2B 的 （企业对企业） 通信。 当两个伙伴建立了关系时，这被称为一种协议。 定义的协议基于通信这两个合作伙伴希望获得和协议或特定的传输。

创建一个贸易伙伴协议所涉及的步骤进行了说明[此处][2]。

## 逻辑应用程序设计器图面中使用 Edifact 连接器
为触发器或某一操作，可以使用 Edifact 连接器。

### 触发器
- 启动 Azure 逻辑应用程序流设计器
- 从右窗格中单击 Edifact 连接器︰  
![触发器设置][3]
- 单击->:  
![触发器选项][4]
- EDIFACT 接口公开一个触发器。 选择*发行批次*︰  
![发行批输入][5]
- 此触发器有没有输入。 单击->:  
![配置版本批][6]
- 作为输出的一部分，该连接器返回 Edifact 负载、 协议 id，以及批处理消息是否，或没有的信息。

### 操作
- 从右窗格中单击 Edifact 连接器︰  
![动作设置][7]
- 单击->:  
![操作列表][8]
- Edifact 连接器支持的许多操作。 选择*编码*︰  
![对输入进行编码][9]
- 为该操作提供输入，并对其进行配置︰  
![编码配置][10]

    参数|类型|参数的说明
---|---|---
内容|string|XML 消息
协议 ID|integer|协议 ID
为批处理消息|布尔|为批处理消息
数据元素分隔符|string|数据元素分隔符
复合元素分隔符|string|复合元素分隔符
段终止符|string|段终止符
十进制点指示器|string|十进制点指示器
重复分隔符|string|重复分隔符
转义字符|string|转义字符
替换字符|string|替换字符
段终止符后缀|string|段终止符后缀

该操作返回一个对象，该对象包含 EDIFACT 负载在成功完成。

## 用做更多您的连接器
创建连接器时，可以将其添加到使用逻辑应用程序的业务流程中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视 API 的应用程序和连接器](../app-service-api/app-service-api-manage-in-portal.md)。


<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-edifact/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-edifact/ListOfTriggers.PNG
[5]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerInput.PNG
[6]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerConfigured.PNG
[7]: ./media/app-service-logic-connector-edifact/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-edifact/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-edifact/EncodeInput.PNG
[10]: ./media/app-service-logic-connector-edifact/EncodeConfigured.PNG

测试
