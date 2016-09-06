---
ms.openlocfilehash: 5c4d316bde5722b22f9e7c3616736ce012386ace
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Server 存储过程活动" 
    description="了解如何使用 SQL Server 存储过程活动来调用数据工厂管线从 SQL Azure 数据库中的存储的过程。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="spelluru"/>

# SQL Server 存储过程活动

可以使用在数据工厂[管线](data-factory-create-pipelines.md)中的 SQL Server 存储过程活动调用**SQL Azure**数据库中的存储的过程。 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章显示概要介绍了数据转换和受支持的转换活动。

## 语法
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## 语法的详细信息

属性 | 说明 | 是否必需
-------- | ----------- | --------
name | 活动名称 | 是
description | 描述该活动的用途的文本 | 否
类型 | SqlServerStoredProcedure | 是
输入 | 输入必须是可 （在就绪状态） 的 dataset(s) 为要执行的存储的过程活动。 为存储的过程活动输入仅作为依赖关系管理时链接此活动，与其他人。 不能在存储过程中作为参数使用输入的 dataset(s) | 否
输出 | 存储的过程活动所产生的输出 dataset(s)。 确保输出表使用链接到数据工厂的 Azure SQL 数据库的链接的服务。 存储的过程活动中输出可以作为链接此活动，与其他人时，传递活动结果的存储的过程 subsquent 处理和/或它的方法可以作为依赖关系管理 | 是
storedProcedureName | SQL Azure 数据库所表示的链接使用服务，则输出表中指定的存储过程的名称。 | 是
storedProcedureParameters | 指定存储的过程参数的值 | 否

## 示例

让我们考虑想要具有两列的 SQL Azure 数据库中创建表的一个示例︰ 

列 | 类型
------ | ----
ID | 唯一标识符
Datetime | 生成对应的 ID 的时间和日期

![示例数据](./media/data-factory-stored-proc-activity/sample-data.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END

> [AZURE.NOTE] **名称**和参数 （在本例中，日期时间） 的**大小写**必须与活动下的 JSON 中指定的参数相匹配。 在存储的过程定义中，确保**@**可作为前缀的参数。   

若要在数据工厂管道中执行此存储的过程，您需要采取的措施︰

1.  创建[链接的服务](data-factory-azure-sql-connector.md/#azure-sql-linked-service-properties)注册应在执行该存储的过程的 SQL Azure 数据库的连接字符串。
2.  创建一个指向输出 SQL Azure 数据库中表的[数据集](data-factory-azure-sql-connector.md/#azure-sql-dataset-type-properties)。 让我们来调用此数据集 sprocsampleout。 此数据集应引用步骤 1 中的链接的服务。 
3.  在 SQL Azure 数据库中创建存储的过程。
4.  创建下面[管道](data-factory-azure-sql-connector.md/#azure-sql-copy-activity-type-properties)与 SqlServerStoredProcedure 活动来调用 SQL Azure 数据库中的存储的过程。

        {
            "name": "SprocActivitySamplePipeline",
            "properties":
            {
                "activities":
                [
                    {
                        "name": "SprocActivitySample",
                        "type": " SqlServerStoredProcedure",
                        "outputs": [ {"name": "sprocsampleout"} ],
                        "typeProperties":
                        {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": 
                            {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        }
                    }
                ]
             }
        }
5.  将部署[管道](data-factory-create-pipelines.md)。
6.  使用数据工厂监视和管理视图[监视管道](data-factory-monitor-manage-pipelines.md)。

> [AZURE.NOTE] 在上面的示例中，SprocActivitySample 有任何输入。 如果您想要此链与上游活动 （即 前处理的），上游活动的输出可以用作输入在此活动中。  在这种情况下，直到完成的上游活动和上游活动的输出 （在就绪状态） 可将不执行此活动。 输入不能直接用作存储的过程活动的参数
> 
> 名称和 （大小） 的 JSON 文件中的存储的过程参数的大小写必须与目标数据库中的存储的过程参数的名称匹配。

现在，让我们考虑包含静态值称为文档示例的表中添加另一列命名为方案。

![示例数据 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

若要实现此目的，通过从存储的过程活动方案参数和值。 在上面的示例中的 typeProperties 节如下所示︰

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

测试
