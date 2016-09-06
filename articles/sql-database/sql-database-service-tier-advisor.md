---
ms.openlocfilehash: 97f1bad9a38f1820dc05d573ed40c90f62b8f885
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="定价为 SQL Azure 数据库层建议" 
   description="更改定价时层在 Azure 门户中，定价层的建议将提供建议，最好的层适合运行现有的 Azure SQL 数据库的工作负荷。" 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="06/30/2015"
   ms.author="sstein"/>

# SQL 数据库定价层建议

 更改定价时层在 Azure 门户中，定价层的建议将提供建议，最好的层适合运行现有的 Azure SQL 数据库的工作负荷。

> [AZURE.NOTE] 定价层建议仅适用于 Web 和业务数据库，并仅在[Azure 门户网站](https://portal.azure.com/)中提供。


## 概述

Azure 分析当前性能和功能要求为 SQL 数据库评估历史资源使用情况。 此外，基于数据库和启用的[业务连续性](https://msdn.microsoft.com/library/azure/hh852669.aspx)功能的大小确定最小可接受的服务层。 

对此信息进行分析和最好的服务层和性能级别适用于运行数据库的典型工作负荷，维护它当前功能集建议。

- 服务检查历史数据 （资源使用情况、 数据库大小以及数据库活动） 的前 15 至 30 天内，并占用的资源数量和当前可用的服务层和性能级别的实际局限性之间执行比较。
- 在 15 秒为间隔中分析数据结果集中每个间隔分为现有服务层和性能级别最适合处理该结果集的工作负载。
- 这些 15 秒再聚合到较大的 15-30 天分析示例，建议可以以最佳方式处理历史负荷的 95%的服务层和性能级别。

### 建议

基于数据库的使用情况，当前有的建议中可能遇到的两个类别︰


| 建议 | 说明 |
| :--- | :--- |
| 升级 | 升级到一个新层。 |
| 不可用 | 数据库需要的最小工作负荷或大约 14 天的活动。 没有足够的数据来提供有效的建议。 |

## 获取定价层的建议

获取通过选择现有 Web 或企业数据库并单击**定价层**铺上定价层的建议。

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 在左侧的菜单中，单击**浏览**。
3. 单击**浏览**刀片式服务器中的**SQL 数据库**。
4. 在**SQL 数据库**刀片式服务器，请单击数据库所需的服务进行分析。

    ![选择数据库][1]

5. 在数据库刀片式服务器，选择**定价层**平铺。

    ![定价层][2]


7. 单击**定价层**平铺后您将会看到**推荐定价层**刀片位置可以单击建议的层，然后单击**选择**按钮更改为这一层。

    ![注册的预览][4]

8. （可选） 单击**查看使用率详细信息**打开**定价层建议详细信息**刀片式服务器，您可以在其中查看数据库的建议的层、 当前和建议的分层，并分析历史资源使用情况的图形之间的功能比较。

    ![注册的预览][5]



## 摘要

定价层建议提供自动收集的每个 SQL 数据库的遥测数据和建议的最佳的服务层中的性能级别组合基于数据库的实际性能需求和功能需求的体验。 单击以查看定价层建议数据库刀片式服务器上的**定价层**平铺。



## 下一步行动

这取决于您的特定数据库的详细信息，通常执行升级或降级不会发生在瞬间完成。 管理门户会将数据库转变为它的新层，作为提供通知，或者您可以通过查询 SQL 数据库服务器的 master 数据库中的[sys.dm_operation_status （在 SQL Azure 的数据库中）](https://msdn.microsoft.com/library/dn270022.aspx)视图来监视升级状态。


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[2]: ./media/sql-database-service-tier-advisor/pricing-tier.png
[3]: ./media/sql-database-service-tier-advisor/preview-sign-up.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 