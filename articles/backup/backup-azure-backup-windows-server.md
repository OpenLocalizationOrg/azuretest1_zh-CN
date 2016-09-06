---
ms.openlocfilehash: b518724800a0dc8dfc6db3d2f093a246b1796b53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
   pageTitle="备份 Windows 服务器或 Windows 客户端的文件和文件夹到 Azure |Microsoft Azure"
   description="了解如何从 Windows 服务器或到 Azure 的 Windows 客户端的备份。"
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
     ms.date="08/13/2015"
     ms.author="jimpark"; "aashishr"/>

# Windows 服务器或 Windows 客户端的文件和文件夹备份到 Azure
本文介绍 Windows 服务器或客户端的 Windows 文件和文件夹备份到 Azure 所需的步骤。

## 在开始之前
在继续之前，请确保满足所有[系统必备组件](backup-configure-vault.md#before-you-start)使用 Microsoft Azure 备份以保护 Windows Server 和 Windows 客户端。 系统必备组件如介绍任务︰ 创建备份的存储库、 下载存储库凭据，安装 Azure 备份代理，和存储库中注册计算机。

## 备份文件
1. 计算机注册后，请打开 Microsoft Azure 备份 mmc 管理单元。

    ![搜索结果](./media/backup-azure-backup-windows-server/result.png)

2. 单击**计划备份**

    ![计划备份](./media/backup-azure-backup-windows-server/schedulebackup.png)

3. 选择要备份的项目。 在 Windows 服务器/Windows 客户端 （即 azure 备份 无需系统中心 Data Protection Manager) 可以保护文件和文件夹。

    ![备份的项目](./media/backup-azure-backup-windows-server/items.png)

4. 指定在下面的[文章](backup-azure-backup-cloud-as-tape.md)中详细介绍的备份时间表和保留策略。

5. 选择发送初始备份的方法。 您选择的完成初始播种是取决于您要备份的数据和您的 internet 上载链接速度。 如果您计划备份 GB 的 / TB 的数据，通过高延迟、 低带宽的连接，建议您通过传送到最近的 Azure 数据中心磁盘完成初始备份。 这被称为"脱机备份"，[文章](backup-azure-backup-import-export.md)中详细介绍。 如果您有足够的带宽连接我们建议您通过网络完成初始备份。

    ![初始备份](./media/backup-azure-backup-windows-server/initialbackup.png)

6. 计划备份过程完成后，返回到 mmc 管理单元中，请单击**立即备份**以完成初始种子通过网络。

    ![立即备份](./media/backup-azure-backup-windows-server/backupnow.png)

7. 初始播种完成后，在控制台中 Azure 备份**作业**视图表示的状态。

    ![红外线 （ir) 完成](./media/backup-azure-backup-windows-server/ircomplete.png)

## 下一步行动
- [管理 Windows 服务器或 Windows 客户端](backup-azure-manage-windows-server.md)
- [从 Azure 还原 Windows 服务器或 Windows 客户端](backup-azure-restore-windows-server.md)
- [Azure 备份常见问题解答](backup-azure-backup-faq.md)

测试
