---
ms.openlocfilehash: 16f065f4f0d3b8658327183c98a34a0782c72058
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在 Azure 上的 SQL Server 中的数据采样 |Microsoft Azure" 
    description="在 Azure 上的 SQL Server 中的示例数据" 
    services="machine-learning" 
    documentationCenter="" 
    authors="fashah" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>在 Azure 上的 SQL Server 中的示例数据

本文探讨了采样数据存储在 SQL Server 在 Azure 使用 SQL 和使用 Python 编程语言上。

>[AZURE.NOTE] 本文档中的示例 SQL 代码假定数据是在 Azure 上 SQL Server 中。 如果不是这样，请参阅将标题[移到 Azure 上的 SQL Server 数据](machine-learning-data-science-move-sql-server-virtual-machine.md)说明将数据移到 SQL Server 在 Azure 中的[高级数据进程的指南](machine-learning-data-science-advanced-data-processing.md)中。

##<a name="SQL"></a>使用 SQL

本部分介绍使用 SQL 在数据库中执行对数据的简单随机抽样的几种方法。 请选择基于您的数据的大小和其分布的方法。

下面的两项显示如何在 SQL Server 中使用 newid 进行采样。 您选择的方法取决于所需的示例是随机程度 （在下面的代码示例中的 pk_id 则假定可自动生成的主键）。

1. 严格的随机样本

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. 更多随机样本 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample 可以被用于采样以及示。 如果您的数据的大小很大 （假定不相关的不同页面上的数据） 和查询，以在合理的时间内完成，这可能是更好的方法。

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] 可以浏览并将其存储在新表中此采样数据生成功能


###<a name="sql-aml"></a>连接到 Azure 的机器学习

可直接用于上面的示例查询在 Azure ML 读卡器模块下双样本动态数据并将其放到 Azure ML 实验。 使用读取器模块来阅读样本的数据的屏幕快照所示︰
   
![读取器 sql][1]

##<a name="python"></a>使用 Python 编程语言 

本节说明如何使用 pyodbc 库连接到 Python 中的 SQL server 数据库。 数据库连接字符串如下: （请替换服务器名、 dbname、 用户名和密码与您的配置）︰

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python 中的[Pandas](http://pandas.pydata.org/)库提供一组丰富的数据结构和数据分析工具的 Python 编程的数据操作。 下面的代码将 0.1%样本数据的 Pandas 数据读入 SQL Azure 数据库中的表︰

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

现在，您可以使用 Pandas 数据帧中的采样数据。 

###<a name="python-aml"></a>连接到 Azure 的机器学习

下面的代码示例可用于下采样数据保存到一个文件并将其上载到 Azure 的 blob。 该 blob 中的数据可以直接读取到 Azure ML 实验使用*读卡器模块*。 步骤如下所示︰ 

1. Pandas 数据帧写入本地文件

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. 将本地文件上载到 Azure 的斑点

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. 从 Azure blob 中屏幕抓取下面所示使用 Azure ML*读卡器模块*中读取的数据︰
 
![读取器 blob][2]

## 在示例中操作高级分析流程和技术 （调整）

端到端演练高级分析过程和技术 （调整） 使用公共数据集的示例，请参阅[Azure 高级分析流程和操作技术︰ 使用 SQL 服务器](machine-learning-data-science-process-sql-walkthrough.md)。

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 
测试
