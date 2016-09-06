---
ms.openlocfilehash: 21eaf26ed0119df68cf27e4f8d708cd474019f40
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="步骤 3︰ 创建新的机器学习实验 |Microsoft Azure" 
    description="制定预防性解决方案演练中的步骤 3︰ 在 Azure 机器学习 Studio 中创建新的培训试验。" 
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


# 演练中步骤 3︰ 创建新的 Azure 机器学习实验

这是演练中，[开发与 Azure ML 的预测性解决方案](machine-learning-walkthrough-develop-predictive-solution.md)的第三步


1.  [创建一个机器学习工作区](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [将现有数据上载](machine-learning-walkthrough-2-upload-data.md)
3.  **创建新的实验**
4.  [训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [发布 web 服务](machine-learning-walkthrough-5-publish-web-service.md)
6.  [访问 web 服务](machine-learning-walkthrough-6-access-web-service.md)

----------

我们需要使用我们上载的数据集的 ML Studio 中创建新的实验。  

1.  在 ML Studio 中，请单击**+ 新建**窗口的底部。
2.  选择**实验**，然后再选择"空白试验"。 选择默认实验画布顶部的名称并将其重命名为有意义

    > [AZURE.TIP] 最好要填写在**属性**窗格中实验的**总结**和**说明**。 这些属性使您可以记录实验，以便以后查看它的任何人将了解您的目标和方法的机会。

3.  在左侧的实验模块组件面板画布、**保存数据集**。
4.  查找**我的数据集**下创建的数据集并将其拖动到画布。 此外可以通过面板上方的**搜索**框中输入名称来查找数据集。  

##准备数据
您可以通过单击该数据集的输出端口并选择**查看结果**查看前 100 行数据和整个数据集的一些统计信息。 请注意，ML Studio 已标识的每个列的数据类型。 它已因为数据文件不是具有列标题对列赋予通用标题。  

列标题不是必要的但它们将使其更方便地处理模型中的数据。 另外，当我们最终在 web 服务发布此模型中，标题将帮助标识该服务的用户的列。  

我们可以添加列标题使用[元数据编辑器][元数据编辑器]模块。
[编辑器中的元数据][的元数据编辑器]模块用于更改与数据集关联的元数据。 在这种情况下，它可以提供更多的列标题的友好名称。 为此，我们将直接[元数据编辑器][编辑器中元数据的]所有列进行都操作，然后提供要添加到列名称的列表。

1.  在模块组件面板中，键入**搜索**框中的"元数据"。 您将看到[元数据编辑器][元数据编辑器]的模块列表。
2.  单击并拖动到画布上的[编辑器中元数据][的元数据编辑器]模块放到下面数据集。
3.  将数据集连接到[编辑器中的元数据][的元数据编辑器]︰ 单击数据集的输出端口，将拖放到[编辑器中的元数据][的元数据编辑器]，输入端口，然后释放鼠标按钮。 即使您移动或者在画布上，数据集和模块将保持连接。
4.  使用[元数据编辑器][元数据编辑器]仍处于选中状态的情况下，在画布上，右侧的**属性**窗格中单击**启动列选定器**。
5.  在**选择列**对话框中，**开始与**将字段设置为"所有列"。
6.  **开始与**下面的行，可包括或排除特定的列[元数据编辑器][元数据编辑器]来修改。 因为我们想要修改的所有列，则删除此行通过单击减号 ("-") 的行的右边。 对话应如下所示︰![用选择的所有列的列选定器][4]
7.  单击**确定**的选中标记。 
8.  在**属性**窗格中，寻找**新的列名称**参数。 在此字段中，输入数据集中，分隔逗号和列顺序的 21 列名称的列表。 您可以提示您在网站上，数据集文档中获取列名称或为方便起见可以复制并粘贴以下项︰  

<!-- try the same thing without upper-case 
        Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
-->

    status of checking account, duration in months, credit history, purpose, credit amount, savings account/bond, present employment since, installment rate in percentage of disposable income, personal status and sex, other debtors, present residence since, property, age in years, other installment plans, housing, number of existing credits, job, number of people providing maintenance for, telephone, foreign worker, credit risk  

属性窗格将如下所示︰

![属性元数据编辑器][1] 

> [AZURE.TIP] 如果您想要验证的列标题，运行试验 （单击**运行**实验画布下面），请单击[编辑器中的元数据][的元数据编辑器]模块的输出端口并选择**查看结果**。 您可以查看任何模块的输出，以相同的方式，可以查看通过实验数据的进度。

实验现在应该看起来如下所示︰  

![添加元数据编辑器][2]
 
##创建培训和测试数据集
实验的下一步是生成不同的数据集将用于培训和测试我们的模型。 为此，我们使用[拆分]的[拆分]模块。  

1.  查找[拆分][拆分]模块，将其拖动到画布上，并将其连接到最后一个[元数据编辑器][元数据编辑器]模块。
2.  默认情况下，拆分比例为 0.5 和**Randomized 拆分**参数设置。 这意味着数据的随机半将通过某一端口的[拆分][拆分]模块，和出另一半的输出。 您可以调整这些数据，以及**随机种子**参数，将培训和评分数据之间拆分。 在本例中我们将使它们作为-是。
    > [AZURE.TIP] 拆分比例基本上确定数据中有多少是通过左的输出端口的输出。 例如，如果将该比率设置为 0.7，70%的数据则通过左端口输出和 30%，至正确的端口。  
    
但是我们喜欢，但让我们选择训练数据中用作左的输出和右输出为评分数据，我们可以使用[拆分][拆分]模块的输出。  

提示您网站上提到过，归类为低高信用风险的成本是与归类为高低信用风险的成本还要高 5 倍。 要考虑到这一点，我们将生成新的数据集反映此成本函数。 在新的数据集，每个高示例复制 5 次，而不被复制的每个低的示例。   

我们可以做此复制使用 R 代码︰  

1.  查找和[执行 R 脚本][执行 r 脚本]模块拖到实验画布并[拆分][拆分]模块的左的输出端口连接到[执行 R 脚本][执行 r 脚本]模块的第一输入端口 ("Dataset1")。
2.  在**属性**窗格中，删除默认文本中**R 脚本**参数并输入此脚本︰ 

        dataset1 <- maml.mapInputPort(1)
        data.set<-dataset1[dataset1[,21]==1,]
        pos<-dataset1[dataset1[,21]==2,]
        for (i in 1:5) data.set<-rbind(data.set,pos)
        maml.mapOutputPort("data.set")


我们需要做[拆分][拆分]模块的每个输出这个同一个复制操作，以便培训和评分数据都具有相同的成本调整。

1.  右键单击[执行 R 脚本][执行 r 脚本]模块，然后选择**复制**。
2.  实验画布上用鼠标右键单击并选择**粘贴**。
3.  将此[执行 R 脚本][执行 r 脚本]模块的第一输入的端口连接到[拆分]的[拆分]模块的右输出端口。  

> [AZURE.TIP] 执行 R 脚本模块的副本包含原始模块的同一个脚本。 当您复制并粘贴在画布上的模块时，该副本保留原文件的所有属性。  
>
我们的实验现在看起来类似下面这样︰
 
![添加分割模块和 R 脚本][3]

在实验过程中使用 R 脚本的详细信息，请参阅[扩展实验与 R](machine-learning-extend-your-experiment-with-r.md)。

**下一步︰[训练和评估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[执行 r 脚本]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[元数据编辑器]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
测试
