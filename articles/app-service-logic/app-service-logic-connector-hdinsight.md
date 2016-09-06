---
ms.openlocfilehash: 1618da199a1acb617d73d2dc7cdbcdbd398d4c8c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="在逻辑的应用程序中使用 HDInsight 接口 |Microsoft Azure 应用程序服务"
   description="如何创建和配置的 HDInsight 接口或 API 的应用程序并在 Azure 应用程序服务中的一个逻辑应用程序中使用它"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/23/2015"
   ms.author="sameerch"/>


# 开始使用 HDInsight 连接器并将其添加到您的逻辑应用程序
HDInsight 连接器可以在 Azure 上创建 Hadoop 群集并提交各种 Hadoop 作业，如配置单元、 小猪、 MapReduce 和流 MapReduce 作业。 Azure 的 HDInsight 服务部署和数据在云中，规定 Apache Hadoop 群集提供的软件框架设计管理、 分析和报告重要的数据。 Hadoop 核心提供了可靠的数据存储与 Hadoop 分布式文件系统 (HDFS) 和简单的 MapReduce 编程模型来处理和分析，同时，此分布式系统中存储的数据。 使用 HDInsight 连接器，您可以创建或删除群集、 提交作业并等待其完成。

作为流程的一部分，可以在对数据进行提取、 进程或推的逻辑应用程序使用连接器。 对业务工作流程和过程数据作为逻辑应用程序在此工作流的一部分，您可以添加 HDInsight 连接器。 

### 基本操作

- 创建群集
- 等待创建群集
- 猪的作业提交
- 提交配置单元作业
- MapReduce 作业提交
- 等待作业完成
- 删除群集


## 创建 HDInsight 连接器 API 应用程序 ##

连接器可以创建逻辑应用程序中，或直接从 Azure 市场创建。 若要从市场创建连接器︰ 

1. 在 Azure 的 startboard 中，选择**市场**。
2. 搜索"HDInsight 连接器"，选中它，然后选择**创建**。
3. 输入的名称、 应用程序服务计划，以及其他属性。
4. 在包设置，输入 HDInsight 群集用户名称和密码。 选择**确定**。
5. 选择**创建**︰  
![][1]  

## 证书配置 （可选） ##

> [AZURE.NOTE] 此步骤是必需的只有当您想要执行的管理操作 （创建和删除群集的） 在应用程序中的逻辑。

浏览到刚创建 HDInsight 连接器 API 的应用程序，您会看到组件显示 0-意味着有是安全不上载到管理证书︰  
![][2]

若要管理证书上传到您的 API 应用程序︰

1. 选择安全的组件。
2. 在安全刀片式服务器，选择**上载证书**。
3. 浏览并选择下一步的刀片式服务器中的证书文件。
4. 一旦选择了证书，请选择**确定**。

一旦成功地上载证书，将显示证书详细信息︰  
![][3]

> [AZURE.NOTE] 如果您想要更改的证书，则可以上载另一个证书;这将替换现有的证书。

## 逻辑应用程序中使用的连接器 ##

HDInsight 连接器只用作逻辑应用程序中的操作。 让我们以一个简单的逻辑应用程序创建的群集、 运行配置单元作业并删除末尾的作业完成群集。


1. 在开始逻辑卡，请单击手动运行此逻辑。
2. 选择 HDInsight 连接器 API 应用程序库中创建。 列出可用的操作︰  
![][5]

3. 选择创建群集，输入所有必需的群集参数并选择 ✓︰   
![][6]

4. 现在操作出现逻辑应用程序中进行配置。 操作的输出显示，并且可以将在任何后续操作中使用的输入︰  
![][7]

5. 库中选择相同的 HDInsight 连接器为一个操作。 选择等待群集创建操作，输入所有必需的参数，然后选择 ✓︰  
![][8]

6. 库中选择相同的 HDInsight 连接器为一个操作。 选择提交配置单元作业的操作，输入所有必需的参数，然后选择 ✓︰  
![][9]

7. 库中选择相同的 HDInsight 连接器为一个操作。 选择等待作业完成操作，输入所有必需的参数，然后选择 ✓︰  
![][10]

8. 库中选择相同的 HDInsight 连接器为一个操作。 选择删除群集操作，输入所有必需的参数，然后选择 ✓︰  
![][11]

9. 保存逻辑应用程序使用 save 命令在设计器的顶部。

若要测试方案，选择**立即运行**手动启动逻辑应用程序。

## 用做更多您的连接器
创建连接器时，可以将其添加到业务工作流使用的逻辑应用程序中。 请参阅[逻辑应用程序是什么？](app-service-logic-what-are-logic-apps.md)。

查看 Swagger REST API 参考[连接器](http://go.microsoft.com/fwlink/p/?LinkId=529766)和应用程序的 API 参考。

您还可以查看性能统计信息和控制安全接头。 请参阅[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)。


<!--Image references-->
[1]: ./media/app-service-logic-connector-hdinsight/Create.jpg
[2]: ./media/app-service-logic-connector-hdinsight/CertNotConfigured.jpg
[3]: ./media/app-service-logic-connector-hdinsight/CertConfigured.jpg
[5]: ./media/app-service-logic-connector-hdinsight/LogicApp1.jpg
[6]: ./media/app-service-logic-connector-hdinsight/LogicApp2.jpg
[7]: ./media/app-service-logic-connector-hdinsight/LogicApp3.jpg
[8]: ./media/app-service-logic-connector-hdinsight/LogicApp4.jpg
[9]: ./media/app-service-logic-connector-hdinsight/LogicApp5.jpg
[10]: ./media/app-service-logic-connector-hdinsight/LogicApp6.jpg
[11]: ./media/app-service-logic-connector-hdinsight/LogicApp7.jpg

测试
