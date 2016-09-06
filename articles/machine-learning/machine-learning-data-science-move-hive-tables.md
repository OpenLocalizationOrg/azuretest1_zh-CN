---
ms.openlocfilehash: 07cd25c86e5846a5413d122bdae9e9f3abdd9501
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建并将数据加载到 Blob 存储中的配置单元表 |Microsoft Azure" 
    description="创建配置单元表并将其加载 blob 以配置单元表中的数据" 
    services="machine-learning,storage" 
    solutions="" 
    documentationCenter="" 
    authors="hangzh-msft" 
    manager="jacob.spoelstra" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2015" 
    ms.author="hangzh;bradsev" />

 
#创建并将数据加载到 Azure blob 存储中的配置单元表
 
## 简介
在本文中，呈现一般配置单元查询创建配置单元表并从 Azure blob 存储加载数据。 在对配置单元表进行分区，使用优化行纵栏式 (ORC) 格式来提高查询性能，还提供一些指导。

## 先决条件
本文假定您具有︰
 
* 创建 Azure 存储帐户。 如果您需要的说明，请参阅[创建 Azure 存储帐户](../hdinsight-get-started.md#storage) 
* 设置自定义的 Hadoop 群集的 HDInsight 服务。  如果您需要的说明，请参阅[高级分析功能的自定义 Azure HDInsight Hadoop 群集](machine-learning-data-science-customize-hadoop-cluster.md)。
* 启用远程访问群集，登录，并打开 Hadoop 命令行控制台。 如果您需要的说明，请参阅[访问 Hadoop 群集头节点](machine-learning-data-science-customize-hadoop-cluster.md#headnode)。 

## 将数据上载到 Azure blob 存储
如果您创建 Azure 的虚拟机中[设置 Azure 的虚拟机的高级分析功能](machine-learning-data-science-setup-virtual-machine.md)提供的说明，此脚本文件应已下载到*c:\\用户\\\<用户名\>\\文档\\数据科学脚本*目录在虚拟机上。 这些配置单元查询只需要插入您自己的数据架构和 Azure blob 存储配置在准备就绪可供提交相应的字段中。

我们假设配置单元表的数据是**未压缩**的表格格式，数据已被上载到默认值 （或其他） 使用 Hadoop 群集的存储帐户的容器。 

如果您希望在_纽约出租车上行程数据_上的练习，您需要首先下载 24<a href="http://www.andresmh.com/nyctaxitrips/" target="_blank">纽约出租车上行程数据</a>（12 旅行文件、 和 12 票费文件） 的文件，**解压**所有文件到.csv 文件，然后上载到 Azure 存储的默认值 （或相应的容器） 的帐户，都由[自定义 Azure HDInsight Hadoop 群集高级分析流程和技术](machine-learning-data-science-customize-hadoop-cluster.md)主题中所述的过程。 在此[页](machine-learning-data-science-process-hive-walkthrough/#upload)上找不到.csv 文件上载到的存储帐户的默认容器的过程。 

## 如何提交配置单元查询
配置单元查询可以提交 Hadoop 群集的头节点上的 Hadoop 命令行控制台。 若要执行此操作，登录到 Hadoop 群集的头节点，打开 Hadoop 命令行控制台，提交配置单元查询，从那里。 有关如何执行此操作的说明，请参阅[提交到高级分析功能过程中的 HDInsight Hadoop 群集配置单元的查询](machine-learning-data-science-process-hive-tables.md)。

用户还可以通过输入 URL 使用查询控制台 （配置单元编辑器中） 

https://&#60Hadoop 群集名称 >.azurehdinsight.net/Home/HiveEditor

在 web 浏览器中。 请注意，您将被提示输入 Hadoop 群集凭据进行登录，因此您应该方便这些凭据。 

或者，您可以[使用 PowerShell 提交配置单元的作业](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell)。 


## <a name="create-tables"></a>创建配置单元数据库和表

配置单元查询在[Github 存储库](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql)共享，可以从这里下载。

下面是创建一个配置单元表配置单元查询。

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string, 
        field2 int, 
        field3 float, 
        field4 double, 
        ...,
        fieldN string
    ) 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>' 
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

下面是用户需要插入的字段以及其他配置的说明︰

- **& #60; 数据库名称 >**︰ 要创建的数据库用户的名称。 如果用户只是想要使用的默认数据库，则可以省略查询*创建数据库...* 。 
- **& #60; 表名称 >**︰ 要在指定的数据库中创建表的用户的名称。 如果用户想要使用的默认数据库，表可以直接引用*& #60; 表名称 >*没有 & #60; 数据库名称 >。
- **& #60; 字段分隔符 >**︰ 分隔数据文件上载到配置单元表中的字段的分隔符。 
- **& #60; 行分隔符 >**︰ 分隔数据文件中的行分隔符。 
- **& #60; 存储位置 >**: Azure 存储位置来保存配置单元表的数据。 如果用户未指定*位置 & #60; 存储位置 >*，数据库和表都存储在*配置单元/仓库/*目录，默认情况下配置单元群集的默认容器中。 如果用户想要指定的存储位置，存储位置必须是在数据库和表的默认容器内。 此位置必须被称为位置相对于默认容器格式的群集的*'wasb: / / & #60; 目录 1 > /'*或*' wasb: / / & #60; 目录 1 > / & #60; 目录 2 > /'*，等等。执行查询后，将在默认容器内创建相对目录。 
- **TBLPROPERTIES("skip.header.line.count"="1")**︰ 如果数据文件包含一个标题行，用户必须添加此属性**结尾处**的*创建表*查询。 否则，将对表的记录作为加载标题行。 如果数据文件不包含一个标题行，则可以省略此配置在查询中。 

## <a name="load-data"></a>将数据加载到配置单元表
这是将数据加载到表中的配置单元的配置单元查询。

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; blob 数据路径 >**︰ 如果 blob 文件上载到配置单元表 HDInsight Hadoop 群集的默认容器中*& #60; blob 数据路径 >*格式应该是*' wasb: / / & #60; 此容器的目录 > / & #60; blob 文件名称 >*。 Blob 文件也可以在 HDInsight Hadoop 群集的其他容器。 在这种情况下， *& #60; blob 数据路径 >*格式应该是*' wasb: / / & #60; 容器名称 > @& #60; 存储帐户名称 >.blob.core.windows.net/ & #60; blob 文件名称 >*。

    >[AZURE.NOTE] Blob 数据上载到的配置单元表必须是默认或 Hadoop 群集的存储帐户的其他容器中。 否则，抱怨它无法访问的数据*负载数据*的查询将会失败。 


## <a name="partition-orc"></a>高级主题︰ 分区 ORC 格式的表和存储配置单元数据

如果数据量大，分区表是有益于那些只需要扫描几个分区表的查询。 例如，它有理由按日期进行分区的 web 站点的日志数据。 

除了分区配置单元表，也很有好处，以优化行纵栏式 (ORC) 格式存储的配置单元数据。 ORC 格式的详细信息，请参阅<a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">使用 ORC 文件可以提高性能，当读取、 写入和处理数据的配置单元</a>。

### 分区的表
下面是创建分区的表，并将数据加载到它的配置单元查询。

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name> 
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

当查询分区的表，建议在**开始**添加分区条件`where`为此子句可提高搜索显著的功效。 

    select 
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name> 
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>ORC 格式存储配置单元数据

用户不能直接从加载数据 blob 存储到以 ORC 格式存储的配置单元表。 下面是用户需要加载要执行的步骤从 Azure 数据 blob ORC 格式存储在配置单元表。 

1. Blob 存储的外部表**存储作为文本文件**和加载数据创建到表中。

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' 
            lines terminated by '<line separator>' STORED AS TEXTFILE 
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

2. 使用第 1 步中，具有相同的域分隔符，外表相同的架构创建一个内部表和 ORC 格式存储配置单元数据。

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name> 
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        ) 
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

3. 从步骤 1 中的外部表中选择数据并插入 ORC 表

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

    >[AZURE.NOTE] 如果文本文件表*& #60; 数据库名称 >。 & #60; 外部文本文件表名称 >*已在步骤 3 中的分区，`SELECT * FROM <database name>.<external textfile table name>`命令将作为中返回的数据集字段中选择的分区变量。 插入*& #60; 数据库名称 >。 & #60; ORC 表名称 >*将失败，因为*& #60; 数据库名称 >。 & #60; ORC 表名称 >*没有分区变量作为表架构中的字段。 在这种情况下，用户需要特别选择要插入到字段*& #60; 数据库名称 >。 & #60; ORC 表名称 >* ，如下所示︰

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name> 
           WHERE <partition variable>=<partition value>;

4. 可以安全地删除*& #60; 外部文本文件的表名称 >*时所有数据后使用以下查询已插入*& #60; 数据库名称 >。 & #60; ORC 表名称 >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

此过程后，您应具有 ORC 格式可以使用的数据的表。  

测试
