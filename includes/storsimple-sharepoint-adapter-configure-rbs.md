---
ms.openlocfilehash: 0d682ca5714d56a5c8ec7dec60deb58714a2a9de
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="为 SharePoint StorSimple 适配器配置 RBS |Microsoft Azure"
   description="描述如何为 SharePoint SharePoint 服务器场中安装 StorSimple 适配器。"
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
   ms.date="07/17/2015"
   ms.author="v-sharos" />

>[AZURE.NOTE] 更改 SharePoint RBS 配置 StorSimple 适配器时，您必须属于域管理员组的用户帐户登录。 此外，必须在管理中心为同一台主机上运行的浏览器访问配置页。

#### 若要配置 RBS

1. 打开 SharePoint 管理中心页上，然后浏览到**系统设置**。 

2. 在**Azure StorSimple**部分中，单击**配置 StorSimple 适配器**。

    ![StorSimple 适配器配置](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 

3. 在**配置 StorSimple 适配器**页上︰

    1. 请确保已选中了**启用编辑路径**复选框。

    2. 在文本框中，键入该 BLOB 存储的通用命名约定 (UNC) 路径。

          >[AZURE.NOTE] BLOB 存储卷必须驻留在 StorSimple 设备上配置 iSCSI 卷上。

    3. 单击下面每个您要为远程存储配置的内容数据库的**启用**按钮。

          >[AZURE.NOTE] BLOB 存储必须共享，可以访问所有 web 前端 (WFE) 服务器，并配置 SharePoint 服务器场的用户帐户必须具有访问该共享。

          ![启用 RBS 提供程序](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)

           When you enable or disable RBS, you will also see the following message.

          ![配置 StorSimple 适配器启用禁用](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

    4. 单击**更新**按钮以应用配置。 当单击**更新**按钮时，RBS 配置状态将更新所有 WFE 在服务器上，并将 RBS 启用整个服务器场。 将出现下面的消息。

           ![Adapter configuration message](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)

           >[AZURE.NOTE] 如果您在为数量非常大的数据库 （大于 200） 与 SharePoint 服务器场配置 RBS，SharePoint 管理中心 web 页可能会超时。 如果出现这种情况，请刷新页面。 这不会影响配置过程。
 
4. 验证配置︰

    1. 登录到该 SharePoint 管理中心网站，并浏览至**配置 StorSimple 适配器**页面。

    2. 请检查以确保它们符合您输入的设置的配置详细信息。 

5. 验证 RBS 工作正常︰

    1. 将文档上载到 SharePoint。 

    2. 浏览到您配置的 UNC 路径。 请确认已创建 RBS 目录结构并且包含上载的对象。

6. （可选）您可以使用 Microsoft RBS `Migrate()` PowerShell cmdlet 附带 SharePoint 迁移到 StorSimple 设备的 BLOB 的现有内容。 有关详细信息，请参阅[迁移内容或 SharePoint 2013 在 RBS][6]或[迁移内容接入或接出 RBS (SharePoint 基础 2010)][7]。

7. （可选）测试安装时，您可以验证，Blob 被移出的内容数据库，如下所示︰ 

    1. 启动 SQL 管理 Studio。

    2. 按照以下方式运行 ListBlobsInDB_2010.sql 或 ListBlobsInDB_2013.sql 查询。

     **ListBlobsInDB_2013.sql**

         使用 WSS_Content 转到
    
         选择 DocStreams.DocId、 LeafName AS 名称、 内容、 AllDocs.Size AS OrigSizeOfContent，LEN （铸造 (内容 AS VARBINARY(MAX))) AS SizeOfContentInDB，DocStreams.RbsId，TimeLastModified
    
         从 DocStreams 内部联接 AllDocs ON DocStreams.DocId = AllDocs.Id 订单 TimeLastModified DESC 转到由

     **ListBlobsInDB_2010.sql**

         使用 WSS_Content 转到

         选择 AllDocStreams.Id，LeafName AS 名称、 内容、 AllDocs.Size AS OrigSizeOfContent，LEN (强制转换 (从 AllDocStreams 内部联接 AllDocs ON AllDocStreams.Id 内容 AS VARBINARY(MAX))) AS SizeOfContentInDB，RbsId，TimeLastModified = AllDocs.Id 订单 TimeLastModified DESC 转由

     如果 RBS 已配置正确，任何对象的已上载和其外部化与 RBS SizeOfContentInDB 列中应出现 NULL 值。

8. （可选）配置 RBS 并将 BLOB 的所有内容都移到 StorSimple 设备后，您可以将内容数据库都移动到设备。 如果您选择要移动的内容数据库，我们建议您将内容数据库存储配置的主要卷作为设备上。 然后，使用建立 SQL Server 将内容数据库迁移到 StorSimple 设备的最佳做法。 

     >[AZURE.NOTE] （它不支持 5000 或 7000 系列） StorSimple 8000 系列仅支持移动设备的内容数据库。
 
     如果在单独的卷 StorSimple 设备上存储 Blob 和内容的数据库，我们建议同一卷容器中配置它们。 这将确保，他们将备份放在一起。

       >[AZURE.WARNING] 如果您没有启用 RBS，建议不要将内容数据库移到 StorSimple 设备。 这是一个未经测试的配置。
 
9. 转到下一步︰[配置垃圾回收](#configure-garbage-collection)。

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
