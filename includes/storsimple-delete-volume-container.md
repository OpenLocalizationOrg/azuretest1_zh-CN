---
ms.openlocfilehash: 45094c8cc4308f4fdc4df1f2d475aacc5963812b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="删除 StorSimple 卷容器"
   description="说明如何使用 StorSimple 管理器服务卷容器页面删除卷容器。"
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


#### 若要删除一个卷的容器

1. 在**设备**页中，选择的设备，双击它，然后单击**卷容器**选项卡。

2. 选择要删除的卷容器。

3. 如果卷容器有没有相关联的卷，然后将其删除。 单击**删除**页面底部可删除该容器。 当提示您确认，请单击**是**。 这将删除卷容器。

如果卷容器具有关联的卷，您首先需要[采取离线的卷](../articles/storsimple/storsimple-manage-volumes.md#take-a-volume-offline)中的步骤，使这些卷脱机。 离线卷后，可以删除它们。 如上面所述，当卷容器没有相关联的卷时，删除卷容器。
