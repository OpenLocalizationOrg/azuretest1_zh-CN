---
ms.openlocfilehash: a87fe1b517f264870550ffec852e549ae192ade0
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="添加卷容器"
   description="说明如何使用 StorSimple 管理器服务卷容器页面可以添加卷的容器。"
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
   ms.date="08/14/2015"
   ms.author="v-sharos" />

#### 若要添加卷容器

1. 在**设备**页中，选择的设备，双击它，然后单击**卷容器**选项卡。

2. 单击页面底部的**添加**。 在**创建卷容器**对话框中，执行下列操作︰

  1. 提供卷容器的唯一**名称**。 此名称可以包含 32 个字符的最多。
  2. 选择要与此卷容器关联**存储帐户**。 可以选择从现有存储帐户内相同的订阅或选择**添加更多**存储帐户待选另一个订阅。 您还可以选择创建服务时首先生成的存储帐户。
  3. 指定为**无限制**带宽，如果您想要使用所有可用的带宽或**自定义**部署带宽控制。 自定义的带宽，提供一个介于 1 到 1000 Mbps。 若要分配带宽根据日程安排，您可以**选择带宽模板**。
  4. 我们建议您保持**启用云存储加密**选择转到云中的数据进行加密。 禁用加密，只有在采用其他方式对数据进行加密。
  5. 提供**云存储加密密钥**包含介于 8 到 32 个字符之间。 该设备使用此键来访问加密的数据。 在**确认云存储加密密钥**字段中，输入云存储加密密钥再次进行确认。
  6. 单击箭头可前进到下一个页面。

    ![使用带宽模板 1 创建卷容器](./media/storsimple-add-volume-container/HCS_CreateVCBT1-include.png) 

3. 如果您指定**选择带宽模板**，请从现有带宽模板的下拉列表中进行选择。 查看日程安排设置，然后单击检查图标![检查图标](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png)。

    ![使用带宽模板 2 创建卷容器](./media/storsimple-add-volume-container/HCS_CreateVCBT2-include.png) 

该卷容器将被保存并将**卷容器**页面上列出的新创建的卷容器。
 
