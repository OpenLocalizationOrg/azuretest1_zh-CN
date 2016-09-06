---
ms.openlocfilehash: 5ad88210e826a838142831e61fa91d38adca4581
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="Azure 备份-备份和恢复的 Windows 服务器或 Windows 客户端" | Microsoft Azure
   description="了解如何备份和还原 Windows 服务器或 Windows 客户端中。 文章还介绍了备用服务器恢复"
   services="backup"
   documentationCenter=""
   authors="aashishr"
   manager="jwhit"
   editor=""/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
     ms.date="08/18/2015"
     ms.author="jimpark"; "aashishr"/>

# 备份和还原 Windows 服务器或 Windows 客户端计算机
本文介绍了从 Windows 服务器或 Windows 客户机备份所需的步骤。 它还介绍了在同一台计算机，并还原备份的文件的任何其他计算机上所需的步骤的文件还原备份所需的步骤。

## 备份文件

1. 计算机注册后，请打开 Microsoft Azure 备份 mmc 管理单元。

    ![搜索结果](./media/backup-azure-backup-and-recover/result.png)

2. 单击**日程安排备份**

    ![计划备份](./media/backup-azure-backup-and-recover/schedulebackup.png)

3. 选择要备份的项目。 在 Windows 服务器/Windows 客户端 （即 azure 备份 无需系统中心 Data Protection Manager) 可以保护文件和文件夹。

    ![备份的项目](./media/backup-azure-backup-and-recover/items.png)

4. 指定在下面的[文章](backup-azure-backup-cloud-as-tape.md)中详细介绍的备份时间表和保留策略

5. 选择发送初始备份的方法。 您选择的完成初始播种是取决于您要备份的数据和您的 internet 上载链接速度。 如果您计划备份 GB 的 / TB 的数据，通过高延迟、 低带宽的连接，建议您通过传送到最近的 Azure 数据中心磁盘完成初始备份。 这被称为"脱机备份"，[文章](backup-azure-backup-import-export.md)中详细介绍。 如果您有足够的带宽连接我们建议您通过网络完成初始备份。

    ![初始备份](./media/backup-azure-backup-and-recover/initialbackup.png)

6. 完成此过程后，回到 mmc 管理单元中，请单击**立即备份**以完成初始播种在网络上。

    ![立即备份](./media/backup-azure-backup-and-recover/backupnow.png)

7. 完成初始播种后，在控制台中 Azure 备份**作业**视图表示的状态。

    ![红外线 （ir) 完成](./media/backup-azure-backup-and-recover/ircomplete.png)

## 在同一台计算机上的数据恢复
如果您意外地删除一个文件，并想还原 （从中获取备份） 的同一台计算机上的文件/卷，则以下步骤将帮助您恢复数据。

1. 打开**Microsoft Azure 备份**管理单元。

2. 单击**恢复数据**来启动工作流。

    ![恢复数据](./media/backup-azure-backup-and-recover/recover.png)

3. 选择**此服务器 (*yourmachinename*) * * 您计划还原备份的同一台计算机上的文件选项。

    ![同一台计算机](./media/backup-azure-backup-and-recover/samemachine.png)

4. 您可以选择**浏览文件**或**搜索文件**。 如果您打算恢复其路径已知的一个或多个文件，请保留默认选项。 如果不能确定的文件夹结构，但想要搜索的文件，请选择**文件搜索**选项。 为便于本部分中，我们将继续使用默认选项。

    ![浏览文件](./media/backup-azure-backup-and-recover/browseandsearch.png)

5. 选择想要还原的文件的卷。 从任何时间点还原的时间屏幕启用。 中**以粗体显示**在日历控件中显示的日期表示可用还原点的日期。 一旦选择了日期，根据备份计划 （并成功的备份操作），您可以选择点时间从**时间**放在向下。

    ![数量和日期](./media/backup-azure-backup-and-recover/volanddate.png)

6. 选择您要恢复的项目。 您可以选择多文件夹/文件要还原。

    ![选择文件](./media/backup-azure-backup-and-recover/selectfiles.png)

7. 指定的故障恢复参数。

    ![恢复选项](./media/backup-azure-backup-and-recover/recoveroptions.png)

  - 有一个选项，将还原到原始位置 （在其中的文件/文件夹将被改写） 或同一台计算机中的其他位置。
  - 如果您想要还原的文件/文件夹存在于目标位置，您可以选择创建副本 （同一文件的两个版本），或覆盖目标位置中的文件或跳过的文件在目标中存在的恢复。
  - 强烈建议您保持默认选项，恢复将恢复的文件上的 Acl。

8. 一旦提供了这些输入，恢复工作流启动的将文件还原到这台机器。

## 恢复到备用计算机
如果整个服务器不可用，您仍然可以恢复另一台计算机中的文件/卷。 以下步骤说明了工作流。  

在步骤中使用的术语如下所示︰
- *源计算机*-从备份和当前无法使用与原始计算机。
- *目标机器*-检索数据的机器。
- *存储库的示例*– 注册了*源计算机*和*目标计算机*的备份的存储库。 <br/>

> [AZURE.NOTE] 从一台计算机备份无法还原的计算机运行的是早期版本的操作系统上。 例如，如果从 Windows 7 计算机进行备份，它可以还原 Windows 8 上或机器上面。 但是相反不为真。

1. 在*目标计算机*中打开**Microsoft Azure 备份**管理单元。

2. 确保*目标计算机*和*源计算机*还原到相同的备份存储库。

3. 单击**恢复数据**来启动工作流。

    ![恢复数据](./media/backup-azure-backup-and-recover/recover.png)

4. 选择**另一个服务器**

    ![另一台服务器](./media/backup-azure-backup-and-recover/anotherserver.png)

5. 提供到*示例存储库*存储库凭据文件对应。 如果存储库凭据文件无效 （或过期），下载一个新的存储库凭据文件从 Azure 门户中的*示例存储库*。 一旦提供该存储库凭据文件，会显示针对存储库凭据文件备份的存储库。

6. 选择从列表中显示的计算机的*源计算机*。

    ![计算机列表](./media/backup-azure-backup-and-recover/machinelist.png)

7. 像以前一样，选择**搜索文件**或**浏览文件**选项。 为便于本部分中，我们将使用**搜索文件**选项。

    ![搜索](./media/backup-azure-backup-and-recover/search.png)

8. 在下一个屏幕中选择的数量和日期。 搜索要还原的文件夹/文件名称。

    ![搜索项目](./media/backup-azure-backup-and-recover/searchitems.png)

9. 选择需要到还原的文件的位置。

    ![将还原位置](./media/backup-azure-backup-and-recover/restorelocation.png)

10. 提供到*示例存储库*在*源计算机上的*注册过程中提供的加密密码。

    ![加密](./media/backup-azure-backup-and-recover/encryption.png)

11. 一旦提供输入，请单击**恢复**按钮触发的目标提供中备份的文件还原。

## 视频的演练

这是本教程的视频演练。

[AZURE.VIDEO azurebackuprestoreserverandclient]

## 下一步行动
- [Azure 备份常见问题解答](backup-azure-backup-faq.md)

测试
