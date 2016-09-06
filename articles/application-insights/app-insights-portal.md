---
ms.openlocfilehash: b0f52cf613a39279073af07b7c5ea8b2574c69b6
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="使用应用程序的见解门户"
    description="使用率分析与应用程序理解的概述"
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="multiple"
    ms.topic="article" 
    ms.date="08/17/2015"
    ms.author="awills"/>

# 使用应用程序的见解门户

[设置应用程序在您的项目的见解](app-insights-get-started.md)后，有关您的应用程序的性能和使用情况的遥测数据将出现在[Azure 的门户](https://portal.azure.com)项目的应用程序理解资源中。

## 在 Azure 中查找您的遥测

登录到[Azure 的门户网站](https://portal.azure.com)，浏览到您为您的应用程序创建的应用程序理解资源。

![单击浏览，选择应用程序的见解，然后您的应用程序。](./media/app-insights-portal/00-start.png)

概述页使您一些基本遥测，再加上链接到更多。
内容取决于您的应用程序类型，并可以进行自定义。

## 应用程序概述刀片式服务器

您的应用程序概述刀片 （页） 显示最重要监视性能和使用情况的图表。 内容取决于您的应用程序的类型，并在任何情况下您可以自定义它。 


### 自定义概述刀片式服务器 

选择您想要查看在概览上。 在自定义，可以插入节标题、 拼贴和图表周围拖动、 移除项，并从库中添加新的图块和图表。

![单击"..."，自定义。 将拼贴和图表。 从库中添加平铺。 ](./media/app-insights-portal/020-customize.png)


## 创建您自己的指标的图表和网格

### 编辑图表和网格

刀片式服务器添加新的图表︰

![在测量数据资源管理器中，选择添加图表](./media/app-insights-metrics-explorer/04-add.png)

选择一种现有的或新的图表来编辑它的显示︰

![选择一个或多个度量标准](./media/app-insights-metrics-explorer/08-select.png)

虽然有的组合在一起可以显示有关的限制，您可以在图表中，显示多个指标。 只要选择一个标准衡量的其他部分将被禁用。 

如果编码到您的应用程序 （调用 TrackMetric 和 TrackEvent） 的[自定义度量](app-insights-api-custom-events-metrics.md#track-metric)它们将在此处列出。

### 把数据分割

选择一个图表或栅格、 分组交换机并选择分组依据的属性︰

![组在分组依据中选择一个属性，然后选择分组上，](./media/app-insights-metrics-explorer/15-segment.png)

如果编码到您的应用程序的自定义指标，它们包括[的属性值](app-insights-api-custom-events-metrics.md#properties)，可以在列表中选择属性。

为图表太小而无法分段的数据？ 调整窗体的高度︰

![调整滑块](./media/app-insights-metrics-explorer/18-height.png)

### 筛选数据

若要查看仅选定的一组属性值的指标︰

![单击筛选，展开属性，并检查某些值](./media/app-insights-metrics-explorer/19-filter.png)

如果未选择任何特定属性的值，它等同于选择全部︰ 在该属性上没有任何筛选器。

请注意每个属性值旁边的事件计数。 选择一个属性的值时，会调整及其他属性值的计数。

### 保存规格刀片

当您创建了一些图表时，请将它们保存为收藏。 如果您使用组织的帐户，可以选择是否与其他团队成员共享。

![选择收藏夹](./media/app-insights-metrics-explorer/21-favorite-save.png)

请参阅刀片式服务器，**请转到概述刀片式服务器**并打开收藏夹:

![在概述刀片式服务器，选择收藏夹](./media/app-insights-metrics-explorer/22-favorite-get.png)

如果您选择了相对的时间范围内，保存时，将使用的最新统计数据更新刀片式服务器。 如果您选择了绝对时间范围内，每次它都将显示相同的数据。

### 重置刀片式服务器

如果您编辑一个叶片，但再想要回到原始保存集，只需单击重置。

![在度量资源管理器顶部的按钮](./media/app-insights-metrics-explorer/17-reset.png)

## 创建搜索页

打开诊断搜索︰

![打开诊断搜索](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

打开刀片式服务器筛选器并选择您想要查看的事件类型。 （如果以后，您想要还原的打开刀片式服务器的筛选器，请单击重置。）

![选择筛选器，然后选择遥测类型](./media/app-insights-diagnostic-search/02-filter-req.png)

### 对属性值筛选

您可以筛选其属性的值的事件。 可用的属性取决于您选择的事件类型。 

例如，挑选出具有特定响应代码的请求。

![展开属性并选择一个值](./media/app-insights-diagnostic-search/03-response500.png)

选择某个特定的属性没有值具有相同的效果与选择所有值;它将关闭对该属性进行筛选。


### 缩小搜索范围

请注意右边的筛选器值的计数显示在当前筛选设置中会有多少处。 

在此示例中，它已清除`Reports/Employees`请求中大部分 500 错误的结果︰

![展开属性并选择一个值](./media/app-insights-diagnostic-search/04-failingReq.png)

另外如果希望，还看到其他的事件都发生在这段时间，您可以检查**包含未定义的属性的事件**。

### 保存搜索

设置所需的所有筛选器后，可以将搜索另存为收藏。 如果您工作中组织的帐户，可以选择是否要将其与其他团队成员共享。

![单击收藏、 设置名称，并单击保存](./media/app-insights-diagnostic-search/08-favorite-save.png)


请参阅搜索，**请转到概述刀片式服务器**并打开收藏夹:

![收藏夹平铺](./media/app-insights-diagnostic-search/09-favorite-get.png)

如果您保存具有相对的时间范围，重新打开刀片式服务器有最新的数据。 如果使用绝对的时间范围内保存，您看到的相同的数据每次。


测试
