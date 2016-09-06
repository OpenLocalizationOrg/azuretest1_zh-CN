---
ms.openlocfilehash: be1519d5cd4f33766e049f398a92cd540e21311e
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="选项 2︰ 使用 Azure 管理门户应用更新 1"
   description="解释如何使用管理门户安装 StorSimple 8000 系列更新 1。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/14/2015"
   ms.author="v-sharos" />

#### 从 Azure 管理门户安装更新 1

1. 在管理门户中，请转到**设备**页上，选择您的设备。
 
2. 导航到**设备** > **配置**。 

3. 在**网络接口**中，找到具有网关分配的网络接口。 这将是数据 0 以外的网络接口。 

4. 清除的网关设置。 请注意，启用云的网络接口上需要网关设置，因为您将需要禁用此接口，以清除设置云访问。

5. 对于具有网关 （不包括数据 0） 指派的任何其他网络接口重复步骤 4。

6. 保存已修改的配置。

7. 您可以立即[使用管理门户安装更新 1](#use-the-management-portal-to-install-update-1)。 


