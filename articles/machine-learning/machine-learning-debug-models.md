---
ms.openlocfilehash: 33e3e22e714b74a0058cb2c6e282fef7056e4d37
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="调试在 Azure 机器学习模型 |Microsoft Azure" 
    description="解释如何如何调试在 Azure 机器学习模型。" 
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
    ms.date="04/29/2015" 
    ms.author="bradsev;garye" />

# 调试在 Azure 机器学习模型

本文介绍如何调试 Microsoft Azure 机器学习中的模型。 具体来说，它涵盖的潜在原因为什么以下两种故障情形之一时，可能遇到运行模型︰

* [训练模型][火车模型]模块将引发错误 
* [评分模型]的[分数模型]模块产生不正确的结果 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 火车模型模块将引发错误

![image1](./media/machine-learning-debug-models/train_model-1.png)

[训练模型][火车模型]模块需要以下 2 输入︰

1. 从集合中提供的 Azure 机器学习模型分类/回归分析模型的类型
2. 培训数据和指定的标签列。 标签列中指定的变量来预测。 包括的列的其余部分假定为特征。

本模块在以下情况将引发错误︰

1. 标签列未正确指定，因为多个列被选定为该标签或者选择了不正确的列索引。 例如，如果 30 列索引所用输入数据集，其中有 25 只列都将应用第二种情况。

2. 数据集不包含任何功能的列。 例如，如果输入数据集有仅 1 列，被标记为标签列，则会没有用来构建模型的功能。 在这种情况下，[训练模型][火车模型]模块将引发错误。

3. 输入数据集 （功能或标签） 包含一个值为无穷大。


## 分数模型模块不会产生正确的结果

![image2](./media/machine-learning-debug-models/train_test-2.png)

典型测试培训/关系图用于监察学习，[拆分][拆分]模块将原始数据集分成两个部分︰ 没有没有被用来训练模型的一部分并保留得分如何更好地培训的模型对数据执行它的部分上进行训练。 培训的模型再用来成绩测试数据的计算结果来确定模型的准确性。

[评分模型]的[分数模型]模块要求两个输入︰

1. 从[训练模型][火车模型]模块培训的模型输出
2. 评分数据集模型已不在接受培训的非

它可能会发生即使实验成功，[评分模型]的[分数模型]模块产生错误的结果。 几种情况可能会导致出现这种情况︰

1. 如果指定的标签是分类和回归模型经过培训的数据，进行[评分模型]的[分数模型]模块将产生错误的输出。 这是因为回归需要连续响应变量。 在这种情况下应该是更适合于使用分类模型。 
2. 同样，如果分类模型经过培训上有浮点数在标签列中的数据集，它可能会产生不良后果。 这是因为分类需要离散响应变量对有限，通常稍小的类集只允许值的范围。
3. 如果评分数据集不包含用来训练模型的所有功能，[分数模型][评分模型]将产生错误。
4. [分数模型][评分模型]不会产生任何输出对应于包含缺少的值或任何其功能无限值评分集内的数据行。
5. [分数模型][评分模型]可能会产生相同的输出结果评分数据集中的所有行。 可能出现该问题，例如，在试图超过可用的训练示例数如果选择的样本，每个叶节点的最小数目是为使用决策林进行分类。


<!-- Module References -->
[分数模型]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[火车模型]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
测试
