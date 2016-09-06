---
ms.openlocfilehash: 58b6b230cfe5181890ee6ddf7d260f89c51e77e2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将数据移动到中，并从文件系统 |Azure 数据工厂" 
    description="了解如何移动到从使用 Azure 数据工厂的内部文件系统数据。" 
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

# 使用 Azure 数据工厂的内部文件系统之间移动数据

这篇文章概括介绍了如何使用数据工厂复制活动以内部文件系统之间移动数据。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

数据工厂支持连接到和从本地文件系统，通过数据管理网关。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解数据管理网关和设置网关执行步骤的说明。 

**注意︰**除了数据管理网关没有其他二进制文件需要被安装为从本地文件系统与通信。 

## Linux 文件共享 

执行以下两个步骤用于 Linux 文件共享文件服务器链接服务︰

- 在 Linux 服务器上安装[Samba](https://www.samba.org/) 。
- 安装和配置 Windows 服务器上的数据管理网关。 不支持在 Linux 服务器上的安装网关。 
 
## 示例︰ 将数据从本地文件系统复制到 Azure Blob

下面的示例演示︰

1.  链接的类型[OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务
3.  [文件](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties)类型输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。 

下面的示例将复制属于数据时间序列内部文件系统到 Azure blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，请不要安装程序按照提供的说明本文[的部署位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)的数据管理网关。 

**内部链接的文件服务器服务︰**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

**Azure Blob 存储链接的服务︰**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**内部文件系统输入数据集︰**

数据拾取从一个新的文件的路径和文件名与小时粒度反映特定日期时间每隔一小时。 

设置为"外部":"真正的"，并指定 externalData 策略通知 Azure 数据工厂服务表是外部数据工厂并不是活动数据工厂中生产。

    {
      "name": "OnpremisesFileSystemInput",
      "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
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
          ]
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
                "format": "%HH"
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

**复制活动︰**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**FileSystemSource** ，**接收器**类型设置为**BlobSink**。 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T19:00:00",
        "description":"Pipeline for copy activity",
        "activities":[  
          {
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "OnpremisesFileSystemInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "FileSystemSource"
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

##示例︰ 将数据从 SQL Azure 复制到本地文件系统 

下面的示例演示︰

1.  链接的类型 AzureSqlDatabase 的服务。
2.  链接的类型 OnPremisesFileServer 的服务。
3.  AzureSqlTable 类型的输入数据集。 
3.  类型文件的输出数据集。
4.  使用 SqlSource 和 FileSystemSink 的复制活动的管道。

此示例复制属于数据时间序列为内部部署文件系统的 SQL Azure 数据库中的表中每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

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

**内部链接的文件服务器服务︰**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

**Azure SQL 输入数据集︰**

该示例假定您已创建表"MyTable"在 SQL Azure 并且它包含名为"timestampcolumn"的时间系列数据的列。 

设置为"外部":"真正的"，并指定 externalData 策略通知数据工厂服务表外部数据工厂并不由数据工厂中的活动。

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

**内部文件系统输出数据集︰**

Blob 小时粒度与反映特定日期时间每隔 1 小时与路径数据被复制到新文件。

    {
      "name": "OnpremisesFileSystemOutput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
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
                "format": "%HH"
              }
            }
          ]
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

**与复制活动管线︰**管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**SqlSource** ，**接收器**类型设置为**FileSystemSink**。 **SqlReaderQuery**属性指定的 SQL 查询选择过去小时要复制的数据。

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T20:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "OnpremisesFileSystemOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "FileSystemSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 3,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## OnPremisesFileServer 链接服务属性

您可以使用内部部署文件服务器链接服务链接到 Azure 数据工厂内部部署文件系统。 下表提供给内部文件服务器链接服务的 JSON 元素特定说明。 

属性 | 说明 | 是否必需
-------- | ----------- | --------
类型 | Type 属性应设置为**OnPremisesFileServer** | 是 
主机 | 服务器的主机名。 使用 \ 用作转义字符，如以下示例所示︰ 如果您的共享文件夹︰\\服务器名称，指定\\\\服务器名。<p>如果网关计算机的本地文件系统，使用本地或本地主机。 如果文件系统不同于网关计算机的服务器上，请使用\\\\服务器名。</p> | 是
用户 id  | 指定有权访问该服务器的用户的 ID | 否 （如果选择了 encryptedCredential）
password | 指定的密码的用户 (用户 id) | 否 （如果您选择 encryptedCredential 
encryptedCredential | 指定您可以通过运行新建 AzureDataFactoryEncryptValue cmdlet 的加密的凭据<p>**注意︰**您必须使用 Azure PowerShell 的版本 0.8.14 或更高版本，如新建 AzureDataFactoryEncryptValue cmdlet 使用类型参数设置为 OnPremisesFileSystemLinkedService</p> | 否 （如果您选择以纯文本格式指定用户 id 和密码）
gatewayName | 数据工厂服务连接到内部文件服务器应使用的网关的名称 | 是

有关设置本地文件系统数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。

**示例︰ 使用用户名和密码以纯文本格式**
    
    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }
    
**示例︰ 使用 encryptedcredential**

    {
      "Name": " OnPremisesFileServerLinkedService ",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "localhost",
          "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
          "gatewayName": "mygateway"
        }
      }
    }

## 内部文件系统数据集的类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (SQL Azure，Azure Blob，Azure 表、 内部文件系统，等等。..)。 

TypeProperties 部分对于每种类型的数据集是不同的并提供信息所在的位置，格式在数据存储区中的数据等。 TypeProperties 部分类型**文件共享**数据集的数据集具有以下属性。

属性 | 说明 | 是否必需
-------- | ----------- | --------
采用文件夹路径 | 指向文件夹的路径。 示例︰ myfolder 文件夹<p>使用转义字符 '\' 特殊字符在字符串中。 例如︰ 指定文件夹的 folder\subfolder，\\子文件夹和 d:\samplefolder，用于指定 d:\\相应的访问。</p><p>可以使用**partitionBy**来具有基于片上的文件夹路径组合这起止日期时间。</p> | 是
文件名 | 在**采用文件夹路径**指定的文件的名称，如果您想要特定文件夹中的文件引用的表。 如果未指定此属性的任何值，表指向该文件夹中的所有文件。<p>当输出数据集未指定文件名时，所生成的文件的名称将在下面的格式︰ </p><p>数据。<Guid>.txt (例如:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</p> | 否
partitionedBy | 可以利用 partitionedBy 来指定动态采用文件夹路径，用于时间序列数据的文件名。 例如采用文件夹路径参数的数据每隔一小时。 | 否
Format | 支持两种格式类型︰**绑定**、 **AvroFormat**。 您需要将格式下的类型属性设置为或者如果此值。 ForAvroFormatmat 格式时可以指定格式的其他可选属性。 请参阅下面的详细格式一节。 | 否
过滤 | 指定的筛选器可用于选择采用文件夹路径中的文件，而不是所有文件的子集。 <p>允许值为︰ *（多个字符） 和？（单个字符）。</p><p>示例 1:"过滤":"*.log"</p>示例 2:"过滤": 2014-1-？。"txt</p><p>**注意**︰ 过滤是适用于输入文件共享数据集</p> | 否
| 压缩 | 指定的类型和级别的数据压缩。 支持的类型包括︰ GZip，Deflate，BZip2 并受支持的级别是︰ 最佳和最快的。 有关更多详细信息请参阅[压缩支持](#compression-support)部分。  | 否 |

> [AZURE.NOTE] 不能同时使用文件名和过滤。

### 利用 partionedBy 属性

如上所述，您可以指定动态采用文件夹路径，文件名与 partitionedBy 的时间系列数据。 您可以使用数据工厂宏和系统变量 SliceStart，SliceEnd，用于指示给定的数据切片的逻辑时间段这样做。 

请参阅[创建数据集](data-factory-create-datasets.md)、[计划与执行](data-factory-scheduling-and-execution.md)、 以及[创建管线](data-factory-create-pipelines.md)的文章，以了解更多详细信息的时间系列数据集、 计划编制和切片。

#### 示例 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

在上述示例 {切片} 替换指定的格式 (YYYYMMDDHH) 中的数据工厂系统变量 SliceStart 的值。 SliceStart 是指切片的开始时间。 采用文件夹路径是不同的每个扇区。 例如︰ wikidatagateway/wikisampledataout/2014100103 或 wikidatagateway/wikisampledataout/2014100104。

#### 示例 2:

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

如果将**TextFormat**，可以在**typeProperties**部分中的**格式**部分中指定以下**可选**属性。 

属性 | 说明 | 是否必需
-------- | ----------- | --------
columnDelimiter | 使用作为一个文件中的列分隔符的字符。 默认值为逗号 （，）。 | 否
rowDelimiter | 用作文件中的原始分隔符的字符。 默认值是以下任一: ["\r\n"、"\r"、"\n"]。 | 否
escapeChar | 用于内容中显示的列分隔符进行转义特殊字符。 没有默认值。 您必须指定此属性不能超过个字符。<p>例如，如果您有列分隔符为逗号 （，），但要有文本中的逗号字符 (示例:"Hello，世界")，可以将 $ 定义为转义字符，并使用字符串"Hello$，世界"源中。</p><p>请注意，您不能指定 escapeChar 和 quoteChar 的表。</p> | 否
quoteChar | 特殊字符用于报价的字符串值。 在引号字符的列和行分隔符将视为字符串值的一部分。 没有默认值。 您必须指定此属性不能超过个字符。<p>例如，如果您有列分隔符为逗号 （，），但要有文本中的逗号字符 (示例︰ < Hello、 世界 >)，您可以定义"' 报价为源中使用 <"Hello，世界"> 字符串和字符。 此属性是适用于输入和输出表。</p><p>请注意，您不能指定 escapeChar 和 quoteChar 的表。</p> | 否
nullValue | 用来表示 null 值斑点文件内容中的字符。 默认值是"\N"。 > | 否
encodingName | 指定编码的名称。 有效的编码名称的列表，请参见︰ Encoding.EncodingName 属性。 <p>例如︰ windows 1250 或 shift_jis。 默认值是︰ utf-8。</p> | 否

#### 示例︰

下面的示例演示一些格式属性的**格式**。

    "typeProperties":
    {
        "folderPath": "MyFolder",
        "fileName": "MyFileName"
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

如果格式设置为**AvroFormat**，您不需要在 typeProperties 部分中的格式部分中指定的任何属性。 示例：

    "format":
    {
        "type": "AvroFormat",
    }
    
要在随后的一个配置单元表使用 Avro 格式，请参阅[Apache 配置单元的教程](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)。       

[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## 文件共享复制活动类型属性

**FileSystemSource**和**FileSystemSink**不支持任何属性这一次。

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]








 

测试
