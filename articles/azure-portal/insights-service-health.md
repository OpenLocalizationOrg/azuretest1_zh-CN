---
ms.openlocfilehash: f13214b171510b100a231eae447c5afb90d0e08b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="跟踪服务运行状况" 
    description="了解何时 Azure 出现性能下降或服务中断。 " 
    authors="stepsic-microsoft-com" 
    manager="kamrani" 
    editor="" 
    services="azure-portal" 
    documentationCenter="na"/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/25/2015" 
    ms.author="stepsic"/>

# 跟踪服务运行状况

Azure publicizes 每次出现的服务中断或性能下降时。 您可以浏览在 Azure 的门户中，这些事件，您可以也使用[REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)以编程方式访问完整的事件集。

## 请参阅服务健全的地区

1. 登录到[Azure 的门户](https://portal.azure.com/)。

2. 在**主页**上，您应该看到图块调用**服务健康**
    ![主页](./media/insights-service-health/Insights_Home.png)

3. 单击该图块将 Azure 中的所有地区的列表。 您可以单击以查看该地区健康服务历史记录任何地区。
    ![主页 ](./media/insights-service-health/Insights_Regions.png)

4. 您还可以通过在表中单击，查看单个事件的详细信息。

## 浏览完整的服务运行状况日志

2. 单击**浏览**，然后选择**审核日志**。  
    ![浏览集线器](./media/insights-service-health/Insights_Browse.png)

3. 这将打开刀片式服务器显示的所有事件已过去 7 天影响任何您的订阅。 服务健康项目会显示在此列表中，但它可能很难查找列表可能很大。

4. 单击**筛选**命令。

5. 选择**事件类别**，然后选择**服务运行状况**︰ ![的所有事件](./media/insights-service-health/Insights_Filter.png)

6. 单击**更新**。

7. 现在您将看到所有过影响您的订阅服务运行状况事件︰ ![资源组](./media/insights-service-health/Insights_HealthEvent.png)

8. 从那里可以转到详细信息刀片式服务器以查看该事件的具体情况。
   
## 下一步行动

* 每当事件发生时，[接收警报通知](insights-receive-alert-notifications.md)。
* [监视服务标准](insights-how-to-customize-monitoring.md)，以确保您的服务是可用的并且能够作出响应。
* [显示器的可用性和响应能力的任何 web 页](../app-insights-monitor-web-app-availability.md)与应用程序使您可以找出如果您的页面的见解是向下。
 
测试
