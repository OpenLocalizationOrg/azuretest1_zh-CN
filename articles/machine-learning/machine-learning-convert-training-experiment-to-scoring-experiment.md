---
ms.openlocfilehash: 4a689c1c7f396eb8ad82a7343de462e9af567ee4
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="将机器学习培训实验转换为分数 |Microsoft Azure" 
    description="如何将机器学习培训实验，用于培训您的预测分析模型，在计分的实验可以作为 web 服务发布到。" 
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

#将机器学习培训实验转换为计分实验

Azure 机器学习使您能够生成、 测试和部署预测分析的解决方案。 

一旦已经创建并在*培训试验*来训练您的预测分析模型，进行迭代，您就可以使用它来成绩新数据，您需要准备并优化实验的评分。 您可以发布此*评分实验*作为 Azure 的 web 服务，以便用户可以将数据发送到您的模型，并接收您的模型的预测。

通过将转换为计分实验，您现在准备培训的模型作为评分的 web 服务发布。 Web 服务的用户将输入的数据发送到您的模型，模型将发送回的预测结果。 因此随着转换计分实验需要记住您希望您的模型可由他人使用。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

转换到计分实验培训实验的过程包括三个步骤︰

1.  保存已接受培训，然后替换已保存培训模型的机器学习算法和[训练模型][火车模型]模块的机器学习模型。
2.  修剪到只有模块所需的评分实验。 在培训实验包括多个模块所必需的培训，但培训并可供使用的评分模型后，则不需要。
3.  在实验中将接受来自 web 服务的用户，并将返回的数据定义。

##创建评分试验按钮

实验画布的底部的**创建记分试验**按钮将为您执行转换计分实验培训实验的三个步骤︰

1.  它保存模块**培训模型**部分模块组件面板 （实验画布的左侧），然后将替换机器学习算法，与已保存培训模型[训练模型][火车模型]模块培训的模型。 
2.  它将删除明确不需要的模块。 在我们的示例中，这包括[拆分][拆分]、 第二个[分数模型][评分模型]和[计算模型][计算模型]模块。
3.  它创建 Web 服务的输入和输出模块并将它们添加在实验中的默认位置。

例如，下面的实验培训两类提升的决策树模型使用人口普查数据的示例︰

![培训实验][figure1]

在此试验中的模块执行基本上是四个不同的功能︰

![模块函数][figure2]

此培训试验转换到计分的实验时，一些这些模块不再需要或他们有不同的用途︰

- 在评分过程中不使用**数据**-此示例数据集中的数据-web 服务的用户将提供数据以进行评分。 但是，从该数据集，如数据类型的元数据使用培训的模型。 因此，您需要以计分，实验中保留数据集，以便它可以提供此元数据。

- **准备**-根据的数据将提交进行评分，这些模块可能会或可能不需要处理传入的数据。 

    例如，在本例中的示例数据集可能有缺少的值，它包括的列不需要训练模型。 因此包括[干净的缺失数据][清洗的缺失的数据]模块来处理缺少的值，并且[项目列][项目列]模块曾从数据流中排除这些额外的列。 如果您知道将提交有关评分通过 web 服务的数据将没有缺少的值，您可以删除[干净缺失数据][清洗的缺失的数据]模块。 但是，由于[项目列][项目列]模块可帮助定义的一套功能进行评定，该模块需要保留。

- **火车**的模型已成功定型后，您将其保存为单个培训的模型模块。 您然后将这些各个模块替换已保存培训模型。

- **分数**-在本例中，拆分模块用于将数据流划分为一组测试数据和训练数据。 计分，实验中这不需要可以删除。 同样，第二个[分数模型][评分模型]模块和[计算模型][计算模型]模块用于比较结果的测试数据，因此这些模块还不需要计分的实验中。 剩余的[分数模型][评分模型]模块，但是，需要返回通过 web 服务的评分结果。

这是我们的示例中单击**创建记分实验**后的外观︰ 

![转换计分实验][figure3]

这可能不足以准备实验，才能发布为 web 服务。 但是，可能需要执行一些附加工作实验所特有。

###调整输入和输出模块

在培训实验，您使用一套培训数据，然后做一些处理，从中获取数据的机器学习算法所需的窗体中。 如果您希望通过 web 服务收到的数据不需要这种处理，可以到不同的节点移动**Web 服务输入的模块**，实验中。

例如，默认情况下**创建记分实验**将**Web 服务输入**模块顶部的数据流，如上面的图中所示。 但是，如果输入的数据将不需要这种处理，然后您可以手动定位**Web 服务输入**过去的数据处理模块︰

![移动 web 服务输入][figure4]

通过 web 服务提供的输入的数据将立即传递到分数模型模块直接而无需任何预处理。

类似地，默认情况下**创建记分实验**将 Web 服务输出模块底部的数据流量。 在此示例中，web 服务将返回到用户[评分模型][评分模型]模块包括完整的输入的数据向量，再加上评分结果的输出。

但是，如果您愿意返回一些与众不同的例如，只有评分结果，不输入的数据，则您的整个向量可以插入[项目列][项目列]模块要排除所有列除外的计分结果。 然后**Web 服务输出**中的模块移到[项目列][项目列]模块的输出︰

![移动 web 服务的输出][figure5]

###添加或删除其他数据处理模块

如果您知道在评分过程中不必需的实验中有多个模块，可以删除这些。 例如，因为我们有后数据处理模块移至**Web 服务输入**模块，我们可以从计分实验删除[干净的缺失数据][清洗的缺失的数据]模块。 

我们计分实验现在如下所示︰

![删除附加模块][figure6]

###添加可选的 Web 服务参数

在某些情况下，您可能想要允许您的 web 服务的用户访问该服务时更改模块的行为。 *Web 服务参数*允许您执行此操作。

一个常见的例子建立[读取器][读取器]模块，以便访问 web 服务时，已发布的 web 服务的用户可以指定不同的数据源。 或者配置该[编写器][写入器]模块，以便可以指定一个不同的目的地。 

您可以定义 Web 服务参数并将其与一个或多个模块参数相关联，您可以指定它们是否必需或可选。 访问该服务，并将相应地修改模块操作时，该 web 服务的用户然后可以为这些参数提供值。

有关 Web 服务参数的详细信息，请参阅[使用 Azure 机器学习 Web 服务参数] [webserviceparameters]。

[webserviceparameters]: machine-learning-web-service-parameters.md


##作为 web 服务发布计分的实验

现在，计分实验已充分准备好，可以将其发布为 Azure 的 web 服务。 使用 web 服务，用户可以将数据发送到您的模型，该模型将返回其预测。

完成发布流程的详细信息，请参阅[发布 Azure 机器学习 web 服务][发布]

[发布]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[图 1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[图 2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[图 4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[清洁的缺失的数据]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[评估模型]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[项目列]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[读取器]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[分数模型]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[火车模型]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[编写器]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
测试
