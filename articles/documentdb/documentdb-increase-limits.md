---
ms.openlocfilehash: 8d9474921c95217804b4c97e921494e92b8372d8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="请求增加 DocumentDB 帐户限制 |Microsoft Azure" 
    description="了解如何申请对 DocumentDB 的限制，例如允许的集合、 存储的过程和查询子句的数目的调整。" 
    services="documentdb" 
    authors="stephbaron" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/28/2015" 
    ms.author="stbaro"/>

# 请求增加的 DocumentDB 帐户限制

[Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/)有一组默认限制和配额执行进行。  可以通过联系 Azure 支持调整几个配额。  本文说明如何请求的帐户限制增加。

阅读这篇文章之后, 您将能够回答以下问题︰  

-   可以通过联系 Azure 支持调整 DocumentDB 帐户配额？
-   如何请求 DocumentDB 帐户配额调整？

##<a id="AdjustableQuotas"></a> 可调节的 DocumentDB 帐户配额

下表描述了可以通过联系 Azure 支持调整 DocumentDB 配额︰   

|Entity |配额 （标准提供）|
|-------|--------|
|数据库帐户     |5
|每个集合的存储的过程、 触发器和 Udf 的数量       |每个 25
|每个数据库帐户的最大集合    |100
|每个数据库 （100 集） 的最大文档存储    |1 TB
|Udf 的每个查询的最大数量     |1
|每个查询的联接的最大数量    |2
|将 AND 子句每个查询的最大数量      |5
|每个查询的 OR 子句的最大数量       |5
|每个表达式中的值的最大数量       |100
|每分钟创建集合的最大数量    |5
|缩放操作，每分钟的最大数量    |5

##<a id="RequestQuotaIncrease"></a> 申请配额调整
下面的步骤演示如何申请配额调整。

1. 在[Azure 预览门户](https://portal.azure.com)中，单击**浏览**，然后单击**帮助 + 支持**。

    ![启动帮助和支持的屏幕抓图](media/documentdb-increase-limits/helpsupport.png)

2. 在**帮助 + 支持**刀片式服务器，请单击**获取支持**。

    ![创建支持票的屏幕抓图](media/documentdb-increase-limits/getsupport.png) 

3. 在**请求的新支持**刀片式服务器，单击**请求类型**，然后单击**请求类型**刀片式服务器中的**配额**。

    ![支持票请求类型的屏幕抓图](media/documentdb-increase-limits/supportrequest1.png) 

4. 在**订阅**刀片式服务器，请选择承载您的 DocumentDB 帐户订阅。

    ![支持票预订选择器的屏幕抓图](media/documentdb-increase-limits/supportrequest2.png)

5. 在**资源**刀片式服务器，请选择**DocumentDB 帐户**。

    ![支持票资源选取器的屏幕抓图](media/documentdb-increase-limits/supportrequest3.png)

6. 在**支持计划**刀片式服务器，请选择**配额免费支持**。

    ![支持票支持计划选取器的屏幕抓图](media/documentdb-increase-limits/supportrequest4.png)

7. 在**问题**刀片式服务器，选择问题类别**配额或提高请求 DocumentDB 的核心**。

    ![支持票问题类别选择器的屏幕抓图](media/documentdb-increase-limits/supportrequest5.png)

8. 在**描述**刀片式服务器，请输入请求的说明。  请务必包括您请求的特定配额调整，以及应该对其进行调整的帐户。

    ![支持票说明文本框的屏幕抓图](media/documentdb-increase-limits/supportrequest6.png)

9. 单击**创建**。

    ![支持票的屏幕快照创建按钮](media/documentdb-increase-limits/supportrequest7.png)

一旦创建了支持票，您将收到通过电子邮件的支持请求号码。  您还可以通过单击**支持请求****帮助 + 支持**的刀片式服务器查看支持请求。

![支持请求刀片式服务器的屏幕抓图](media/documentdb-increase-limits/supportrequest8.png)
  

##<a name="NextSteps"></a> 下一步行动
- 若要了解有关 DocumentDB 的详细信息，请单击[此处](http://azure.com/docdb)。
 
测试
