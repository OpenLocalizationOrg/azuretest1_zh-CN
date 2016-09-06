---
ms.openlocfilehash: abfce89d055ffc05ff48f66a641f1a52aa9fcd7b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 HDInsight 中使用 Hadoop Sqoop |Microsoft Azure"
    description="了解如何运行 Sqoop 的导入和导出 SQL Azure 数据库 HDInsight 群集上基于 Linux 的 Hadoop 之间。"
    editor="cgronlun"
    manager="paulettm"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2015"
    ms.author="larryfr"/>

#使用 Hadoop 在 HDInsight (SSH) Sqoop

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

了解如何使用 Sqoop 来导入和导出基于 Linux 的 HDInsight 群集 SQL Azure 数据库或 SQL Server 数据库之间。

> [AZURE.NOTE] 这篇文章中的步骤使用 SSH 连接到基于 Linux 的 HDInsight 群集。 Windows 客户端也可以使用 Azure PowerShell 使用 Sqoop 的基于 Linux 的群集[使用 Hadoop HDInsight (PowerShell) 中使用 Sqoop](hdinsight-use-sqoop.md)中所述。

##Sqoop 是什么？

尽管 Hadoop 是一个自然的选择，用于处理非结构化和 semistructured 数据，例如日志文件和文件，也可能需要处理结构化的数据存储在关系数据库中。

[Sqoop][sqoop 用户指南 1.4.4]是旨在 Hadoop 群集和关系数据库之间传输数据的工具。 您可以使用它从关系数据库管理系统 (RDBMS) 导入数据，如 SQL Server、 MySQL 或到 Hadoop 分布式文件系统 (HDFS) 的 Oracle 转变 Hadoop 使用 MapReduce 或配置单元时中的数据并将数据导出到 RDBMS。 在本教程中，您为您的关系数据库使用 SQL Server 数据库。

Sqoop 在 HDInsight 群集受支持的版本，请参阅[由 HDInsight 提供的群集版本中的新增功能？][hdinsight 版本]。


##先决条件

在开始本教程之前，您必须具有以下︰

- **工作站**︰ SSH 客户端的计算机。

- **Azure CLI**︰ 有关详细信息，请参见[安装和配置 Azure CLI](../xplat-cli.md)

- **基于 linux * 的 HDInsight 群集**︰ 群集调配有关说明，请参阅[开始使用 HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)或[HDInsight 提供群集][hdinsight 规定]。

- **SQL azure 数据库**︰ 本文档提供了有关创建示例 SQL 数据库的说明。 对 SQL 数据库的详细信息，请参阅[开始使用 SQL Azure 数据库][sqldatabase 获取启动]。

* **SQL Server**︰ 本文档中的步骤也可，并进行一些修改以与 SQL Server。 到 SQL Server 中使用这篇文章的特定要求的详细信息，请参阅部分中[使用的 SQL Server](#using-sql-server) 。

##了解该方案

HDInsight 群集提供了一些示例数据。 您将使用一个名为**hivesampletable**，它引用的数据文件位于的配置单元表**wasb: / 配置单元/仓库/hivesampletable/**。 表中包含某些移动设备数据。 配置单元表架构是︰

| 字段 | 数据类型 |
| ----- | --------- |
| 客户机 id | string |
| querytime | string |
| 市场 | string |
| deviceplatform | string |
| devicemake | string |
| devicemodel | string |
| 状态 | string |
| 国家/地区 | string |
| querydwelltime | double |
| 会话标识符 | bigint |
| sessionpagevieworder | bigint |

将首先导出**hivesampletable**到 SQL Azure 数据库或 SQL Server 在名为**mobiledata**，一个表中，然后将表导入回 HDInsight 在**wasb: / 教程/usesqoop/importeddata/**。

##创建数据库

1. 打开一个终端或命令提示符下，使用以下命令来创建新的 SQL Azure 数据库服务器︰

        azure sql server create <adminLogin> <adminPassword> <region>

    对于示例中， `azure sql server create admin password "West US"`。

    在此命令完成后，您将收到类似下面的响应︰

        info:    Executing command sql server create
        + Creating SQL Server
        data:    Server Name i1qwc540ts
        info:    sql server create command OK

    > [AZURE.IMPORTANT] 请注意此命令返回的服务器名称。 这是已创建的 SQL 数据库服务器的短名称。 完全限定的域名称 (FQDN) 是**&lt;shortname&gt;。 database.windows.net**。

2. 使用下面的命令创建名为**sqooptest**的 SQL 数据库服务器上的数据库︰

        sql db create [options] <serverName> sqooptest <adminLogin> <adminPassword>

    当它完成时，这将返回"OK"消息。

    > [AZURE.NOTE] 如果您收到一个错误，指示您没有访问权限，您可能需要将客户端工作站的 IP 地址添加到 SQL 数据库防火墙使用下面的命令︰
    >
    > `sql firewallrule create [options] <serverName> <ruleName> <startIPAddress> <endIPAddress>`

##创建表

> [AZURE.NOTE] 有许多方法来连接到 SQL 数据库创建表。 下列步骤将从 HDInsight 群集使用[FreeTDS](http://www.freetds.org/) 。

1. 使用 SSH 连接到基于 Linux 的 HDInsight 群集。 连接时要使用的地址是`CLUSTERNAME-ssh.azurehdinsight.net`，端口为`22`。

    使用 SSH 连接到 HDInsight 的详细信息，请参阅以下文档︰

    * **Linux、 Unix 或 OS X 客户端**︰[连接到基于 Linux 的 HDInsight 从 Linux、 OS X 或 Unix 群集](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

    * **Windows 客户端**︰[连接到基于 Linux 的 HDInsight 从 Windows 群集](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)，请参阅

3. 使用下面的命令来安装 FreeTDS:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. FreeTDS 安装后，使用以下命令连接到您以前创建的 SQL 数据库服务器︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    您将收到类似于下面的输出︰

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. 在`1>`提示下，输入以下命令行︰

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    当`GO`输入语句时，将计算了上述语句。 首先，创建了**mobiledata**表，然后聚集的索引添加到它 （所必需的 SQL 数据库）。

    使用以下方法来验证已创建表︰

        SELECT * FROM information_schema.tables
        GO

    您应该看到类似于下面的输出︰

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. 输入`exit`在`1>`提示退出 tsql 实用程序。

##Sqoop 导出

2. 使用下面的命令给 SQL Server JDBC 驱动程序从 Sqoop 的 lib 目录中创建一个链接。 这使 Sqoop 使用该驱动程序可以与 SQL 数据库︰

        sudo ln /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /usr/hdp/current/sqoop-client/lib/sqljdbc41.jar

3. 使用下面的命令以验证 Sqoop，可以看到您的 SQL 数据库︰

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    这应返回的数据库，包括您在前面创建的**sqooptest**数据库的列表。

4. 使用下面的命令从**hivesampletable**到**mobiledata**表中导出数据︰

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    这将指示连接到 SQL 数据库， **sqooptest**数据库，并将数据从导出的 Sqoop **wasb: / 配置单元/仓库/hivesampletable/** （ *hivesampletable*，物理文件） 到**mobiledata**表。

5. 在命令完成后，使用以下连接到使用 TSQL 数据库︰

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    建立连接后，可以使用以下语句以验证到**mobiledata**表中导出数据︰

        SELECT * FROM mobiledata
        GO

    您应该看到表中数据的列表。 类型`exit`退出 tsql 实用程序。

##Sqoop 导入

1. 使用以下方法来导入数据从 SQL 数据库中的**mobiledata**表上时，将为**wasb: / 教程/usesqoop/importeddata/**在 HDInsight 目录︰

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    导入的数据必须由制表符分隔的字段，将新行字符的终止线。

2. 完成导入后，在新目录中使用下面的命令给出数据的列表︰

        hadoop fs -text wasb:///tutorials/usesqoop/importeddata/part-m-00000

##使用 SQL Server

您还可以使用 Sqoop 来导入和导出数据从 SQL Server，或者在您的数据中心在 Azure 托管的虚拟机上。 使用 SQL 数据库和 SQL Server 之间的差异包括︰

* HDInsight 和 SQL Server 必须位于相同的 Azure 虚拟网络

    > [AZURE.NOTE] HDInsight 支持仅基于位置的虚拟网络，并且它目前不能使用基于相似性组的虚拟网络。

    当您在您的数据中心中使用 SQL Server 时，则必须为*站点到站点*或*点到站点*配置虚拟网络。

    > [AZURE.NOTE] 为虚拟网络中**点到网站**，SQL Server 必须在 VPN 客户端配置的应用程序，可从 Azure 的虚拟网络配置**仪表板**。

    有关创建和配置虚拟网络的详细信息，请参阅[虚拟网络配置任务](../services/virtual-machines/)。

* SQL Server 必须配置为允许 SQL 身份验证。 有关详细信息，请参阅[选择身份验证模式](https://msdn.microsoft.com/ms144284.aspx)

* 您可能需要配置为接受远程连接的 SQL Server。 有关详细信息，请参阅[如何解决连接到 SQL Server 数据库引擎](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)

* 您必须在 SQL Server 使用**SQL Server 管理 Studio**或**tsql**之类的实用程序创建的**sqooptest**数据库-使用 Azure CLI 的步骤仅适用于 SQL Azure 数据库

    要创建**mobiledata**表的 TSQL 语句相近所使用的 SQL 数据库中，除了创建 clusterd 索引-这不是必需的 SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* 当从 HDInsight 连接到 SQL Server，您可能需要使用 SQL Server 的 IP 地址，除非您已配置了域名系统 (DNS) 来解析 Azure 虚拟网络上的名称。 例如︰

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##下一步行动

现在您已了解如何使用 Sqoop。 若要了解详细信息，请参阅︰

- [与 HDInsight 使用 Oozie][hdinsight-使用 oozie]: Oozie 工作流中的使用 Sqoop 操作。
- [使用 HDInsight 分析航班延迟数据][hdinsight-分析-飞行的数据]︰ 使用配置单元来分析飞行延迟数据，然后使用 Sqoop 将数据导出到 SQL Azure 数据库。
- [上载到 HDInsight 的数据][hdinsight 上载数据]︰ 找到其他方法，用于将数据上载到 HDInsight/Azure Blob 存储。



[hdinsight 版本]:  hdinsight-component-versioning.md
[hdinsight 规定]: hdinsight-provision-clusters.md
[hdinsight--入门]: ../hdinsight-get-started.md
[hdinsight 存储]: ../hdinsight-use-blob-storage.md
[hdinsight-分析-飞行的数据]: hdinsight-analyze-flight-delay-data.md
[hdinsight-使用 oozie]: hdinsight-use-oozie.md
[hdinsight 上载数据]: hdinsight-upload-data.md
[hdinsight 提交作业]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase--入门]: ../sql-database-get-started.md
[创建 sqldatabase-configue]: ../sql-database-create-configure.md

[powershell 开始]: http://technet.microsoft.com/library/hh847889.aspx
[powershell 安装]: ../install-configure-powershell.md
[powershell 脚本]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop 用户指南 1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

测试
