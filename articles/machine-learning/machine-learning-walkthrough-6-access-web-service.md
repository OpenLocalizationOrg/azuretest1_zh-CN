---
ms.openlocfilehash: 84dd52fb8e0503d176f5261f5834e452960ff2ea
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="第 6 步︰ 访问机器学习 web 服务 |Microsoft Azure" 
    description="制定预防性解决方案演练中的步骤 6︰ 访问活动的 Azure 机器学习 web 服务。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/10/2015" 
    ms.author="garye"/>


# 演练第 6 步︰ 访问 Azure 机器学习 web 服务

这是演练中，[开发与 Azure ML 的预测性解决方案](machine-learning-walkthrough-develop-predictive-solution.md)的最后一步


1.  [创建一个机器学习工作区](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [将现有数据上载](machine-learning-walkthrough-2-upload-data.md)
3.  [创建新的实验](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [发布 web 服务](machine-learning-walkthrough-5-publish-web-service.md)
6.  **访问 web 服务**

----------

对于 web 服务可用，用户需要能够向其发送数据和接收结果。 Web 服务是 Azure 的 web 服务，可以接收和返回的数据中有两种︰  

-   **请求/响应**的用户通过使用 HTTP 协议，向服务发送单个数据集的信用和服务响应与单个结果集。
-   **执行批处理**的用户向服务发送包含一个或多个数据行的信贷 Azure blob 的 URL。 该服务将结果存储在另一个 blob 中并返回该容器的 URL。  

为 web 服务**面板**选项卡上，有两个链接，可帮助开发人员编写代码以访问该服务的信息。 单击该行**请求/响应**的**API 帮助页**链接，页面会打开一个包含示例代码，以使用该服务的请求/响应协议。 同样，在**批处理执行**该行上的链接对该服务进行批处理请求提供代码示例。  

API 的帮助页包括 R C# 和 Python 编程语言的示例。 

有关如何访问和使用 web 服务的详细信息，请参阅[如何使用机器学习实验从已发布的 Azure 机器学习 web 服务](machine-learning-consume-web-services.md)。
测试
