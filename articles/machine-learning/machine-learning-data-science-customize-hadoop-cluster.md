---
ms.openlocfilehash: dbff0a3f3f30e7a5b98d148cbc0d28ebed2672f2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="自定义高级分析流程和技术的 Hadoop 群集 |Microsoft Azure" 
    description="可自定义 Azure HDInsight Hadoop 群集中流行的 Python 模块。"
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="hangzh-msft" 
    manager="paulettm" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="hangzh;bradsev" />

# 自 Azure HDInsight Hadoop 群集定义的高级分析流程和技术

本文介绍如何通过安装 64 位 Anaconda (Python 2.7) 来自定义一个 HDInsight Hadoop 群集每个节点上配置群集时在 HDInsight 服务时。 该自定义项准备用于高级分析过程和技术 （调整） 在 Azure 机器学习中使用群集。 它还演示如何访问 headnode，若要自定义将作业提交到该群集。

此自定义使得 Anaconda 方便可用于旨在处理群集中的配置单元记录的用户定义函数 (Udf) 中包括的许多流行 Python 模块。 在此方案中使用的过程的说明，请参阅[提交到高级分析功能过程中的 HDInsight Hadoop 群集配置单元的查询](machine-learning-data-science-hive-queries.md)。


## <a name="customize"></a>自定义 Azure HDInsight Hadoop 群集

若要创建自定义的 HDInsight Hadoop 群集时，用户需要登录到[**管理门户的 Azure**](https://manage.windowsazure.com/)，在左下角，单击**新建**，然后选择数据服务-> HDINSIGHT->**创建自定义**以显示**群集的详细信息**窗口。 

![创建工作区][1]

输入将在 1、 中的配置页上创建群集的名称，并接受其他字段的默认值。 单击箭头可转到下一步的配置页。 

![创建工作区][2]

配置第 2 页，输入**数据节点**的数目、 选择**区域/虚拟网络**，以及选择的**头节点**和**数据节点**的大小。 单击箭头以转到下一步的配置页。

>[AZURE.NOTE] **地区/虚拟网络**必须是打算用于 HDInsight Hadoop 群集的存储帐户的区域相同。 否则，在第四个配置页面，用户想要使用的存储帐户不会在**帐户名称**的下拉列表。

![创建工作区][3]

在配置页面 3，HDInsight Hadoop 群集提供用户名和密码。 **不要**选择_输入配置单元/Oozie Metastore_。 然后，单击箭头以转到下一步的配置页。 

![创建工作区][4]

在配置第 4 页，指定存储帐户名称，HDInsight Hadoop 群集的默认容器。 如果用户选择下拉列表**默认容器**中_创建默认容器_，则将创建与群集名称相同的容器。 单击箭头可转到最后一个配置页。

![创建工作区][5]

在最终**脚本操作**配置页上，单击**添加脚本操作**按钮，并使用下面的值填充的文本字段。
 
* **名称**-此脚本操作的名称的任何字符串。 
* **节点类型**-选择**所有节点**。 
* **脚本的 URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts*是一种公共容器中存储帐户 
    * 我们使用共享 PowerShell 脚本文件，以便于用户工作在 Azure 中的*getgoing* 。 
* **参数**-（请保留空白）

最后，单击复选标记以开始创建自定义的 HDInsight Hadoop 群集上。 

![创建工作区][6]

## <a name="headnode"></a> 访问 Hadoop 群集的头节点

用户必须启用远程访问在 Azure Hadoop 群集才能通过 RDP 访问 Hadoop 群集的头节点。 

1. 登录到[**管理门户的 Azure**](https://manage.windowsazure.com/)中、 在左侧选择**HDInsight** 、 选择 Hadoop 群集中的群集列表，单击**配置**选项卡，然后单击页面底部的**远程启用**图标。
    
    ![创建工作区][7]

2. 在**配置远程桌面**窗口中，输入用户名和密码字段，并选择用于远程访问的到期日期。 然后单击复选标记以启用对 Hadoop 群集的头节点的远程访问权限。
    
    >[AZURE.NOTE] 
    >
    >1. 用户名称和密码用于远程访问的用户名称和密码在您创建了 Hadoop 群集时使用不是。 这些都是单独的一组凭据
    >
    >2. 远程访问权限的到期日期必须为当前日期的 7 天内。

    ![创建工作区][8]

3. 启用远程访问权限后，单击**连接**到远程页面底部成头节点。 输入先前指定的远程访问用户的凭据登录到 Hadoop 群集的头节点。

     ![创建工作区][9]

高级分析功能过程中的下一个步骤映射中[学习指南︰ 高级 Azure 中的数据处理](machine-learning-data-science-advanced-data-processing.md)，可能包括将数据移动到 HDInsight，过程和示例不准备使用 Azure 机器学习数据中学习过程中的步骤。

有关如何访问 Anaconda 中包含用户定义函数 (Udf) 是用来处理存储在群集中配置单元记录在该群集的头节点从 Python 模块的说明，请参阅[提交到高级分析功能过程中的 HDInsight Hadoop 群集配置单元的查询](machine-learning-data-science-process-hive-tables.md)。

[1]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png
[2]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img2.png
[3]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png
[4]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png
[5]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png
[6]: ./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png
[7]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png
[8]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png
[9]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png

 
测试
