---
ms.openlocfilehash: 0d420b0a957b69975458aeb4ab1c08a894cf00be
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="查看电源 BI 应用程序理解数据" 
    description="双电源用于监视的性能和应用程序的使用情况。" 
    services="application-insights" 
    documentationCenter=""
    authors="noamben" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="awills"/>
 
# 电源 BI 数据视图的应用程序的见解

[Microsoft 电源 BI](https://powerbi.microsoft.com/)提供数据丰富多变的视觉效果，在整合来自多个源的信息的能力。 您可以传输有关的性能和用法的网站或设备应用程序从应用程序的见解到电源 BI 的遥测数据。

![电源 BI 应用程序理解使用数据视图的示例](./media/app-insights-export-power-bi/010.png)

在本文中，我们将介绍如何从应用程序的见解中导出数据和使用流分析，将数据移动到电源双。 [流分析](http://azure.microsoft.com/services/stream-analytics/)是 Azure 服务，我们将使用适配器。

![电源 BI 应用程序理解使用数据视图的示例](./media/app-insights-export-power-bi/020.png)

## 视频

Noam Ben Zeev 显示我们在这篇文章中介绍。

> [AZURE.VIDEO export-to-power-bi-from-application-insights]

## 监视您的应用程序与应用程序的见解

如果您还没有尝试过它，现在是开始的时间。 应用程序的见解可以监视在范围广泛的平台，包括 Windows 和 iOS、 Android、 J2EE，等等任何设备或 web 应用程序。 [入门](app-insights-get-started.md)。

## 在 Azure 创建存储

连续出口始终将数据输出到 Azure 存储帐户，所以您需要首先创建存储区。

1. 在[Azure 门户网站](https://portal.azure.com)订阅中创建存储帐户。

    ![在 Azure 门户中，选择新建、 数据、 存储](./media/app-insights-export-power-bi/030.png)

2. 创建容器

    ![在新的存储中，选择容器，然后添加](./media/app-insights-export-power-bi/040.png)

3. 复制存储访问键

    您将需要它即将设置的输入流分析服务。

    ![在存储中，打开设置键，并需要一份主要的访问键](./media/app-insights-export-power-bi/045.png)

## 开始连续导出到 Azure 存储

[连续导出](app-insights-export-telemetry.md)将数据从应用程序深入 Azure 存储移动。

1. 在 Azure 的门户中，浏览到您为您的应用程序创建的应用程序理解资源。

    ![选择浏览应用程序的见解，您的应用程序](./media/app-insights-export-power-bi/050.png)

2. 创建的连续输出。

    ![选择设置，连续输出添加](./media/app-insights-export-power-bi/060.png)


    选择您在前面创建的存储帐户︰

    ![设置导出目标](./media/app-insights-export-power-bi/070.png)
    
    设置您想要查看的事件类型︰

    ![选择事件类型](./media/app-insights-export-power-bi/080.png)

3. 让积累一些数据。 休息一下，让人们使用您的应用程序一段时间。 遥测会，您将看到[公制的资源管理器](app-insights-metrics-explorer.md)中的统计图表和[诊断搜索](app-insights-diagnostic-search.md)中的单个事件。 

    同时，将数据导出到您的存储。 

4. 检查导出的数据。 在 Visual Studio 中，选择**查看 / 云资源管理器**，并打开 Azure / 存储。 (如果您没有此菜单选项，您需要安装 Azure SDK︰ 打开新建项目对话框，并打开 C# / 云 /.net 获取 Microsoft Azure SDK。)

    ![](./media/app-insights-export-power-bi/04-data.png)

    记下从应用程序名称和检测密钥派生的路径名的公共部分。 

事件会写入到 blob 以 JSON 格式的文件。 每个文件可以包含一个或多个事件。 因此我们想要读取事件数据，筛选出我们想要的字段。 所有类型的数据，我们可以做的事，但今天我们的计划是使用流分析管道到电源 BI 数据。

## 创建 Azure 流分析实例

从[经典的 Azure 门户](https://manage.windowsazure.com/)，选择 Azure 流分析服务，并创建新的流分析作业︰


![](./media/app-insights-export-power-bi/090.png)



![](./media/app-insights-export-power-bi/100.png)

创建新作业时，展开其详细信息︰

![](./media/app-insights-export-power-bi/110.png)


#### 设置斑点位置

将其设置为使您连续导出 blob 中的输入︰

![](./media/app-insights-export-power-bi/120.png)

现在，您将从您的存储帐户，前面提到需要为主的访问键。 此设置为存储帐户密码。

![](./media/app-insights-export-power-bi/130.png)

#### 设置路径的前缀模式 

![](./media/app-insights-export-power-bi/140.png)


请务必设置日期格式 YYYY MM DD 到 （包括短划线）。

该路径的前缀模式指定从何处流分析的存储区中查找的输入的文件。 您需要将其设置为对应于连续导出将数据的存储。 将其设置如下︰

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

在此示例中︰

* `webapplication27` 是应用程序的见解资源**全部小写**的名称。
* `1234...` 检测至关重要的见解应用程序资源，**省略虚线**。 
* `PageViews` 是您想要分析的数据的类型。 可用的类型取决于在连续导出设置的筛选器。 检查以查看其他可用类型的导出的数据，请参阅[导出数据模型](app-insights-export-data-model.md)。
* `/{date}/{time}` 一种模式编写按其原义。

> [AZURE.NOTE] 检查以确保确保路径正确的存储。

#### 完成初始设置

确认的序列化格式︰

![确认并关闭向导](./media/app-insights-export-power-bi/150.png)

关闭该向导，并等待安装程序以完成。

## 将输出设置

现在选择的作业，并将输出设置。

![选择新的通道，请单击输出、 添加、 双电源](./media/app-insights-export-power-bi/160.png)

授权流分析来访问您的电源双资源，然后创建输出，以及目标电源 BI 数据集和表的名称。

![发明三个姓名](./media/app-insights-export-power-bi/170.png)

## 将查询设置

查询管理从输入到输出的转换。

![选择该作业并单击查询。 将粘贴下面的示例。](./media/app-insights-export-power-bi/180.png)

粘贴此查询︰

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* 导出输入是我们赋予输入流的别名
* pbi 输出是我们定义的输出别名
* 我们使用 GetElements，因为在嵌套的 JSON arrray 是该事件的名称。 然后选择将选取事件名称，以及使用该名称在该时间段的实例数的计数。 Group By 子句的元素分组到 1 分钟的时间段。

## 运行作业

在过去，若要启动的作业，可以选择一个日期。 

![选择该作业并单击查询。 将粘贴下面的示例。](./media/app-insights-export-power-bi/190.png)

等待，直到该作业运行。

## 在电源 BI 查看结果

打开电源 BI 和选择数据集和表定义为流分析作业的输出。

![在电源双向选择您的数据集和字段。](./media/app-insights-export-power-bi/200.png)

现在您可以使用此报表中的数据集和[双电源](https://powerbi.microsoft.com)的仪表板。


![在电源双向选择您的数据集和字段。](./media/app-insights-export-power-bi/210.png)

## 视频

Noam Ben Zeev 演示如何将导出到电源 BI。

> [AZURE.VIDEO export-to-power-bi-from-application-insights]

## 相关的资料

* [连续的导出](app-insights-export-telemetry.md)
* [详细的数据模型的属性类型和属性值的引用。](app-insights-export-data-model.md)
* [应用程序的见解](app-insights-overview.md)
* [更多示例和演练](app-insights-code-samples.md)

测试
