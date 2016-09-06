---
ms.openlocfilehash: 681a67ad9de263bc0d6f9f138f810da18c199458
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="BizTalk XML 验证器"
   description="BizTalk XML 验证器"
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

# BizTalk XML 验证器

BizTalk XML 验证连接器可帮助您针对预定义的 XML 架构验证 XML 数据的应用程序。 用户可以使用任一现有架构，或生成架构出平面文件实例，JSON 基于实例或现有的连接器。

##使用 BizTalk XML 验证程序
1. 若要使用 BizTalk XML 验证器，您需要首先创建的 BizTalk XML 验证器 API 的应用程序实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 BizTalk XML 验证器 API 的应用程序。

###配置 BizTalk XML 验证器
BizTalk XML 验证器将架构作为其配置的一部分。 用户可以启动 API 的应用程序配置刀片式服务器，通过启动 API 的应用程序直接从 Azure 的门户网站，或者通过双击该 API 应用程序在设计器图面上。

![BizTalk XML 验证器配置][1]

在 API 的应用程序刀片式服务器，用户可以通过单击*架构*部分配置架构

![BizTalk XML 验证器架构部件][2]

用户可以上载架构从磁盘或生成一个从平面文件实例或 JSON 实例构造函数。

![验证程序的 BizTalk XML 架构][3]


###在设计图面中使用 BizTalk 平面文件编码
配置完成后，用户可以单击*->*，从操作的操作列表中选择。

![BizTalk XML 验证器的操作的列表][4]

####验证 Xml

验证给定的 xml 输入预先配置架构验证的 Xml 操作。

![BizTalk XML 验证程序验证 Xml][5]

参数|类型|参数的说明
---|---|---
输入的 Xml|string|要验证的输入的 Xml

操作对象形式返回输出。 输出结果包含表示响应的 Xml 验证器的模型。 它包含的结果、 架构名称、 根节点和错误说明。

![6]

<!-- References -->
[1]: ./media/app-service-logic-xml-validator/XmlValidator.ClickToConfigure.PNG
[2]: ./media/app-service-logic-xml-validator/XmlValidator.SchemasPart.PNG
[3]: ./media/app-service-logic-xml-validator/XmlValidator.SchemaUpload.PNG
[4]: ./media/app-service-logic-xml-validator/XmlValidator.ListOfActions.PNG
[5]: ./media/app-service-logic-xml-validator/XmlValidator.ValidateXml.PNG
[6]: ./media/app-service-logic-xml-validator/img1.PNG
 

测试
