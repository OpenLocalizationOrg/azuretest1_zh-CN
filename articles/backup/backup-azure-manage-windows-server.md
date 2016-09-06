---
ms.openlocfilehash: cfc3e25d603af4449a843c7de0000aa255ed16f5
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="管理 Azure 备份存储库和服务器 |Microsoft Azure"
    description="使用本教程来学习如何管理 Azure 备份存储库和服务器。"
    services="backup"
    documentationCenter=""
    authors="aashishr"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/13/2015"
    ms.author="jimpark"; "aashishr"/>


# 管理 Azure 备份存储库和服务器
在本文中，您会发现可通过管理门户的备份管理任务的概述。

1. 登录到[管理门户](https://manage.windowsazure.com)。
2. 单击**恢复服务**，然后单击备份存储库以查看快速启动页的名称。

    通过选择快速启动页顶部的选项，可以查看可用的管理任务。

    ![受保护的项](./media/backup-azure-manage-windows-server/RS_tabs.png)

## 仪表板
选择**仪表板**以查看服务器的使用情况概览。 底部的仪表板可执行以下任务︰

- **管理证书**。 如果证书被用于注册服务器，然后使用此更新证书。 如果您使用的存储库的凭据，请使用**管理证书**。
- **删除**。 删除当前的备份存储库。 如果不再使用备份存储库时，您可以删除它以释放存储空间。 已从存储库中删除所有已注册的服务器后，才会启用**删除**。
- **保险存储的凭据**。 使用此快速粗略地看菜单项来配置存储库凭据。

## 受保护的项
选择要查看的项目，从服务器备份的**保护项目**。 此列表是仅用于信息。

![受保护的项](./media/backup-azure-manage-windows-server/RS_protecteditems.png)

## 注册的项
选择要查看的服务器已注册到此电子仓库的名称的**注册项**。

![已删除的服务器](./media/backup-azure-manage-windows-server/RS_deletedserver.png)

从这里您可以执行以下任务︰
- **允许重新注册**-选中此选项，则服务器可以使用**注册向导**在代理中注册该服务器与备份存储库是第二次。 您可能需要重新注册由于证书错误或如果服务器必须重新生成。 每个服务器名称只能一次允许重新注册。
- **删除**-从该备份的存储库中删除服务器。 立即删除所有与该服务器关联的存储数据。

## 下一步行动
- [从 Azure 还原 Windows 服务器或 Windows 客户端](backup-azure-restore-windows-server.md)
- 若要了解有关 Azure 备份的详细信息，请参阅[Azure 备份概述](backup-introduction-to-azure-backup.md)
- 访问[Azure 备份论坛](http://go.microsoft.com/fwlink/p/?LinkId=290933)

测试
