---
ms.openlocfilehash: a69688f902167d8a5609312477b49237151a8571
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="机器学习与信用风险的预防性解决方案 |Microsoft Azure"
    description="演示如何创建在 Azure 机器学习 Studio 中的信用风险评估的一个预测分析的解决方案的详细的演练。"
    keywords="信用风险、 预测分析的解决方案、 风险评估"   
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
    ms.topic="get-started-article" 
    ms.date="07/10/2015"
    ms.author="garye"/>


# 演练︰ 开发 Azure 机器学习中的信用风险评估的预测分析的解决方案

假设您需要预测个人的信用风险，根据它们提供一项信用申请有关的信息。  

当然，信用风险评估是一个复杂的问题，但让我们有点简化问题的参数。 然后，我们可以将其用作举例说明如何使用机器学习 Studio 和机器学习的 web 服务中使用 Microsoft Azure 机器学习来创建这样一个预测分析的解决方案。  

在此详细的演练中，我们将采用开发中机器学习 Studio 的预测分析模型，然后将它发布作为 Azure 机器学习的 web 服务的过程。 我们将开始与公开信用风险数据，开发基于该数据，预测模型进行训练，然后发布 web 服务可以被其他人使用信用风险评估模型。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

打开机器学习 Studio: [https://studio.azureml.net/Home](https://studio.azureml.net/Home)。 有关入门机学习 Studio 的详细信息，请参阅[Microsoft Azure 机器学习 Studio 住宅](https://studio.azureml.net/)。

若要创建一个信用风险评估的解决方案，我们将执行以下步骤︰  

1.  [创建一个机器学习工作区](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [将现有数据上载](machine-learning-walkthrough-2-upload-data.md)
3.  [创建新的实验](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [发布 web 服务](machine-learning-walkthrough-5-publish-web-service.md)
6.  [访问 web 服务](machine-learning-walkthrough-6-access-web-service.md)

本演练基于[信用风险预测样本实验](../machine-learning-sample-credit-risk-prediction.md)机器学习 Studio 附带的简化版本。
 
测试
