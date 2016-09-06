---
ms.openlocfilehash: a4202d0dd94951361bca3e05e71197ea3a8c3600
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在应用程序逻辑中使用框连接器 |Microsoft Azure 应用程序服务"
   description="如何创建和配置框中接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
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
   ms.author="andalmia"/>

# 入门框连接器并将其添加到您的逻辑应用程序 
连接到您的框获取、 上载、 删除。 和更多的文件。 逻辑应用程序中使用连接器作为"工作流"的一部分。 

您可能需要框，它允许您与任何人 – 安全地共享数据，即使它们位于防火墙之外工作的方案。 可以触发逻辑应用程序基于各种数据源和提供接口来获取和处理数据作为流程的一部分。


## 触发器和操作
框中的库应用程序提供的框中进行交互的机制作为操作︰

**操作**︰ 操作可让您帐户上的框中配置的逻辑应用程序执行预定义的操作。 以下是可以使用框中连接器的框帐户执行的操作︰

一。 *列出的文件︰*此操作将返回文件夹中所有文件的信息。 该操作所需的参数的列表︰  

参数名称 | 说明 | 是否必需
--- | --- | ---
文件夹路径 | 对列表中的文件夹的路径。 | 是

> [AZURE.NOTE] 它不返回任何文件的内容。

b。 *获取文件︰*此操作将检索包括其内容和属性的文件。 该操作所需的参数的列表︰

参数名称 | 说明 | 是否必需
--- | --- | ---
文件路径 | 在该文件所在的文件夹的路径。 | 是
文件类型 | 指定该文件是否为文本或二进制文件。 | 否

> [AZURE.NOTE] 此操作不会阅读它后删除该文件。


c。 *上载的文件*︰ 如名称所示，该操作将文件上载到框帐户。 如果文件已经存在，然后不覆盖，则会引发错误。 该操作所需的参数的列表︰

参数名称 | 说明 | 是否必需
--- | --- | ---
文件路径 | 该文件的路径。 | 是
文件内容 | 内容要上载的文件。 | 是
内容传输编码 | 编码内容的类型︰ Base64 或无。 | 

d。 *删除文件*︰ 行动从文件夹中删除指定的文件。 如果找不到文件/文件夹，则会引发异常。 该操作所需的参数的列表︰

参数名称 | 说明 | 是否必需
--- | --- | ---
文件路径 | 完成包括文件夹的文件路径。 | 是


## 为您的逻辑应用程序创建框连接器

连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. 搜索"框中连接器"，选中它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性︰  
    ![][1]
4. 选择**创建**。


## 在您的应用程序逻辑中使用框连接器

创建您的 API 应用程序后，在您的逻辑应用程序现在为一个操作使用框连接器。 若要此操作︰

1. 逻辑应用程序中打开**触发器和操作**来打开逻辑应用程序设计器和配置您的流。 在库列出框连接器。 选择它可自动添加到您在逻辑应用程序设计器。

    > [AZURE.NOTE] 如果在逻辑的应用程序的开始处选择框连接器时，它可以用作触发器。 否则，可以使用该连接器，分期付款中采取操作。 框中连接器没有撰写本文的所有触发器。

2. 身份验证和授权逻辑应用程序上以您的名义执行操作。 选择框中的连接器上的**授权**:  
    ![][2]

3. 输入框帐户，您要在其执行的操作的详细信息中的符号︰  
    ![][3]

4. 逻辑应用程序访问到您的帐户，在以您的名义执行操作︰  
    ![][4]

5. 显示的操作的列表，您可以选择您想要执行的相应操作︰  
    ![][5]

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--Image references-->
[1]: ./media/app-service-logic-connector-box/image_0.jpg
[2]: ./media/app-service-logic-connector-box/image_1.jpg
[3]: ./media/app-service-logic-connector-box/image_2.jpg
[4]: ./media/app-service-logic-connector-box/image_3.jpg
[5]: ./media/app-service-logic-connector-box/image_4.jpg

测试
