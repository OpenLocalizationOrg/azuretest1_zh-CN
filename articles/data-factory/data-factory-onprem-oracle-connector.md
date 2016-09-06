---
ms.openlocfilehash: 5724ab26f83038fd83dbe69eb1ffe22d8cf6ea42
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Oracle 之间转移数据 |Azure 数据工厂" 
    description="了解如何移动 Oracle 数据库即为来自内部使用 Azure 数据工厂。" 
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
    ms.date="08/26/2015" 
    ms.author="spelluru"/>

# 将数据移动到内部部署 Oracle 使用 Azure 数据工厂 

这篇文章概括介绍了如何使用数据工厂复制活动将从 Oracle 的数据移动到另一个数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

## 示例︰ 将数据从 Oracle 复制到 Azure Blob

下面的示例演示︰

1.  链接的类型[OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。 
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
5.  [管线](data-factory-create-pipelines.md)与使用作为源和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) ，作为接收器的复制活动。

该示例将数据从复制上部署 Oracle 数据库中的表对 blob 每隔一小时。 在下面的示例中使用的各种属性的详细信息请参阅以下示例部分中的不同属性的文档。

**链接的 oracle 服务︰**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle 输入数据集︰**

该示例假定您创建了一个表"MyTable"Oracle 中并且包含名为"timestampcolumn"的时间系列数据的列。 

设置为"外部":"真"，并指定 externalData 策略会告诉数据工厂，这是表的外部数据工厂并不是活动数据工厂中生产。

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
            },
          "policy": {     
               "externalData": {        
                    "retryInterval": "00:01:00",    
                    "retryTimeout": "00:10:00",       
                    "maximumRetry": 3       
                }     
              }
        }
    }


**Azure Blob 的输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 Blob 的文件夹路径和文件名称动态地计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。
    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%M"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%d"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "%H"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**管线与复制活动︰**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。  **OracleReaderQuery**属性指定的 SQL 查询选择过去小时要复制的数据。

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## Oracle 链接服务属性

下表提供了 JSON 元素特定于 Oracle 链接服务的说明。 

属性 | 说明 | 是否必需
-------- | ----------- | --------
类型 | 类型属性必须设置为︰ **OnPremisesOracle** | 是
连接字符串 | 指定连接到 Oracle 数据库实例的连接字符串属性所需的信息。 | 是 
gatewayName | 将用于连接到 onpremises Oracle 服务器的网关的名称 | 是

有关设置内部 Oracle 数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。
## Oracle 数据集类型属性

部分和属性可用于定义数据集的完整列表请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (Oracle，Azure blob，Azure 表，等等。..)。
 
TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 为数据集类型 OracleTable 的 typeProperties 部分具有以下属性。

属性 | 说明 | 是否必需
-------- | ----------- | --------
表名 | 链接的服务指 Oracle 数据库中的表的名称。 | 是

## Oracle 复制活动类型属性

部分和可用于定义活动属性的完整列表请参阅文章[创建管线](data-factory-create-pipelines.md)。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。 

**注意︰**复制活动采用一个输入，并生成一个输出。

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型 SqlSource 以下属性部分提供了 typeProperties:

属性 | 说明 |允许的值 | 是否必需
-------- | ----------- | ------------- | --------
oracleReaderQuery | 使用自定义查询中读取数据。 | SQL 查询字符串。 
例如︰ 选择*从 MyTable<p>如果未指定，则执行的 SQL 语句︰ 选择*从 MyTable</p> | 否

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### 适用于 Oracle 的类型映射

所述[数据移动活动](data-factory-data-movement-activities.md)文章复制活动执行从要水池下面 2 个步骤方法类型的源类型的自动类型转换的自动类型转换︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

从 Oracle 中移动数据，以下映射将使用从 Oracle 数据类型为.NET 类型，反之亦然。

Oracle 数据类型 | .NET Framework 数据类型
---------------- | ------------------------
BFILE | Byte]
BLOB | Byte]
CHAR | String
CLOB | String
DATE | DateTime
FLOAT | 小数
INTEGER | 小数
间隔年到月 | Int32
间隔一天中第二个 | 时间跨度
长 | String
长时间裸 | Byte]
NCHAR | String
NCLOB | String
编号 | 小数
NVARCHAR2 | String
原始 | Byte]
ROWID | String
时间戳 | DateTime
具有本地时区的时间戳 | DateTime
与时区的时间戳 | DateTime
无符号的整数 | 数字
VARCHAR2 | String
XML | String


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]




测试
