---
ms.openlocfilehash: 261691d3b655f3f80651e2fde2af80bc928dcc76
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="SQL Azure 之间转移数据 |Azure 数据工厂" 
    description="了解如何移动到从 Azure 使用 Azure 数据工厂的 SQL 数据库的数据。" 
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

# Azure SQL 使用 Azure 数据工厂之间转移数据

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂移到 SQL Azure 数据从另一个数据存储和将数据从其他数据存储区移动到 SQL Azure。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

## 示例︰ 将数据从 SQL Azure 复制到 Azure Blob

下面的示例演示︰

1. 链接的类型[AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)的服务。
2. 链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。 
3. [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。 
4. 类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4. [管线](data-factory-create-pipelines.md)与使用[SqlSource](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将属于时序对 blob 的 SQL Azure 数据库中的表中的每个小时的数据复制。 在这些示例中使用的 JSON 属性详见以下示例部分。  

**Azure SQL 链接服务**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Blob 存储链接服务**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure SQL 输入数据集**

该示例假定您已创建表"MyTable"在 SQL Azure 并且它包含名为"timestampcolumn"的时间系列数据的列。 

设置为"外部":"真正的"，并指定 externalData 策略通知 Azure 数据工厂服务，这是表的外部数据工厂并不是活动数据工厂中生产。

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
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

**Azure Blob 输出数据集**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。 

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

**管线与复制活动**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**SqlSource** ，**接收器**类型设置为**BlobSink**。 **SqlReaderQuery**属性指定的 SQL 查询选择过去小时要复制的数据。

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
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## 示例︰ 将 Azure Blob 数据复制到 SQL Azure

下面的示例演示︰

1.  链接的类型[AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)和[SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)的复制活动。

该示例将属于从 Azure 系列斑点到 Azure SQL 数据库的表中每隔一小时一次的数据复制。 在这些示例中使用的 JSON 属性详见以下示例部分。 


**Azure SQL 链接服务**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Blob 存储链接服务**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Blob 输入数据集**

数据挑选新的斑点从每小时 (频率︰ 小时、 间隔︰ 1)。 Blob 的文件夹路径和文件名称动态地计算基于切片所处理的开始时间。 文件夹路径使用的年、 月和日部分的开始时间和文件的名称使用的小时部分的开始时间。 "外部":"真正的"设置通知数据工厂服务，此表是外部数据工厂并不是活动数据工厂中生产。

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
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

**Azure 的 SQL 输出数据集**

该示例将数据复制到名为"MyTable"SQL Azure 中的表。 按照您的预期来包含 Blob CSV 文件，应在 Azure SQL 具有相同的列数创建的表。 新行被添加到表的每个小时。 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**管线与复制活动**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**BlobSource** ，**接收器**类型设置为**SqlSink**。

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
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

## SQL azure 链接服务属性

下表提供了 JSON 元素特定于 Azure SQL 链接服务的说明。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- |
| 类型 | 类型属性必须设置为︰ AzureSqlDatabase | 是 |
| 连接字符串 | 指定连接到连接字符串属性的 SQL Azure 数据库实例所需的信息。 | 是 |

**注意︰**您需要将[Azure SQL 数据库防火墙](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)配置。 您需要配置为[允许访问该服务器的 Azure 服务](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)数据库服务器。 此外，如果您将从内部数据源与数据工厂网关，您需要配置相应的 IP 地址范围，机外 Azure 包括数据复制到 SQL Azure 的将数据发送到 SQL Azure。 

## Azure SQL 数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (第 SQL Azure Azure blob，Azure 表，等等。..)。 

TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 为数据集类型**AzureSqlTable**的**typeProperties**部分具有以下属性。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- |
| 表名 | 是指链接服务的 SQL Azure 数据库实例中的表的名称。 | 是 |

## Azure SQL 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅文章[创建管线](data-factory-create-pipelines.md)。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。

> [AZURE.NOTE] 复制活动采用一个输入，并生成一个输出。

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型**SqlSource**以下属性部分提供了**typeProperties** :

| 属性 | 说明 | 允许的值 | 是否必需 |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | 使用自定义查询中读取数据。 | SQL 查询字符串。例如︰ 选择 * 从 MyTable。 如果未指定，则执行的 SQL 语句︰ 从 MyTable 中进行选择。 | 否 |

**SqlSink**支持以下属性︰

| 属性 | 说明 | 允许的值 | 是否必需 |
| -------- | ----------- | -------------- | -------- |
| sqlWriterStoredProcedureName | Upsert （更新/插入） 到目标表中的数据到用户指定的存储的过程名称。 | 存储过程的名称。 | 否 |
| sqlWriterTableType | 用户指定的表的类型名称，将在上述存储过程中使用。 复制活动使正在移动的数据可与此表类型的临时表中。 存储的过程代码可以将合并现有数据与所复制的数据。 | 表类型名称。 | 否 |
| writeBatchTimeout | 批量插入操作完成超时之前的等待时间。 | (单位 = 时间跨度)示例:"00: 30:00"（30 分钟）。 | 否 | 
| sqlWriterCleanupScript | 用户指定的复制活动来执行，这样将会清除特定扇区的数据的查询。 请参见下面的更多详细信息的可再现部分。 | 一条查询语句。  | 否 |
| sliceIdentifierColumnName | 用户指定的列名称复制活动填充自动生成片标识符，用于清除数据的特定切片时重新运行。 请参见下面的更多详细信息的可再现部分。 | 使用数据类型的 binary(32) 列的列名称。 | 否 |

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### SQL 服务器和 SQL Azure 的类型映射

所述[数据移动活动](data-factory-data-movement-activities.md)文章复制活动执行从要水池下面 2 个步骤方法类型的源类型的自动类型转换的自动类型转换︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

当将数据移动到与从 Azure SQL，SQL 服务器，Sybase 以下映射将使用从 SQL 类型为.NET 类型，反之亦然。

映射是相同 ADO.NET SQL Server 数据类型映射。

| SQL Server 数据库引擎类型 | .NET Framework 类型 |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| 二进制文件 | Byte] |
| 位 | Boolean |
| 字符 | 字符串中，Char] |
| date | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| 方法 | 方法 |
| 小数 | 小数 |
| 文件流特性 (varbinary(max)) | Byte] |
| 浮点型 | Double |
| image | Byte] | 
| int | Int32 | 
| 资金 | 小数 |
| nchar | 字符串中，Char] |
| ntext | 字符串中，Char] |
| 数字 | 小数 |
| nvarchar | 字符串中，Char] |
| 现实 | Single |
| 行版本 | Byte] |
| 短格式 | DateTime |
| smallint | Int16 |
| smallmoney | 小数 | 
| sql_variant | 对象 * |
| text | 字符串中，Char] |
| 时间 | 时间跨度 |
| 时间戳 | Byte] |
| tinyint | 字节 |
| 唯一标识符 | Guid |
| 低级 |  Byte] |
| varchar | 字符串中，Char] |
| xml | Xml |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-azure-sql-connector.md)。



     










测试
