---
ms.openlocfilehash: 1126cc4ee98e996854b2963cc4c3769f3e97b2a3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="机器学习在 Azure 上是什么？ |Microsoft Azure"
    description="解释完全管理机器学习服务，可用于创建、 实施，以及他们的解决方案的云技术的基本的概念。"
    keywords="什么是机器学习，云技术预测的什么是预测分析、 实施"
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="neerajkh"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2015"
    ms.author="cgronlun;tedway;olgali"/>


# 机器学习在 Microsoft Azure 上简介

## 机器学习是什么？

机器学习使用计算机运行从现有数据了解的预测模型，以预测未来的行为、 成果、 和趋势。

这些预测或从机器学习的预测可以使应用程序和设备更加智能化。 当您在线购物时，机器学习有助于建议根据您已购买其他产品可能也会喜欢。 当刷您的信用卡时，机器学习比较数据库的交易记录的交易记录，并帮助银行进行欺诈检测。

## 机器学习在 Microsoft Azure 上是什么？

Azure 的机器学习是一种功能强大的基于云的预测分析服务，就可以快速创建和部署预测模型作为分析的解决方案。

Azure 机器学习不仅提供了一些工具来建模预测分析，而且还提供了可用于发布已准备好使用 web 服务作为您预测模型完全管理的服务。 Azure 的机器学习在云中创建完整的预测分析的解决方案提供了工具︰ 快速创建、 测试、 实施，和管理预测模型。 不需要购买任何硬件，也不能手动管理的虚拟机。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 预测分析是什么？

预测分析使用各种统计技术，在此情况下，机器学习的模式或趋势的收集或当前数据进行分析来预测未来的事件。

Azure 的机器学习是预测分析尤其是功能强大的方法︰ 您可以随时可用的算法库中，而无需购买其他设备或基础设施，互联网连接的 PC 上创建模型和快速部署预防性解决方案。 [机器学习库](http://gallery.azureml.net/)的[Microsoft Azure 市场](https://datamarket.azure.com/browse?query=machine+learning)中还可以找到现成的示例和解决方案。

## 生成完整的机器学习中云解决方案

Azure 的机器学习的一切您需要创建大算法库中时，从云到摄影棚的构建模型，对部署为 web 服务模型的简便方法在预测分析的解决方案。

### 机器学习 Studio︰ 创建预测模型

通过拖动、 除去，并连接模块，在[机器学习 Studio](machine-learning-what-is-ml-studio.md)中，基于浏览器的工具，创建预测模型。

![什么是预测分析︰ 在 Azure 机器学习 Studio 在预测分析实验示例](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

* 使用大型[机器学习算法和模块](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7)中机器学习 Studio 库迅速开始您的预测模型。 从样本试验、 R 和 Python 包和 Microsoft 的业务，如 Xbox 和 Bing 的同类最佳的算法库中选择。 将扩展与您自己的自定义[R](machine-learning-r-quickstart.md)和[Python](machine-learning-execute-python-scripts.md)脚本 Studio 模块。
* 在[机器学习社区库](machine-learning-gallery-how-to-use-contribute-publish.md)中，您可以开始使用 Azure 机器学习和社区学习他人。 尝试由其他人创作的实验、 提出问题或发表评论有关的实验，或者发布自己的实验。 您还可以共享的实验通过 LinkedIn 和 Twitter 等社交网络的链接。  

    ![请尝试预测实验样本或参与自己在 Azure 机器学习库](./media/machine-learning-what-is-machine-learning/azure-machine-learning-gallery-resources.png)

### 实施预测分析的解决方案︰ 购买 web 服务或发布您自己

* 从[Microsoft Azure 市场](https://datamarket.azure.com/browse?query=machine+learning)，如建议、 文本分析和异常检测中购买准备使用 web 服务。

* 实施预测分析模型︰
    * [发布 web 服务](machine-learning-publish-a-machine-learning-web-service.md)
    * [培训和重新培训通过 Api 模型](machine-learning-retrain-models-programmatically.md)
    * [管理 web 服务终结点](machine-learning-create-endpoint.md)
    * [扩展 web 服务](machine-learning-scaling-endpoints.md)
    * [使用 web 服务](machine-learning-consume-web-services.md)

## 关键的机器学习的术语和概念
### 数据研究、 描述性分析和预测分析

**数据开采**是收集大和经常非结构化数据集的顺序有关的信息的进程查找重点分析特征。 **数据挖掘**是指自动化的数据研究。

**描述性分析**是为了总结怎么分析数据集的过程。 绝大多数的业务分析-如销售报表、 web 标准和社会网络分析的进行描述。

**预测分析**是为了预测未来的结果构建从历史或当前的数据模型的过程。


### 监察和监督学习
 已标记的数据与经过培训的**Supervised 学习**算法-换句话说，数据组成的想要的答案的示例。 例如，标识伪造信用卡使用的模型将数据集中的数据标记点表示已知的虚假和无效费用接受培训。 大多数机器学习被监督。

 **Unsupervised 学习**使用不带标签，数据，目的是查找数据中的关系。 例如，您可以查找客户人口统计数据类似的购买习惯的分组。

### 模型的培训和评估
机器学习模型是一种抽象的想要回答的问题或需要进行预测的结果。 模型是培训，根据现有数据计算。

#### 从数据的培训
在 Azure 机器学习，从培训数据处理算法模块和功能模块，如评分模块生成模型。

在监察学习，如果您正在培训欺诈检测模型，您将使用一组交易记录标记为欺诈或有效。 将随机拆分数据集，并使用部分训练模型和测试或评估模型的一部分。

#### 评估数据
一旦经过培训的模型，评估使用剩余的测试数据的模型。 您使用的数据已经知道结果，以便您可以知道您的模型的预测是否准确。

### 其他常用的机器学习术语

* **算法**︰ 一套独立的规则用来解决问题，通过数据处理，计算，或自动推理。
* **分类数据**︰ 数据，按类别进行组织，并可分成组。 例如自动分类数据集可指定年、 制造商、 型号和价格。
* **分类**︰ 分为组织数据点的模型基于分组已经知道哪些类别的一组数据。
* **工程功能**︰ 提取或选择功能的过程相关的数据集，以增强数据集和改进成果。 例如，飞机票价数据可以增强按周和假日的天数。 请参阅[功能选择和 Azure 机器学习中的工程](machine-learning-feature-selection-and-engineering.md)。
* **模块**︰ 机器学习 Studio 模型，如使输入和编辑数据集较小的输入数据模块中的功能元素。 算法也是一种在 Studio 中机器学习模块。
* **模型**︰ 模型的管理学习，是学习训练数据集、 算法模块和功能模块，如分数模型模块组成，试验机的产品。
* **数字数据**︰ 含义与度量 （连续数据） 或计算 （离散数据） 的数据。 也称作*定量数据*。
* **分区**︰ 通过它您可以将数据划分为样本的方法。 有关详细信息，请参阅[分区和示例](https://msdn.microsoft.com/library/azure/dn905960.aspx)。
* **预测**︰ 预测是一个值或值为机器学习模型的预测。 您还可能看到术语"预期的成绩";但是，预测的成绩不是一种模型的最终输出。 模型计算出分数。
* **回归分析**︰ 对于预测基于自变量，例如预测的汽车价格的连续值模型根据其年度并进行。
* **成绩**︰ 从培训分类或回归模型，在机器学习 Studio 中使用[分数模型模块](https://msdn.microsoft.com/library/azure/dn905995.aspx)生成的预测的值。 分类模型也返回的概率预测值的得分。 一旦已经从模型生成分数，您可以评估使用[评估模型模块](https://msdn.microsoft.com/library/azure/dn905915.aspx)模型的准确性。
* **示例**︰ 数据集的一部分应是整体的代表。 示例可以随机选择，或根据特定数据集的功能。



## 下一步行动
您可以了解基本的预测分析和机器学习使用[步骤详细的教程](machine-learning-create-experiment.md)和样本上进行[构建](machine-learning-sample-experiments.md)。  


<!-- Module References -->
[学习与计数]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/

测试
