---
ms.openlocfilehash: 2d4e64cbfd26387d4ebb1c91fe8864369aa76e8a
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="SharePoint 的垃圾回收的 StorSimple 适配器 |Microsoft Azure"
   description="描述如何使用 SharePoint StorSimple 适配器时立即删除 Blob。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/10/2015"
   ms.author="v-sharos" />

在此过程中，您将能够︰

1. [运行可执行文件维护员的准备](#to-prepare-to-run-the-maintainer)。

2. [准备的内容数据库和孤立 Blob 的即时删除回收站](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs)。

3. [运行 Maintainer.exe](#to-run-the-maintainer)。

4. 将[还原的内容数据库和回收站设置](#to-revert-the-content-database-and-recycle-bin-settings)。

#### 准备运行维护

1. 在 Web 前端服务器上，以管理员身份打开 SharePoint 2013 管理外壳。

2. 导航到文件夹<boot drive>: SQL 远程 Blob 存储 10.50\Maintainer \Program Files\Microsoft\.

3. 重命名**web.config** **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** 。

4. 使用`aspnet_regiis -pdf connectionStrings`web.config 文件进行解密。

5. 在解密的 web.config 文件中，在**<connectionStrings>**节点中，添加您的 SQL server 实例和内容数据库名称的连接字符串。 下面的示例，请参阅。

    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`

6. 使用`aspnet_regiis –pef connectionStrings`若要重新加密的 web.config 文件。 

7. 将 web.config 重命名为 Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config。 

#### 若要准备内容数据库和回收站，以立即删除孤立 Blob

1. 在 SQL Server 中，在 SQL 管理 Studio 中，运行以下更新查询的目标内容数据库︰ 

       `use WSS_Content`

       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`

       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`

2. 在**管理中心**web 前端服务器上编辑的**Web 应用程序常规设置**所需的内容数据库暂时禁用回收站。 对于任何相关的网站集，此操作还将清空回收站。 若要执行此操作，请单击**管理中心** -> **应用程序管理** -> **（管理 web 应用程序） 的 Web 应用程序** -> **SharePoint-80** -> **应用程序的常规设置**。 将**回收站状态**设置为**OFF**。

    ![Web 应用程序常规设置](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### 若要运行维护

- 在 web 前端服务器上，在 SharePoint 2013 的管理外壳程序中，运行维护员，如下所示︰

      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`

    >[AZURE.NOTE] 只有`GarbageCollection`这一次的 StorSimple 支持的操作。 此外请注意，颁发给 Microsoft.Data.SqlRemoteBlobs.Maintainer.exe 的参数区分大小写。 
 
#### 若要还原的内容数据库和回收站设置

1. 在 SQL Server 中，在 SQL 管理 Studio 中，运行以下更新查询的目标内容数据库︰

      `use WSS_Content`

      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`

      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`

      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`

2. 在**管理中心**，web 前端服务器上编辑的**Web 应用程序常规设置**所需的内容数据库来重新启用回收站。 若要执行此操作，请单击**管理中心** -> **应用程序管理** -> **（管理 web 应用程序） 的 Web 应用程序** -> **SharePoint-80** -> **应用程序的常规设置**。 设置回收箱状态为**ON**。
