---
ms.openlocfilehash: 93539a2e401e7956168afd6e0161be7a756f022b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="监视工作负荷使用 Dmv |Microsoft Azure"
   description="了解如何监视您使用 Dmv 的工作负载。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/06/2015"
   ms.author="sahajs"/>

# 监视使用 Dmv 工作负荷

本文介绍如何使用动态管理视图 (Dmv) 来监视工作负荷，并调查在 Azure SQL 数据仓库的查询执行。




## 权限

在 SQL 数据仓库中，查询动态管理视图需要**查看数据库状态**的权限。 **查看数据库状态**权限返回有关当前数据库中所有对象的信息。
要**查看数据库状态**权限授予特定的数据库用户，运行下面的查询︰

```

GRANT VIEW DATABASE STATE TO database_user;

```




## 监视连接

*Sys.dm_pdw_nodes_exec_connections*视图可用于检索有关建立到 Azure SQL 数据仓库数据库的连接信息。 此外， *sys.dm_exec_sessions*视图检索有关所有活动用户连接信息时是很有帮助。

```

SELECT * FROM sys.dm_pdw_nodes_exec_connections;
SELECT * FROM sys.dm_pdw_nodes_exec_sessions;

```


使用下面的查询来检索有关当前连接的信息。

```

SELECT * 
FROM sys.dm_pdw_nodes_exec_connections AS c 
   JOIN sys.dm_pdw_nodes_exec_sessions AS s 
   ON c.session_id = s.session_id 
WHERE c.session_id = @@SPID;

```





## 调查执行查询
您可能会遇到查询未完成的或正在运行时间比预期时间长的情况。 在这种情况下您可以使用以下步骤来收集数据并缩小问题。



### 步骤 1︰ 查找查询，以调查

```

-- Monitor running queries
SELECT * FROM sys.dm_pdw_exec_requests WHERE status = 'Running';

-- Find the longest running queries
SELECT * FROM sys.dm_pdw_exec_requests ORDER BY total_elapsed_time DESC;

```

保存查询的请求 ID。


  
### 第 2 步︰ 检查该查询正在等待资源

```

-- Find waiting tasks for your session.
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status, 
      requests.start_time,  
      waits.type,  
      waits.object_type, 
      waits.object_name,  
      waits.state  
FROM   sys.dm_pdw_waits waits 
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id 
WHERE waits.request_id = 'QID33188'
ORDER BY waits.object_name, waits.object_type, waits.state;

```


上面的查询结果中将显示您请求的等待状态。

- 如果该查询正在等待来自另一个查询的资源，则状态将**AcquireResources**。
- 如果查询具有所需的所有资源，并且不等待，则状态将**授权**。 在这种情况下，继续进行查看的查询步骤。




### 第 3 步︰ 找不到查询的长运行的步骤

使用请求 ID 检索分布式的查询的所有步骤的列表。 找到的长时间运行的步骤在总的运行时间。 

```

-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.
 
SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID33209'
ORDER BY step_index;

```

保存的长时间运行步骤步骤索引。

检查*operation_type*列中的长时间运行的查询步骤︰

- 对于**SQL 操作**继续进行步骤 4a: OnOperation，RemoteOperation，ReturnOperation。
- 为**数据移动操作**继续进行步骤 4b: ShuffleMoveOperation、 BroadcastMoveOperation、 TrimMoveOperation、 PartitionMoveOperation、 MoveOperation、 CopyOperation。




### 步骤 4a︰ 查找 SQL 步骤的执行进度

使用申请 ID 和步骤索引来检索信息的 SQL Server 查询分布作为查询中的 SQL 步骤的一部分。 保存的节点 ID 和 SPID。

```

-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID33209' AND step_index = 2;

```


使用下面的查询来检索 SQL 步骤的特定节点上的 SQL Server 执行计划。

```

-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node. 
-- Replace node_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);

```



### 步骤 4b︰ 查找 DMS 步骤的执行进度

使用申请 ID 和步骤索引来检索有关数据移动步骤运行在每个通讯组的信息。 

```

-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.
 
SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID33209' AND step_index = 2;

```

- 检查以查看特定通讯是否正在明显长于其他数据移动的*total_elapsed_time*列。 
- 对于长时间运行的分布中，检查*rows_processed*列明显大于其他人从该分发移动的行数。 这表明您查询数据倾斜。





## 调查数据不一致

```

-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED("dbo.FactInternetSales");

```


此查询的结果将显示在每个数据库的 60 分配存储的表行数。 为获得最佳性能，您的分布式表中的行应均匀地分布在所有分布。
若要了解详细信息，请参阅[表设计][]。



## 下一步行动
管理您的 SQL 数据仓库的更多提示，请参阅[管理概述][]。

<!--Image references-->

<!--Article references-->
[管理概述]: sql-data-warehouse-overview-manage.md
[表设计]: sql-data-warehouse-develop-table-design.md

<!--MSDN references-->


