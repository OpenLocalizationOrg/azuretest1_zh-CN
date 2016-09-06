---
ms.openlocfilehash: 7447c07f9a7ebfdc155722f3e83a4eed0dad3509
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="创建、 编辑和对 DocumentDB 集合使用查询资源管理器中运行 SQL 查询 |Microsoft Azure" 
    description="了解 DocumentDB 查询浏览器预览 Azure 门户工具来创建、 编辑和对 DocumentDB 集合运行 SQL 查询。" 
    services="documentdb" 
    authors="stephbaron" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="06/10/2015" 
    ms.author="stbaro"/>

# 创建、 编辑和对 DocumentDB 集合使用查询资源管理器中运行 SQL 查询 #

这篇文章概括介绍了[Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/)查询浏览器 Microsoft Azure 预览门户工具，使您可以创建、 编辑和运行针对 DocumentDB 集合的查询。 

通过完成本教程，您将能够回答以下问题︰  

-   如何轻松地创建、 编辑，并通过 web 浏览器中运行查询对 DocumentDB 集合吗？
-   如何可以轻松地导航通过 DocumentDB 查询结果通过 web 浏览器的页？
-   如何解决与我的 DocumentDB 查询语法错误？ 

##<a id="Launch"></a>启动和导航查询资源管理器##

可以从任何的 DocumentDB 帐户、 数据库和集合刀片式服务器启动查询资源管理器。
  
1. 顶部的 DocumentDB 帐户或数据库刀片式服务器，只需单击**查询资源管理器**命令。

    ![命令查询资源管理器的屏幕抓图](./media/documentdb-query-collections-query-explorer/queryexplorercommand.png)

2. 或者，每个刀片底部附近是**开发人员工具**镜头，其中包含**查询资源管理器**图块。
    
    ![资源管理器屏幕截图的查询部分](./media/documentdb-query-collections-query-explorer/queryexplorerpart.png) 

2. 只需单击该图块可以启动查询浏览器。

    **数据库**和**集合**下拉列表框预先填充您启动查询资源管理器的上下文中。  例如，如果您从数据库刀片式服务器启动，则当前数据库是预先填充。 如果您从集合刀片式服务器启动，则预填充的当前集合。

    ![查询资源管理器的屏幕抓图](./media/documentdb-query-collections-query-explorer/queryexplorerinitial.png)

##<a id="Create"></a>创建、 编辑和查询资源管理器中运行查询##

查询资源管理器使您可以轻松地创建、 编辑和对 DocumentDB 集合，运行查询，包括基本的关键字和值突出显示，以提高创作体验的查询。  

- 当提供最初从 c 打开查询资源管理器中，选择*的默认查询。 您可以接受默认的查询或构建自己的控件，然后单击**运行查询** 按钮以查看结果。 查询浏览器支持 DocumentDB SQL 查询语言[查询 DocumentDB](documentdb-sql-query.md)中所述。

    ![资源管理器屏幕截图的查询的查询结果](./media/documentdb-query-collections-query-explorer/queryresults1.png) 

- 您可以输入多个查询、 突出显示您想要运行，然后单击**运行查询**按钮查看结果。

    ![突出显示，然后运行查询资源管理器的屏幕抓图](./media/documentdb-query-collections-query-explorer/queryexplorerhighlightandrun.png) 

- 您可以加载**加载文件**命令使用现有文件的内容。

    ![查询负载文件资源管理器的屏幕抓图](./media/documentdb-query-collections-query-explorer/loadqueryfile.png) 

- 默认情况下，查询资源管理器中的 100 集返回结果。  如果您的查询将产生 100 多个结果，只需使用**下一页**和**上一页**命令浏览结果集。

    ![资源管理器屏幕截图的查询分页支持](./media/documentdb-query-collections-query-explorer/queryresultspagination.png)

- 成功的查询提供了信息，例如申请费用，当前所显示的结果集，是否有更多的结果，则可以访问为**下一页**命令，通过上文所述。

    ![资源管理器屏幕截图的查询查询信息](./media/documentdb-query-collections-query-explorer/queryinformation.png)

- 同样，如果查询完成时有错误，查询资源管理器中将显示可以帮助进行故障排除工作的错误的列表。

    ![资源管理器屏幕截图的查询查询错误](./media/documentdb-query-collections-query-explorer/queryerror.png)

##<a name="NextSteps"></a>下一步行动

- 若要了解有关 DocumentDB 的详细信息，请单击[此处](http://azure.com/docdb)。
- 若要了解有关 DocumentDB SQL 语法查询浏览器支持的详细信息，请单击[此处](documentdb-sql-query.md)。
 

测试
