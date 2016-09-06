---
ms.openlocfilehash: 03516c04f63a808a7c02ea3dd4ad4ddc374e4452
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="猪的活动" 
    description="了解如何使用猪的活动在 Azure 数据工厂上请求/您自己的 HDInsight 群集上运行 Pig 脚本。" 
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
    ms.date="07/26/2015" 
    ms.author="spelluru"/>

# 猪的活动

数据工厂[管线](data-factory-create-pipelines.md)中的 HDInsight 猪的活动[您自己](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或[按需](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)HDInsight 群集上执行猪的查询。. 本文基于[数据变换活动](data-factory-data-transformation-activities.md)文章显示概要介绍了数据转换和受支持的转换活动。

## 语法

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## 语法的详细信息

属性 | 说明 | 是否必需
-------- | ----------- | --------
name | 活动名称 | 是
description | 描述该活动的用途的文本 | 否
类型 | HDinsightPig | 是
输入 | 输入由猪的活动 | 否
输出 | 猪的活动所产生的输出 | 是
linkedServiceName | 对 HDInsight 群集注册为链接服务数据工厂中引用 | 是
脚本 | 指定 Pig 脚本内联 | 否
脚本路径 | 在 Azure blob 存储中存储 Pig 脚本并提供文件的路径。 使用脚本或 scriptPath 属性。 两者不能在一起 | 否
定义 | 猪的脚本中引用的键/值对的形式指定参数 | 否

## 示例

让我们考虑一种游戏日志分析中需要鉴别用户玩游戏启动公司所用的时间。
 
下面是一个示例游戏日志，以逗号 （，） 分隔且包含下列字段 – 档案 Id、 SessionStart、 持续时间、 SrcIPAddress 和 GameType。

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**猪的脚本**来处理该数据如下所示︰

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

若要在数据工厂管道中执行此猪的脚本，您需要采取的措施︰

1. 创建链接的服务注册[您自己的 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)或配置[拨号 HDInsight 计算群集](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)。 所谓"HDInsightLinkedService"此链接的服务。
2.  创建[链接的服务](data-factory-azure-storage-connector.md)来配置到 Azure Blob 存储存放数据的连接。 所谓"StorageLinkedService"此链接的服务。
3.  创建指向输入和输出数据的[数据集](data-factory-create-datasets.md)。 所谓"PigSampleIn"的输入数据集和输出数据集"PigSampleOut"。
4.  在第 2 步中配置复制猪的查询在 Azure Blob 存储文件。 如果承载数据的链接的服务不同于承载该查询文件，创建一个单独的链接的 Azure 存储服务和活动配置中引用它。 **ScriptPath**用于指定 pig 脚本文件和**scriptLinkedService**指定 Azure 存储包含脚本文件的路径。 
    
    > [AZURE.NOTE] 您还可以通过以下方式提供 Pig 脚本内联在活动定义中的使用的**脚本**属性，但不是推荐在 JSON 文档中脚本的所有特殊字符需要进行转义，并可能会导致调试的问题。 最佳做法是按照步骤 4。
5. 创建包含要处理的数据的 HDInsightPig 活动管线下方。

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. 部署该管道。 有关详细信息请参阅[创建管线](data-factory-create-pipelines.md)文章。 
7. 监视使用数据工厂监视和管理视图的管道。 请参阅[监视并管理数据工厂管线](data-factory-monitor-manage-pipelines.md)了解详细信息。

## 指定的猪的脚本使用参数定义的元素，

请考虑位置到 Azure Blob 存储每日 ingested 游戏的日志并存储在文件夹中使用日期和时间划分的示例。 您想要进行参数化处理的猪的脚本在运行时动态地将输入的文件夹位置和还产出分区与日期和时间。
 
使用参数化猪的脚本，请执行以下操作︰

- 在**定义**中定义的参数。

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- 在猪的脚本中，请在下面的示例所示，使用**$parameterName**的参数︰

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


 
## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-pig-activity.md)。


测试
