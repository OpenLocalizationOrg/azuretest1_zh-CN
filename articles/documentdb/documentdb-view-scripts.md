---
ms.openlocfilehash: a9887c0c4338745d4122a1de450e65ce41322f68
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="查看存储的过程、 触发器和用户定义的函数使用 DocumentDB 脚本资源管理器 |Microsoft Azure"
    description="了解 DocumentDB 脚本资源管理器，Azure 预览门户工具来查看 DocumentDB 服务器端编程项目包括存储的过程、 触发器和用户定义的函数。"
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
    ms.topic="article" 
    ms.date="09/02/2015"
    ms.author="stbaro"/>

# 查看、 编辑和创建存储的过程、 触发器和用户定义的函数使用 DocumentDB 脚本资源管理器

这篇文章概括介绍了[Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/)脚本资源管理器，Azure 预览门户工具使您能够查看 DocumentDB 服务器端编程项目包括存储的过程、 触发器和用户定义的函数。  DocumentDB 服务器端编程[这里](documentdb-programming.md)阅读的详细信息。

通过完成本教程，您将能够回答以下问题︰  

-   如何轻松地查看通过 web 浏览器的 DocumentDB 存储过程？
-   如何轻松地查看 DocumentDB 触发器通过 web 浏览器？
-   如何轻松地查看 DocumentDB 通过 web 浏览器的用户定义函数？

## 启动并定位脚本资源管理器

可以从任何的 DocumentDB 帐户、 数据库和集合刀片式服务器启动脚本资源管理器。  

1. 顶部的 DocumentDB 帐户或数据库刀片式服务器，只需单击**脚本资源管理器**命令。

    ![脚本资源管理器命令的屏幕抓图](./media/documentdb-view-scripts/scriptexplorercommand.png)
 
2. 另外，每个刀片底部附近是包含**脚本资源管理器**部件的**开发人员工具**镜头。

    ![脚本资源管理器部件的屏幕抓图](./media/documentdb-view-scripts/scriptexplorerpart.png)

2. 只需单击命令或部分脚本资源管理器启动。

    <p>**数据库**和**集合**下拉列表框预先填充您启动脚本资源管理器的上下文中。  例如，如果您从数据库刀片式服务器启动，则当前数据库是预先填充。  如果您从集合刀片式服务器启动，则预填充的当前集合。

    ![脚本资源管理器的屏幕抓图](./media/documentdb-view-scripts/scriptexplorerinitial.png)


3. **数据库**和**集合**下拉列表框可以用于方便地更改从该脚本正在查看而无需关闭并重新启动脚本资源管理器的集合。  

4. 脚本资源管理器还支持通过其 id 属性筛选当前加载的脚本集。  只需在筛选器框中键入。

    ![屏幕截图的脚本资源管理器中突出显示的筛选器](./media/documentdb-view-scripts/scriptexplorerfilter.png)

    和脚本资源管理器中的对结果进行筛选基于您提供的条件。

    ![屏幕截图的脚本资源管理器与筛选结果](./media/documentdb-view-scripts/scriptexplorerfilterresults.png)


    > [AZURE.IMPORTANT] 脚本资源管理器从***当前***加载的脚本功能仅筛选器筛选并不会自动刷新当前所选的集合。

5. 若要刷新加载的脚本资源管理器中的脚本的列表，只需单击刀片式服务器顶部的**刷新**命令。

    ![屏幕截图的脚本资源管理器刷新命令](./media/documentdb-view-scripts/scriptexplorerrefresh.png)


## 查看、 编辑、 创建和删除的存储的过程、 触发器和用户定义的函数的脚本资源管理器

脚本资源管理器允许您轻松执行 CRUD 操作对 DocumentDB 服务器端编程项目。  

- 若要创建一个脚本，只需单击相应创建脚本资源管理器中的命令提供 id、 输入脚本的内容，单击**保存**命令。

    ![屏幕截图的脚本资源管理器创建选项](./media/documentdb-view-scripts/scriptexplorercreatecommand.png)

- 当创建一个触发器，则还必须指定触发器类型和触发器操作

    ![屏幕截图的脚本资源管理器中创建触发器选项](./media/documentdb-view-scripts/scriptexplorercreatetrigger.png)

- 若要查看一个脚本，只需单击您感兴趣的脚本。

    ![屏幕截图的脚本资源管理器查看脚本经验](./media/documentdb-view-scripts/scriptexplorerviewscript.png)

- 若要编辑脚本，只需使所需的更改并单击**保存**命令。

    ![屏幕截图的脚本资源管理器查看脚本经验](./media/documentdb-view-scripts/scriptexplorereditscript.png)

- 若要放弃所有挂起的更改脚本，只需单击**放弃**命令。

    ![资源管理器屏幕截图的脚本放弃更改体验](./media/documentdb-view-scripts/scriptexplorerdiscardchanges.png)

- 脚本资源管理器还允许您可以很容易地通过单击**属性**命令查看当前加载的脚本的系统属性。

    ![资源管理器屏幕截图的脚本脚本属性视图](./media/documentdb-view-scripts/scriptproperties.png)

    > [AZURE.NOTE] 时间戳 (_ts) 属性在内部表示为日期时间，但人类可读的格林威治标准时间格式的脚本资源管理器中显示的值。

- 若要删除一个脚本，在脚本资源管理器中选择它并单击**删除**命令。

    ![屏幕截图的脚本资源管理器删除命令](./media/documentdb-view-scripts/scriptexplorerdeletescript1.png)

- 单击**是**确认删除操作，或单击**否**取消删除操作。

    ![屏幕截图的脚本资源管理器删除命令](./media/documentdb-view-scripts/scriptexplorerdeletescript2.png)

## 下一步行动

若要了解有关 DocumentDB 的详细信息，请单击[此处](http://azure.com/docdb)。
 

测试
