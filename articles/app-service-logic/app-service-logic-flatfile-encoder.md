---
ms.openlocfilehash: 036831921b4442cd5c8234c9483bc73c731b9ebd
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="BizTalk 平面文件编码" 
   description="BizTalk 平面文件编码" 
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

# BizTalk 平面文件编码

BizTalk 平面文件编码解码接口，有助您的应用程序互操作之间平面文件数据 (示例 excel，csv) 和 XML 数据。 它可以将转换为 XML，反之亦然的给定平面文件实例。

##使用 BizTalk 平面文件编码
1. 若要使用 BizTalk 平面文件编码，您需要首先创建的 BizTalk 平面文件编码 API 的应用程序实例。 进行这种线内创建一个逻辑应用程序时，或者从 Azure 市场选择 BizTalk 平面文件编码 API 的应用程序。

###配置 BizTalk 平面文件编码
BizTalk 平面文件编码器采用架构作为其配置的一部分。 用户可以启动 API 的应用程序配置刀片式服务器，通过启动 API 的应用程序直接从 Azure 的门户，或者通过双击该 API 应用程序在设计器图面上。

![BizTalk 平面编码器配置文件][1]

在 API 的应用程序刀片式服务器，用户可以通过单击*架构*部分配置架构。

![BizTalk 平面文件编码架构部分][2]

用户可以上载架构从磁盘或生成一个从平面文件实例或 JSON 实例构造函数。

![BizTalk 平面文件编码架构部分][3]


###在设计图面中使用 BizTalk 平面文件编码
配置完成后，用户可以单击*->*并从操作列表中选择一个操作。

![BizTalk 平面文件编码的操作列表][4]

####平面文件为 Xml

![BizTalk 平面文件编码的操作列表][5]

参数|类型|参数的说明
---|---|---
平面文件|string|输入平面文件的内容
架构名称|string|这表示输入平面文件的架构的名称
根名称|string|平面文件架构的根节点名称


操作输出 Xml 字符串的形式返回输出。 输出 Xml 包含的 xml 表示形式输入平面文件内容。

####为平面文件的 Xml

![BizTalk 平面文件编码的操作列表][6]

参数|类型|参数的说明
---|---|---
输入的 Xml|string|输入 Xml 内容

操作返回字符串的平面文件形式输出。 输出结果包含输入的 xml 内容的平面文件表示。

<!-- References -->
[1]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.ClickToConfigure.PNG
[2]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.SchemasPart.PNG
[3]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.SchemaUpload.PNG
[4]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.ListOfActions.PNG
[5]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.FlatFileToXml.PNG
[6]: ./media/app-service-logic-flatfile-encoder/FlatFileEncoder.XmlToFlatFile.PNG
 

测试
