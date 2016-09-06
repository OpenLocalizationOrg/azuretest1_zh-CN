---
ms.openlocfilehash: c4084cf5a900ca054fa6d8bda070aedae440b497
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="多地区帮助文档 |Microsoft Azure"
   description="了解如何创建一个工作区并发布 Azure 的南部中心美国 (SCUS) 从不同区域中的 web 服务 Azure 的地区。"
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="paulettm"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2015"
   ms.author="tedway; neerajkh"/>

# 多地区帮助文档

目前，Azure 机器学习的所有资产都位于南部中心美国 (SCUS) 在 Azure 的地区。 本文介绍了如何创建一个工作区，并在其他 Azure 地区发布 web 服务。

## 创建工作区

1. 登录到 Azure 的管理门户。

2.  单击**新建 +** > **数据服务** > **机器学习** > **快速创建**。  在**位置**下选择另一个区域，例如**东南亚**。
![多地区帮助映像 1][1]
3. 选择的工作区，然后单击**登录到 ML Studio**。
![多地区帮助图像 2][2]

4. 就像任何其他工作区，您可以使用的另一个地区现在有一个工作区。 若要在您的工作区之间切换，查看您的屏幕的右上角。 单击下拉列表，选择该区域，然后选择的工作区。 一切都是本地的工作区区域中。例如，所有从工作区中创建 web 服务都将在该工作区位于同一区域。
![多地区帮助图像 3][3]

## 从库中打开一个实验

如果从库中打开一个实验，还可以选择您想复制到实验的地区。

![多地区帮助图 4][4a]

## Web 服务管理

若要以编程方式管理 web 服务，例如对人员进行再培训，使用特定于区域的地址︰ **https://asiasoutheast.management.azureml.net**

### 需要注意的事项

1.  只能复制属于同一区域的工作区之间的试验。 将来，我们将允许跨多个地区的工作区之间复制实验。
2.  一次，区域选择器将只显示一个地区的工作区。 将来，您将能够查看您有权访问所有区域间在同一时间工作区的完整列表。  
3.  可用的工作区或来宾访问 （匿名） 工作区并将其承载于南中心美国将来，您将能够在您选择的区域中创建自由/来宾访问工作区。  
4.  从在东南亚区部署的 web 服务还将承载在东南亚。 将来，您将能够灵活地创建一个区域，在实验的和部署生成到不同区域的 web 服务终结点。  

## 详细信息

在[Azure 机器学习论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning)上提出问题。

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
测试t
