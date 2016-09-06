---
ms.openlocfilehash: c98e36f0bda3e9223d8b929e4f449ce3871c1a4b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理您的 StorSimple 卷容器 |Microsoft Azure"
   description="说明如何使用 StorSimple 管理器服务卷容器页以添加、 修改或删除一个卷的容器。"
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
   ms.date="09/02/2015"
   ms.author="v-sharos" />

# 使用 StorSimple 管理器服务管理 StorSimple 卷容器

## 概述

本教程介绍了如何使用 StorSimple 管理器服务创建和管理 StorSimple 卷容器。

在 Microsoft Azure StorSimple 设备中的卷容器包含共享存储帐户、 加密和带宽消耗设置的一个或多个卷。 一个设备可以有多个卷容器的所有卷。 

一个卷容器具有下列特性︰

- **卷**卷容器中包含的精简资源调配 StorSimple 卷。 一个卷的容器可能包含最多 256 个精简资源调配的 StorSimple 卷。

- **加密**--可以定义为每个卷容器的加密密钥。 此密钥用于加密的数据来从 StorSimple 设备发送到云中。 使用用户输入密钥使用军用级别 AES 256 位的密钥。 若要保护您的数据，我们建议您始终启用云存储加密。

- **存储帐户**– 存储帐户链接到您的云存储服务提供商。 驻留在卷容器中的所有卷都共享此存储帐户。 您可以从现有列表中，选择存储帐户或创建一个新帐户，当您创建卷容器，然后指定该帐户的访问凭据。

- **云的带宽**— 带宽消耗设备，在设备中的数据发送到云时。 您可以通过指定一个介于 1 到 1000 Mbps 定义此容器时实施带宽控制。 如果您想要使用所有可用的带宽的设备，请将此字段设置为无限制。 您还可以创建和应用带宽模板分配带宽基于计划。

![卷的容器页面](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

这下面的过程介绍如何使用 StorSimple**卷容器**页面完成以下常见的操作︰

- 添加卷容器 
- 修改卷容器 
- 删除卷容器 

## 添加卷容器

执行以下步骤来创建卷容器。

[AZURE.INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]


## 修改卷容器

执行下列步骤来修改卷容器。

[AZURE.INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]


## 删除卷容器

一个卷容器有卷内。 只有当被首先删除其中包含的所有卷，它可以被都删除。 执行以下步骤来删除一个卷的容器。

[AZURE.INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## 下一步行动

了解更多关于[管理 StorSimple 卷](storsimple-manage-volumes.md)。 
 