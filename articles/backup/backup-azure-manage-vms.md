---
ms.openlocfilehash: d3ad8e62047fbd28758473e7c7ac531befb190e3
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

<properties
    pageTitle="Azure 备份-管理的虚拟机"
    description="了解如何管理 Azure 的虚拟机"
    services="backup"
    documentationCenter=""
    authors="aashishr"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/17/2015"
    ms.author="aashishr"; "jimpark"/>

# 管理虚拟机


## 管理受保护的虚拟机

1. 查看和管理备份虚拟机的设置，请单击**受保护的项**选项卡。

  - 单击要**备份详细信息**选项卡，其中显示您上次备份的信息，请参阅受保护的项的名称。

        ![Virtual machine backup](./media/backup-azure-manage-vms/backup-vmdetails.png)

2. 若要查看和管理备份策略为虚拟机的设置，请单击**策略**选项卡。

  - **备份策略**选项卡会显示现有的策略。 您可以根据需要进行修改。 如果您需要创建一个新策略将在**策略**页上单击**创建**。 请注意，是否您想要删除的策略它不应该有任何与它相关联的虚拟机。

        ![Virtual machine policy](./media/backup-azure-manage-vms/backup-vmpolicy.png)

3. **作业**页上的虚拟机中可以获得有关动作或状态的详细信息。 单击该列表以获取详细信息，在作业或筛选器为特定虚拟机的作业。

    ![作业](./media/backup-azure-manage-vms/backup-job.png)

## 虚拟机的按需备份
一旦它被配置为保护，您可以执行按需备份一个虚拟。 如果初始备份将挂起虚拟机，按需备份将创建一套完整的虚拟机在 Azure 的备份存储库中。 完成第一个备份时，如果按需备份将从以前的备份只发送更改，到 Azure 的备份存储库。

若要执行按需备份虚拟机︰

1. 导航到**受保护的项目**页面 （如果尚未选中），选择**Azure 虚拟机**作为**类型**并单击**选择**按钮。

    ![虚拟机类型](./media/backup-azure-manage-vms/vm-type.png)

2. 选择您要在其执行按需备份，然后单击页面底部**立即备份**按钮上的虚拟机。

    ![现在备份](./media/backup-azure-manage-vms/backup-now.png)

    这将在选定的虚拟机上创建一个备份作业。 通过此作业创建的恢复点的保留范围将与虚拟机相关联的策略中指定的相同。

    ![创建备份作业](./media/backup-azure-manage-vms/creating-job.png)

    >[AZURE.NOTE] 若要查看与虚拟机相关联的策略，向下钻取到虚拟机中的**受保护的项**页并转到备份策略选项卡。

3. 创建作业后，您可以单击**查看作业**祝酒栏以查看相应的作业在作业页中的按钮上。

    ![创建备份作业](./media/backup-azure-manage-vms/created-job.png)

4. 作业成功完成之后, 恢复点将创建可以用于还原虚拟机。 这也将增加 1**保护项目**页中恢复点列的值。

## 停止保护虚拟机
您可以选择停止将来的备份虚拟机具有以下选项︰

- 保留与 Azure 备份存储库中的虚拟机的备份数据
- 删除与虚拟机的备份数据

如果您已选择第一个选项，您可以使用备份数据还原虚拟机。 价格的这种虚拟机的详细信息，请单击[此处](http://azure.microsoft.com/pricing/details/backup/)。

若要停止保护虚拟机︰

1. 导航到**受保护的邮件**页面选择**Azure 的虚拟机**作为筛选器类型，（如果尚未选中） 并单击**选择**按钮。

    ![虚拟机类型](./media/backup-azure-manage-vms/vm-type.png)

2. 选择虚拟机并单击页面底部的**停止保护**。

    ![停止保护](./media/backup-azure-manage-vms/stop-protection.png)

3. 默认情况下，Azure 备份并不会删除与该虚拟机的备份数据。

    ![确认停止保护](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    如果您想要删除备份数据，请选择复选框。

    ![复选框](./media/backup-azure-manage-vms/checkbox.png)

    请选择一个理由停止备份。 虽然这是可选的提供一个原因有助于 Azure 备份处理反馈意见并确定其优先级的客户方案。

4. 单击**提交**按钮来提交**停止保护**作业。 **查看作业**以查看相应的作业在**作业**页上单击。

    ![停止保护](./media/backup-azure-manage-vms/stop-protect-success.png)

    如果您没有选择**停止保护**向导然后开机自检作业完成时**删除关联的备份数据**的选项，保护状态将变为**停止保护**。 直到被显式删除，数据将保留使用 Azure 的备份。 您始终可以通过在**受保护的项**页中选择虚拟机并单击**删除**删除的数据。

    ![停止的保护](./media/backup-azure-manage-vms/protection-stopped-status.png)

    如果您已选择**删除关联的备份数据**选项，虚拟机将不会是**保护项目**页面的一部分。

## 重新保护虚拟机
如果不选择**删除关联的备份数据**选项中**停止保护**，可以按照下面的步骤类似于备份注册虚拟机重新保护虚拟机。 一旦受到保护，此虚拟机将具有备份数据保留之前停止保护和恢复点创建后重新保护。

后重新保护，虚拟计算机的保护状态将更改为**受保护**如果有之前**停止保护**的恢复点。

  ![Reprotected 的虚拟机](./media/backup-azure-manage-vms/reprotected-status.png)

>[AZURE.NOTE] 后重新保护虚拟机，您可以选择不同的策略比虚拟机被保护的最初策略。

## 取消注册虚拟机

如果您希望备份的存储库中删除虚拟机︰

1. 单击页面底部的**注销**按钮。

    ![禁用保护](./media/backup-azure-manage-vms/unregister-button.png)

    Toast 通知将出现在请求确认屏幕的底部。 单击**是**以继续。

    ![禁用保护](./media/backup-azure-manage-vms/confirm-unregister.png)

## 删除备份数据
您可以删除与虚拟机，或者关联的备份数据︰

- 在过程中停止保护作业
- 停止保护作业被完成之后的虚拟机上

若要删除在"停止保护"状态的虚拟机上的备份数据后**停止备份**作业成功完成︰

1. 导航到**受保护的邮件**页面选择**Azure 虚拟机**作为类型并单击**选择**按钮。

    ![虚拟机类型](./media/backup-azure-manage-vms/vm-type.png)

2. 选择虚拟机。 虚拟机将处于**停止保护**状态。

    ![停止保护](./media/backup-azure-manage-vms/protection-stopped-b.png)

3. 单击页面底部的**删除**按钮。

    ![删除备份](./media/backup-azure-manage-vms/delete-backup.png)

4. 在**删除备份数据**向导中，选择删除备份数据 （强烈推荐） 的原因并单击**提交**。

    ![删除备份数据](./media/backup-azure-manage-vms/delete-backup-data.png)

5. 这将创建一个作业以删除所选的虚拟机的备份数据。 **查看作业**以查看相应的作业在作业页上单击。

    ![数据删除成功](./media/backup-azure-manage-vms/delete-data-success.png)

    完成作业后，虚拟机所对应的条目将从**受保护的项**中删除。


###仪表板

在**仪表板**页上可以查看有关 Azure 的虚拟机，其存储和最近 24 小时内与它们关联的作业的信息。 您可以查看备份状态以及任何关联的备份错误。

  ![仪表板](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

测试
