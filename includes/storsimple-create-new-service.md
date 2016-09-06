---
ms.openlocfilehash: e9d2a29bdb3da3022a23188756b68595efd40bf9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建一个新的 StorSimple 管理器服务"
   description="描述如何创建新实例的 StorSimple 管理器服务。"
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
   ms.date="08/18/2015"
   ms.author="v-sharos" />


#### 若要创建新的服务

1. 使用 Microsoft 帐户凭据登录到管理门户网站位于此 URL: [http://azure.microsoft.com/](http://azure.microsoft.com/)。

2. 在管理门户中，单击**新建** > **数据服务** > **StorSimple 管理器** > **快速创建**。

3. 在表单中显示，请执行以下操作︰
  1. 提供您的服务的唯一**名称**。 这是一个可用于标识服务的友好名称。 该名称可以有 2 到 50 个字符可以是字母、 数字和连字符。 该名称必须开始和结束以字母或数字。
  2. 提供服务的**位置**。 一般情况下，选择您要在其中部署您的设备的地理区域与最接近的位置。 您可能还想要在下列因素︰ 
     
        - 如果您还想要部署与 StorSimple 设备的 Azure 中有现有的工作负载，则应使用该数据中心。
        - 您的 StorSimple 管理器服务和 Azure 存储可以在两个不同的位置。 在这种情况下，您需要单独创建的 StorSimple 管理器和 Azure 存储帐户。 要创建 Azure 存储帐户，转到 Azure 存储服务管理门户中，请按照[创建 Azure 存储帐户](storage-create-storage-account.md#create-a-storage-account)。 创建此帐户后，将其添加到 StorSimple 管理器服务[配置新的存储帐户为服务](storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service)中的步骤。
         
  3. 从下拉列表中选择**预订**。 该订阅已链接到您的付款帐户。 此字段不存在，如果您只有一个订阅。
  4. 选择要自动创建存储帐户，与该服务**创建一个新的存储帐户**。 此存储帐户将具有一个特殊的名称，例如"storsimplebwv8c6dcnf"。 如果您需要一个不同的位置中的数据，请取消选中此框。 
  5. 单击**创建 StorSimple 管理器**来创建服务。

   ![创建服务](./media/storsimple-create-new-service/HCS_CreateAService-include.png)

  您将会转向**服务**登录页。 创建服务将需要几分钟。 成功创建该服务后，将相应地通知您，该服务的状态将更改为**活动状态**。
 
   ![创建服务](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)
