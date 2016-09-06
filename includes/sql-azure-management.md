---
ms.openlocfilehash: d6562522bb9f92e6d8d7137f7227eacc51192eb0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

# 使用 SQL Server 管理 Studio 管理 Azure SQL 数据库 

您可以使用 SQL Server 管理 Studio (SSMS) 来管理 SQL Azure 数据库逻辑服务器和数据库。 本主题将指导您完成与 SSMS 常见任务。 您应该已经逻辑服务器并在开始之前，在 SQL Azure 数据库中创建数据库。 首先，[创建第一个 SQL Azure 数据库](sql-database-get-started.md)中读取并再回来。

建议使用最新版本的 SSMS，只要您使用 SQL Azure 数据库。 请访问[下载 SQL Server 管理 Studio](https://msdn.microsoft.com/en-us/library/mt238290.aspx)即可使用它。 


## 连接到 SQL 数据库的逻辑服务器

连接到 SQL 数据库时，需要知道服务器名称在 Azure 上。 您可能需要登录到门户网站来获取此信息。

1.  登录到[Azure 的管理门户](http://manage.windowsazure.com)。

2.  在左窗格中，单击**SQL 数据库**。

3.  在 SQL 数据库主页上，单击**服务器**在页面的顶部列出所有与订阅相关的服务器。 找到您想要连接并将其复制到剪贴板的服务器的名称。

    接下来，您 SQL 数据库将防火墙配置为允许从您的本地计算机的连接。 通过将您本地计算机的 IP 地址添加到防火墙例外列表来执行此操作。

1.  在 SQL 数据库主页上，单击**服务器**，然后单击您想要连接的服务器。

2.  单击顶部的页面的**配置**。

3.  复制当前的客户端 IP 地址的 IP 地址。

4.  在配置页**允许 IP 地址**包括可以作为起始值和结束值指定一个规则名称和 IP 地址范围的三个框。 规则名称，您可能输入您的计算机的名称。 开始和结束范围，粘贴到这两个框，在您的计算机的 IP 地址，然后单击显示复选框。

    规则名称必须是唯一的。 如果这是您的开发计算机，您可以在 IP 范围开始框和 IP 范围结束框中输入的 IP 地址。 否则，您可能需要输入范围更广的范围内的 IP 地址，以容纳来自您组织中的其他计算机的连接。
 
5. 单击页面底部的**保存**。

    **注意︰**可能会有向上尽可能多五分钟延迟更改防火墙设置才会生效。

    现在您就可以使用连接到 SQL 数据库管理 Studio。

1.  在任务栏上，单击**开始**，指向**所有程序**，都指向**Microsoft SQL Server 2014**年，然后单击**SQL Server 管理 Studio**。

2.  在**连接到服务器**，*服务器名称*作为指定完全限定的服务器名。 database.windows.net。 在 Azure，服务器名称是自动生成的字符串组成的字母数字字符。

3.  选择**SQL Server 身份验证**。

4.  在**登录**框中，输入您指定在门户中创建您的服务器时 SQL Server 管理员登录信息。

5.  在**密码**框中，输入密码时创建您的服务器指定的门户。

8.  单击**连接**以建立连接。

SQL Server 2014 SSMS 与最新的更新提供了对任务，如创建和修改 SQL Azure 数据库的扩展的支持。 此外，您还可以使用事务处理 SQL 语句以完成这些任务。 下面的步骤提供这些语句的示例。 有关事务处理 SQL 中使用 SQL 数据库，包括有关哪些支持命令，详细信息请参阅[事务处理 SQL 参考 （SQL 数据库）](http://msdn.microsoft.com/library/bb510741.aspx)。

## 创建和管理 SQL Azure 数据库

连接到**主**数据库时，可以在服务器上创建新数据库并修改或删除现有的数据库。 下面的步骤描述了如何通过管理 Studio 几个常见的数据库管理任务。 要执行这些任务，请确保已连接到**主**数据库创建时设置您的服务器的服务器级别主体登录。

若要在管理 Studio 中打开的查询窗口，打开数据库文件夹，展开**系统数据库**文件夹、**主**，右击，然后单击**新建查询**。

-   **创建数据库**语句用于创建新的数据库。 有关详细信息，请参阅[创建的数据库 （SQL 数据库）](https://msdn.microsoft.com/library/dn268335.aspx)。 下面的语句创建一个名为**myTestDB**的新数据库，并指定它是默认最大大小为 250 GB 标准 S0 版数据库。

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

单击**执行**运行查询。

-   使用**ALTER DATABASE**语句来修改现有的数据库，例如，如果您想要更改的名称和版本的数据库。 有关详细信息，请参阅[更改的数据库 （SQL 数据库）](https://msdn.microsoft.com/library/ms174269.aspx)。 下面的语句修改您要更改上一步中创建的数据库版到标准 S1。

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   **删除数据库**语句用于删除现有数据库。
    有关详细信息，请参阅[拖放的数据库 （SQL 数据库）](https://msdn.microsoft.com/library/ms178613.aspx)。 下面的语句删除**myTestDB**数据库，但不放到现在，因为您将用它的下一步创建登录名。

        DROP DATABASE myTestBase;

-   Master 数据库具有**sys.databases**视图可用于查看有关所有数据库的详细信息。 若要查看所有现有数据库，请执行以下语句︰

        SELECT * FROM sys.databases;

-   在 SQL 数据库中**使用**语句不支持数据库间进行切换。 相反，您需要建立到目标数据库的直接连接。

>[AZURE.NOTE] 许多创建或修改数据库的事务处理 SQL 语句必须在他们自己的批处理中运行，不能与其他事务处理 SQL 语句进行分组。 有关详细信息，请参阅上面列出的链接中可用的特定语句的信息。

## 创建和管理登录

跟踪的**master**数据库的登录和哪些登录有权创建数据库或其他登录名。 通过连接到与服务器级别主体登录时您可以设置您的服务器创建**master**数据库中管理登录。 **创建登录**、**更改登录**信息或**删除登录**语句可用于执行主跨整个服务器管理登录名的数据库查询。 有关详细信息，请参阅[管理数据库和 SQL 数据库中的登录名](http://msdn.microsoft.com/library/azure/ee336235.aspx)。 


-   使用**创建登录**语句来创建新的服务器级登录。 有关详细信息，请参阅[创建登录 （SQL 数据库）](https://msdn.microsoft.com/library/ms189751.aspx)。 下面的语句创建新的登录名为**login1**。 替换为您所选择的密码**密码 1** 。

        CREATE LOGIN login1 WITH password='password1';

-   使用**创建用户**语句授予数据库级别权限。 必须在**master**数据库中创建所有登录，但对于要连接到不同数据库的登录，您必须授予它使用在该数据库上**创建用户**语句的数据库级别权限。 有关详细信息，请参阅[创建用户 （SQL 数据库）](https://msdn.microsoft.com/library/ms173463.aspx)。 

-   要为 login1 到名为**myTestDB**的数据库的权限，请完成以下步骤︰

 1.  若要刷新对象资源管理器来查看您刚刚创建的**myTestDB**数据库，用鼠标右键单击对象资源管理器中的服务器名称，然后单击**刷新**。  

     如果您关闭了连接，您可以通过选择文件菜单上的**连接对象资源管理器**重新连接。

 2. 右键单击**myTestDB**数据库，然后选择**新建查询**。

    3.  执行以下语句对 myTestDB 数据库，创建一个名为**login1User** ，对应于服务器级别登录**login1**的数据库用户。

            CREATE USER login1User FROM LOGIN login1;

-   使用**sp\_addrolemember**存储过程，以提供适当级别的数据库的权限的用户帐户。 有关详细信息，请参见[sp_addrolemember (事务处理 SQL)](http://msdn.microsoft.com/library/ms187750.aspx)。 下面的声明添加到**login1User**通过使**login1User**对数据库的只读权限**db\_datareader**角色。

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   使用**ALTER LOGIN**语句来修改现有的登录，例如，如果您想要更改登录名的密码。 有关详细信息，请参阅[更改登录 （SQL 数据库）](https://msdn.microsoft.com/library/ms189828.aspx)。 **ALTER LOGIN**语句应该对**主**数据库运行。 切换回已连接到该数据库的查询窗口。 

    下面的语句修改**login1**登录并重设密码。
    替换您的选择和**旧密码**使用当前密码登录的密码为**新密码**。

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   **除去登录**语句用于删除现有的登录。
    删除登录服务器级别也会删除关联的数据库的任何用户帐户。 有关详细信息，请参阅[拖放的数据库 （SQL 数据库）](https://msdn.microsoft.com/library/ms178613.aspx)。 **除去登录**语句应该对**主**数据库运行。 下面的语句删除**login1**登录。

        DROP LOGIN login1;

-   Master 数据库具有**sys.sql\_登录**查看可用于查看登录名。 若要查看所有现有的登录名，请执行以下语句︰

        SELECT * FROM sys.sql_logins;

## 监视使用动态管理视图的 SQL 数据库</h2>

SQL 数据库支持可用于监视单个数据库的多个动态管理视图。 下面是几个示例监视器通过这些视图可以检索的数据的类型。 有关完整的详细信息和使用情况的更多示例，请参阅[监视 SQL 数据库使用动态管理视图](https://msdn.microsoft.com/library/azure/ff394114.aspx)。

-   查询动态管理视图需要**查看数据库状态**的权限。 若要**查看数据库状态**权限授予特定的数据库用户，连接到您想要管理您的服务器级原则登录并执行下面的语句对数据库的数据库︰

        GRANT VIEW DATABASE STATE TO login1User;

-   计算数据库大小使用**sys.dm\_db\_分区\_统计**视图。 **Sys.dm\_db\_分区\_统计**视图返回的每个分区页和行计数信息在数据库中，您可以使用它来计算数据库的大小。 下面的查询返回您的数据库大小 （兆字节）︰

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   使用**sys.dm\_exec\_连接**和**sys.dm\_exec\_会话**视图来检索有关当前用户连接和内部与数据库相关联的任务的信息。 以下查询用于返回当前连接信息︰

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   使用**sys.dm\_exec\_查询\_统计**要检索聚合性能统计数据视图缓存的查询计划。 下面的查询返回有关按平均 CPU 时间的排名的前五个查询的信息。

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
