---
ms.openlocfilehash: 32d6625a9429763506eec77ecb0beb2083684809
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 DocumentDB 中的一致性级别 |Microsoft Azure" 
    description="DocumentDB 具有四个具有关联的性能级别，以帮助应用程序开发人员进行可预测的一致性可用性延迟权衡的一致性级别。" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/25/2015" 
    ms.author="mimig"/>

# 使用一致性级别最大化可用性和 DocumentDB 的性能

开发人员经常面对强和最终一致性两个极端之间进行选择的难题。 实际情况是，有多个一致性单击-停止这两个极端之间。 在最真实的全球情况下，应用程序受益于正常精细权衡之间的一致性、 可用性和滞后时间。 DocumentDB 提供了四个具有关联的性能级别的明确定义的一致性级别。 这允许应用程序开发人员进行可预测的一致性可用性延迟权衡。  
 
所有的系统资源，包括数据库帐户、 数据库、 集合、 用户和权限读取和查询都高度一致。 一致性级别仅适用于用户定义的资源。 用于查询和读取的操作上定义的用户资源，包括文档、 附件、 存储的过程、 触发器和 Udf，DocumentDB 提供了四种不同的一致性级别︰ 

 - 强
 - 有限的失效 
 - 会话
 - 最终 

这些细致、 明确定义的一致性级别允许您进行声音之间的折衷一致性、 可用性和性能。 这些一致性级别有后盾确保一致的结果，您的应用程序的可预测的性能级别。   

## 对于数据库的一致性级别

您将应用于所有集合 （跨所有数据库） 数据库帐户下的数据库帐户，您可以配置默认的一致性级别。 默认情况下，所有读取和对用户定义资源发出的查询将都使用的数据库帐户上指定的默认一致性级别。 但是，您可以通过指定 [x-ms-一致性-级别] 请求标头降低特定的读取查询请求的一致性级别。 有四种类型的 DocumentDB 复制协议支持的一致性级别-这些简要说明如下。 

>[AZURE.NOTE] 在将来的版本中，我们打算支持重写默认的一致性级别在每个集合的基础上。  

**强**︰ 强一致性保证写只可见后由多数仲裁副本的持久承诺。 写或者同步致力持久的主和次映像的仲裁或中止。 读取被始终承认大多数阅读仲裁-客户端可以永远不会看到未提交或部分写入并始终保证阅读最新确认的写。   
 
强的一致性提供绝对保证数据的一致性，但提供了最低级别的读写性能。  

**Bounded 失效**︰ Bounded 失效一致性保证传播写入读取最 K 前缀后面写入延迟的可能性的总订单。 阅读总是被承认多数仲裁的副本。 读取请求的响应指定其相对新鲜 （根据 K)。  

同时还提供了最低的延迟写入，有限的失效提供读一致性更可预知的行为。 读取由多数仲裁双方均认同，当读取延迟时间不是由系统提供最低。    

>[AZURE.NOTE] 有限的失效程度保证只有在显式读取请求的单调性读取。 用于写入请求回显服务器响应不提供限制的失效的保证。

**会话**︰ 与强和有限失效的一致性级别所提供的全球一致性模型，不同的是"会话"一致性定制特定的客户端会话。 因为它提供了有保证的单调性读取和写入和能够阅读自己写会话一致性就足够了。 读取的请求会话一致性对副本可用于客户端发出的请求的版本 （会话 cookie 的一部分）。  

会话的一致性同时提供最低的延迟写入会话提供可预知的读取的数据的一致性。 也是低延迟为除极少数情况下的，读取将由单个副本中读取。  

**Eventual**: Eventual 一致性是薄弱的客户端可能会收到的值比它有一段时间出现过，之前的一致性。 在没有任何进一步的写操作的情况下，最终将汇聚组内的副本。 读取的请求提供的任何辅助索引。  

最终一致性提供弱读的一致性，但为读取和写入操作提供最低的延迟。 

### 改变数据库的一致性级别

1.  在[Azure 预览门户网站](https://portal.azure.com/)中，请单击**浏览所有**。

2.  在**浏览所有**刀片式服务器，请单击**DocumentDB 帐户**。

3. 在**DocumentDB 帐户**刀片式服务器，选择要修改的数据库帐户。

4. 在帐户刀片式服务器，在**配置**镜头中，单击**默认一致性**拼贴。

5. 选择新的一致性级别，然后单击**保存**。 

    ![屏幕快照的默认一致性拼贴、 一致性设置和保存按钮突出显示](./media/documentdb-consistency-levels/database-consistency-level.png)

## 查询的一致性级别

对于用户定义的资源，默认情况下查询的一致性级别是读取相同。 默认情况下，对每个插入、 替换或删除的集合的文档同步更新索引。 这使得接受文档读取操作的一致性级别相同的查询。 而 DocumentDB 是写优化并支持持续的增长的文档写入，以及同步的索引维护和提供一致的查询服务，您可以配置某些延迟更新其索引的集合。 惰性索引进一步提高写入性能，非常适合于批量接收方案，主要是读密集型工作负载时。  

索引模式|  读取|  查询  
-------------|-------|---------
一致 （默认值）|   选择从强、 Bounded 失效、 会话或 Eventual|    选择从强、 Bounded 失效、 会话或 Eventual|
惰性|   选择从强、 Bounded 失效、 会话或 Eventual|    最终  

作为与读取请求，您可以降低特定查询请求的一致性级别通过指定 [x-ms-一致性-级别] 请求标头。  

## 下一步行动

如果您想要做更多阅读关于一致性级别和权衡，我们建议以下资源︰

-   Doug Terry。 已复制的数据的一致性说明通过棒球。   
[http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
-   Doug Terry。 弱一致的已复制数据的会话保证。   
[http://dl.acm.org/citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
-   Daniel Abadi。 现代分布式数据库系统设计中的一致性权衡︰ 帽只是一部分的故事"。   
[http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html) 
-   Peter Bailis、 Shivaram Venkataraman、 Michael J.富兰克林，Joseph M.Hellerstein、 离子 Stoica。 概率实用部分仲裁包围失效 (PBS)。   
[http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
-   Werner Vogels。 最终一致-再次访问。    
[http://allthingsdistributed.com/2008/12/eventually_consistent.html](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
 
测试
