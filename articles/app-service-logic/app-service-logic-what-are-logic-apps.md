---
ms.openlocfilehash: 7062fa78c7686e7ebd6239830f875519e608eca6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="逻辑应用程序有哪些？" 
    description="了解有关应用程序服务的逻辑应用程序详细信息" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/08/2015"
    ms.author="klam"/>

#逻辑应用程序有哪些？

| 快速参考 |
| --------------- |
| [逻辑应用程序定义语言](https://msdn.microsoft.com/library/azure/dn948512.aspx?f=255&MSPPError=-2147217396) |
| [逻辑应用程序连接器文档](https://azure.microsoft.com/en-us/documentation/articles/app-service-logic-connectors-list/) |
| [逻辑应用程序论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Azure 应用程序服务使其更易于生成 web、 移动和集成应用程序的开发人员是完全托管的 PaaS （平台即服务）。 逻辑应用程序是此套件的一部分，并允许任何技术用户或开发人员可以自动执行业务流程执行和工作流，通过便捷的可视化设计器。

尤其可贵的是，可以使用[接头][连接器]从我们的市场，以帮助您轻松地解决更复杂的集成方案组合逻辑的应用程序。

![流的应用程序设计器](./media/app-service-logic-what-are-logic-apps/Designer.png)

如果您想要自动复制您的 SQL 数据库中的新记录和邮件前台，或自动查找负 tweets 并将它们发送到可宽延时间通道 

##为什么逻辑应用程序？

逻辑应用程序允许开发人员从触发器启动，然后每同时安全地来处理身份验证的调用应用程序服务 API 的应用程序执行一系列步骤，如检查点和持久执行最佳做法的设计工作流。

如果您要自动执行任何业务流程 （例如查找负 tweets，并发布到您的内部可宽延时间通道或复制新的客户记录从 SQL，到达您的 CRM 系统） 的逻辑应用程序进行集成完全不同的数据源，从云到内部部署简单。 检查出更多灵感，并[开始][创建]现在来看看您可以做为我们[连接器][连接器]。 

更重要的是，与[biztalk]我们[BizTalk API 的应用程序]可以扩展以成熟的[规则引擎][规则]、[交易商合作伙伴管理][tpm]和更多具有能力的集成方案。

- **易于使用的设计工具**的应用程序逻辑可以是设计的端到端的浏览器中。 有关您的公司开始与触发器︰ 从简单的日程安排，任何时候向 tweet 的出现。 然后可以协调任意数量的操作使用连接器格式库。

- **撰写的 SaaS 轻松**-易于描述的甚至撰写任务很难在代码中实现。 逻辑应用程序更加简单，连接不同的系统。 要在 CRM 软件的基础将活动从 Facebook 或者 Twitter 帐户创建任务？ 要连接到您在部署解决方案市场营销您云帐单系统？ 逻辑应用程序是速度最快、 最可靠的方法为这些问题提供解决方案。

- **快速从模板开始**-来帮助您入门，我们提供了使您可以快速创建一些常见的解决方案[库模板的][模板]。 从简单的 SaaS 连接到高级 BizTalk 解决方案和几所生活的-库是最快的方法，以了解应用程序逻辑的力量。

- **内置的可扩展性**-看不到所需的接口？ 逻辑应用程序是应用程序服务套件的一部分，旨在使用 API 的应用程序;您可以轻松创建您自己的 API 应用程序用作连接器。 为您生成一个新应用程序或共享和金钱在市场上。

- **真正的集成马力**-开始轻松并随需要而增长。 逻辑应用程序可以方便地利用 BizTalk，Microsoft 的业界领先的集成解决方案启用集成专业人员，以生成所需的解决方案的能力。 了解更多有关[biztalk] [BizTalk 功能提供应用程序服务]。

## 逻辑应用程序概念

下面是一些构成的逻辑应用程序体验的主要部分。 

- **工作流**的逻辑应用程序提供了对业务流程建模为一系列的步骤或工作流的图形方式。
- **连接器**的逻辑应用程序需要访问的数据和服务。 连接器是一种特殊类型的 API 的应用程序。 创建专门用于帮助您将连接到并使用您的数据时。 请参阅连接器现在可[使用连接器]的[连接器]的列表。
- **触发器**的某些连接器还可以充当一个触发器。 触发器启动工作流基于特定的事件，如电子邮件或在 Azure 存储帐户的更改何时到达的一个新实例。
-  **操作**在工作流中的触发器调用某个操作后的每一步。 通常，每个操作将映射到连接器或自定义的 API 应用程序上的操作。
- 对于更高级的集成方案，Azure 应用程序服务**BizTalk** -包括从 Biztalk 功能。 Biztalk 是 Microsoft 的业界领先的集成平台。 BizTalk API 的应用程序，可以方便地包括验证、 转换、 规则和详细到您的应用程序逻辑的工作流。 了解[什么是 BizTalk API 应用程序][biztalk]中的更多。

## 入门教程

入门的逻辑应用程序，请按照[创建逻辑应用程序][创建]教程。

在 Azure 应用程序服务平台上的详细信息，请参阅[Azure 应用程序服务][appservice]。

[biztalk]: app-service-logic-what-are-biztalk-api-apps.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[创建]: app-service-logic-create-a-logic-app.md
[连接器]: app-service-logic-connectors-list.md
[tpm]: app-service-logic-create-a-trading-partner-agreement.md
[规则]: app-service-logic-use-biztalk-rules.md
[模板]: app-service-logic-use-logic-app-templates.md
 

测试
