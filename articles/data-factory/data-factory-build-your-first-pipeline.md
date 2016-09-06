---
ms.openlocfilehash: 6367573b276f349d62e6502f67e5bf84652e4f89
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="构建第一个管道使用 Azure 数据工厂"
    description="本教程展示如何创建示例数据管道将使用 Azure HDInsight 的数据转换。"
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
    ms.topic="get-started-article" 
    ms.date="07/27/2015"
    ms.author="spelluru"/>

# 构建第一个管道使用 Azure 数据工厂
> [AZURE.SELECTOR]
- [教程概述](data-factory-build-your-first-pipeline.md)
- [使用数据工厂编辑器](data-factory-build-your-first-pipeline-using-editor.md)
- [使用 PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [使用 Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)

这篇文章可以帮助您开始构建您的第一个管道，并将其部署到 Azure 数据工厂。 

> [AZURE.NOTE] 这篇文章不提供 Azure 数据工厂服务的概念性概述。 该服务的详细概述，请参见[Azure 数据工厂简介](data-factory-introduction.md)文章。

## 教程概述
本教程将指导您完成步骤需要起来，第一管线和运行。 您将创建管线，并指定从零开始所需的所有资源。

如果您想要快速，探索数据工厂的各种功能，而无需创建一个从零开始，您可以使用 Azure 预览门户中，我们提供的示例。 请参阅[Azure 数据工厂更新︰ 简化示例部署](http://azure.microsoft.com/blog/2015/04/24/azure-data-factory-update-simplified-sample-deployment/)有关如何部署使用 Azure 预览门户的用例根据的样本。

## 必备组件
在开始本教程之前，您必须具有以下先决条件︰

1.  **Azure 订阅**-如果没有 Azure 的订阅，可以在几分钟创建一个免费试用帐户。 有关如何获得免费的试用帐户，请参阅[免费试用版](http://azure.microsoft.com/pricing/free-trial/)的文章。

2.  **Azure 存储**--将 Azure 存储帐户用于将数据存储在本教程中。 如果没有 Azure 存储帐户，请参阅文章[创建存储帐户](../storage-create-storage-account/#create-a-storage-account)。 创建存储帐户后，您需要获取帐户密码用来访问存储。 请参阅[查看、 复制和重新生成存储访问键](../storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys)。

## 在本教程中包含的内容？ 
Azure 数据工厂可以撰写为数据驱动的工作流的数据移动和数据处理任务。 您将学习如何创建使用 HDInsight 来转换和分析每月的 web 日志您第一个渠道。  

在本教程中，您将执行以下步骤︰

1.  创建的数据工厂。
2.  创建以下链接的服务︰
    1.  **Azure 存储帐户**– Azure 存储帐户将用于存储所使用的按需 HDInsight 群集文件。
    2.  **按需 HDInsight 群集**– HDInsight 群集将启动按需转换和分析数据。
3.  创建输出数据集 
4.  创建管线运行配置单元的脚本，并将结果存储在输出数据集。 该配置单元脚本首先创建一个引用存储在 Azure blob 存储原始 web 日志数据的外部表。 在配置单元脚本中的下一步然后按年和月分区的原始数据。

第一个管道，称为**MyFirstPipeline**，使用配置单元活动转换和分析作为群集的一部分 HDInsight、 部署和****存储/HdiSamples/WebsiteLogSampleData/SampleLog/在 web 日志。 

![关系图视图](./media/data-factory-build-your-first-pipeline/diagram-view.png)

该配置单元脚本执行后，结果将存储在 Azure blob 存储容器︰**数据/partitioneddata**。

在**AzureBlobOutput**数据集上定义的可用性确定该配置单元活动的运行频率。 在本教程中，这是设置为每月一次。

## 本教程为准备 Azure 存储
开始之前本教程，您需要准备 Azure 存储本教程所需的文件。

1. 启动记事本、 粘贴以下文本，并将其保存为**partitionweblogs.hql** C:\adfgettingstarted 文件夹中，在您的硬盘上。 此配置单元脚本创建两个外部表︰ **WebLogsRaw**和**WebLogsPartitioned**。

    > [AZURE.IMPORTANT] **Storageaccountname**中的最后一行替换为您的存储帐户的名称。 

        set hive.exec.dynamic.partition.mode=nonstrict;

        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE EXTERNAL TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        LOCATION '/HdiSamples/WebsiteLogSampleData/SampleLog/'
        tblproperties ("skip.header.line.count"="2");
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';

        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

     
 
2. 要准备教程 Azure 存储︰
    1. 下载[最新版本**AzCopy**](http://aka.ms/downloadazcopy)或[最新的预览版本](http://aka.ms/downloadazcopypr)。 请参阅有关使用该实用程序说明[如何使用 AzCopy](../storage/storage-use-azcopy.md)文章。
    2. 安装 AzCopy 后，可以直接将其添加到系统路径，通过在命令提示符下运行以下命令。 
    
            set path=%path%;C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    

    3. 导航到 c:\adfgettingstarted 文件夹中，然后运行以下命令上载该配置单元。HQL 文件复制到的存储帐户。 更换**< StorageAccountName\>**您的存储帐户的名称和**< 存储键\>**使用的存储帐户。

            AzCopy /Source:. /Dest:https://<StorageAccountName>.blob.core.windows.net/script /DestKey:<Storage Key>
    4. 文件已成功上载后，您将看到以下输出 AzCopy。
    
            Finished 1 of total 1 file(s).
            [2015/06/15 15:47:13] Transfer summary:
            -----------------
            Total files transferred: 1
            Transfer successfully:   1
            Transfer skipped:        0
            Transfer failed:         0
            Elapsed time:            00.00:00:01

请执行以下操作︰

- 单击[使用数据工厂编辑器](data-factory-build-your-first-pipeline-using-editor.md)顶部使用数据工厂编辑器，它是 Azure 门户的一部分来执行的教程的链接。
- 单击[使用 PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)顶部使用 Azure PowerShell 执行本教程的链接。
- 单击[使用 Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)在顶部使用 Visual Studio 执行本教程的链接。 

## 发送反馈
我们真诚欢迎您的反馈意见，在这篇文章。 请花几分钟的时间来提交您的反馈意见通过[电子邮件](mailto:adfdocfeedback@microsoft.com?subject=data-factory-build-your-first-pipeline.md)。 测试
