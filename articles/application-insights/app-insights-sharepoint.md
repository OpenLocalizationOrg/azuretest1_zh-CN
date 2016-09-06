---
ms.openlocfilehash: 71dcc7f7b520d62013dec73e995f303ac6d8b5fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="监视应用程序的见解与 SharePoint 网站" 
    description="开始监视新的应用程序，使用新的检测密钥" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/13/2015" 
    ms.author="awills"/>

# 监视应用程序的见解与 SharePoint 网站


[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Visual Studio 应用程序理解监控可用性、 性能和应用程序的使用情况。 在此处，您将学习如何设置 SharePoint 网站。


## 创建应用程序的见解资源


在[Azure 门户](http://portal.azure.com)，创建新的应用程序理解资源。 作为应用程序类型，选择 ASP.NET。

![单击属性，选择的注册表项，然后按 ctrl + C](./media/app-insights-sharepoint/01-new.png)


打开刀片式服务器都将有关您的应用程序看到的性能和使用情况数据。 若要返回到它下次您登录到 Azure，您应该开始在屏幕上发现它平铺。 或者单击浏览进行查找。
    


## 将脚本添加到您的 web 页

在快速启动获取 web 页的脚本︰

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

之前插入脚本&lt;头/&gt;想要跟踪的每一页的标记。 如果您的网站具有一个主页，您可以将脚本放那里。 例如，在 ASP.NET MVC 项目中，您将它置于 View\Shared\_Layout.cshtml

该脚本包含的检测项，将定向到您的应用程序理解资源的遥测数据。

### 将代码添加到您的站点网页

#### 在母版页上

如果您可以编辑该网站的主页上，将提供监视站点中每一页。

签出该母版页和使用 SharePoint 设计器或任何其他编辑器编辑它。

![](./media/app-insights-sharepoint/03-master.png)


添加代码之前</head>标记。 


![](./media/app-insights-sharepoint/04-code.png)

#### 或在单个页面上

若要监视一组有限的页面，每个页面分别添加的脚本。 

插入 web 部件和嵌入的代码段。


![](./media/app-insights-sharepoint/05-page.png)


## 查看有关您的应用程序的数据

重新部署您的应用程序。

返回到您应用程序刀片在[Azure 的门户](http://portal.azure.com)。

第一个事件将出现在诊断搜索。 

![](./media/app-insights-sharepoint/09-search.png)

如果您期待更多的数据，请单击刷新后几秒钟。

**使用率分析**链接到用户、 会话和网页视图的图表︰

![](./media/app-insights-sharepoint/06-usage.png)

例如，单击页面视图以查看更多详细信息︰ 

![](./media/app-insights-sharepoint/07-pages.png)

新用户和它们的位置的详细信息，请参阅用户通过单击。


![](./media/app-insights-sharepoint/08-users.png)



## 下一步行动

* [Web 测试](app-insights-monitor-web-app-availability.md)，以监视站点的可用性。

* 对于其他类型的应用程序的[应用程序的见解](app-insights-get-started.md)。



<!--Link references-->


 
测试
