---
ms.openlocfilehash: 16a61fbaba78068bd484ea8bdace02e59761d90e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="与 SQL 数据仓库使用电源 BI |Microsoft Azure"
   description="双电源使用 Azure SQL 数据仓库开发解决方案的提示。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/22/2015"
   ms.author="lodipalm"/>

# 与 SQL 数据仓库中使用双电源
作为与 Azure 的 SQL 数据库，SQL 数据仓库直接连接允许用户利用旁边的电源双向分析功能的强大逻辑下推。  直接连接，请使用查询发送回 Azure SQL 数据仓库实时了解如何将数据。  这样，结合 SQL 数据仓库，使用户能够在几分钟内对数万亿字节的数据创建动态报表的缩放比例。  此外中双电源按钮, 打开的简介使用户直接连接到其 SQL 数据仓库不收集信息，从 Azure 的其他部分电源 BI。 

使用时直接连接请注意︰ 

+ 连接 （请参阅下面的详细信息） 时，请指定完全限定的服务器名
+ 确保数据库的防火墙规则配置为"允许访问到 Azure 服务"。
+ 每个操作，例如选择一列或添加筛选器将直接查询数据仓库 
+ 拼贴被刷新大约每隔 15 分钟 （刷新不需要安排）
+ 问与答不能用于直接连接的数据集
+ 架构更改不会自动拾取

这些限制和说明可能发生变化的随着我们继续改进经验。 下面详细列出连接的步骤。  

## 使用电源 BI 中打开按钮
SQL 数据仓库和电源 BI 之间移动的最简单方法是打开电源双按钮中。 此按钮允许您可以无缝地开始在电源 BI 中创建新的仪表板。  

1.  若要开始导航到您在 Azure 门户中的 SQL 数据仓库实例。
2.  单击 [打开电源双按钮中。
3.  如果我们不能直接登录您或您不具有电源 BI 帐户，您需要登录。  
4.  将引导您进入 SQL 数据仓库连接页中您预填充的 SQL 数据仓库的信息。
5.  输入您的凭据后您将完全连接到您的 SQL 数据仓库。 

## 通过电源 BI 门户连接
除了使用双电源按钮中的打开，用户还可以连接到电源 BI 门户通过其 SQL 数据仓库。 

1.   单击导航窗格底部的获取数据。
2.  大数据和更多选择。
3.  一次在大的数据和更多的页面，选择 SQL 数据仓库
4.  输入所需的连接信息。  下面一节显示可在其中找到此数据中查找参数。  
5.  钻取到数据集，您可以浏览所有的表和列的数据库中。 选择一列将发送回源，在您可视化动态创建查询。 这些视觉效果可以在新报表中，保存并重新固定到您的仪表板。

## 查找参数值
Azure 门户网站中，可以找到您完全限定的服务器名称和数据库名称。  请注意，SQL 数据仓库只存在在 Azure 预览门户这次。

## 下一步行动
集成的概述，请参阅[SQL 数据仓库集成概述][]。
开发的更多提示，请参阅[SQL 数据仓库开发概述][]。

<!--Image references-->

<!--Article references-->
[SQL 数据仓库开发概述]:  ./sql-data-warehouse-overview-develop/
[SQL 数据仓库集成概述]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->


