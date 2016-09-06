---
ms.openlocfilehash: 65582e2e0d881a36c74d3023c84cc93ad090a74d
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="以编程方式重新培训机器学习模型 |Microsoft Azure" 
    description="了解如何以编程方式重新培训模型并更新 web 服务使用 Azure 机器学习在新培训的模型。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services" 
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2015"
    ms.author="raymondl;garye"/>


#以编程方式重新培训机器学习模型  
 
作为 operationalization 的机器学习模型在 Azure 机器学习的过程，模型需要经过培训并保存，然后用于创建记分的 web 服务。 然后可以在网站、 面板和移动应用程序使用 web 服务。  

通常情况下，您需要重新培训中的第一步，用新数据创建的模型。 以前，这只是可能通过 Azure ML 用户界面，但再培训的编程 API 功能的引入，现在可以重新培训模型并更新 Web 服务中，要使用新培训的模型，以编程方式使用再培训 Api。  

本文档描述上面的过程，并演示如何使用人员进行再培训的 Api。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 

##为什么重新培训︰ 定义问题  
ML 培训过程的一部分，作为一个模型经过培训使用的一组数据。 模型需要被重新训练方案中在新数据变得可用，或当 API 的使用者都有他们自己的数据训练模型，或者当数据所需的筛选和模型训练等数据的子集。  

在这些情况下，编程 API 提供了允许您或您的 Api 的使用者创建一个客户端可以在一次性或定期的基础上对使用其自己的数据模型的一种简便方法。 他们可以评估结果的再培训，并更新 Web 服务 API 使用新培训的模型。  

##如何重新培训︰ 端到端流程  
若要开始，该过程包括以下组件︰ 培训实验和评分的实验发布为 Web 服务。 若要启用培训模型的再培训，培训实验不得不还发布为 Web 服务与培训模型的输出。 这使 API 访问的模型，对人员进行再培训。 设置重新培训的过程包括以下步骤︰  

![][1]
 
图 1︰ 重新培训过程概述  

1. *创建培训实验*  
    我们将使用实验"示例 5 (二进制分类的培训，测试，评估︰ 成人的数据集)"从 Azure ML 取样试验本示例。 您将看到下面，我已经通过删除某些模块简化示例。 我也有称为实验"人口普查模型"。

    ![][2]

    我们现在可以与这些组件之后，单击屏幕底部的运行以运行此试验。  
2. *创建评分的实验，并作为 Web 服务发布*  
    
    ![][3]  

    实验运行完毕后，单击创建评分进行试验。 这将评分的实验，为培训模型保存模型并添加 Web 服务的输入和输出模块，如下所示。 我们下一步，单击运行。  

    实验运行完毕后，单击"发布 Web 服务"上将发布评分试验作为 Web 服务并创建默认终结点。 此 web 服务中的培训的模型是可更新的、，如下所示。 此终结点的详细信息然后将显示在屏幕上。  
3. *作为 Web 服务发布培训实验*   
    若要重新训练培训的模型，我们需要发布培训实验我们在上面的步骤 1 作为 Web 服务中创建。 此 Web 服务将需要 Web 服务输出模块连接到[训练模型][火车模型]模块，能够产生新的培训的模式。
试验在左窗格中，图标上单击，然后单击称为人口普查回到培训实验的模型试验。  

    我们然后将一个 Web 服务的输入和两个 Web 服务输出模块添加到该工作流。 火车模型的 Web 服务输出将为我们提供了新的培训的模式。 附加到评估模型的输出将返回该模块的评估模型输出。   

    我们现在可以单击运行。 实验完成运行后，生成工作流应该如下所示︰
 
    ![][4]

    我们下一步单击发布 Web 服务按钮，然后单击是。 这将作为培训的模型和模型计算结果会产生一个 Web 服务发布培训实验。 Web 服务面板将显示执行批处理的 API 键和 API 的帮助页。 请注意，可以用于仅执行批处理方法创建培训模型。  
4. *添加新的终结点*  
    我们在上面的步骤 2 中公布评分 Web 服务创建与默认终结点。 默认终结点将保留与原始的培训和评分试验，同步，因此不能替换默认终结点的培训的模型。
若要创建可更新的终结点就诊 Azure 门户，单击添加终结点上 (更多详细信息[这里](machine-learning-create-endpoint.md))。    

5. *对具有新数据和 BES 模型*  
    调用重新培训的 Api，我们在 Visual Studio 中创建一个新 C# 控制台应用程序 (新-> 项目-> Windows 桌面-> 控制台应用程序)。  

    我们然后从培训 Web 服务 API 帮助页 （在上面的步骤 3 中创建） 的批处理执行复制示例 C# 代码并将其粘贴到 Program.cs 文件中，并确保该命名空间将保持不变。  

    请注意，示例代码注释，这种情况表明需要更新的代码部分。 
    此外，当明确指定"output1"Reqeust 有效负载中的位置，"RelativeLocation"的文件扩展名有要更改在"输出"的".ileaner": {全局参数...{"output1": {"连接字符串":"DefaultEndpointsProtocol = https。帐户名 = mystorageacct;AccountKey = Dx9WbMIThAvXRQWap/aLnxT9LV5txxw = ="，"RelativeLocation":"mycontainer/output1results.ilearner"}}。

    1. 到 Azure 存储提供了 Azure 存储信息 BE 的示例代码将上载的文件从本地驱动器 (例如"C:\temp\CensusIpnput.csv")，处理它，并将结果写回 Azure 存储。  

        为实现这一点，您需要检索存储帐户名称、 密钥和容器信息从您的存储帐户和更新的代码在此处 Azure 管理门户。 您还需要确保输入的文件可在代码中指定的位置。  

        因此，结果将存储位置信息的这两项内容，如下所示，我们必须设置具有两个输出，此培训试验。 "output1"是经过培训的模型中，"output2"的输出输出的计算模型。  此外请注意培训模型 (Output1) 的输出的文件扩展名为".ileaner"和不".csv"。

        ![][6]
 
6. *评估再培训结果*  
    使用"output2"BaseLocation、 RelativeLocaiton 和 SasBlobToken 从上面的输出结果的组合我们可以通过将完整 URL 粘贴到浏览器地址栏中看到的 retrained 模型的绩效结果。  

    这将告诉我们是否很好地执行新培训的模型来替换现有。 

7. *更新添加终结点的培训模型*  
    若要完成此过程，我们需要更新我们在上面的步骤 4 中创建的计分端点的培训的模型。  

    BES 输出上面显示的信息包含重新训练模型位置信息的"output1"的再培训的结果。 我们现在需要采取此种培训的模型并更新计分的终结点。 （在上面的步骤 4 中创建）

    ![][7]
  
    "ApiKey"和"endpointUrl"此调用是在终结点操控板上可见。

    此调用成功，新的终结点将开始使用大约在 15 秒内的 retrained 的模型。  

##摘要  
使用人员进行再培训的 Api，我们可以更新培训的模型的实现方案，如定期再培训用新数据或分发给客户的模型的目的是为了让他们重新培训使用他们自己的数据模型的模型预测的 Web 服务。  

[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[火车模型]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 

测试
