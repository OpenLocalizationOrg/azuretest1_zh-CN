---
ms.openlocfilehash: 518ea3fd39a94b0646f2b85ad26d664ce9062cdc
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将数据从 MySQL 移动 |Azure 数据工厂" 
    description="了解如何将数据从使用 Azure 数据工厂的 MySQL 数据库移动。" 
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

# 移动数据从 MySQL 使用 Azure 数据工厂

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从 MySQL 到另一个数据存储的数据移动。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

数据工厂服务支持连接到内部部署 MySQL 源使用数据管理网关。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 

**注意︰**您需要利用网关连接到 MySQL，即使承载于 Azure IaaS Vm。 如果您试图连接到 MySQL 承载在云实例，还可以在 IaaS VM 安装网关实例。

数据工厂目前支持仅移动数据从 MySQL 到其他数据存储，而不是将数据从其他数据存储区移动到 MySQL。

## 安装 
对于数据管理网关连接到 MySQL 数据库，您需要为数据管理网关在同一系统上安装[MySQL 连接器/Net Microsoft Windows 的 6.6.5](http://go.microsoft.com/fwlink/?LinkId=278885) 。

## 示例︰ 将 MySQL 数据复制到 Azure Blob

下面的示例演示︰

1.  链接的类型[OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 MySQL 数据库中的查询结果对 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，请设置数据管理网关根据[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章中的说明进行操作。 

**MySQL 链接服务**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }

**Azure 存储链接服务**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**MySQL 的输入数据集**

该示例假定您创建了一个表"MyTable"在 MySQL 中，它包含名为"timestampcolumn"的时间系列数据的列。

设置为"外部":"true"，并指定 externalData 策略将通知数据工厂服务表是外部数据工厂并不是活动数据工厂中生产。
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**管线与复制活动**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。 **查询**属性指定的 SQL 查询选择过去小时要复制的数据。
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## MySQL 链接服务属性

下表提供了 JSON 元素特定于 MySQL 链接服务的说明。

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- | 
| 类型 | 类型属性必须设置为︰ **OnPremisesMySql** | 是 |
| 服务器 | MySQL 服务器的名称。 | 是 |
| 数据库 | MySQL 数据库的名称。 | 是 | 
| 架构  | 在数据库中的架构的名称。 | 否 | 
| authenticationType | 用于连接到 MySQL 数据库的身份验证类型。 可能的值包括︰ 匿名的基本的和窗口。 | 是 | 
| 用户名 | 如果您使用基本或 Windows 身份验证，请指定用户名。 | 否 | 
| password | 指定对您指定的用户名的用户帐户的密码。 | 否 | 
| gatewayName | 数据工厂服务连接到内部部署 MySQL 数据库应使用的网关的名称。 | 是 |

有关设置为内部部署 MySQL 数据源凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。

## MySQL 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (第 SQL Azure Azure blob，Azure 表，等等。..)。

**TypeProperties**节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 TypeProperties 部分类型**RelationalTable** （其中包括 MySQL 数据集） 的数据集具有以下属性

| 属性 | 说明 | 是否必需 |
| -------- | ----------- | -------- |
| 表名 | 链接的服务指的 MySQL 数据库实例中的表的名称。 | 是 | 

## MySQL 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。 

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型**RelationalSource** （其中包括 MySQL） 以下属性部分提供了 typeProperties:

| 属性 | 说明 | 允许的值 | 是否必需 |
| -------- | ----------- | -------------- | -------- |
| 查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ 选择 * 从 MyTable。 | 是 | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### MySQL 的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行自动类型转换来水池下面 2 个步骤方法类型的源类型︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

将数据移动到 MySQL 将以下映射从 MySQL 类型中使用.NET 类型中。

| MySQL 数据库类型 | .NET Framework 类型 |
| ------------------- | ------------------- | 
| 无符号的 bigint | 小数 |
| bigint | Int64 |
| 位 | 小数 |
| blob | Byte] |
| bool |  Boolean | 
| 字符 | String |
| date | Datetime |
| 日期时间 | Datetime |
| decimal | 小数 |
| 双精度 | Double |
| double | Double |
| 枚举 | String |
| float | Single |
| 无符号整型 | Int64 |
| int | Int32 |
| 无符号的整数 | Int64 |
| integer | Int32 | 
| 长的低级 | Byte] |
| 长 varchar | String |
| longblob | Byte] |
| 长文本 | String | 
| mediumblob |  Byte] | 
| mediumint 无符号 | Int64 |
| mediumint | Int32 | 
| mediumtext | String |
| 数字 | 小数 |
| 现实 |  Double |
| 设置 | String |
| 无符号 smallint | Int32 |
| smallint | Int16 |
| text | String |
| 时间 | 时间跨度 |
| 时间戳 | Datetime |
| tinyblob | Byte] |
| tinyint 无符号 |  Int16 | 
| tinyint | Int16 |
| tinytext | String | 
| varchar | String | 
| 年份 | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]




测试
