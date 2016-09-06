---
ms.openlocfilehash: 65fe296d1815f5aedecac7a1a4065eb5ae1558af
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="从 Azure 管理门户安装更新 1"
   description="解释如何使用管理门户安装 StorSimple 8000 系列更新 1。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/12/2015"
   ms.author="v-sharos" />

#### 可以从管理门户安装更新 1

1. 在 StorSimple 服务页上，选择您的设备。 导航到**设备** > **维护**。

2. 在页面的底部，单击**扫描更新**。 若要检查可用更新，将创建一个作业。 作业已成功完成时通知您。

3. 在同一页上的**软件更新**部分中，您将看到新的软件更新可用。 我们建议您在您的设备上应用更新 1.0 之前查阅发行说明。

    ![安装软件更新](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates1-include.png)

4. 在页面的底部，单击**安装更新**。

5. 系统将提示您进行确认。 单击**确定**。

6. 将显示**安装更新**对话框。 请确保您的设备满足在此对话框中列出的检查。 **我了解上面的要求，并已准备好更新我的设备**选择。 单击检查图标。

    ![确认消息](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates2-include.png)

7. 您将会收到通知更新前检查正在进行。
  
    ![预检查通知](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates3-include.png)

    下面是升级前检查失败的一个例子。 您需要验证设备控制器正常运行并处于联机状态。 您还需要检查硬件组件的运行状况。 在本例中，控制器 0 和 1 控制器组件需要关注。 您可能需要与 Microsoft 支持部门联系，如果您不能自行解决这些问题。

    ![前检查失败](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

8. 成功完成升级前的检查后，将创建更新作业。 成功创建更新作业时，您将收到通知。
 
    ![更新创建作业](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates4-include.png)

    将您的设备上应用此更新。
 
9. 若要监视更新作业的进度，请单击**查看作业**。 在作业页中，您可以看到更新进度。 

    ![更新作业进度](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates5-include.png)

10. 此更新将需要几个小时才能完成。 您可以随时查看作业的详细信息。

    ![更新作业详细信息](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates6-include.png)

11. 完成作业后，导航到**维护**页上，然后向下滚动到**软件更新**。

12. 验证您的设备在运行**StorSimple 8000 系列更新 1.0 (6.3.9600.17491)**。 **上次更新日期**应还修改。

    ![维护页](./media/storsimple-install-update-via-portal/HCS_SoftwareUpdates7-include.png)

