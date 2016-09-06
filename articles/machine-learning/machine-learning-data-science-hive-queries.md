---
ms.openlocfilehash: be2e8be53e70c306ecd3cac7786d87eba8f149e0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="提交高级分析功能过程中群集配置单元对 Hadoop 的查询 |Microsoft Azure" 
    description="从配置单元表处理数据" 
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
    ms.date="07/17/2015" 
    ms.author="hangzh;bradsev" /> 

#<a name="heading"></a> 将配置单元查询提交到高级分析功能过程中的 HDInsight Hadoop 群集

本文档描述配置单元向提交查询 Hadoop 群集管理 Azure 中的 HDInsight 服务的各种的方法。 可以使用提交配置单元查询︰ 

* 在群集中的 headnode Hadoop 命令行
* IPython 笔记本 
* 配置单元编辑器
* Azure PowerShell 脚本 

提供了可用于研究数据或生成使用嵌入配置单元用户定义函数 (Udf) 的功能的通用配置单元查询。 

查询的示例[Github 存储库](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)中还提供了具体到[纽约出租车上行程数据](http://chriswhong.com/open-data/foil_nyc_taxi/)方案。 这些查询已有指定的数据架构，并准备好提交运行此方案。 

在最后的部分中，将讨论用户可以调整以改进的配置单元查询性能的参数。

## 先决条件
本文假定您具有︰
 
* 创建 Azure 存储帐户。 如果您需要此任务的说明，请参阅[创建 Azure 存储帐户](../hdinsight-get-started.md#storage) 
* 调配的 Hadoop 群集，HDInsight 服务。  如果您需要说明，请参阅[设置 HDInsight 群集](../hdinsight-get-started.md#provision)。
* 已在 Azure HDInsight Hadoop 群集配置单元表上载数据。 如果还没有，请按照说明在[配置单元表的创建和加载数据](machine-learning-data-science-hive-tables.md)首先配置单元表上载数据。
* 启用远程访问群集的权限。 如果您需要的说明，请参阅[访问 Hadoop 群集头节点](machine-learning-data-science-customize-hadoop-cluster.md#remoteaccess)。 


## <a name="submit"></a>如何提交配置单元查询

1. [提交在 headnode 的 Hadoop 群集配置单元查询通过 Hadoop 命令行](#headnode)
2. [提交配置单元编辑器配置单元查询](#hive-editor)
3. [提交配置单元查询使用 Azure PowerShell 命令](#ps)
 
###<a name="headnode"></a> 1.将提交中 headnode 的 Hadoop 群集配置单元查询通过 Hadoop 命令行

如果配置单元查询很复杂，将其提交直接在 Hadoop 的头节点群集通常会更快地处理了提交它的配置单元编辑器或 Azure PowerShell 脚本比。 

登录到 Hadoop 群集的头节点，打开桌面上的头节点，Hadoop 命令行并输入命令`cd %hive_home%\bin`。

用户可以通过三种方式提交配置单元查询在 Hadoop 命令行︰ 

* 直接
* 使用.hql 文件
* 使用配置单元命令控制台

#### 提交配置单元查询直接在 Hadoop 命令行。 

用户可以运行如下命令`hive -e "<your hive query>;`提交简单配置单元查询直接在 Hadoop 命令行。 下面是一个示例，其中红色框中列出了提交该配置单元查询时，该命令和绿色框中列出了配置单元查询的输出。

![创建工作区][10]

#### 提交配置单元.hql 文件中的查询

当配置单元查询更为复杂并具有多个行时，编辑命令行或配置单元命令控制台中的查询并不可行。 另一种方法是使用 Hadoop 群集的头节点中的文本编辑器在头节点的本地目录中的.hql 文件中保存的配置单元查询。 然后可以通过提交的.hql 文件中的配置单元查询`-f`参数，如下所示︰
    
    `hive -f "<path to the .hql file>"`

![创建工作区][15]


**禁止显示进度状态屏幕打印的配置单元查询**

默认情况下，配置单元查询提交在 Hadoop 命令行之后, 地图/减少作业的进度将打印在屏幕上。 若要禁止显示的地图/减少作业进度屏幕打印，也可以使用参数`-S`(以大写的"S") 中的命令行，如下所示︰

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### 提交配置单元命令控制台中的配置单元查询。

用户可以通过运行命令还第一次输入配置单元命令控制台`hive`在 Hadoop 命令行，然后提交配置单元命令控制台中的配置单元查询。 下面是一个示例。 在此示例中，两个红色框突出显示用于输入配置单元命令控制台的命令，在命令控制台中的配置单元，分别提交配置单元查询。 绿色框中突出显示该配置单元查询的输出结果。 

![创建工作区][11]

前面的示例直接输出配置单元查询结果在屏幕上。 用户还可以写入输出到一个本地文件在头节点上，或到 Azure 的 blob。 然后，用户可以使用其他工具进行进一步分析的配置单元查询输出。

**输出配置单元到本地文件的查询结果。** 

要输出到的头节点上的本地目录的配置单元查询结果，用户需要提交配置单元查询 Hadoop 命令行中，如下所示︰

    `hive -e "<hive query>" > <local path in the head node>`

在以下示例中，配置单元查询的输出写入到文件`hivequeryoutput.txt`目录中`C:\apps\temp`。

![创建工作区][12]

**输出配置单元到 Azure 的 blob 的查询结果**

用户还可以输出到 Azure 的斑点，Hadoop 群集的默认容器内配置单元查询结果。 配置单元查询必须有如下︰

    `insert overwrite directory wasb:///<directory within the default container> <select clause from ...>`

在以下示例中，配置单元查询的输出中写入 blob 目录`queryoutputdir`Hadoop 群集的默认容器内。 在这里，您只需要提供目录名称，没有斑点的名称。 错误将会引发提供目录和 blob 名称，如`wasb:///queryoutputdir/queryoutput.txt`。 

![创建工作区][13]

如果您打开使用 Azure 存储浏览器之类的工具的 Hadoop 群集的默认容器，您将看到配置单元查询的输出如下所示。 您可以应用筛选器 （由红色方框突出显示） 仅检索与指定的字母名称中的 blob。

![创建工作区][14]

###<a name="hive-editor"></a> 2.提交配置单元编辑器配置单元查询

用户还可以通过 web 浏览器中输入 URL 使用查询控制台 （配置单元编辑器） `https://<Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor` （您可能需要输入的 Hadoop 群集凭据进行登录），

###<a name="ps"></a> 3.提交配置单元查询使用 Azure PowerShell 命令

用户还可以在我们 PowerShell 提交配置单元查询。 有关说明，请参阅[使用 PowerShell 提交配置单元的作业](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell)。 

## <a name="explore"></a>数据开采工程的功能和配置单元参数优化

我们描述了本节使用 Azure HDInsight Hadoop 群集配置单元中的以下数据 wrangling 任务︰

1. [数据研究](#hive-dataexploration)
2. [功能生成](#hive-featureengineering)

> [AZURE.NOTE] 配置单元查询示例假设数据已被上载到 Azure HDInsight Hadoop 群集配置单元表。 如果还没有，请按照[创建和加载数据的配置单元表](machine-learning-data-science-hive-tables.md)将数据上载到表配置单元。

###<a name="hive-dataexploration"></a>数据研究
下面是一些示例配置单元脚本可以用来了解如何配置单元表中的数据。

1. 获取每个分区的观察值的计数 `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. 获取每日观察的计数 `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);` 

3. 获取分类列中的级别  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. 两个分类列组合中获得级别的数 `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. 获取数字列的分布  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. 从联接两个表中提取的记录 

        SELECT 
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>, 
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>, 
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

###<a name="hive-featureengineering"></a>功能生成

在本节中，我们描述生成使用配置单元查询功能的方法︰ 
  
1. [频率基于功能生成](#hive-frequencyfeature)
2. [在二进制分类分类变量的风险](#hive-riskfeature)
3. [从日期时间字段中提取功能](#hive-datefeature)
4. [从文本字段中提取功能](#hive-textfeature)
5. [计算 GPS 坐标之间的距离](#hive-gpsdistance)

> [AZURE.NOTE] 一旦生成附加的功能，可以将它们作为列添加到现有的表或创建新表的其他功能和主键，则可以与原始表联接。

####<a name="hive-frequencyfeature"></a> 频率基于功能生成

有时，是很有价值来计算某一分类变量的级别的频率或级别的多个分类变量的组合的频率。 用户可以使用以下脚本来计算频率︰

        select 
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select 
                <column_name1>,<column_name2>, count(*) as sub_count 
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;
    

####<a name="hive-riskfeature"></a> 在二进制分类分类变量的风险

在二元分类，有时我们需要替换为非数字级别数字的风险，因为某些模型可能仅需要数字功能非数值分类变量转换数值的功能。 在本节中，我们介绍了计算某一分类变量的风险值 （日志几率） 的某些一般配置单元查询。 


        set smooth_param1=1;
        set smooth_param2=20;
        select 
            <column_name1>,<column_name2>, 
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select 
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select 
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename> 
                )a
            group by <column_name1>, <column_name2>
            )b 

在此示例中，变量`smooth_param1`和`smooth_param2`设置为平滑的数据计算得出的风险值。 风险的范围是-Inf 和 Inf 之间。 风险 > 0 代表目标等于 1 的概率大于 0.5。 

风险表被计算之后，用户可以通过加入此网络与风险表为表指派风险值。 上一节中给出联接查询该配置单元。

####<a name="hive-datefeature"></a> 从日期时间字段中提取功能

配置单元是一套的 Udf 用于处理日期时间字段。 在配置单元时，默认的日期时间格式是 yyyy-月-日 00:00:00 (如 1970年-01-01 12:21:32)。 在本节中，我们显示日期时间字段中，从提取的每月和每月一天的示例和示例的默认将以默认格式之外的其他格式的日期时间字符串转换为日期时间字符串设置格式。 

        select day(<datetime field>), month(<datetime field>) 
        from <databasename>.<tablename>;

此配置单元查询假定`<datetime field>`默认日期时间格式。

如果日期时间字段不是默认的格式，我们需要首先将 datetile 字段转换为 Unix 时间戳，然后到默认格式的日期时间字符串转换的 Unix 时间戳。 日期时间的默认格式后，用户可以应用 Udf 中提取功能的嵌入式日期时间。

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

在此查询中，如果`<datetime field>`的图案像`03/26/2015 12:04:39`，`'<pattern of the datetime field>'`应为`'MM/dd/yyyy HH:mm:ss'`。 若要测试它，用户可以运行

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

在此查询中，`hivesampletable`时，所有的 Azure HDInsight Hadoop 群集默认群集调配。 


####<a name="hive-textfeature"></a> 从文本字段中提取功能

假设的配置单元表具有一个文本域，它是单词之间用空格分隔的字符串，下面的查询中提取的长度字符串和字符串中的单词数。

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num 
        from <databasename>.<tablename>;

####<a name="hive-gpsdistance"></a> 计算 GPS 坐标之间的距离

在这一节中给出的查询可以直接应用在纽约出租车上行程数据上。 此查询的目的是展示如何应用配置单元生成功能中嵌入的数学函数。 

在此查询中使用的字段是挑选和 dropoff 位置，名为拾取的 GPS 坐标\_经度，拾取\_纬度、 dropoff\_经度和 dropoff\_纬度。 要计算的装货和 dropoff 坐标之间的直接距离的查询如下︰

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, 
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance 
        from nyctaxi.trip 
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10; 

在[这里](http://www.movable-type.co.uk/scripts/latlong.html)，作者为 Peter Lapisu 找不到两个 GPS 坐标计算距离的数学公式。 在他 Javascript 函数 toRad() 只是`lat_or_lon*pi/180`，将度数转换为弧度。 在这里，`lat_or_lon`是纬度或经度。 因为配置单元不提供函数`atan2`，提供的功能，但`atan`，`atan2`函数由`atan`上面的配置单元查询，基于[维基百科](http://en.wikipedia.org/wiki/Atan2)在其定义中的函数。 

![创建工作区][1]

完整列表，配置单元嵌入的 Udf 可以找到[这里](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions)。 

## <a name="tuning"></a> 高级主题︰ 为提高查询速度优化配置单元参数

群集配置单元的默认参数设置可能不适合配置单元查询和处理数据的查询。 在本部分中，我们将讨论用户可以调节，以便配置单元查询的性能可以提高某些参数。 用户需要添加调整之前处理数据的查询的查询参数。 

1. Java 堆空间︰ 涉及加入大型数据集，或处理长记录的查询，为典型的错误是**堆空间不足**。 这可以通过设置参数进行调整`mapreduce.map.java.opts`和`mapreduce.task.io.sort.mb`为所需值。 下面是一个示例︰

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;
    

    此参数分配给 Java 堆空间的 4 GB 内存，还可以使分类更加有效率由为其分配更多内存。 它是一个好主意，如果有任何作业失败错误相关的堆空间与它玩。

2. DFS 块大小︰ 此参数设置文件系统存储的数据的最小单位。 例如，如果 DFS 块大小为 128 MB，然后任意大小的数据小于号和达 128 MB 存储在一个块，而大于 128 MB 分配额外的块的数据。 选择一个非常小的块大小会导致大型投影机 Hadoop 由于要处理很多更多的请求，以查找与该文件相关的相关块名称节点。 推荐的设置，当与千兆字节 （或更大） 的数据︰

        set dfs.block.size=128m;

3. 优化配置单元中的联接操作︰ 虽然联接操作地图/减少框架中的通常都发生在归约阶段，有时，可以通过在映射阶段 （也称作"mapjoins"） 计划联接实现巨大收益。 若要将配置单元要这样做，只要有可能，我们可以设置︰

        set hive.auto.convert.join=true;

4. 指定的配置单元的 mappers 数︰ 虽然 Hadoop 允许用户设置的变径、 mappers 数可能通常不由用户设置。 允许某种程度上这个数字控制的关键是选择 hadoop 变量，mapred.min.split.size 和 mapred.max.split.size。 每个映射任务的大小取决于︰

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    通常情况下，mapred.min.split.size 的默认值为的 0，这 mapred.max.split.size 是 Long.MAX 和 dfs.block.size 的是 64 MB。 正如我们所看到的给出的数据大小，优化这些参数的"设置"它们使我们能够调整 mappers 使用的数量。 

5. 下面介绍了优化配置单元性能的几个其他更多高级的选项。 这些允许映射并减少任务，而分配的内存的设置，可用于调整性能。 请注意，`mapreduce.reduce.memory.mb`不能大于 Hadoop 群集中每个工作节点的物理内存大小。

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

[1]: ./media/machine-learning-data-science-hive-queries/atan2new.png
[10]: ./media/machine-learning-data-science-hive-queries/run-hive-queries-1.png
[11]: ./media/machine-learning-data-science-hive-queries/run-hive-queries-2.png
[12]: ./media/machine-learning-data-science-hive-queries/output-hive-results-1.png
[13]: ./media/machine-learning-data-science-hive-queries/output-hive-results-2.png
[14]: ./media/machine-learning-data-science-hive-queries/output-hive-results-3.png
[15]: ./media/machine-learning-data-science-hive-queries/run-hive-queries-3.png


 
测试
