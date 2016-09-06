---
ms.openlocfilehash: f692259289d66313fc4a5234bcf252f472b9b65d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="Azure 机器学习常见问题 |Microsoft Azure" 
    description="Azure 机器学习简介︰ 常见问题解答覆盖计费、 功能和云服务的优化预测模型的局限性。" 
    keywords="机器学习简介、 预测模型、 什么机器学习"
    services="machine-learning" 
    documentationCenter="" 
    authors="pablissima" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2015" 
    ms.author="paulettm"/>

#Azure 的机器学习的常见问题 (FAQ): 计费、 功能，限制和支持

本常见问题解答回答 Azure 机器学习，预测建模和留给通过 web 服务解决方案的云服务有关的问题。 此常见问题解答解释有关使用该服务，包括计费模型、 功能、 限制和支持问题。 
 
##一般性问题

**什么是 Azure 机器学习？**
 
Azure 的机器学习是一个完全托管的服务可用于创建、 测试、 操作和管理云中的预测分析的解决方案。 只有浏览器，您可以登录、 上载数据，并立即开始机器学习实验。 拖放预测模型、 模块、 大托盘和库既简单又快速启动模板使通用机器学习任务。  有关详细信息，请参阅[Azure 机器学习服务概述](/services/machine-learning/)。 有关范围涵盖关键术语和概念的机器学习介绍，请参见[介绍 Azure 机器学习](machine-learning-what-is-machine-learning.md)。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 
**机器学习 Studio 是什么？**

机器学习 Studio 是您通过 web 浏览器访问一个工作台环境。 机器学习 Studio 承载托盘的具有可视化撰写界面，使您可以构建端到端，数据科学实验的窗体中的工作流的模块。 

有关机器学习工作室的详细信息，请参阅[什么是机器学习 Studio](machine-learning-what-is-ml-studio.md)

**机器学习 API 服务是什么？**

机器学习 API 服务使您能够部署可扩展、 可容错的 web 服务作为机器学习 Studio 中生成的预测模型。 机器学习 API 服务创建的 web 服务是 REST Api，提供一个接口，外部应用程序和您的预测分析模型之间的通信。 

有关详细信息，请参阅[连接到机器学习的 web 服务](machine-learning-connect-to-azure-machine-learning-web-service.md)。


##帐单问题

**机器学习付费是如何工作的？**

计费和定价信息，请参阅[计算机学习定价](http://azure.microsoft.com/pricing/details/machine-learning/)。

**机器学习是否有免费的试用版？**

 当您注册的免费试用版，您可以尝试在一个月的任何 Azure 服务 Azure。 若要了解有关 Azure 的免费试用版的详细信息，请访问[Azure 免费试用常见问题解答](/pricing/free-trial-faq/)。

## 机器学习 Studio 问题

###创建一个实验   
**有版本控制或实验曲线图的 Git 集成吗？**

没有，但是实验每次运行该版本的 graph 保留，不能由其他用户修改。

###导入和导出数据的机器学习
**机器学习支持哪种数据源？**

可以将数据装载到计算机学习 Studio 中两种方式之一︰ 通过将本地文件上载作为数据集或使用读卡器模块来导入数据。 可以将本地文件上载通过机器学习 Studio 中添加新的数据集。 请参阅[导入到计算机学习 Studio 培训数据](machine-learning-import-data.md)以了解受支持的文件格式。 


####<a id="ModuleLimit"></a>大数据集可我模块？

在 Studio 中机器学习模块支持常见的使用案例达 10GB 的密集的数字数据的数据集。 如果一个模块采用多个输入，10 GB 是所有输入的大小的总和。 此外可以取样较大数据集，通过配置单元或 SQL Azure 数据库查询，或通过统计学习预先处理前摄取。  

以下类型的数据功能正常化过程可以扩展到更大的数据集，并仅限于小于 10 GB:

- 稀疏
- 分类
- 字符串
- 二进制数据

以下模块仅限于小于 10 GB 的数据集︰

- 导购模块
- SMOTE 的模块
- 脚本编写模块︰ R，Python，SQL
- 模块输出数据的大小可能会大于输入数据的大小，例如联接或哈希处理功能。
- 交叉验证扫描参数、 序数回归和一个 vs 全部 Multiclass，当迭代的数字是非常大。

用于大于几 GB 的数据集，应将数据上载到 Azure 存储或 SQL Azure 数据库或使用 HDInsight，而不是直接从本地文件上传。


####<a id="UploadLimit"></a>数据上载的限制有哪些？
大于几 GB 的数据集，用于将数据上载到 Azure 存储或 SQL Azure 数据库或使用 HDInsight，而不是直接从本地文件上载。

**可以从 Amazon S3 中读取数据？**

如果您具有少量的数据，想要将其公开通过 http URL，然后可以使用该[读取器][读取器]模块。 对于任何大量的要先转移到 Azure 存储，然后使用[读取器][读取器]模块将它引入实验数据。 
<!--
<SEE CLOUD DS PROCESS>
--> 

**有一个内置的图像输入的功能吗？** 

您可以了解有关[图像读取][图像读取器]引用中的图像输入功能。

###模块 

**算法、 数据源、 数据格式或要查找的数据的转换操作不在 Azure ML Studio 中，我有哪些选择？**

您可以访问[用户的反馈论坛](http://go.microsoft.com/fwlink/?LinkId=404231)以查看我们正在跟踪的功能请求。 如果您正在寻找一个功能已请求为此请求添加您的投票。 如果您正在寻找的能力不存在，则创建新的请求。 太可在此论坛中查看您的请求的状态。 我们将密切跟踪此列表并经常更新的功能的可用性状态。 此外对 R 和 Python 的内置支持，自定义转换可以创建根据需要。 


**可以将现有代码导入 ML Studio 中？** 

是，可以使 ML Studio 中的现有代码，R 和在同一试验提供 Azure 机器学习的学员中运行它，发布应用程序作为 web 服务通过 Azure 机器学习。 请参阅[扩展实验与 R ](machine-learning-extend-your-experiment-with-r.md)。 

**是否可能使用类似[PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language)模型定义？**

否，不支持，但是自定义 R 和 Python 代码可用于定义模块。


###数据处理 
**没有可视化实验中以交互方式 （超出 R 可视化项） 数据的能力？**

通过单击模块的输出可以直观地显示数据并获取统计信息。 

**预览结果或数据在浏览器中，行和列的数目时，有限，为什么？**

数据传输到浏览器，并且可能非常大，因为数据大小受到限制以防止减慢 ML studio。 最好是要下载的数据结果并使用 Excel 或其他工具来直观地显示整个数据。

###算法
**哪些现有算法的机器学习 Studio 中受支持？**

机器学习 Studio 提供了先进的算法，如可扩展提升的决策树、 贝叶斯建议系统、 深层的神经网络和决策在微软研究院开发的 Jungles。 此外，还提供可扩展的开放源计算机教学套件，像 Vowpal Wabbit。 机器学习 Studio multiclass 和二元分类、 回归，以及群集支持机器学习算法。 [机器学习模块][机器学习模块]的完整列表，请参阅。

**不要自动提示右机器学习算法用于我的数据？** 

不对，但也有许多机器学习 Studio 中的方法来确定适合您的问题每个算法的结果进行比较。 

**您是否有任何其他提供算法选取一个算法的指南？** 了解[如何选择算法](machine-learning-algorithm-choice.md)。 

**用 R 或 Python 编写提供的算法？** 

否，这些算法主要语言编写的已编译可提供更高的性能。 

**所提供的算法的所有详细信息？** 

文档也提供了一些信息的算法，并介绍了为调整提供参数以优化供您使用的算法。  

**是否有任何支持的在线学习？** 

否，支持目前只能以编程方式重新进行培训。 

**可直观显示使用内置模块神经网络模型的层** 

No. 

**我用 C# 或某些其他语言创建自己的模块** 

目前新的自定义模块只能创建在。 

###R 模块 
**机器学习 Studio 中可获得什么 R 包？** 

机器学习 Studio 现在，支持 400 + R 包，此列表在不断增大。 请参阅[扩展与 R 实验](machine-learning-extend-your-experiment-with-r.md)，以了解如何获取支持的 R 包的列表。 如果所需的包，不在此列表中提供了[用户反馈论坛](http://go.microsoft.com/fwlink/?LinkId=404231)在包的名称。 

**是否可以生成自定义 R 模块？** 

是的看到[作者在 Azure 机器学习中的自定义 R 模块](machine-learning-custom-r-modules.md)的详细信息。

**R 对于是否有复制环境？** 

不对，是 studio 中的没有复制环境。 

###Python 模块 

**是否可以生成自定义的 Python 模块？** 

没有，但与标准的 Python 模块或一套可以得到相同的结果。 

**有一个复制环境的 Python 吗？** 

在机器学习 Studio 中，您可以使用 Jupyter 笔记本电脑。 有关详细信息，请参阅 [Azure ML Studio 中引入 Jupyter 笔记本] (http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)

## Web 服务
###Retrainining 模型以编程方式

**如何重新培训 AzureML 模型以编程方式？**
使用再培训的 Api。 示例代码位于[此处](https://azuremlretrain.codeplex.com/)。

###创建

**在本地或在未连接到 internet 的应用程序是否可以部署该模型？**
No.


**有预期的所有 web 服务的基准等待时间吗？** 

请参阅[Azure 订阅限制](../azure-subscription-service-limits.md)

###使用
**何时要作为批处理执行服务请求的响应服务和运行我预测模型？**

请求响应服务 (RR) 是用于提供创建并发布在实验环境中的无状态模型接口的低延迟、 高比例的 web 服务。 批处理执行服务 (BES) 是针对异步评分一批数据记录的服务。 BE 的输入是类似于数据输入的 RR 中使用。 主要的区别是 BE 从各种各样的来源，如 Blob 服务以及在 Azure，Azure SQL 数据库，HDInsight （配置单元查询），表服务和 HTTP 源中读取的记录块。 有关详细信息，请参阅[如何使用机器学习的 web 服务](machine-learning-consume-web-services.md)。 

**如何更新已部署的 web 服务的模型？** 

更新已部署的服务的预测模型是简单，修改并重新运行用于创作和保存培训的模型试验。 一旦有新版本可用的培训模型，ML Studio 将询问您是否要更新临时 web 服务。 将更新应用于临时的 web 服务后，同一更新将成为可供您应用于生产 web 服务。 有关如何更新已部署的 web 服务的详细信息，请参阅[发布机器学习的 web 服务](machine-learning-publish-a-machine-learning-web-service.md)。

您还可以使用再培训 Api。 该示例代码位于[此处](https://azuremlretrain.codeplex.com/)。


**如何监视我在生产环境中部署的 Web 服务？** 

一旦投入提出了预测模型，您可以监视它从 Azure 的门户。 每个已部署的服务都有其自己的仪表板，您可以查看监视该服务的信息。 

**有，我可以看到我的 RR/BE 的输出的位置吗？** 

对于 RR，web 服务响应通常是您在其中查看结果。 您还可以编写它的斑点。 为 BE，则输出，默认情况下写入到 blob。 您可以写入数据库或表使用编写器模块输出。
 
 * * 可以仅从在 Studio 中创建的模型创建的 web 服务？
No. 您还可以直接从 Jupyter 笔记本和 RStudio 创建 web 服务。
 

##可扩展性 

**什么是 web 服务的可扩展性？** 

目前，最大为 20 的并发请求，每个终结点，但它可以扩展到 10000 的终点。 这将转换为 4800 并发请求如果我们使用的每个资源 （300 工人）。  


**分布在节点 R 作业吗？** 

No.  


**可以训练上多少数据？** 

在 Studio 中机器学习模块支持常见的使用案例达 10GB 的密集的数字数据的数据集。 如果一个模块采用多个输入，10 GB 是所有输入的大小的总和。 此外可以取样较大数据集，通过配置单元或 SQL Azure 数据库查询，或通过统计学习预先处理前摄取。  

以下类型的数据功能正常化过程可以扩展到更大的数据集，并仅限于小于 10 GB:

- 稀疏
- 分类
- 字符串
- 二进制数据

以下模块仅限于小于 10 GB 的数据集︰

- 导购模块
- SMOTE 的模块
- 脚本编写模块︰ R，Python，SQL
- 模块输出数据的大小可能会大于输入数据的大小，例如联接或哈希处理功能。
- 交叉验证扫描参数、 序数回归和一个 vs 全部 Multiclass，当迭代的数字是非常大。

用于大于几 GB 的数据集，应将数据上载到 Azure 存储或 SQL Azure 数据库或使用 HDInsight，而不是直接从本地文件上传。


**是否有任何向量大小限制？** 

行和列的每个限制为最大值 Int 的.NET 限制︰ 2147483647。 

**可以调整此运行在虚拟机大小** 

No.  

##安全性和可用性 

**谁有权访问 http 终结点的 web 服务部署在生产环境中，默认情况下？ 如何限制对终结点的访问？** 

在发布 web 服务之后，我们将创建该服务的默认终结点。 该默认终结点部署到生产环境，可以通过 API 键调用。 可以使用自己的密钥从 Azure 门户或使用 Web 服务管理 Api 以编程方式添加其他终结点。 访问键所需进行生产和转移对 web 服务的调用。 有关详细信息，请参阅[连接到机器学习的 web 服务](machine-learning-connect-to-azure-machine-learning-web-service.md)。


**如果找不到我的存储客户，会怎么样？** 

机器学习 Studio 依赖用户提供的 Azure 存储帐户来执行工作流时保存的中间数据。 此存储帐户为提供到机器学习 Studio 时将创建一个工作区。 创建工作区后，如果存储帐户被删除，并且不能再找，工作区将停止运行，所有试验中，工作区将会失败。
 
如果您意外地删除存储帐户，从其恢复的唯一方法是重新创建该存储客户，与完全相同的名称与已删除的确切同一区域中。 在此之后，请重新同步访问键。
 

**如果我存储帐户访问键是同步的，怎么样？** 机器学习 Studio 依赖用户提供的 Azure 存储帐户来执行工作流时保存的中间数据。 此存储帐户为提供到机器学习 Studio 时将创建一个工作区，访问密钥与该工作区相关联。 如果更改了访问键创建工作区后，存储帐户，将无法再访问该工作区和它将停止工作，该工作区中的所有实验将都失败。

如果您更改了存储帐户访问键，请确保要重新同步工作区设置 Azure 门户中访问键  


##Azure 的市场 

请参见[发布和使用应用程序中机器学习市场常见问题解答](machine-learning-marketplace-faq.md)

##技术支持和培训 

**从哪里可以获得 Azure ML 的培训？** 

[Azure 机器学习文档中心](/services/machine-learning/)承载的视频教程，以及操作方法指南。 这些逐步骤指南提供服务的简介，并遍历数据科学生命周期导入数据、 数据清洗、 构建预测模型和 Azure ML 与生产环境中部署它们。 

我们将添加新的材料到计算机教学中心在日常的基础上。 您可以提交请求[用户反馈论坛](https://windowsazure.uservoice.com/forums/257792-machine-learning)在机器学习中心其他学习材料。 

您还可以找到在[Microsoft 虚拟学院](http://www.microsoftvirtualacademy.com/training-courses/getting-started-with-microsoft-azure-machine-learning)培训

**如何获取对 Azure 机器学习的支持？** 

若要获取 Azure 机器学习的技术支持，请转到[Azure 支持](/support/options/)选择**机器学习**。

Azure 机器学习还有上，您可以提出 Azure ML 的 MSDN 社区论坛相关的问题。 论坛由 Azure ML 团队监控。 访问[Azure 的论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning)。 


<!-- Module References -->
[图像读取器]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[联接]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[机器学习模块]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[分区示例]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 

测试
