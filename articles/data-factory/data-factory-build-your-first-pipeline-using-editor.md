---
ms.openlocfilehash: 03165b5f9afae995ecd3995430a321aa37508b25
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="构建第一个 Azure 数据工厂管线使用数据工厂编辑器"
    description="在本教程中，您将创建示例 Azure 数据工厂管线在 Azure 门户中使用数据工厂编辑器。"
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
    ms.date="07/27/2015"
    ms.author="spelluru"/>

# 构建第一个 Azure 数据工厂管线使用数据工厂编辑器 （Azure 门户）
> [AZURE.SELECTOR]
- [教程概述](data-factory-build-your-first-pipeline.md)
- [使用数据工厂编辑器](data-factory-build-your-first-pipeline-using-editor.md)
- [使用 PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [使用 Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)


在本文中，您将学习如何使用[Azure 预览门户](https://portal.azure.com/)创建您的第一个管道。 本教程包括以下步骤︰

1.  创建数据工厂
2.  创建链接的服务 （数据存储、 计算） 和数据集
3.  创建管道

这篇文章不提供 Azure 数据工厂服务的概念性概述。 该服务的详细概述，请参见[Azure 数据工厂简介](data-factory-introduction.md)文章。

## 步骤 1︰ 创建数据工厂

1.  在登录到[Azure 预览门户](http://portal.azure.com/)之后, 执行以下任务︰
    1.  单击左侧菜单上的**新建**。 
    2.  单击**创建**刀片式服务器中的**数据分析**。
    3.  单击**数据分析**刀片式服务器上的**数据工厂**。

        ![创建刀片式服务器](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  在**新数据工厂**刀片式服务器，输入名称**DataFactoryMyFirstPipeline** 。

    ![新数据工厂刀片式服务器](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] Azure 数据工厂名称是全局唯一的。 您将需要前缀数据工厂的名称与您的姓名，以便成功创建了该工厂。 
3.  如果您尚未创建任何资源组，您将需要创建的资源组。 若要此操作︰
    1.  单击**资源组名称**。
    2.  刀片式服务器**资源组**中选择**创建新的资源组**。
    3.  输入**ADF**中**创建资源组**刀片式服务器的**名称**。
    4.  单击**确定**。
    
        ![创建资源组](./media/data-factory-build-your-first-pipeline-using-editor/create-resource-group.png)
4.  所选资源组后，请验证您正在使用正确的订阅所需数据工厂创建的位置。
5.  在**新数据工厂**刀片式服务器，请单击**创建**。
6.  您将看到数据工厂创建在 Azure 预览门户网站**Startboard** ，如下所示︰   

    ![创建数据工厂状态](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. 祝贺您 ！ 您已成功创建您的第一个数据工厂。 数据工厂创建成功后，您将看到数据工厂页中，显示的内容的数据工厂。  

    ![数据工厂刀片式服务器](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

在随后的步骤中，您将学习如何创建链接的服务、 数据集和将在本教程中使用的管道。 

## 步骤 2︰ 创建链接的服务和数据集
在此步骤中，将链接到数据工厂的 Azure 存储帐户，并按需 Azure HDInsight 群集，然后创建一个数据集来表示从配置单元处理的输出数据。

### 创建链接的 Azure 存储服务
1.  单击**作者和部署**的**DataFactoryFirstPipeline****数据工厂**刀片上。 这将启动数据工厂编辑器。 
     
    ![编写和部署图块](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  单击**新的数据存储**，并选择**Azure 存储**
    
    ![Azure 存储链接服务](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

    您应该看到 JSON 脚本用于在编辑器中创建链接的 Azure 存储服务。 
4. **帐户名称**替换您 Azure 存储帐户和**帐户密钥**使用 Azure 存储帐户的访问键的名称。 若要了解如何获取您的存储访问密钥，请参阅[查看、 复制和重新生成存储访问键](../storage/storage-create-storage-account.md/#view-copy-and-regenerate-storage-access-keys)
5. 若要将链接的服务部署的命令栏上，单击**部署**。

    ![部署按钮](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

### 创建链接的 Azure HDInsight 服务
现在，您将创建用于运行该配置单元脚本按需 HDInsight 群集的链接的服务。 

1. 在**数据工厂编辑器**中，单击命令栏上的**新建计算**和选择**点播 HDInsight 群集**。

    ![新的计算](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. 复制并粘贴到草稿 1 窗口下面的小块。 JSON 段描述的属性，可用于创建 HDInsight 群集的点播。 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.1",
              "clusterSize": 1,
              "timeToLive": "00:05:00",
              "jobsContainer": "adfjobs",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }
    
    下表提供了使用代码段中的 JSON 属性的说明︰
    
    属性 | 说明
    -------- | -----------
    版本 | 该选项用于指定创建 HDInsight 的版本是 3.1。 
    ClusterSize | 这将创建一个节点 HDInsight 群集。 
    TimeToLive | 这是指定 HDInsight 群集的空闲时间之前将其删除。
    JobsContainer | 此参数指定将创建用于存储由 HDInsight 生成的日志作业容器的名称
    linkedServiceName | 此参数指定用于存储由 HDInsight 生成的日志的存储帐户
3. 若要将链接的服务部署的命令栏上，单击**部署**。 
4. 确认您看到 StorageLinkedService 和 HDInsightOnDemandLinkedService 在树中查看在左边。

    ![树视图中的链接服务](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)
 
### 创建输出数据集
现在，您将创建输出数据集来表示 Azure Blob 存储中存储的数据。 

1. 在**数据工厂编辑器**中，单击命令栏上的**新数据集**，并选择**Azure Blob 存储**。  

    ![新的数据集](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. 复制并粘贴到草稿 1 窗口下面的小块。 在 JSON 段中，您将创建数据集称为**AzureBlobOutput**，并指定将生成的脚本在配置单元的数据结构。 此外，您可以指定结果存储在称为**数据**blob 容器和名为**partitioneddata**的文件夹中。 **可用性**部分指定输出数据集，在每月的基础上产生。
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "data/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

3. 若要部署新创建的数据集的命令栏上，单击**部署**。
4. 验证已成功创建该数据集。

    ![树视图中的链接服务](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## 步骤 3︰ 创建您的第一个管道
在此步骤中，您将创建您的第一个管道。

1. 在**数据工厂编辑器**中，单击**省略号 （...）** ，然后单击**新管道**。
    
    ![新管线按钮](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. 复制并粘贴到草稿 1 窗口下面的小块。

    > [AZURE.IMPORTANT] 在 JSON 中存储帐户的名称替换**storageaccountname** 。

        {
          "name": "MyFirstPipeline",
          "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
              {
                "type": "HDInsightHive",
                "typeProperties": {
                  "scriptPath": "script/partitionweblogs.hql",
                  "scriptLinkedService": "StorageLinkedService",
                  "defines": {
                    "partitionedtable": "wasb://data@<storageaccountname>.blob.core.windows.net/partitioneddata"
                  }
                },
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "policy": {
                  "concurrency": 1,
                  "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
              }
            ],
            "start": "2014-01-01",
            "end": "2014-01-02"
          }
        }
 
    在 JSON 段中，您将创建管线的使用流程数据 HDInsight 群集上配置单元的单个活动组成。
    
    配置单元脚本文件， **partitionweblogs.hql**，存储在 Azure 存储帐户 （由 scriptLinkedService，称为**StorageLinkedService**），并调用**脚本**的容器中。

    **ExtendedProperties**部分用于指定将传递给配置单元脚本以配置单元配置值 （例如 ${hiveconf:PartitionedData}） 的运行时设置。

    管道的**start**和**end**属性指定管道的有效期间。

    在 JSON 的活动，您可以指定配置单元脚本运行指定的链接服务 – **HDInsightOnDemandLinkedService**计算上。
3. 要部署管线的命令栏上，单击**部署**。
4. 确认您看到在树视图中的管道。

    ![与管线的树视图](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. 祝贺您，您已成功创建您的第一个管道 ！
6. 单击**X**关闭数据工厂编辑器刀片和导航回数据工厂刀片式服务器，并在**关系图**上单击。
  
    ![平铺关系图](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. 在关系图视图中，您将看到概述的管线，以及在本教程中使用的数据集。
    
    ![关系图视图](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. 在关系图视图中，双击**AzureBlobOutput**的数据集。 您将看到当前正在处理的扇区。

    ![数据集](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. 当处理完成后时，您将看到**就绪**状态中的扇区。 请注意，创建的按需 HDInsight 群集通常需要一段时间。 

    ![数据集](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
10. 当切片处于**就绪**状态时，检查您的输出数据的 blob 存储中的**数据**容器中的**partitioneddata**文件夹。  
 

 

## 下一步行动
在本文中，您创建管线与按需 HDInsight 群集运行的配置单元脚本转换活动 （HDInsight 活动）。 若要查看如何使用将数据复制到 Azure SQL Azure Blob 复制活动，请参阅[教程︰ Azure 的数据复制到 SQL Azure 的 blob](./data-factory-get-started.md)。
  

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-build-your-first-pipeline-using-editor.md)。 测试
