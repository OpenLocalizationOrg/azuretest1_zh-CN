---
ms.openlocfilehash: 9287ed2f3d3dbc2bf363e3f37a6abc5faf3ba602
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="调用从 Azure 数据工厂的 MapReduce 程序" 
    description="了解如何通过从 Azure 数据工厂 Azure HDInsight 群集上运行 MapReduce 程序来处理数据。" 
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
    ms.date="07/31/2015" 
    ms.author="spelluru"/>

# 调用数据工厂的 MapReduce 节目
本文介绍如何通过使用**HDInsight MapReduce 活动**调用从 Azure 数据工厂管道的**MapReduce**程序。 

## 简介 
在 Azure 数据工厂管道处理链接的存储服务中的数据通过使用链接的计算服务。 它包含一系列活动，其中每个活动执行特定处理操作。 本文介绍如何使用 HDInsight 活动的 MapReduce 变换。
 
请参阅 Azure 数据工厂管道从 HDInsight 群集上使用 HDInsight 活动的小猪/配置单元转换的[猪](data-factory-pig-activity)和[配置单元](data-factory-hive-activity.md)文章有关正在运行的配置单元猪/脚本的详细信息。 

## HDInsight 活动使用 MapReduce 转换为 JSON 

在 HDInsight 活动的 JSON 定义︰ 
 
1. 将**活动****类型**设置为**HDInsight**。
3. 指定**类名**属性的类的名称。
4. 指定的路径包括文件名为**jarFilePath**属性的 JAR 文件。
5. 指定链接指的是所包含的 JAR 文件的**jarLinkedService**属性的 Azure Blob 存储的服务。   
6. 在**参数**部分中指定的 MapReduce 程序的任何参数。 

 

        {
          "name": "MahoutMapReduceSamplePipeline",
          "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
            "activities": [
              {
                "name": "MyMahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "inputs": [
                  {
                    "Name": "MahoutInput"
                  }
                ],
                "outputs": [
                  {
                    "Name": "MahoutOutput"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightMapReduce",
                "typeProperties": {
                  "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                  "jarFilePath": "<container>/Mahout/Jars/mahout-core-0.9.0.2.1.3.2-0002-job.jar",
                  "jarLinkedService": "StorageLinkedService",
                  "arguments": [
                    "-s",
                    "SIMILARITY_LOGLIKELIHOOD",
                    "--input",
                    "$$Text.Format('wasb://<container>@<accountname>.blob.core.windows.net/Mahout/Input/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                    "--output",
                    "$$Text.Format('wasb://<container>@<accountname>.blob.core.windows.net/Mahout/Output/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                    "--maxSimilaritiesPerItem",
                    "500",
                    "--tempDir",
                    "wasb://<container>@<accountname>.blob.core.windows.net/Mahout/temp/mahout"
                  ]
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

您可以使用 MapReduce 转换运行 HDInsight 群集上的任何 MapReduce jar 文件。 下面的示例 JSON 的定义中的管道，HDInsight 活动配置为运行 Mahout JAR 文件。

## 示例
您可以下载的 HDInsight 活动中使用 MapReduce 转换示例︰[数据工厂在 GitHub 上的样本](data-factory-samples.md)。  


[开发人员参考]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet 引用]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-get-started.md
[adfgetstartedmonitoring]:data-factory-get-started.md#MonitorDataSetsAndPipeline 
[adftutorial]: data-factory-tutorial.md

[开发人员参考]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure 门户]: http://portal.azure.com
 
测试
