---
ms.openlocfilehash: 427760cc3a34de49229e232a161c4d7e6312d32c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="在机器学习中使用线性回归 |Microsoft Azure" 
    description="在 Excel 中，Azure 机器学习 Studio 中的线性回归模型的比较" 
    metaKeywords="" 
    services="machine-learning" 
    solutions="" 
    documentationCenter="" 
    authors="garyericson" 
    manager="paulettm" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/14/2015" 
    ms.author="kbaroni;garye" />

# 在 Azure 机器学习中使用线性回归

> *Kate Baroni*和*Ben Boatman*都是在微软数据见解卓越中心的企业解决方案架构师。 在这篇文章，它们描述了他们将现有回归分析套件迁移到基于云的解决方案使用 Azure 机器学习的经验。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 目标

我们的项目着手构思中的两个目标︰  

1. 使用预测分析，以提高我们组织的每月收入预测的准确性  
2. 使用 Azure ML 确认、 优化、 提高速度，和我们的结果的小数位数。  

许多公司，像我们组织经历每月的收入预测过程。 我们小的团队，业务分析师的任务就是使用机器学习来支持过程并提高预测的准确性。  小组花了几个月前从多个来源收集数据和运行数据特性，通过识别相关的服务销售预测的关键特性的统计分析。  接下来的步骤是开始原型在 Excel 中的数据的统计回归模型。  在几个星期内我们就 outperforming 的当前字段和财务预测过程，Excel 回归模型。 这已经成为基线算法的预测结果。  


然后我们到我们预测的分析上移动到 Azure ML，以找出如何 Azure ML 无法提高预测性能上采取下一步。


## 实现预测性能奇偶校验

我们第一优先是达到 Azure ML 和 Excel 回归模型之间的奇偶校验。  培训和测试数据，我们想要实现 Excel 和 Azure ML 之间的预测性能奇偶校验已知完全相同的数据和相同的拆分。   最初我们失败。 Excel 模型 outperformed Azure ML 模型。   因缺乏理解在 Azure ML 的基本工具设置的故障。 后与 Azure ML 产品小组同步，我们获得了更好地了解我们数据集时，需要设置基，势均力敌的两个模型之间。  

### 在 Excel 中创建的回归模型
我们 Excel 回归使用标准的线性回归模型在 Excel 分析工具库中找到。 

我们计算*意味着绝对 %错误*并将其作为性能度量模型。  花了 3 个月内到达使用 Excel 的工作模型。  我们将学习到 Azure ML 实验最终是有益的了解需求的很多。

### 在 Azure 机器学习中创建比较试验  
我们遵循这些步骤以创建在 Azure ML 我们实验︰  

1.  上载数据集为 csv 文件到 Azure ML （非常小的文件）
2.  创建新的实验和[项目列][项目列]模块用于选择在 Excel 中使用的相同数据功能   
3.  [拆分][拆分]模块 （带有*相对表达式*模式） 用于将数据划分为完全相同的训练集，当在 Excel 中所做  
4.  尝试了[线性回归][线性回归]模块 （仅适用于默认选项），记录，并且相比于我们 Excel 回归模型的结果

### 检查初始结果
首先，Excel 模型清楚地 outperformed Azure ML 模型︰  

|   |Excel|Azure ML|
|---|:---:|:---:|
|表现|   |  |
|<ul style="list-style-type: none;"><li>调整 R 平方值</li></ul>| 0.96 |N/A|
|<ul style="list-style-type: none;"><li>系数 <br />确定</li></ul>|N/A|   0.78<br />（准确性较低）|
|意味着绝对的错误 |  9 美元。 5 米|  美元 19.4 M|
|意味着绝对错误 （%）|   6.03%|  12.2%

当我们运行我们的过程和结果的开发人员和数据科学家在 Azure ML 团队时，他们快速提供一些有用的提示。  

* 当您在 Azure ML 使用[线性回归][线性回归分析]模块时，提供了两种方法︰
    *  在线的渐变下降︰ 可能是更适合大规模的问题
    *  普通最小二乘法︰ 这是大多数人认为的当他们听到线性回归的方法。 对小型数据集，普通最小二乘法可以更好的选择。
*  考虑调整 L2 Regularization 量参数以提高性能。 默认情况下，我们较小的数据集设置为 0.001，我们将它设置为 0.005 以提高性能。    

### 解决神秘 ！
当我们应用的建议时，我们实现了 Azure ML 作为与 Excel 中相同的基准性能︰   

|| Excel|Azure ML （首次）|与最小二乘法的 azure ML|
|---|:---:|:---:|:---:|
|标记的值  |实际值 （数字）|相同|相同|
|学员  |Excel-> 数据分析-> 回归|线性回归分析。|线性回归|
|学员选项|N/A|默认值|普通最小二乘法<br />L2 = 0.005|
|数据集|26 行，3 种功能，1 的标签。   所有的数字。|相同|相同|
|拆分︰ 火车|Excel 在首先 18 行，测试的最后 8 行接受培训。|相同|相同|
|拆分︰ 测试|Excel 的回归公式应用于最后 8 行|相同|相同|
|**表现**||||
|调整 R 平方值|0.96|N/A||
|确定系数|N/A|0.78|0.952049|
|意味着绝对的错误 |9 美元。 5 米|美元 19.4 M|9 美元。 5 米|
|意味着绝对错误 （%）|<span style="background-color: 00FF00;"> 6.03%</span>|12.2%|<span style="background-color: 00FF00;"> 6.03%</span>|

此外，Excel 系数也与 Azure 的培训模型中的特征权重比较︰

||Excel 系数|Azure 的功能权重|
|---|:---:|:---:|
|截距/偏置|19470209.88|19328500|
|功能 A|0.832653063|0.834156|
|B 功能|11071967.08|11007300|
|功能 C|25383318.09|25140800|

## 下一步行动

我们希望使用 Azure ML Excel 内部的 web 服务。  我们的业务分析师依靠 Excel 和我们需要调用 Azure ML web 服务使用的 Excel 数据行并将其返回到 Excel 的预测的值的方法。   

我们还希望优化我们的模型，使用选项和可用在 Azure ML 算法。

### 与 Excel 集成
我们的解决方案是通过从培训的模型中创建 web 服务实施我们 Azure ML 的回归模型。  在几分钟内创建 web 服务，我们可以称其为直接从 Excel 返回预计的收入值。    

*Web 服务仪表板*部分包含可下载的 Excel 工作簿。  工作簿嵌入 web 服务 API 和架构信息的附带预先格式化。   单击*下载 Excel 工作簿*时，它将打开，并可以将其保存到本地计算机。    

![][1]
 
与打开工作簿时，将复制您预定义的参数到蓝色的参数部分如下所示。  一旦输入参数、 Excel 到 AzureML 的 web 服务调用和预测评分标签将显示在绿色预测值部分中。  工作簿将继续创建基于输入参数下的所有行项培训模型参数的预测。   有关如何使用此功能的详细信息，请参阅[使用 Excel 从 Azure 机器学习 Web 服务](machine-learning-consuming-from-excel.md)。 

![][2]
 
### 优化和进一步实验
现在，我们必须与我们 Excel 模型的基准，我们向前来优化我们 Azure ML 线性回归模型。  我们使用模块[基于筛选器的功能选择][筛选器基于-功能-选]为改善我们选择初始数据元素的它帮助我们实现了性能提高了 4.6%意味着绝对的错误。   为将来的项目中，我们将使用此功能，它可以节省我们周中循环访问数据属性来查找正确的功能用于建模集。  

下一步我们计划我们的实验比较性能中包括其他算法与[贝叶斯][贝叶斯线性回归]或[提升决策树][提升决策树回归]一样。    

如果您想要试验回归，完好的数据集，可以尝试将是能源效率回归示例数据集，包含多个数值特性。 数据集提供 ML Studio 中的示例数据集的一部分。  可以使用各种学习模块加热负载或冷却负载进行预测。  下面的图表是根据目标变量冷却负载能效数据集预测学习不同回归的性能比较︰ 

|模型|意味着绝对的错误|根平均平方错误|相对绝对错误|相对平方错误|确定系数
|---|---|---|---|---|---
|提升的决策树|0.930113|1.4239|0.106647|0.021662|0.978338
|线性回归 （梯度下降）|2.035693|2.98006|0.233414|0.094881|0.905119
|神经网络回归|1.548195|2.114617|0.177517|0.047774|0.952226
|线性回归 （普通最小二乘法）|1.428273|1.984461|0.163767|0.042074|0.957926  

## 记住的要点 

我们从中学到了很多由正在运行的 Excel 回归和并行试验 Azure 机器学习。 创建基准模型 Excel 和与使用 Azure ML[线性回归]模型中[线性回归]有助于我们了解 Azure ML，我们发现，改进数据的选择和模型性能的机会。         

我们还发现，最好使用[基于筛选器的功能选择][筛选器基于-功能-选择]加快未来预测的项目。  通过对数据应用功能选择，可以在 Azure ML 创建一个改进的模型，具有更好的整体性能。 

转移预测分析预测从 Azure ML 到 Excel 共同的能力允许成功地广泛的商业用户受众提供结果的能力大大提高。     


## 资源
帮助您使用回归分析列出了一些资源︰  

* 在 Excel 中的回归。  如果您已经永远不会尝试在 Excel 中的回归，本教程很容易︰ [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* 回归分析与预测。  Tyler Chessman 写了博客文章解释如何进行时间序列预测在 Excel 中，其中包含良好的初级描述的线性回归。 [http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   普通最小二乘法进行线性回归︰ 缺陷、 问题和缺陷。  介绍和讨论的回归︰ [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[贝叶斯线性回归]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[放大的决策树回归]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[-基于-功能-选定内容筛选]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[线性回归]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[项目列]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[拆分]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 

测试
