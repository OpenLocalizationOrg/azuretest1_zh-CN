---
ms.openlocfilehash: b449047b5a568e9ffa6641f91d39173c3c52003c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将数据从 Sybase 移动 |Azure 数据工厂" 
    description="了解如何将数据从使用 Azure 数据工厂的 Sybase 数据库移动。" 
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

# 将数据从 Sybase 使用 Azure 数据工厂 

这篇文章概括介绍了如何使用复制活动在 Azure 数据工厂将数据从 Sybase 移动到另一个数据存储区。 本文基于[数据移动活动](data-factory-data-movement-activities.md)文章复制活动和支持的数据存储组合呈现移动数据的一般概述。

数据工厂服务支持连接到内部 Sybase 源使用数据管理网关。 请查看[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章以了解有关数据管理网关和网关设置的分步指导。 

**注意︰**您需要利用网关连接到 Sybase，即使承载于 Azure IaaS Vm。 如果要连接到的 Sybase 承载在云实例，也可以在 IaaS VM 安装网关实例。

数据工厂目前支持仅移动数据从 Sybase 到其他数据存储区而不是 Sybase 到其他数据存储区。

## 安装

对于数据管理网关连接到 Sybase 数据库，您需要为数据管理网关在同一系统上安装[的 Sybase 的数据提供程序](http://go.microsoft.com/fwlink/?linkid=324846)。

## 示例︰ 将数据从 Sybase 复制到 Azure Blob

下面的示例演示︰

1.  链接的类型[OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties)的服务。
2.  喜欢的类型[AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)的服务。
3.  [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties)类型的输入的[数据集](data-factory-create-datasets.md)。
4.  类型[AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)的输出[数据集](data-factory-create-datasets.md)。
4.  [管线](data-factory-create-pipelines.md)与使用[RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties)和[BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)的复制活动。

该示例将数据从 Sybase 数据库中查询结果对 blob 每隔一小时。 在这些示例中使用的 JSON 属性详见以下示例部分。 

作为第一步，请设置数据管理网关根据[内部位置和云之间移动数据](data-factory-move-data-between-onprem-and-cloud.md)文章中的说明进行操作。

**Sybase 链接的服务︰**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase 输入数据集︰**

该示例假定您创建了一个表"MyTable"在 Sybase 和它包含名为"时间戳"时间系列数据的列。

设置为"外部": 真并指定 externalData 策略告诉数据工厂这是表的外部数据工厂并不是活动数据工厂中生产。 请注意，链接的服务**类型**设置为︰ **RelationalTable**。 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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


**Azure Blob 的输出数据集︰**

数据写入到新的斑点每小时 (频率︰ 小时、 间隔︰ 1)。 该 blob 的文件夹路径动态计算基于切片所处理的开始时间。 使用文件夹路径的年、 月、 日和小时部分的开始时间。

    {
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**管线与复制活动︰**

管道中包含一个配置为使用上面的输入和输出数据集的复制活动，并计划每小时运行一次。 在管线 JSON 定义中，将**源**类型设置为**RelationalSource** ，**接收器**类型设置为**BlobSink**。 指定的**查询**属性从数据库管理员选择数据的 SQL 查询。在数据库中订单表中。


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## Sybase 链接服务属性

下表提供了 JSON 元素特定于 Sybase 链接服务的说明。

属性 | 说明 | 是否必需
-------- | ----------- | --------
类型 | 类型属性必须设置为︰ **OnPremisesSybase** | 是
服务器 | Sybase 服务器的名称。 | 是
数据库 | Sybase 数据库的名称。 | 是 
架构  | 在数据库中的架构的名称。 | 否
authenticationType | 用于连接到 Sybase 数据库的身份验证类型。 可能的值包括︰ 匿名的基本的和窗口。 | 是
用户名 | 如果您使用基本或 Windows 身份验证，请指定用户名。 | 否
password | 指定对您指定的用户名的用户帐户的密码。 |  否
gatewayName | 数据工厂服务连接到内部 Sybase 数据库应使用的网关的名称。 | 是 

有关设置数据源的内部 Sybase 的凭据的详细信息，请参阅[设置凭据和安全](data-factory-move-data-between-onprem-and-cloud.md#setting-credentials-and-security)。

## Sybase 数据集类型属性

部分和属性可用于定义数据集的完整列表，请参阅文章[创建数据集](data-factory-create-datasets.md)。 如结构、 可用性和策略的 JSON 数据集分区的所有数据集的类型相似 (第 SQL Azure Azure blob，Azure 表，等等。..)。

TypeProperties 节对于每种类型的数据集是不同的并提供有关的数据存储区中的数据位置的信息。 **TypeProperties**部分类型**RelationalTable** （其中包括 Sybase 数据集） 的数据集具有以下属性。

属性 | 说明 | 是否必需
-------- | ----------- | --------
表名 | 链接的服务指的 Sybase 数据库实例中的表的名称。 | 是

## Sybase 复制活动类型属性 

节和可用于定义活动属性的完整列表，请参阅文章[创建管线](data-factory-create-pipelines.md)。 属性，如名称、 说明、 输入和输出表，各种策略等都可用于所有类型的活动。 

另一方面该活动的 typeProperties 部分中可用的属性随每种活动类型和复制活动对于他们因种源和接收器。

如果复制活动当源类型**RelationalSource** （其中包括 Sybase） 以下属性部分提供了**typeProperties** :

属性 | 说明 | 允许的值 | 是否必需
-------- | ----------- | -------------- | --------
查询 | 使用自定义查询中读取数据。 | SQL 查询字符串。 例如︰ 选择 * 从 MyTable。 | 否

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Sybase 的类型映射

所述数据移动活动文章复制活动执行从要水池下面 2 个步骤方法类型的源类型的自动类型转换的自动类型转换︰

1. 从本机源类型转换为.NET 类型
2. 从.NET 类型转换为本机的接收器类型

Sybase 支持 T SQL 和 T SQL 类型。 为.NET 类型从 sql 类型映射表，请参阅[Azure SQL 连接器](data-factory-azure-sql-connector.md)项目。

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]


测试
