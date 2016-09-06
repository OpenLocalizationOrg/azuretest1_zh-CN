---
ms.openlocfilehash: 599b6f225e831d1497b82d26d8dda42b6d66bc9b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将数据从 Teradata 移动 |Azure 数据工厂" 
    description="允许您将数据从 Teradata 数据库移动数据工厂服务了解 Teradata 连接器" 
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

# 将数据从 Teradata 使用 Azure 数据工厂

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将从 Teradata 到另一个数据存储的数据移动。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

数据工厂支持连接到数据管理网关通过内部 Teradata 源。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 

**注意︰**您需要利用网关连接到 Teradata，即使承载于 Azure IaaS Vm。 如果您尝试连接到承载云在 Teradata 实例也可以安装网关实例 IaaS VM 中。

数据工厂支持仅移动数据从 Teradata 到其他数据存储区而不是 Teradata 到其他数据存储区。

## 安装 

对于数据管理网关连接到 Teradata 数据库，您需要安装[.NET 数据提供程序 Teradata](http://go.microsoft.com/fwlink/?LinkId=278886)数据管理网关与相同的系统上。

### 示例︰ 将 Teradata 数据复制到 Azure Blob

下面的示例演示︰

1.  链接的类型[OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties)的服务。
2.  链接的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。 
4.  [管线](data-factory-create-pipelines.md)与使用[RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从复制 Teradata 数据库中查询结果对 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，设置数据管理网关根据[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章中的说明进行操作。

**Teradata 链接的服务︰**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure Blob 存储链接的服务︰**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Teradata 输入数据集︰**

该示例假定您创建了一个表"MyTable"在 Teradata 和它包含名为"时间戳"时间系列数据的列。

设置为"外部": 真并指定 externalData 策略告知数据工厂表是外部数据工厂并不是活动数据工厂中生产。

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
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


**Azure Blob 的输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**管线与复制活动︰**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。 **查询**属性指定的 SQL 查询选择过去小时要复制的数据。

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## Teradata 链接服务属性

下表提供了 JSON 元素特定于 Teradata 链接服务的说明。 

属性 | 说明 | 是否必需
-------- | ----------- | --------
类型 | 类型属性必须设置为︰ **OnPremisesTeradata** | 是
服务器 | Teradata 服务器的名称。 | 是
数据库 | Teradata 数据库的名称。 | 是 
架构  | 在数据库中的架构的名称。 | 否
authenticationType | 用于连接到 Teradata 数据库的身份验证类型。 可能的值包括︰ 匿名的基本的和窗口。 | 是
用户名 | 如果您使用基本或 Windows 身份验证，请指定用户名。 | 否 
password | 指定对您指定的用户名的用户帐户的密码。 | 否 
gatewayName | 数据工厂服务连接到内部 Teradata 数据库应使用的网关的名称。 | 是

有关设置内部 Teradata 数据源的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。

## Teradata 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (第 SQL Azure Azure blob，Azure 表，等等。..)。

TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 **TypeProperties**部分类型**RelationalTable** （其中包括 Teradata 数据集） 的数据集具有以下属性。

属性 | 说明 | 是否必需
-------- | ----------- | --------
表名 | 链接的服务指 Teradata 数据库实例中的表的名称。 | 是 

## Teradata 复制活动类型属性

节和可用于定义活动属性的完整列表，请参阅[创建管线](data-factory-create-pipelines.md)文章。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。 

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型**RelationalSource** （其中包括 Teradata） 以下属性部分提供了**typeProperties** :

属性 | 说明 | 允许的值 | 是否必需
-------- | ----------- | -------------- | --------
查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ 选择 * 从 MyTable。 | 否

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Teradata 的类型映射

如[数据移动活动](data-factory-data-movement-activities.md)文章中提到，复制活动执行从要水池下面 2 个步骤方法类型的源类型的自动类型转换的自动类型转换︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

将数据移动到 Teradata 将以下映射从 Teradata 类型中使用.NET 类型中。

Teradata 数据库类型 | .NET Framework 类型
----------------- | ---------------------------
字符 | String
Clob | String
图形 | String
VarChar | String
VarGraphic | String
Blob | Byte]
字节 | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
小数 | 小数
Double | Double
Integer | Int32
数字 | Double
SmallInt | Int16
日期 | DateTime
时间 | 时间跨度
与时区的时间 | String
Timestamp | DateTime
与时区的时间戳 | 方法
间隔一天 | 时间跨度
小时间隔一天 | 时间跨度
间隔一天分钟 | 时间跨度
间隔一天中第二个 | 时间跨度
间隔小时 | 时间跨度
分钟为间隔小时 | 时间跨度
到第二个时间间隔小时 | 时间跨度
间隔分钟 | 时间跨度
到第二个时间间隔分钟 | 时间跨度
间隔第二 | 时间跨度
间隔年 | String
间隔年到月 | String
间隔的月 | String
Period(Date) | String
Period(Time) | String
期间 （时间时区） | String
Period(Timestamp) | String
期间 （时间戳与时区） | String
Xml | String

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]
测试
