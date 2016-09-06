---
ms.openlocfilehash: 4c5dda9fdb5c0719d6fd270ccf4686b2aac3b563
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建卷容器"
   description="描述如何在 StorSimple 设备上创建一个卷的容器。"
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
   ms.date="04/01/2015"
   ms.author="v-sharos" />

#### 若要创建一个卷的容器

1. 在设备**快速启动**页中，单击**添加卷容器**。 显示**创建卷容器**对话框。

    ![创建卷容器](./media/storsimple-create-volume-container/HCS_CreateVolumeContainerM-include.png)

2. 在**创建卷容器**对话框中︰
  1. 提供卷容器的**名称**。 该名称必须是 3 到 32 个字符长。
  2. 选择要此卷容器相关联的**存储帐户**。 您可以选择在创建服务时生成的默认帐户。 您可以使用**添加新**选项来指定不会链接到此服务订阅的存储帐户。
  3. 选择**启用云存储加密**启用从设备发送到云中的数据加密。
  4. 提供并确认您的**云存储加密密钥**长度 8 到 32 个字符。 此项由设备用于访问加密的数据。
  5. 如果您想要使用所有可用的带宽，**指定带宽**下拉列表中选择**无限制**。 您还可以将此选项设置为**自定义**部署带宽控制并指定一个介于 1 和 1000 Mbps。 
  如果您有您可用的带宽使用情况信息，可能可以动态分配带宽，通过指定**选择带宽模板**基于的计划。 分步过程，请转到[带宽模板添加](https://msdn.microsoft.com/library/dn757746.aspx#addBT)。
  6. 单击检查图标 ![检查图标](./media/storsimple-create-volume-container/HCS_CheckIcon-include.png) 若要保存此卷容器并退出向导。 

  将**卷容器**页面上列出的新创建的卷容器。
