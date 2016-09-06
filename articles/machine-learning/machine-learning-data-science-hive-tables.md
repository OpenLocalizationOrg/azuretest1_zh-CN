---
ms.openlocfilehash: b1c62570109f021993a5c22d339853e38a895208
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建并将数据加载到 Blob 存储中的配置单元表 |Microsoft Azure" 
    description="创建配置单元表并将其加载 blob 以配置单元表中的数据" 
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="hangzh-msft" 
    manager="paulettm" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/02/2015" 
    ms.author="hangzh;bradsev" />

 
#创建并将数据加载到 Azure blob 存储中的配置单元表
 
在本文中，呈现一般配置单元查询创建配置单元表并从 Azure blob 存储加载数据。 在 GitHub 存储库中共享这些配置单元查询。 [Github 资料库](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql)。 


如果您在"设置 Azure 虚拟机与 IPython 笔记本服务器"中的说明创建 Azure 的虚拟机，该脚本文件下载到`C:\Users\<user name>\Documents\Data Science Scripts`目录在虚拟机上。 您需要插入您自己的数据架构中，这些查询和这些配置单元查询中的相应字段中的 Azure blob 存储配置应该准备提交。 

我们假定配置单元表的数据是**未压缩**的表格格式，并将数据上载到默认或其他容器中使用 Hadoop 群集的存储帐户。 如果用户想在_纽约出租车上行程数据_上的练习，他们需要第一个[下载 24 的所有文件](http://www.andresmh.com/nyctaxitrips/)（12 旅行文件、 和 12 公平文件），**解压**所有文件到.csv 文件，并将其上载到默认或其他容器中是 Azure 存储帐户的时使用[Azure HDInsight Hadoop 群集进行自定义](machine-learning-data-science-customize-hadoop-cluster.html)。 

在 Hadoop 群集的头节点上的 Hadoop 命令行可以提交配置单元查询。 您需要︰

1. [启用对 Hadoop 群集的头节点远程访问和登录到 head 节点](machine-learning-data-science-customize-hadoop-cluster.md)。
2. [提交配置单元查询在 Hadoop 命令行在头节点上](machine-learning-data-science-hive-queries.md)。

用户还可以通过 web 浏览器中输入 URL 使用 [查询控制台 （配置单元编辑器）] https://<Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor （将要求您输入登录的 Hadoop 群集凭据），或者可以[将使用 PowerShell 的配置单元作业提交](hdinsight-submit-hadoop-jobs-programmatically.md)。 

- [步骤 1︰ 创建配置单元数据库和表](#create-tables)
- [步骤 2︰ 将数据加载到配置单元表](#load-data)
- [高级主题︰ 分区 ORC 格式的表和存储配置单元数据](#partition-orc)

## <a name="create-tables"></a>创建配置单元数据库和表

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

- `<database name>`︰ 要创建的数据库用户的名称。 如果用户只是想要使用的默认数据库，查询`create database...`，则可以省略。 
- `<table name>`︰ 要指定的数据库中创建表的用户的名称。 如果用户想要使用的默认数据库，表可以直接引用`<table name>`不带`<database name>.`。
- `<field separator>`︰ 用于分隔数据文件上载到配置单元表中的字段的分隔符。 
- `<line separator>`︰ 将数据文件中的行分开的分隔符。 
- `<storage location>`︰ 配置单元表中的数据保存的 Azure 存储位置。 如果用户未指定`LOCATION '<storage location>'`、 默认数据库和表存储在`hive/warehouse/`目录中的配置单元群集的默认容器。 如果用户想要指定的存储位置，存储位置必须是在数据库和表的默认容器内。 此位置必须为群集中的格式为默认容器的相对位置来引用`'wasb:///<directory 1>/'`或`'wasb:///<directory 1>/<directory 2>/'`等。执行查询后，将在默认容器内创建相对目录。 
- `TBLPROPERTIES("skip.header.line.count"="1")`︰ 如果数据文件包含一个标题行，用户必须将此属性添加**最终**的`create table`查询。 否则，将对表的记录作为加载标题行。 如果数据文件不包含一个标题行，则可以省略此配置在查询中。 

## <a name="load-data"></a>将数据加载到配置单元表

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- `<path to blob data>`︰ 如果 blob 文件上载到配置单元表 HDInsight Hadoop 群集的默认容器中`<path to blob data>`的格式应为`'wasb:///<directory in this container>/<blob file name>'`。 Blob 文件也可以在 HDInsight Hadoop 群集的其他容器。 在这种情况下，`<path to blob data>`的格式应为`'wasb://<container name>@<storage account name>.blob.windows.core.net/<blob file name>'`。

> [AZURE.NOTE] Blob 数据上载到的配置单元表必须是默认或 Hadoop 群集的存储帐户的其他容器中。 否则为`LOAD DATA`查询将失败，抱怨它无法访问的数据。 


## <a name="partition-orc"></a>高级主题︰ 分区 ORC 格式的表和存储配置单元数据

如果数据量大，分区表将有益于那些只需要扫描几个分区表的查询。 例如，它有理由按日期进行分区的 web 站点的日志数据。 

除了分区表，它也是有益 ORC 格式存储配置单元数据。 ORC 代表"优化行纵栏式"。  请参考_"[使用 ORC 文件可以提高性能，当读取、 写入和处理数据的配置单元](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles)。"_ 有关更多详细信息。

### 分区的表

创建已分区的表并将数据加载到它的查询如下所示︰

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name> 
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

当查询分区的表，建议在**开始**添加分区条件`where`子句，以便可显著提高搜索的功效。 

    select 
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name> 
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>ORC 格式存储配置单元数据

用户不能直接加载配置单元中的表 ORC 存储格式的 blob 中的数据。 下面是用户需要加载要执行的步骤从 Azure 数据 blob ORC 格式存储在配置单元表。 

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

2. 创建具有相同的架构在第 1 步中的外部表的内部表和相同的域分隔符。 和 ORC 格式存储配置单元数据

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name> 
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        ) 
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

3. 从步骤 1 中的外部表中选择数据并插入 ORC 表

        INSERT OVERWRITE TABLE <database name>.<ORC table name> SELECT * FROM <database name>.<external textfile table name>;

> [AZURE.NOTE] 如果文本文件表`<database name>.<external textfile table name>`已在步骤 3 中的分区，`SELECT * FROM <database name>.<external textfile table name>`将作为中返回的数据集的字段中选择的分区变量。 插入到`<database name>.<ORC table name>`将失败，因为`<database name>.<ORC table name>`没有分区变量作为表架构中的字段。 在这种情况下，用户需要特别选择要插入到字段`<database name>.<ORC table name>`类似如下所示︰

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
              SELECT field1, field2, ..., fieldN
              FROM <database name>.<external textfile table name> 
              WHERE <partition variable>=<partition value>;

4. 可以安全地删除`<external textfile table name>`在所有数据中使用下面的查询已插入`<database name>.<ORC table name>`:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

现在我们有 ORC 格式可以使用包含数据的表。 
 

测试
