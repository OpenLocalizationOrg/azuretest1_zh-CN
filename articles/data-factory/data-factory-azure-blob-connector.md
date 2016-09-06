---
ms.openlocfilehash: c6b9f48dd40e2f03b7347939f30b11000184705f
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将数据移动到 Azure Blob 与 |Azure 数据工厂" 
    description="了解如何移动数据到从 Azure Blob 存储使用 Azure 数据工厂" 
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

# 使用 Azure 数据工厂的 Azure 斑点之间转移数据
这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂移至 Azure Blob 数据从另一个数据存储和将数据从其他数据存储区移动到 Azure Blob。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

## 示例︰ 将 Azure Blob 数据复制到 SQL Azure
下面的示例演示︰

1.  链接的类型[AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](#azure-storage-linked-service-properties)的服务。
3.  [AzureBlob](#azure-blob-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  使用[BlobSource](#azure-blob-copy-activity-type-properties)和[SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)的复制活动的[管道](data-factory-create-pipelines.md)。

该示例将属于从 Azure 系列斑点到 Azure SQL 数据库的表中每隔一小时一次的数据复制。 在这些示例中使用的 JSON 属性详见以下示例部分。 

**SQL azure 链接的服务︰**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Blob 输入数据集︰**

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

**SQL azure 的输出数据集︰**

示例将数据复制到表的 SQL Azure 数据库中名为"MyTable"。 像您预期的 Blob CSV 文件包含，则应在 SQL Azure 数据库具有相同的列数创建表。 新行被添加到表的每个小时。

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

**管线与复制活动︰**

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

## 示例︰ 将数据从 SQL Azure 复制到 Azure Blob
下面的示例演示︰

1.  链接的类型[AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](#azure-storage-linked-service-properties)的服务。
3.  [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[SqlSource](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)和[BlobSink](#azure-blob-copy-activity-type-properties)的复制活动。


该示例将属于时序对 blob 的 SQL Azure 数据库中的表中的每个小时的数据复制。 在这些示例中使用的 JSON 属性详见以下示例部分。 

**SQL azure 链接的服务︰**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure SQL 输入数据集︰**

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

**Azure Blob 的输出数据集︰**

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

**管线与复制活动︰**

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

## Azure 存储链接服务属性

可以将 Azure 存储帐户链接到 Azure 数据工厂使用 Azure 存储链接服务。 下表提供了 JSON 元素特定于 Azure 存储链接服务的说明。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- |
| 类型 | 类型属性必须设置为︰ **AzureStorage** | 是 |
| 连接字符串 | 指定连接到 Azure 存储连接字符串属性所需的信息。 您可以从 Azure 门户 Azure 存储获取连接字符串。 | 是 |

## Azure Blob 数据集的类型属性

JSON 部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (第 SQL Azure Azure blob，Azure 表，等等。..)。

**TypeProperties**部分对于每种类型的数据集是不同的并提供信息所在的位置，格式在数据存储区中的数据等。 TypeProperties 部分类型**AzureBlob**数据集的数据集具有以下属性。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- | 
| 采用文件夹路径 | 容器和 blob 存储中的文件夹的路径。 示例︰ myblobcontainer\myblobfolder\ | 是 |
| 文件名 | <p>该 blob 名称。 文件名是可选的。 </p><p>如果您指定一个文件名，活动 （包括副本） 将适用于特定的 Blob。</p><p>未指定文件名复制将包括所有 Blob 中的输入数据集采用文件夹路径。</p><p>当输出数据集未指定文件名时，所生成的文件的名称将在下面的格式︰ 数据。<Guid>.txt (例如:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</p> | 否 |
| partitionedBy | partitionedBy 是一个可选属性。 可用于指定的动态采用文件夹路径和文件名的时间系列数据。 例如，可以为每个小时的数据参数化采用文件夹路径。 利用 partitionedBy prperty 下面的部分详细信息和示例，请参阅。 | 否
| 格式 | 支持两种格式类型︰**绑定**、 **AvroFormat**。 您需要将下面格式的类型属性设置为这些值。 格式化后的 TextFormat 可以指定格式的其他可选属性。 请参阅下面的详细[指定格式](#specifying-textformat)一节。 | 否
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括︰ GZip，Deflate，BZip2 并受支持的级别是︰ 最佳和最快的。 有关更多详细信息请参阅[压缩支持](#compression-support)部分。  | 否 |

### 利用 partitionedBy 属性
如上所述，您可以使用**partitionedBy**部分、 数据工厂宏和系统变量指定的动态采用文件夹路径和文件名的时间系列数据︰ SliceStart 和 SliceEnd，指示给定的数据片的开始和结束时间。

请参阅[创建数据集](data-factory-create-datasets.md)，并[计划与执行](data-factory-scheduling-and-execution.md)的文章，以了解更多详细信息的时间系列数据集、 计划编制和切片。

#### 示例 1

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

在上述示例 {切片} 替换指定的格式 (YYYYMMDDHH) 中的数据工厂系统变量 SliceStart 的值。 SliceStart 是指切片的开始时间。 采用文件夹路径是不同的每个扇区。 例如︰ wikidatagateway/wikisampledataout/2014100103 或 wikidatagateway/wikisampledataout/2014100104

#### 示例 2

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

在上面的示例中，年、 月、 日和时间 SliceStart 提取到不同采用文件夹路径和文件名属性所使用的变量。

### 指定格式

如果格式设置为**格式**，可以在**格式**部分中指定以下**可选**属性。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- |
| columnDelimiter | 使用作为一个文件中的列分隔符的字符。此标记是可选的。 默认值为逗号 （，）。 | 否 |
| rowDelimiter | 用作文件中的原始分隔符的字符。 此标记是可选的。 默认值是以下任一: ["\r\n"、"\r"、"\n"]。 | 否 |
| escapeChar | <p>用于内容中显示的列分隔符进行转义特殊字符。 此标记是可选的。 没有默认值。 您必须指定此属性不能超过个字符。</p><p>例如，如果您有列分隔符为逗号 （，），但要有文本中的逗号字符 (示例:"Hello，世界")，可以将 $ 定义为转义字符，并使用字符串"Hello$，世界"源中。</p><p>请注意，您不能指定 escapeChar 和 quoteChar 的表。</p> | 否 | 
| quoteChar | <p>特殊字符用于报价的字符串值。 在引号字符的列和行分隔符将视为字符串值的一部分。 此标记是可选的。 没有默认值。 您必须指定此属性不能超过个字符。</p><p>例如，如果您有列分隔符为逗号 （，），但要有文本中的逗号字符 (示例︰ < Hello、 世界 >)，您可以定义"' 报价为源中使用 <"Hello，世界"> 字符串和字符。 此属性是适用于输入和输出表。</p><p>请注意，您不能指定 escapeChar 和 quoteChar 的表。</p> | 否 |
| nullValue | <p>用来表示 null 值斑点文件内容中的字符。 此标记是可选的。 默认值是"\N"。</p><p>例如，上面的示例的基础，"NaN"blob 将被转换为空值时复制到例如 SQL Server。</p> | 否 |
| encodingName | 指定编码的名称。 有效的编码名称的列表，请参见︰ [Encoding.EncodingName 属性](https://msdn.microsoft.com/library/system.text.encoding.aspx)。 例如︰ windows 1250 或 shift_jis。 默认值是︰ utf-8。 | 否 | 

#### 示例
下面的示例演示一些格式属性的格式。

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
        }
    },

若要使用 escapeChar 而不是 quoteChar，替换 quoteChar 与以下行︰

    "escapeChar": "$",

### 指定 AvroFormat
如果格式设置为 AvroFormat，您不需要在 typeProperties 部分中的格式部分中指定的任何属性。 示例：

    "format":
    {
        "type": "AvroFormat",
    }

若要配置单元表中使用 Avro 格式，您可以参考[Apache 配置单元的教程](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)。

[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## Azure Blob 复制活动类型属性  
节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器

**BlobSource**在**typeProperties**部分支持以下属性︰

| 属性 | 说明 | 允许的值 | 是否必需 |
| -------- | ----------- | -------------- | -------- | 
| treatEmptyAsNull | 指定是否将空值为 null 或空字符串。 | TRUE<br/>FALSE | 否 |
| skipHeaderLineCount | 指示需要跳过的行数。 仅当输入数据集使用**绑定**是适用。 | 从 0 到最大值的整数。 | 否 | 


**BlobSink**支持以下属性**typeProperties**部分︰

| 属性 | 说明 | 允许的值 | 是否必需 |
| -------- | ----------- | -------------- | -------- |
| blobWriterAddHeader | 指定是否要添加标题的列定义。 | TRUE<br/>FALSE （默认值） | 否 |


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]



## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-azure-blob-connector.md)。













测试
