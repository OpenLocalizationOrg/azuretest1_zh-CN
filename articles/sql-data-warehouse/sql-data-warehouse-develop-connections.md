---
ms.openlocfilehash: 6520c2ceb90d871605dbb636e5abe26aab37d511
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="连接到 SQL 数据仓库 |Microsoft Azure"
   description="用于连接到 SQL 数据仓库开发解决方案的提示。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/26/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# 连接到 SQL 数据仓库 
若要连接到 SQL 数据仓库需要传入安全凭据以进行身份验证。 在建立连接时您还可以找到的某些连接设置配置为建立查询会话的一部分。

本文详细介绍了连接到 SQL 数据仓库的以下方面︰

- 身份验证
- 连接设置
- 会话和请求标识符


## 身份验证
若要连接到 SQL 数据仓库需要提供以下信息︰

- 完全限定的服务器名 
- 指定 SQL 身份验证
- Username 
- Password
- 默认数据库 （可选）

值得注意的是，用户必须使用 SQL 身份验证验证。 这一次不支持受信任的身份验证。

默认情况下您的连接将连接到主服务器数据库并不是您的用户数据库。 连接到用户数据库可以选择执行以下两种情况之一︰

1. 当在 SSDT 或您应用程序的连接字符串中 SQL Server 对象资源管理器中注册您的服务器指定的默认数据库。 例如通过包括 ODBC 连接的 InitialCatalog 参数。
2. 第一次突出显示之前在 SSDT 创建会话的用户数据库。

> [AZURE.NOTE] 为连接到 SQL 数据仓库与 SSDT 指导请参阅重新[连接并查询][]获取入门文章。 

再次是一定要注意事务处理 SQL 语句**使用<your DB>**不支持更改连接的数据库 

## 应用程序连接协议
您可以连接到 SQL 数据仓库使用任何下列协议︰

- ADO.NET
- ODBC
- PHP
- JDBC

### ADO.NET 连接字符串示例

```
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### ODBC 连接字符串示例

```
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### PHP 连接字符串示例

```
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### JDBC 连接字符串示例

```
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## 连接设置
SQL 数据仓库连接和对象创建期间标准化几个设置。 这些不能被重写。

| 数据库设置   | 值                        |
| :----------------- | :--------------------------- |
| ANSI_NULLS         | ON                           |
| QUOTED_IDENTIFIERS | ON                           |
| NO_COUNT           | 关闭                          |
| 日期格式         | mdy                          |
| DATEFIRST          | 7                            |
| 数据库排序规则 | SQL_Latin1_General_CP1_CI_AS |

## 会话和请求
一旦连接，您就可以编写并提交到 SQL 数据仓库的查询已建立会话。

每个查询将一个或多个请求标识符来表示。 提交在该连接上的所有查询单个会话的一部分，因此将由单个会话 id。

但是，作为 SQL 数据仓库是一个分布式的 MPP 系统会话和请求标识符与 SQL Server 相比有点以不同的方式公开。 

会话和请求从逻辑上由其各自的标识符表示。
    
| 标识符 | 示例值 |
| :--------- | :------------ |
| 会话 ID | SID123456     |
| 请求 ID | QID123456     |

请注意，会话 ID 前缀通过 SID 的会话 ID 的简写-并且请求前缀 QID 这是速记的查询 id。

您将需要此信息来帮助您识别您的查询监控查询性能时。 您可以通过使用 [Azure 门户] 和动态管理视图监视查询性能。

若要确定哪些会话当前使用以下函数︰

```
SELECT SESSION_ID()
;
```

若要查看所有对您数据仓库运行的查询运行或最近已可以使用查询如下所示︰

```
CREATE VIEW dbo.vSessionRequests
AS
SELECT   s.[session_id]                                 AS Session_ID
        ,s.[status]                                     AS Session_Status
        ,s.[login_name]                                 AS Session_LoginName
        ,s.[login_time]                                 AS Session_LoginTime
        ,r.[request_id]                                 AS Request_ID
        ,r.[status]                                     AS Request_Status
        ,r.[submit_time]                                AS Request_SubmitTime
        ,r.[start_time]                                 AS Request_StartTime
        ,r.[end_compile_time]                           AS Request_EndCompileTime
        ,r.[end_time]                                   AS Request_EndTime
        ,r.[total_elapsed_time]                         AS Request_TotalElapsedDuration_ms
        ,DATEDIFF(ms,[submit_time],[start_time])        AS Request_InitiateDuration_ms
        ,DATEDIFF(ms,[start_time],[end_compile_time])   AS Request_CompileDuration_ms
        ,DATEDIFF(ms,[end_compile_time],[end_time])     AS Request_ExecDuration_ms
        ,[label]                                        AS Request_QueryLabel
        ,[command]                                      AS Request_Command
        ,[database_id]                                  AS Request_Database_ID
FROM    sys.dm_pdw_exec_requests r
JOIN    sys.dm_pdw_exec_sessions s  ON  r.[session_id] = s.[session_id]
WHERE   s.[session_id] <> SESSION_ID()
;
```

请注意，此查询封装在一个视图中，以便您可以将其合并到您的解决方案。 但要看到的结果将需要创建视图和执行它。

## 下一步行动
一次连接就可以开始设计您的表。 请参阅[表设计]文章以获得详细信息。

<!--Image references-->

<!--Azure.com references-->
[连接和查询]: sql-data-warehouse-connect-query.md
[表设计]: sql-data-warehouse-develop-table-design.md

<!--MSDN references-->

<!--Other references-->
