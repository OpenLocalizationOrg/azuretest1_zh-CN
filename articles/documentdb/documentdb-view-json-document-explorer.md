---
ms.openlocfilehash: 0be0d69c0c381b8ea91c1f1ce80b0a99e430c731
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="查看、 编辑、 创建和使用 DocumentDB 文档资源管理器的 JSON 文档上载 |Microsoft Azure"
    description="了解 DocumentDB 文档资源管理器，可以查看、 编辑、 创建和使用 DocumentDB 的 JSON 文档上载 Azure 预览门户工具。"
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

# 查看、 编辑、 创建和使用 DocumentDB 文档资源管理器的 JSON 文档上载 #

本文概述了[Microsoft Azure DocumentDB](http://azure.microsoft.com/services/documentdb/)文档资源管理器，使您可以查看、 编辑、 创建和使用 DocumentDB 的 JSON 文档上载 Azure 预览门户工具。

通过完成本教程，您将能够回答以下问题︰  

-   如何可以我轻松地创建、 查看、 编辑和删除单独的 web 浏览器的 DocumentDB 文档？
-   如何轻松地查看 DocumentDB 文档通过 web 浏览器的系统属性？
-   如何轻松地执行批量接收的文档到 DocumentDB 通过 web 浏览器？

##<a id="Launch"></a>启动和导航文档资源管理器##

文档资源管理器可以从任一 DocumentDB 帐户、 数据库和集合刀片式服务器启动。  

1. 顶部的 DocumentDB 帐户或数据库刀片式服务器，只需单击**文档资源管理器**命令。

    ![文档资源管理器命令的屏幕抓图](./media/documentdb-view-JSON-document-explorer/documentexplorercommand.png)
 
2. 另外，每个刀片底部附近是包含部件的**文档资源管理器**的**开发人员工具**镜头。

    ![部分文档资源管理器的屏幕抓图](./media/documentdb-view-JSON-document-explorer/documentexplorerpart.png)

2. 只需单击该图块可以启动文档资源管理器。

    <p>**数据库**和**集合**下拉列表框预先填充您启动文档资源管理器的上下文中。  例如，如果您从数据库刀片式服务器启动，则当前数据库是预先填充。  如果您从集合刀片式服务器启动，则预填充的当前集合。

    ![文档资源管理器的屏幕抓图](./media/documentdb-view-JSON-document-explorer/documentexplorerinitial.png)

3. 默认情况下，文档资源管理器对选定收藏集，按从最早到最晚及其创建日期中的前 100 个文档加载。  您可以通过选择文档资源管理器刀片底部**更加载**选项加载其他文档 （在 100 个批次）。  通过单击刀片式服务器文档资源管理器顶部的设置命令，可以修改默认行为。

    ![文档资源管理器中设置刀片式服务器的屏幕抓图](./media/documentdb-view-JSON-document-explorer/documentexplorersettings.png)


4. 在设置刀片式服务器，您可以调整每页返回以及提供 WHERE 子句来负载匹配的文档在文档资源管理器网格中的项的数目。  阅读有关 DocumentDB SQL 语法的详细信息[在此处](documentdb-sql-query.md)。

    ![文档资源管理器中设置刀片式服务器的屏幕抓图](./media/documentdb-view-JSON-document-explorer/documentexplorersettings2.png)

    > [AZURE.NOTE] 修改后文档资源管理器设置，您必须单击**刷新**命令以便应用新设置。  设置仅在当前浏览器会话中将保持不变。
    
5. **数据库**和**集合**下拉列表框可以用于方便地更改的当前正在查看文档而无需关闭并重新启动文档资源管理器的集合。  

5. 文档资源管理器还支持通过其 id 属性筛选当前加载的文档集。  只需在筛选器框中键入。

    ![屏幕抓图的文档资源管理器中突出显示的筛选器](./media/documentdb-view-JSON-document-explorer/documentexplorerfilter.png)

    和文档资源管理器列表中的对结果进行筛选基于您提供的条件。

    ![屏幕抓图的文档资源管理器与筛选结果](./media/documentdb-view-JSON-document-explorer/documentexplorerfilterresults.png)


    > [AZURE.IMPORTANT] 文档资源管理器功能仅过滤器从***当前***加载的文档集，并不会执行对当前所选集合的查询。

6. 若要刷新加载文档资源管理器中的文档的列表，只需单击刀片式服务器顶部的**刷新**命令。

    ![屏幕抓图的文档资源管理器刷新命令](./media/documentdb-view-JSON-document-explorer/documentexplorerrefresh.png)


##<a id="Create"></a>查看、 创建和编辑与文档资源管理器中的文档##

文档资源管理器允许您轻松地创建、 编辑和删除文档。  

- 为了创建文档，只需单击**创建文档**命令和最小的 JSON 段提供了。

    ![屏幕抓图的文档资源管理器中创建文档的经验](./media/documentdb-view-JSON-document-explorer/createdocument.png)

- 只需键入或粘贴 JSON 文档内容的您想要创建并单击**保存**命令提交文档。

    ![屏幕抓图的文档资源管理器保存命令](./media/documentdb-view-JSON-document-explorer/savedocument1.png)

    > [AZURE.NOTE] 如果未提供"id"属性，，然后文档资源管理器自动添加 id 属性，并生成一个 GUID 作为标识值。

- 如果您已经从 JSON 数据文件，MongoDB，SQL Server、 CSV 文件，Azure 表存储或与其他 DocumentDB 集合，您可以使用 DocumentDB 的[数据迁移工具](documentdb-import-data.md)快速地将数据导入。

- 若要编辑一个现有的文档，只需在文档资源管理器中选择它，根据您的习惯，并单击**保存**命令编辑文档。

    ![屏幕抓图的文档资源管理器编辑文档功能](./media/documentdb-view-JSON-document-explorer/editdocument.png)

- 如果您正在编辑的文档，并决定要放弃当前集的编辑，只需单击放弃命令，确认放弃行动要求，以及文档的上一状态被重新加载。

    ![屏幕抓图的文档资源管理器中放弃命令](./media/documentdb-view-JSON-document-explorer/discardedit.png)

- 您可以通过选择它，单击**删除**命令，然后确认删除删除文档。 确认后，该文档是立即从列表中删除文档资源管理器︰

    ![屏幕抓图的文档资源管理器删除命令](./media/documentdb-view-JSON-document-explorer/deletedocument.png)

- 请注意文档资源管理器验证任何新的或编辑后的文档包含有效的 JSON。  甚至可以悬停在不正确的部分，以获取有关验证错误的详细信息。

    ![屏幕抓图的文档资源管理器无效 JSON 突出显示](./media/documentdb-view-JSON-document-explorer/invalidjson1.png)

- 此外，文档资源管理器阻止您保存文档使用 JSON 内容无效。

    ![屏幕抓图的文档资源管理器无效 JSON 保存错误](./media/documentdb-view-JSON-document-explorer/invalidjson2.png)

- 最后，文档资源管理器可以轻松地通过单击**属性**命令查看当前加载的文档的系统属性。

    ![屏幕抓图的文档资源管理器文档属性视图](./media/documentdb-view-JSON-document-explorer/documentproperties.png)

    > [AZURE.NOTE] 时间戳 (_ts) 属性在内部表示为日期时间，但人类可读的格林威治标准时间格式的文档资源管理器中显示的值。

##<a id="BulkAdd"></a>批量添加文档的文档资源管理器##

文档资源管理器支持批量接收一个或多个现有的 JSON 文档。  

1. 若要启动上载过程中，请单击**添加文档**命令。

    ![屏幕抓图的文档资源管理器批量接收功能](./media/documentdb-view-JSON-document-explorer/adddocument1.png)

2. 将打开新的刀片。  单击浏览按钮以打开文件资源管理器窗口，然后选择一个或多个要上载的 JSON 文档。

    ![屏幕抓图的文档资源管理器批量接收过程](./media/documentdb-view-JSON-document-explorer/adddocument2.png)

    > [AZURE.NOTE] 文档资源管理器目前支持最多 100 个的 JSON 文档，每个单独的上载操作。

3. 一旦您对您的选择感到满意，请单击**上载**按钮。  这些文档将自动添加到文档资源管理器中的网格并上载结果显示执行操作的过程。 导入失败报告为单个文件。

    ![屏幕抓图的文档资源管理器批量接收结果](./media/documentdb-view-JSON-document-explorer/adddocument3.png)

4. 完成此操作后，您最多可以选择另一个 100 个文档上载。

##<a name="NextSteps"></a>下一步行动

若要了解有关 DocumentDB 的详细信息，请单击[此处](http://azure.com/docdb)。
 

测试
