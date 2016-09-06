---
ms.openlocfilehash: 64707795fc2031e0ae923180340d6c1228e3f0ee
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="媒体服务 REST API，概述 |Microsoft Azure" 
    description="REST API，媒体服务概述" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/11/2015" 
    ms.author="juliako"/>


# REST API，媒体服务概述 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Microsoft Azure 媒体服务接受 OData 基于 HTTP 请求的服务，并且可以响应回中详细 JSON 或原子 + pub。 因为媒体服务符合 Azure 的设计准则，则需要连接到媒体服务时，必须使用每个客户端的 HTTP 标头集，以及一套可以使用的可选标头。 以下部分描述了邮件头时可以使用的 HTTP 谓词创建请求并接收响应从介质服务中。


## 介质服务所支持的标准 HTTP 请求标头

成为媒体服务的每次调用，还有一组需要必须包含在请求中的标头和一组可选标题您可能还要包括。 下表列出了所需的标头︰


页眉|类型|值
---|---|---
授权|持有者|载体是唯一接受的授权机制。 该值还必须包括 ACS 提供的访问令牌。
x-ms-版本|小数|2.11
DataServiceVersion|小数|3.0
MaxDataServiceVersion|小数|3.0



>[AZURE.NOTE] 介质服务使用 OData 公开其基础资产元数据存储库通过 REST Api，因为 DataServiceVersion 和 MaxDataServiceVersion 头应包含在任何请求;但是，如果不是，则当前介质服务假定中使用的 DataServiceVersion 值为 3.0。

下面是一组可选标题︰

页眉|类型|值
---|---|---
日期|RFC 1123 日期|请求的时间戳
接受|内容类型|如下所示的响应请求的内容类型︰<p> -应用程序/json; odata = 详细<p> -应用程序/atom + xml<p> 响应可能有不同的内容类型，如 blob 回迁，其中到成功的响应将包含作为负载 blob 流。
接受编码|Gzip，地道|GZIP 以及 DEFLATE 编码时适用。 注︰ 对于较大的资源，媒体服务可能会忽略此标头并返回未压缩的数据。
接受语言|"en"、"es"，等等。|指定响应的首选的语言。
接受字符集|像"utf-8"字符集类型|默认为 utf-8。
X-HTTP 方法|HTTP 方法|允许客户端或不支持 HTTP 方法放置或删除以使用隧道通过 GET 调用这些方法，如防火墙。
内容类型|内容类型|内容类型放置或 POST 请求在请求正文。
客户端请求 id|String|调用方定义的值标识给定的请求。 如果指定，将作为一种方法来映射该请求响应消息中包含此值。 <p><p>**重要说明**<p>值应该被限制在 2096b (2k)。

## 介质服务所支持的标准 HTTP 响应标头

下面是一组可能会返回给您，这取决于您所请求的资源和您想要执行的操作的标题。


页眉|类型|值
---|---|---
请求 id|String|当前操作，生成的服务的唯一标识符。
客户端请求 id|String|如果有在原始请求中，调用方指定一个标识符。
日期|RFC 1123 日期|处理请求时的日期。
内容类型|因情况而异|响应正文的内容类型。
内容编码|因情况而异|Gzip 或地道，根据需要。


## 介质服务所支持的标准 HTTP 谓词

以下是可以发出 HTTP 请求时使用的 HTTP 动作的完整列表︰


Verb|说明
---|---
获取|返回当前对象的值。
发布|创建一个对象，该对象提供的数据，或提交一个命令。
放置|将替换的对象，或创建一个已命名的对象 （如果适用）。
删除|删除一个对象。
合并|更新现有对象的命名的属性更改。
头部|返回一个对象，用于获取响应的元数据。


## 发现媒体服务模型

要使介质服务实体更加明显，可以使用 $metadata 操作。 它允许您检索所有有效的实体类型、 实体属性、 关联、 功能、 操作和等等。 下面的示例演示如何构造 URI: https://media.windows.net/API/$ 元数据。

应附加"？ api version=2.x"如果想要在浏览器中查看元数据，或在您的请求中未包括的 x-ms-版本标头的 URI 的末尾。


<!-- Anchors. -->


<!-- URLs. -->
  
  [管理门户]: http://manage.windowsazure.com/



 

测试
