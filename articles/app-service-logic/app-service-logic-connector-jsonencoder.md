---
ms.openlocfilehash: 94f967bbb062c6ba7c875fe57ba7753582492e3a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在逻辑的应用程序中使用 BizTalk JSON 编码器接口 |Microsoft Azure 应用程序服务 "
   description="如何创建和配置 BizTalk JSON 编码器接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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

# 开始使用 BizTalk JSON 编码并将其添加到您的逻辑应用程序 
BizTalk JSON 编码解码接口，有助 JSON 和 XML 数据之间您的应用程序互操作。 它可以将给定的 JSON 实例为 XML，反之亦然。

对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 BizTalk JSON 编码。 

## 使用 BizTalk JSON 编码
若要使用 BizTalk JSON 编码，您需要首先创建的 BizTalk JSON 编码 API 的应用程序实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 BizTalk JSON 编码 API 的应用程序。

## 逻辑应用程序设计器图面中使用 BizTalk JSON 编码
请按照步骤[创建逻辑应用程序]。 为一个操作，可以使用 BizTalk JSON 编码。 它没有任何触发器。

### 操作
- 从右窗格中，单击在 BizTalk JSON 编码

    ![动作设置][3]
- 单击->

    ![操作列表][4]
- BizTalk JSON 编码支持两种操作。 选择*为 JSON 的 Xml*

    ![对 JSON 输入的 Xml][5]
- 为该操作提供输入，并对其进行配置

    ![编码并发送配置][6]

参数|类型|参数的说明
---|---|---
输入的 Xml|对象|输入 Xml 内容
删除外部信封|string|设置标志以从 Xml 内容中删除根节点

该操作返回的 json 表示形式的输入内容。

## 用做更多您的连接器
创建连接器时，可以将其添加到使用逻辑应用程序的业务流程中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视 API 的应用程序和连接器](../app-service-api/app-service-api-manage-in-portal.md)。

<!--References -->
[1]: app-service-logic-connector-tpm
[2]: app-service-logic-create-a-trading-partner-agreement
[3]: ./media/app-service-logic-json-encoder/ActionSettings.PNG
[4]: ./media/app-service-logic-json-encoder/ListOfActions.PNG
[5]: ./media/app-service-logic-json-encoder/EncodeInput.PNG
[6]: ./media/app-service-logic-json-encoder/EncodeConfigured.PNG

<!--Links -->
[创建一个逻辑应用程序]: app-service-logic-create-a-logic-app.md

测试
