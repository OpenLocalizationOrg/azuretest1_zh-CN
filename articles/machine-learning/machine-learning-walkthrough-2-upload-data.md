---
ms.openlocfilehash: 1e3a0726a9d386bbf4a42d84f248b8cc81304eff
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="步骤 2︰ 将数据上载到机器学习实验 |Microsoft Azure" 
    description="制定预防性解决方案演练中的第 2 步︰ 上载到 Azure 机器学习 Studio 存储公用的数据。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/10/2015" 
    ms.author="garye"/>


# 演练中步骤 2︰ 将现有的数据上传到 Azure 机器学习实验

这是演练中，[开发与 Azure ML 的预测性解决方案](machine-learning-walkthrough-develop-predictive-solution.md)的第二步


1.  [创建一个机器学习工作区](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **将现有数据上载**
3.  [创建新的实验](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [发布 web 服务](machine-learning-walkthrough-5-publish-web-service.md)
6.  [访问 web 服务](machine-learning-walkthrough-6-access-web-service.md)

----------

若要开发信用风险的预测模型，我们将从提示您机器学习资料库使用"提示您 Statlog （德国信用数据） 的数据集"。 您可以在这里找到︰  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

我们将使用名为**german.data**的文件。 将此文件下载到本地硬盘。  

此数据集包含 1000年过去申请人信用 20 变量的行。 这些 20 变量表示数据集的特征矢量，它为每个信用卡申请人提供标识特征。 其他列中的每一行代表申请人的信用风险，与 700 的申请人为高风险标识为低信用风险和 300。   

提示您网站提供的属性的特征向量，包括财务信息、 信用历史记录、 雇用状态和个人信息的说明。 每个申请人中，二进制评级已经给定，指示它们是否较低或高信贷风险。  

我们将使用此数据来训练预测分析模型。 当我们完成时，我们的模型应该是能够接受的新的个人信息和预测它们是否低或高的信用风险。  

这是一个有趣。 数据集描述解释时实际上高信用风险较低的信用风险金融机构为 5 倍成本比归类为高低信用风险归类的人。 这考虑到我们实验中的一个简单方法是复制 （5 次） 的那些条目表示具有高信用风险的人。 然后，如果模型体验作为低高信用风险，它将执行该 misclassification 5 次，一次每个副本。 这将增加培训结果中的该错误的成本。  

##将数据集格式转换
原始数据集使用空白分隔的格式。 机器学习 Studio 的工作更好地用逗号分隔值 (CSV) 文件，因此我们将通过用逗号替换空格转换数据集。  

我们可以使用下面的 Windows PowerShell 命令来执行此操作︰   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

我们还可以通过使用 Unix sed 命令做这︰  

    sed 's/ /,/g' german.data > german.csv  

##对机器学习 Studio 上载数据集

一旦数据已转换为 CSV 格式，我们需要将它上传到计算机学习 Studio。  

1.  使用您指定为所有者的工作区中，Microsoft 帐户登录到计算机学习 Studio ([https://studio.azureml.net](https://studio.azureml.net))，然后单击顶部的**Studio**选项卡。
2.  请单击**+ 新建**窗口的底部。
3.  选择**数据集**。
4.  选择**从本地文件**。
5.  在**上载一个新数据集**对话框中，单击**浏览**，查找您创建的**german.csv**文件。
6.  输入数据集的名称。 对于本示例，我们将称之为"提示您德语信用卡数据"。
7.  对于数据类型，选择**没有头与一般的 CSV 文件 (。 nh.csv)**。
8.  如果您想，添加说明。
9.  单击**确定**。  

![上载数据集][1]  

 
这将数据上载到我们可以在实验中使用的数据集模块。

关于导入一个实验的各种类型的数据的详细信息，请参阅[导入到 Azure 机器学习 Studio 培训数据](machine-learning-import-data.md)。

**下一步︰[创建新的实验](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
 
测试
