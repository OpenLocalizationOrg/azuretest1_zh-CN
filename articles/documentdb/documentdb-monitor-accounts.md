---
ms.openlocfilehash: 831cb793d4511b5f061904bcfcd52bdbd47d8dc5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="监视使用 Azure 预览门户 DocumentDB 帐户 |Microsoft Azure" 
    description="了解如何监视您的 DocumentDB 帐户为性能指标，如请求和服务器错误和使用情况指标，如存储消耗量。" 
    services="documentdb" 
    documentationCenter="" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="mimig"/>

# 监视器使用 Azure 预览门户的 DocumentDB 帐户 

您可以监视您的 DocumentDB 帐户在[Microsoft Azure 预览门户](https://portal.azure.com/)。 对于 DocumentDB 的每个帐户，提供了这两个性能指标，如请求和服务器错误和使用情况指标，如存储消耗量。

## 如何︰ 查看 DocumentDB 帐户的性能指标
1.  在[Azure 预览门户](https://portal.azure.com/)，单击**浏览所有** **DocumentDB 帐户**，然后都单击要查看的性能指标的 DocumentDB 帐户的名称。
2.  在**监视**镜头可以在默认情况下，请参阅︰
    *   当天的请求总数。
    *   平均每秒请求数的当前日期。 
    
    ![监视镜头屏幕抓图](./media/documentdb-monitor-accounts/madocdb1.png)


3.  **总请求数**或**每秒的平均请求**部分单击打开详细的**规格，**刀片式服务器。
4.  **跃点数**刀片式服务器显示有关所选的度量值的详细信息。  刀片式服务器的顶部是一个图形和下面的表格显示的选定指标如平均值、 最小值和最大值的聚合值。  公制刀片式服务器还会显示已定义，到出现在当前度量刀片式服务器的指标筛选警报的列表 （这种方式，如果您有大量的通知后，您将仅看到此处介绍相关的）。   

    ![公制刀片式服务器的屏幕截图](./media/documentdb-monitor-accounts/madocdb2.png)


## 自定义性能指标 DocumentDB 帐户视图

1.  要自定义显示在某一特定部分的指标，该指标的图表中，用鼠标右键单击，然后选择**编辑图表**。  
    ![使用编辑图表按钮突出显示的总请求数图表的屏幕抓图](./media/documentdb-monitor-accounts/madocdb3.png)

2.  在**编辑图表**刀片式服务器，有选项来修改显示在部件，以及它们的时间范围内的度量标准。  
    ![编辑图表刀片式服务器的屏幕抓图](./media/documentdb-monitor-accounts/madocdb4.png)

3.  若要更改显示在部件中的指标，只需选择或清除的可用的性能指标，然后单击底部的刀片式服务器**保存**。  
4.  要更改的时间范围，选择不同的范围 （例如，**自定义**）、，然后单击刀片底部的**保存**。  

    ![编辑图表刀片式服务器显示如何输入自定义时间范围的时间范围内部分的屏幕抓图](./media/documentdb-monitor-accounts/madocdb5.png) 


## 创建通过并行性能指标的图表
Azure 预览门户允许您创建并排比较指标的图表。  

1.  首先，请右键单击您要复制和修改，并选择**自定义**的图表。 

    ![总请求数图表的自定义选择突出显示的屏幕抓图](./media/documentdb-monitor-accounts/madocdb6.png)

2.  单击要复制的部件，然后单击**完成自定义**的菜单上的**克隆**。 

    ![屏幕快照与克隆的总请求数图表并执行突出显示的自定义选项](./media/documentdb-monitor-accounts/madocdb7.png)  


您可能现在将这一部分视为任何其他指标的部分，自定义显示在部件中的指标和时间范围。  通过执行此操作，您可以同时看到两个不同的指标图表通过并行。  
    ![总请求数图表和新的请求总数超过小时图表的屏幕抓图](./media/documentdb-monitor-accounts/madocdb8.png)  

## 查看使用情况指标为 DocumentDB 帐户的
1.  在[Azure 预览门户](https://portal.azure.com/)，单击**浏览** **DocumentDB 帐户**，然后单击要查看使用情况指标的 DocumentDB 帐户的名称。
2.  **使用**镜头中可以查看默认情况下列︰
    *   估计到目前的 DocumentDB 帐户当前记帐期间的开销。
    *   使用帐户内的存储。
    *   最大可用存储帐户 （阈值）。
    *   用户和权限的使用情况。
    *   附件使用。

    ![使用镜头的屏幕抓图](./media/documentdb-monitor-accounts/madocdb9.png)
 
## 设置性能指标预警为 DocumentDB 帐户
1.  在[Azure 预览门户](https://portal.azure.com/)，单击**浏览所有** **DocumentDB 帐户**，然后都单击您要为其设置性能指标预警的 DocumentDB 帐户的名称。
2.  在**操作**镜头，单击**预警规则**的一部分。  
    ![所选操作镜头，预警规则部分的屏幕抓图](./media/documentdb-monitor-accounts/madocdb10.png)

3.  在预警规则刀片式服务器，单击**添加通知**。  
    ![预警规则刀片式服务器，使用添加通知按钮突出显示的屏幕抓图](./media/documentdb-monitor-accounts/madocdb11.png)

4.  在**预警规则中添加**刀片式服务器，请指定︰
    *   您设置的预警规则的名称。
    *   新的预警规则的描述。
    *   指标预警规则。
    *   确定当警报激活条件、 阈值和时间段。 例如，在最后 15 分钟内服务器错误计数大于 5。
    *   是否服务管理员和 coadministrators 是通过电子邮件发送时将触发警报。
    *   警报通知的其他电子邮件地址。  
    ![添加的警报规则刀片的屏幕快照](./media/documentdb-monitor-accounts/madocdb12.png)

## 下一步行动
若要了解有关 DocumentDB 容量的详细信息，请参阅[管理 DocumentDB 容量](documentdb-manage.md)。 
 
测试
