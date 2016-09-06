---
ms.openlocfilehash: 81a7f78cd453cfa33cc8b06e210d371364718ae6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 的 SQL 数据库的连接问题" 
    description="识别和确定 SQL 数据库连接失败。" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.devlang="NA" 
    ms.workload="data-management" 
    ms.topic="article" 
    ms.tgt_pltfrm="NA" 
    ms.date="07/24/2015" 
    ms.author="sstein"/>


# SQL 数据库的连接问题

本文提供有关如何使用各种疑难解答时无法连接到 SQL Azure 数据库的概述。


## 确定连接问题是特定于单个应用程序，或者是否只是不能连接到数据库？

使用 SQL Server 管理 Studio 或 SQLCMD。EXE，请验证可以连接到数据库。

- 连接到 SQL 数据库与 SQL Server 管理 Studio (SSMS) 的说明，请参阅[如何连接到与 SSMS SQL Azure 数据库](sql-database-connect-to-database.md)。
- 连接到 SQL 数据库使用 SQLCMD 的说明，请参阅[如何︰ 连接到 Azure SQL 数据库使用 sqlcmd](https://msdn.microsoft.com/library/azure/ee336280.aspx)。



## 是尝试连接到 SQL 数据库在 Azure 上运行的应用程序 （例如，是未能云服务或 Web 应用程序连接到数据库的应用程序）？

请确保该防火墙规则以允许所有 Azure 服务启用服务器数据库。

- 有关防火墙规则和 Azure 的启用连接的信息，请参阅[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)。



## 尝试从专用网络连接到 SQL 数据库应用程序？

确保服务器数据库启用了防火墙规则以允许来自特定网络访问。

- 有关防火墙规则的常规信息，请参阅[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/azure/ee621782.aspx)。
- 设置防火墙规则的说明，请参阅[如何︰ 配置防火墙设置 （Azure SQL 数据库）](https://msdn.microsoft.com/library/azure/jj553530.aspx)。


## 如果防火墙规则的配置是否正确，然后尝试[解决 Microsoft Azure SQL 数据库连接](https://support2.microsoft.com/common/survey.aspx?scid=sw;en;3844&showpage=1)指导演练。

上述支持文章下面的 SQL 数据库连接问题提供帮助︰

- 无法打开服务器连接，因为防火墙设置 
- 服务器找不到或无法访问 
- 无法登录到服务器 
- 连接超时错误 
- 瞬态错误 
- 由于碰到一些系统定义的限制数的连接终止 
- 连接失败从 SQL Server 管理 Studio (SSMS) 


## 其他信息

- 有关连接到 SQL 数据库的其他信息，请参见[连接到 Azure SQL 数据库编程方式准则](https://msdn.microsoft.com/library/azure/ee336282.aspx)。   

- 可以在[SQL 数据库客户端程序的错误消息](sql-database-develop-error-messages.md#bkmk_connection_errors)**瞬时故障，连接丢失错误**部分中找到有关特定连接错误的详细信息。

- 可以通过查询连接事件使用的[**sys.event_log （在 SQL Azure 的数据库中）**](https://msdn.microsoft.com/library/dn270018.aspx)视图访问连接事件数据。

- 可以通过查询[**sys.database_connection_stats （在 SQL Azure 的数据库中）**](https://msdn.microsoft.com/library/dn269986.aspx)视图访问数据库连接事件的衡量标准。

 