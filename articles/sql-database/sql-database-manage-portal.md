---
ms.openlocfilehash: c3f625d18567368fb16453d8d9db4154244c5d9e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="管理 Azure SQL 数据库使用 Azure 管理门户" 
    description="了解如何使用 Azure 管理门户管理在云中使用 Azure 管理门户关系数据库。" 
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
    ms.date="04/14/2015" 
    ms.author="sstein"/>


# 管理 Azure SQL 数据库使用 Azure 管理门户

[Azure 管理门户][管理门户]允许您创建、 监视和管理 SQL Azure 数据库和服务器。 这篇文章将突出显示可通过管理门户数据库操作。 您可以了解更多关于其他 SQL Azure 数据库管理工具[这里][AzureDb 管理概述]。

>[AZURE.NOTE] 如果您熟悉 Azure 的管理门户，此[视频教程提供的快速概述][Azure 门户网站推介]其一般功能和概念。

![数据库概述](./media/sql-database-manage-portal/sqldatabase_annotated.png)

## 1.数据库管理操作
![数据库管理操作](./media/sql-database-manage-portal/sqldatabase_actions.png)

Azure 管理门户提供了一套通用数据库操作可以访问一个数据库叶片顶部。 可以将数据库还原到前一个点，在时间、 在 Visual Studio 中打开数据库、 数据库复制到新的服务器，并将数据库导出到 Azure 存储帐户。 

## 2.数据库监视
![数据库监视](./media/sql-database-manage-portal/sqldatabase_monitoring.png)

通过对数据库吞吐量单元 (DTU)、 数据库大小以及连接的运行状况监视图表的默认功能 azure 的 SQL 数据库。 这些监控图表都可以定制，并可以扩展以另外的图表 CPU 百分比，数据 IO 百分比，死锁，日志 IO 百分比或甚至被防火墙阻止请求的百分比。 找不到更多有关如何自定义监视图表[这里][监视 Azure 的一部分]。

此外，预警规则可以设置监视指定的指标并达到预先设定的阈值时通知指定的管理员和共同的管理员。 如何设置 Azure 管理门户中的预警规则的详细信息可以找到[这里][Azure 部分监视]。

## 3.数据库安全性和审核
![数据库安全](./media/sql-database-manage-portal/sqldatabase_security.png)

可以配置 azure 的 SQL 数据库来跟踪所有数据库事件和写到审核日志在 Azure 存储帐户。 此功能可以帮助您保持法规遵从性、 了解数据库的活动，并深入矛盾的存在可能的业务问题或怀疑违反安全的行为。 SQL Azure 数据库审核详细信息可[在此处][AzureDb 审核]

Azure 的 SQL 数据库还可以配置为掩码给非特权用户的敏感数据。 找不到动态数据屏蔽功能的 SQL Azure 数据库的详细信息[在此处][AzureDb datamasking]

## 4.地区复制
![Geo 复制](./media/sql-database-manage-portal/sqldatabase_georeplication.png)

可以将 azure 的 SQL 数据库配置为异步复制到辅助数据库的已提交的事务。 管理门户上的 geo 复制部件允许您选择要将辅助数据库驻留在 Azure 地区。 在 Azure 数据库 geo 复制的详细信息可以发现[这里][地理数据库复制]

##其他资源
* [介绍 SQL 数据库][]   
* [使用 SQL Server 管理 Studio 管理 Azure SQL 数据库][]   
* [监视使用动态管理视图的 SQL 数据库][]   
* [事务处理 SQL 参考 （SQL 数据库）][]
  
  [Azure 门户教程]: https://go.microsoft.com/fwlink/?LinkID=522341
  [管理门户]: https://portal.azure.com
  [Azure 部分监视]: ../documentdb-monitor-accounts.md
  [AzureDb 管理概述]: http://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/
  [介绍 SQL 数据库]: http://azure.microsoft.com/services/sql-database
  [数据库同步复制地理]: http://azure.microsoft.com/blog/2014/07/12/spotlight-on-sql-database-active-geo-replication/
  [使用 SQL Server 管理 Studio 管理 Azure SQL 数据库]: sql-database-manage-azure-ssms.md
  [监视使用动态管理视图的 SQL 数据库]: http://msdn.microsoft.com/library/windowsazure/ff394114.aspx
  [事务处理 SQL 参考 （SQL 数据库）]: http://msdn.microsoft.com/library/bb510741(v=sql.120).aspx
  [AzureDb 审核]: http://azure.microsoft.com/documentation/articles/sql-database-auditing-get-started/
  [AzureDb datamasking]: http://azure.microsoft.com/documentation/articles/sql-database-dynamic-data-masking-get-started/

 
 