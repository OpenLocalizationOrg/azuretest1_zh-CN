---
ms.openlocfilehash: 684262b5afe61efeed5fd6e61f9bfeebcb478d95
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="与 SQL 数据仓库使用 Azure 机器学习 |Microsoft Azure"
   description="开发解决方案与 Azure SQL 数据仓库使用 Azure 机器学习的教程。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/23/2015"
   ms.author="sahajs"/>

# 与 SQL 数据仓库使用 Azure 的机器学习

Azure 的机器学习是一个完全托管的预测分析服务可以使用在 SQL 数据仓库中，创建针对您的数据的预测模型并将其发布为准备使用 web 服务。 您可以了解预测分析和机器学习阅读[机器学习在 Azure 上介绍][]的基本知识。  然后，您可以学习如何创建、 训练、 成绩和测试使用[试验教程创建][]一个机器学习模型。

在本文中，您将学习如何执行以下操作使用[Azure 机器学习 Studio][]:
-   从数据库创建、 培训和成绩预测模型中读取数据 
-   向数据库中写入数据 


## 从 SQL 数据仓库中读取数据

我们将从 AdventureWorksDW 数据库中的产品表中读取数据。

### 第 1 步
通过单击来启动新实验 + 新建底部的机器学习 Studio 窗口中，选择试验，，然后选择空白试验。 选择默认实验画布顶部的名称并将其重命名为有意义的名称，例如，自行车价格预测。

### 第 2 步
查看数据集的组件面板中的读取器模块和模块在实验画布的左侧。 将模块拖到实验画布。
![][drag_reader]

### 第 3 步
选择读卡器模块，然后填写属性窗格
1. 选择 SQL Azure 数据库作为数据源
2. 数据库服务器名称︰ 键入服务器名称。 您可以使用[Azure 门户][]发现这。
![][server_name]
3. 数据库名称︰ 键入刚指定的服务器上的数据库的名称。 
4. 服务器用户帐户名称︰ 键入具有对数据库的访问权限的帐户的用户名。 
5. 服务器用户帐户密码︰ 提供指定的用户帐户的密码。
6.  接受任何服务器证书︰ 使用此选项 （较低安全级别），如果您想跳过读取数据之前查看该网站的证书。
7.  数据库查询︰ 输入描述您要读取的数据的 SQL 语句。 在这种情况下，我们将使用以下查询产品表中读取的数据。


```
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### 第 4 步
1. 通过实验画布下单击运行运行实验。
2. 当实验完成后时，读卡器模块将有一个绿色的复选标记，以指示已成功完成。 此外请注意已完成运行状态中的右上角。
![][run]
3. 若要查看导入的数据，请单击底部的汽车数据集的输出端口并选择可视化。

## 创建、 培训和评分模型
现在，您可以使用与此数据集︰
-   创建一个模型︰ 处理数据并定义功能
-   培训模型︰ 选择并应用一种学习算法
-   成绩和测试模型︰ 预测新自行车价格
    
![][model]

要了解有关如何创建，进行训练，成绩和测试机器学习模型使用[创建试验教程][]。

## 将数据写入到 Azure SQL 数据仓库

我们将编写结果集 ProductPriceForecast AdventureWorksDW 数据库中的表。

### 第 1 步
查看数据集的组件面板中的编写器模块和模块在实验画布的左侧。 将模块拖到实验画布。
![][drag_writer]

### 第 2 步
选择编写器模块，并填写属性窗格。
1. 选择 SQL Azure 数据库作为数据目标。
2. 数据库服务器名称︰ 键入服务器名称。 您可以使用[Azure 门户][]发现这。 
3. 数据库名称︰ 键入刚指定的服务器上的数据库的名称。 
4. 服务器用户帐户名称︰ 键入具有对数据库的写入权限的帐户的用户名。 
5. 服务器用户帐户密码︰ 提供指定的用户帐户的密码。
6. 接受服务器证书 （不安全）︰ 如果您不想查看的证书，请选择此选项。
7. 逗号分隔的列保存列表︰ 提供您想要输出的数据集或结果列的列表。
8. 数据表格名称︰ 指定的数据表的名称。
9. 数据表列的逗号分隔列表︰ 指定要使用新的表中的列名称。 列名称可以不同于源数据集中，但必须列出相同数量的列，则输出表中定义。
10. 每个 SQL Azure 操作写入的行数︰ 可以配置写入 SQL 数据库在一个操作中的行数。

![][writer_properties]

### 第 3 步
1. 通过实验画布下单击运行运行实验。
2. 当实验完成后时，所有模块将都有一个绿色的复选标记，以指示它们已成功完成。 



## 下一步行动
集成的概述，请参阅[SQL 数据仓库集成概述][]。
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->
[drag_reader]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[服务器名称]:./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[运行]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[模型]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]:./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png


<!--Article references-->

[SQL 数据仓库开发概述]:  ./sql-data-warehouse-overview-develop/
[SQL 数据仓库集成概述]:  ./sql-data-warehouse-overview-integration/
[创建实验教程]: ./machine-learning-create-experiment/
[机器学习在 Azure 上简介]: ./machine-learning-what-is-machine-learning/
[Azure 机器学习 Studio]: https://studio.azureml.net/Home
[Azure 门户]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->
[Azure 机器学习文档]: http://azure.microsoft.com/documentation/services/machine-learning/

