---
ms.openlocfilehash: 364440e1e827682a2e9df410addb1ae44ac437f8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="掩蔽的审核和动态数据的 SQL 数据库下层客户端支持 |Microsoft Azure" 
    description="掩蔽的审核和动态数据的 SQL 数据库下层客户端支持" 
    services="sql-database" 
    documentationCenter="" 
    authors="nadavhelfman" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2015" 
    ms.author="nadavhelfman"/>
 
# SQL 数据库的审核和动态数据掩盖下层客户端支持 


[审核](sql-database-auditing-get-started.md)和[动态数据屏蔽](sql-database-dynamic-data-masking-get-started.md)处理 SQL 支持 TDS 重定向的客户端。 

任何客户端实现 TDS 7.4 也应支持重定向。 例外情况包括 JDBC 4.0 中的重定向功能不完全支持和未实现的重定向的 Node.JS 的单调。

对于"普通客户"，即哪些支持 TDS 版本 7.3 和下面的连接中的服务器 FQDN 字符串应修改︰

在连接字符串中的原始服务器 FQDN: <*服务器名称*>。 database.windows.net

修改的连接字符串中的服务器 FQDN: <*服务器名称*>.database。**安全**。 windows.net

"下层客户端"部分列表中包含︰ 

- .NET 4.0 和更低，
- ODBC 10.0 及以下。
- JDBC 4.0 和更低 （虽然 JDBC 4.0 支持 TDS 7.4，TDS 重定向功能不完全支持）
- （对于 Node.JS) 单调乏味

**评语︰**上述服务器 FDQN 修改可能也用于应用而无需为每个数据库 （临时缓解） 中配置步骤需要 SQL Server 级别的审核策略。     

 
