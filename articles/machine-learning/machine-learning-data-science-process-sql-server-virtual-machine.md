---
ms.openlocfilehash: 4a8eec955f46773c25356a23aff079a72601927e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="处理来自 SQL Azure 数据 |Microsoft Azure" 
    description="从 SQL Azure 的流程数据" 
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="fashah" 
    manager="paulettm" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="fashah;garye" /> 

#<a name="heading"></a>SQL Server 在 Azure 上的虚拟机中的流程数据

本文档介绍探索数据和存储在一个 SQL Server 在 Azure 上的虚拟机中的数据生成功能。 这可以通过数据纠纷持续使用 SQL 或使用类似 Python 编程语言完成。


> [AZURE.NOTE] 本文档中的示例 SQL 语句假定数据是在 SQL Server 中。 如果不是这样，请参阅以了解如何将数据移到 SQL Server 的云数据科学过程映射。

##<a name="SQL"></a>使用 SQL

我们描述了本节使用 SQL 中的以下数据 wrangling 任务︰

1. [数据研究](#sql-dataexploration)
2. [功能生成](#sql-featuregen)

###<a name="sql-dataexploration"></a>数据研究
以下是可用于浏览数据存储在 SQL Server 中的几个示例 SQL 脚本。


> [AZURE.NOTE] 针对一个实际的示例，可以使用[纽约出租车上的数据集](http://www.andresmh.com/nyctaxitrips/)并请参阅标题为的端到端浏览[纽约数据纠纷持续使用 IPython 笔记本和 SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) IPNB。

1. 获取每日观察的计数

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. 获取分类列中的级别

    `select  distinct <column_name> from <databasename>`

3. 两个分类列组合中获得级别的数 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. 获取数字列的分布

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>功能生成

在本节中，我们描述生成使用 SQL 功能的方法︰  

1. [计数基于功能生成](#sql-countfeature)
2. [Binning 功能生成](#sql-binningfeature)
3. [一列从推出功能](#sql-featurerollout)


> [AZURE.NOTE] 一旦生成附加的功能，可以作为列添加到现有的表或创建新表的附加功能和可以加入与原始表的主键。 

###<a name="sql-countfeature"></a>计数基于功能生成

本文档演示两种方法可以生成计数功能。 第一种方法使用条件求和，第二种方法使用位置子句。 这些可以再加入与原始表 （使用主键列） 具有计数功能旁边的原始数据。

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Binning 功能生成

下面的示例演示如何通过 binning （使用 5 储料箱） 生成 binned 的功能可使用作为一项功能的数字列︰

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>一列从推出功能

在此部分中，我们将演示如何推广一列来生成附加功能表中。 该示例假定您要从中生成功能表中没有的纬度或经度列。

这里纬度/经度位置数据是简短的初级读本 (资源从 stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`)。 这是在 featurizing 之前了解位置字段很有用︰

- 符号告诉我们是否我们是北或南，东或西在地球上。
- 非零的数百个数字告诉我们我们使用经度，不纬度 ！
- 数十位提供给大约 1000 公里的位置。 它为我们提供了有关哪些洲或海洋我们是有用的信息。
- 个位 （一个十进制的度） 提供职位达 111 公里 （60 海里，约 69 英里）。 它还可以告知我们大约什么大状态或国家/地区我们处于。
- 第一个小数位值得达 11.1 公里︰ 它可以区分从邻近的大城市的一个大城市的位置。
- 第二个数位值得达 1.1 公里︰ 它可以与下分开一个村庄。
- 第三个小数位值得达 110 m︰ 它可以确定大型农业字段或机构的校园。
- 第四个小数位值得 11 个 m︰ 它可以确定包裹的土地。 它相当于未更正的 GPS 设备的典型准确性无干扰。
- 第五个小数位值得达 1.1 m︰ 它将树与彼此区分开来。 只能带差异校正实现精确到此级别与专业的 GPS 设备。
- 第六个小数位值得达 0.11 m︰ 您可以使用此版式中的设计环境，明细数据结构的构建道路。 它应该是多足够用于跟踪移动的 glaciers 和河流。 这可以通过采取 GPS，例如 differentially 修正 GPS 与辛苦的措施。

位置信息可以可以是 featurized，如下所示，分离地区、 位置和城市信息。 请注意，一次还可以调用如 Bing 地图 API 可用在 REST 端点`https://msdn.microsoft.com/library/ff701710.aspx`以获得区域/地区的信息。

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

上面的位置基于功能可以进一步用于生成其他计数功能，如前面所述。 


> [AZURE.TIP] 您可以以编程方式插入使用您选择的语言的记录。 您可能需要将数据以块的形式来提高写入效率[检出如何使用 pyodbc 在此处执行此操作的示例](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)。 
 

> [AZURE.TIP] 另一种选择是在使用[BCP 实用工具](https://msdn.microsoft.com/library/ms162802.aspx)的数据库中插入数据

###<a name="sql-aml"></a>连接到 Azure 的机器学习

新生成的功能可以作为列添加到现有的表或存储在新表中，与机器学习的原始表联接。 功能可以生成或访问如果已经创建，使用 Azure ML 中的[读取器][读取器]模块，如下所示︰

![azureml 的读者][1] 

##<a name="python"></a>使用像 Python 编程语言

使用 Python 浏览数据，并生成功能在 SQL Server 中的数据时就类似于处理作为记录[在此处](machine-learning-data-science-process-data-blob.md)使用 Python 的 Azure blob 中的数据。 数据需要从数据库加载到了 pandas 数据帧，然后可以进行进一步处理。 我们将文档连接到数据库并将数据加载到此节中的数据帧的过程。

下面的连接字符串格式可用于连接到 SQL Server 数据库从 Python 使用 pyodbc （替换服务器名、 dbname、 具有用户名和密码您特定的值）︰

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python 中的[Pandas 库](http://pandas.pydata.org/)提供一组丰富的数据结构和数据分析工具的 Python 编程的数据操作。 下面的代码读取到 Pandas 数据帧，从 SQL Server 数据库返回的结果︰

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

现在您可以使用 Pandas 数据帧根据主题[数据科学环境中的流程 Azure Blob 数据](machine-learning-data-science-process-data-blob.md)中的介绍。

## Azure 数据科学在操作示例

端到端演练使用公共数据集 Azure 数据科学过程的示例，请参见[Azure 数据科学过程中操作](machine-learning-data-science-process-sql-walkthrough.md)。

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
测试
