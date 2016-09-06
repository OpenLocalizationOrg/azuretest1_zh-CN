---
ms.openlocfilehash: 4c3d7eff6c85d2e079b2ee87a97d48168f631cd1
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="使用 BizTalk 贸易合作伙伴管理连接器中的逻辑应用程序 |Microsoft Azure 应用程序服务" 
   description="如何创建和配置 BizTalk 贸易合作伙伴管理接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它" 
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

# 开始使用 BizTalk 贸易合作伙伴管理并将其添加到您的逻辑应用程序
可以使用 BizTalk 贸易合作伙伴管理 (TPM) 服务定义并保持业务到业务关系，例如，合作伙伴和协议以及相关的项目，如架构和证书。 然后可以通过相关的 API 服务如 AS2、 EDIFACT、 和 X12 强制执行这些关系。

TPM API 应用程序是基本要求 AS2 连接器，X12 的 API 的应用程序，并 EDIFACT API 的应用程序。 您可以添加 BizTalk 贸易合作伙伴管理业务工作流程和过程数据作为业务到业务逻辑应用程序中工作流的一部分。 

## 先决条件
- 您需要创建一个新的 TPM API 的应用程序之前首先，创建一个空的 SQL Azure 数据库空 SQL Azure 数据库的。

## 了解合作伙伴、 协议和配置文件
若要了解有关贸易伙伴协议的详细信息，请单击[此处][1]。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--References-->
[1]: app-service-logic-create-a-trading-partner-agreement.md
测试t
