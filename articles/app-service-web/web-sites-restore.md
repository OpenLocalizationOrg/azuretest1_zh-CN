---
ms.openlocfilehash: 9523818a8359a4a4cbce134c6001f9afaf622d0c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="还原在 Azure 应用程序服务 web 应用程序" 
    description="了解如何从备份中还原您的 web 应用程序。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/03/2015" 
    ms.author="cephalin"/>

# 还原在 Azure 应用程序服务 web 应用程序

本文介绍了如何还原您以前备份使用的[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)备份功能的 web 应用程序。 有关详细信息，请参阅[应用程序服务 Web 应用程序的备份](web-sites-backup.md)。 

Web 应用程序还原功能允许您将您 web 应用程序按需恢复到以前的状态，或创建新 web 应用程序基于一个原始 web 应用程序的备份。 创建新 web 应用程序到最新版本并行运行可用于 A / B 测试。

Web 应用程序还原功能，在[Azure 预览门户](http://portal.azure.com)中，**备份**刀片式服务器上的可用是仅在标准模式和高级模式中可用。 有关扩展您的应用程序使用标准或特优模式的信息，请参阅[缩放在 Azure 应用程序服务 web 应用程序](web-sites-scale.md)。 高级模式允许在标准模式下执行的每日备份的更多的注意。

<a name="PreviousBackup"></a>
## 从以前的备份还原 web 应用程序

1. 在 Azure 门户 web 应用程序**设置**刀片式服务器，请单击显示**备份**刀片式服务器的**备份**选项。 在此刀片式服务器中滚动并选择一个基于**备份时间**，并将**状态**从备份列表的备份项。
    
    ![选择备份源][ChooseBackupSource]
    
2. 选择**立即还原****备份**刀片式服务器的顶部。 

    ![选择立即还原][ChooseRestoreNow]

3. 在**还原**刀片式服务器，要恢复现有的 web 应用程序，请验证所有显示的详细信息，然后单击**确定**。 
    
您还可以通过从**还原**刀片式服务器选择的**WEB 应用程序**部分和**创建新的 web 应用程序**一部分到新的 web 应用程序还原您的 web 应用程序。
    
<a name="StorageAccount"></a>
## 下载或从存储帐户中删除备份
    
1. 从主的 Azure 的门户网站，**浏览**刀片式服务器选择**存储帐户**。
    
    将显示您现有的存储帐户的列表。 
    
2. 选择包含您想要下载或删除的备份的存储帐户。
    
    将显示**存储**刀片式服务器。

3. 选择**容器**部件**存储**刀片式服务器显示**容器**刀片式服务器中。
    
    将显示列表的容器。 此列表还会显示 URL 和日期时上次修改此容器。
    
    ![视图容器][ViewContainers]

4. 在列表中，选择该容器，显示刀片式服务器显示列表的文件名称，以及每个文件的大小。
    
5. 通过选择一个文件，您可以选择**下载**或**删除**的文件。 请注意，有两个主要文件类型、.zip 文件和.xml 文件。 

<a name="OperationLogs"></a>
## 查看审核日志
    
1. 若要查看有关的成功或失败的 web 应用程序还原操作的详细信息，请选择主**审核日志**部分**浏览**刀片式服务器。 
    
    刀片式服务器**音频日志**显示所有您的运营，以及级别、 状态、 资源和时间的详细信息。
    
2. 向下滚动刀片式服务器以查找与您的 web 应用程序相关的操作。
3. 若要查看有关操作的详细信息，请在列表中选择的操作。
    
刀片式服务器的详细信息将显示与该操作相关的可用信息。
    
>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。
    
## 会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
* 旧的门户与新门户的更改的指南，请参阅︰[用于预览门户导航的引用](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- IMAGES -->
[ChooseBackupSource]: ./media/web-sites-restore/01ChooseBackupSource.png
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
测试
