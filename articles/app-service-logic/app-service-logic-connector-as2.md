---
ms.openlocfilehash: 0928da295c21c5c42daf734d6b605d2bb651e110
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="在应用程序逻辑中使用 AS2 连接器 |Microsoft Azure 应用程序服务" 
   description="如何创建和配置 AS2 连接器或 API 的应用程序以及在 Azure 应用程序服务中的一个逻辑应用程序中使用它" 
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

# 开始使用 AS2 连接器并将其添加到您的逻辑应用程序
使用 AS2 连接器接收并通过 AS2 发送邮件 (适用性语句 2) 在公司间通信的传输协议。 数据传输安全可靠地通过互联网。 使用数字证书和加密实现安全。

您可以添加 AS2 Connecto 对业务工作流程和过程数据作为业务到业务逻辑应用程序中工作流的一部分。 

## 触发器和操作
触发器启动基于特定的事件，如从伙伴 AS2 消息的到达一个新实例。 对策是结果，像后收到 AS2 消息，然后使用 AS2 发送消息。

AS2 连接器可以用作触发器或 JSON 和 XML 格式的逻辑应用程序，并支持数据中的操作。 AS2 连接器具有以下触发器和操作可用︰ 

触发器 | 操作
--- | ---
接收和解码 | 编码并发送

## 若要开始的要求
可供 AS2 连接器之前，必须由您创建以下各项︰

要求 | 说明
--- | ---
TPM API 的应用程序 | AS2 连接器之前，必须创建一个[BizTalk 贸易合作伙伴管理连接器][1]。 <br/><br/>**请注意**知道您的 TPM API 应用程序的名称。 
SQL azure 数据库 | 存储 B2B 项目包括合作伙伴、 架构、 证书和 agreeements。 每个 B2B API 应用程序需要自己 Azure 的 SQL 数据库。 <br/><br/>**请注意**将连接字符串复制到此数据库。<br/><br/>[创建 SQL Azure 数据库](../sql-database-get-started.md)
Azure Blob 存储容器 | 启用 AS2 存档后，商店将消息属性。 如果您不需要 AS2 邮件存档，不需要存储容器。 <br/><br/>**请注意**如果要启用存档，请复制到此 Blob 存储的连接字符串。<br/><br/>[关于 Azure 存储帐户](../storage-create-storage-account.md)。

## 创建 AS2 连接器

连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰  

1. 在 Azure 的 startboard 中，选择**市场**。
2. "AS2 连接器"的搜索，请选择它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性。
4. 输入以下程序包设置︰

    属性 | 说明
--- | --- 
数据库连接字符串 | 输入您创建的 Azure SQL 数据库的 ADO.NET 连接字符串。 复制连接字符串时，密码不会添加到连接字符串。 请确保在连接字符串中输入密码，在粘贴之前。
启用传入消息存档 | 可选项。 启用此属性来存储传入 AS2 消息从伙伴处接收的消息属性。 
Azure Blob 存储连接字符串  | 输入您创建的 Azure Blob 存储容器的连接字符串。 当启用存档时，编码和解码的消息存储在此存储容器。
TPM 实例名称 | 输入的**BizTalk 贸易合作伙伴管理**API 应用程序以前创建的名称。 当创建 AS2 连接器时，该连接器执行 AS2 协议将在此特定的 TPM 实例。

5. 选择**创建**。 

贸易伙伴是实体参与 B2B 的 （企业对企业） 通信。 当两个伙伴建立了关系时，这被称为一种协议。 定义的协议基于通信这两个合作伙伴希望获得和协议或特定的传输。

创建一个贸易伙伴协议所涉及的步骤记录[这里][2]。

## 为触发器中使用的连接器

1. 在创建或编辑逻辑应用程序，选择从右窗格中创建 AS2 连接器︰  
    ![触发器设置][3]

2. 单击向右箭头 →︰  
    ![触发器选项][4]

3. AS2 连接器公开一个触发器。 选择*接收和解码*︰  
    ![接收并解码输入][5]

4. 此触发器有没有输入。 单击向右箭头 →︰  
    ![接收并解码配置][6]

作为输出的一部分，该连接器返回 AS2 负载以及 AS2 特定元数据。

AS2 负载作为 POST 到 https://{Host 的 URL 时，将触发触发器} / 解码。  设置 API 应用程序中查找主机 URL。  您可能需要更改为公共 （经过身份验证或匿名） API 的应用程序在应用程序设置的访问级别。

## 为操作中使用的连接器
1. 之后您的触发器 （或选择手动运行此逻辑），添加 AS2 连接器从右窗格中创建︰  
    ![动作设置][7]

2. 单击向右箭头 →︰  
    ![操作列表][8]

3. AS2 连接器支持只有一个操作。 选择*编码并发送*︰  
    ![编码，并将输入发送][9]

4. 操作输入的输入并对其进行配置︰  
    ![编码并发送配置][10]

    参数包括︰ 

    参数 | 类型 | 说明
--- | --- | ---
有效负载 | 对象| 对有效负载进行编码，然后发送到配置的终结点使用的内容。 作为一个 JSON 对象提供所需要的负载。
从 AS2 | string | AS2 AS2 消息的发件人标识。 此参数用于查找适当用于发送邮件的协议。
AS2 到 | string | AS2 AS2 消息的接收者的标识。 此参数用于查找适当用于发送邮件的协议。
合作伙伴 URL | string | 合作伙伴需要向其发送消息的终结点。
启用存档 | 布尔 | 确定是否应该存档的出站邮件。

该操作返回成功完成 HTTP 200 响应代码。

## 用做更多您的连接器
对逻辑应用程序在多个[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。

<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-as2/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-as2/TriggerOptions.PNG
[5]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeInput.PNG
[6]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeConfigured.PNG
[7]: ./media/app-service-logic-connector-as2/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-as2/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-as2/EncodeAndSendInput.PNG
[10]: ./media/app-service-logic-connector-as2/EncodeAndSendConfigured.PNG

测试
