---
ms.openlocfilehash: d2164a820bdc7c3141024dc1772cb5b4718378da
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="连接到计算机学习 Web 服务 |Microsoft Azure" 
    description="使用 C# 或者 Python，连接到使用身份验证密钥的 Azure 机器学习 web 服务。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/17/2015" 
    ms.author="derrickv" />


# 连接到 Azure 机器学习 Web 服务 
Azure 机器学习开发人员的体验是 web 服务 API 实时或在批处理模式下进行输入数据的预测。 您可以使用 Azure 机器学习 Studio 创建的预测和发布 Azure 机器学习 web 服务。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

若要了解有关如何创建和发布使用 Studio Azure 机器学习 web 服务︰

- [将机器学习 web 服务发布](machine-learning-publish-a-machine-learning-web-service.md)
- [ML Studio 入门](http://azure.microsoft.com/documentation/videos/getting-started-with-ml-studio/)
- [Azure 机器学习预览](https://studio.azureml.net/)
- [机器学习文档中心](http://azure.microsoft.com/documentation/services/machine-learning/)

## Azure 机器学习 web 服务 ##

Azure 机器学习 (ML) web 服务中，与外部应用程序通信实时计分模型 ML 工作流。 ML web 服务调用返回到外部应用程序的预测结果。 若要使 ML web 服务调用，您传递时发布的预测，则创建该 API 密钥。 ML web 服务基于 REST，web 编程项目的流行的体系结构选择。

Azure 机器学习有两种类型的服务︰

- 请求-响应服务 (RR)-低延迟、 高可扩展性服务，从 ML Studio 提供无状态模型创建并发布了一个接口。
- 批处理执行服务 (BES) – 异步服务的一批数据记录的成绩。

关于 Azure 机器学习 web 服务的详细信息，请参阅[发布机器学习的 web 服务](machine-learning-publish-a-machine-learning-web-service.md)。

## 获取使用 Azure 机器学习身份验证密钥 ##
可获得的 ML web 服务的 web 服务 API 密钥。 您可以从 Microsoft Azure 机器学习 studio 或 Azure 管理门户来获取它。
### Studio Microsoft Azure 机器学习 ###
1. 在 Microsoft Azure 机器学习 studio 中，单击左侧的**WEB 服务**。
2. 单击 web 服务。 "API 密钥"是在**仪表板**选项卡上。

### Azure 的管理门户 ###

1. 单击左侧的**机器学习**。
2. 单击工作区。
3. 单击**WEB 服务**。
4. 单击 web 服务。
5. 单击某个终结点。 "API 密钥"下位于右下角。

## <a id="connect"></a>连接到 Azure 机器学习 web 服务

您可以连接到使用任何编程语言，它支持 HTTP 请求和响应的 Azure 机器学习 web 服务。 从 Azure ML web 服务帮助页，可以在 C#、 Python 和 R 查看示例。

### 若要查看 Azure ML Web 服务 API 帮助页 ###
当您发布 web 服务时创建 Azure ML API 帮助页。 请参阅[Azure 机器学习演练的发布 Web 服务](machine-learning-walkthrough-5-publish-web-service.md)。


**若要查看 Azure ML API 帮助页**在 Microsoft Azure 机器学习 Studio 中︰

1. 选择**WEB 服务**。
2. 选择 web 服务。
3. **API 帮助页**中选择 - **请求/响应**或**执行批处理**。


**Azure ML API 帮助页**Azure ML API 的帮助页包含有关预测 web 服务的详细信息。



### C# 示例 ###

若要连接到 Azure ML 的 web 服务，请使用传递 ScoreData **HttpClient** 。 ScoreData 包含 FeatureVector，表示 ScoreData n 维向量的数值特征。 使用 API 密钥验证到 Azure ML 服务。

若要连接到 ML 的 web 服务，则必须安装**Microsoft.AspNet.WebApi.Client** Nuget 程序包。

**在 Visual Studio 中安装 Microsoft.AspNet.WebApi.Client Nuget**

1. 将提示您从下载数据集的发布︰ 成人 2 类数据集 Web 服务。
2. 单击**工具** > **Nuget 程序包管理器** > **程序包管理器控制台**。
2. 选择**安装软件包 Microsoft.AspNet.WebApi.Client**。

**若要运行此代码示例**

1. 发布"示例 1︰ 从提示您下载数据集︰ 成人 2 类数据集"实验，Azure ML 样本集合的一部分。
2. 从 web 服务使用密钥分配 apiKey。 请参阅如何获取使用 Azure ML 身份验证密钥。
3. 将 serviceUri 与请求的 URI。 


### Python 的示例 ###

若要连接到 Azure ML 的 web 服务，请使用传递 ScoreData **urllib2**库。 ScoreData 包含 FeatureVector，表示 ScoreData n 维向量的数值特征。 使用 API 密钥验证到 Azure ML 服务。


**若要运行此代码示例**

1. 发布"示例 1︰ 从提示您下载数据集︰ 成人 2 类数据集"实验，Azure ML 样本集合的一部分。
2. 从 web 服务使用密钥分配 apiKey。 请参阅如何获取使用 Azure ML 身份验证密钥。
3. 将 serviceUri 与请求的 URI。 请参阅如何获取请求的 URI。

    
 

测试
