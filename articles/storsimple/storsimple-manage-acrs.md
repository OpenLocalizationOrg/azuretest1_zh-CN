---
ms.openlocfilehash: 5c57fe69bcdc1060220f9027bd6c5c864640dfe9
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="管理访问控制记录在 StorSimple |Microsoft Azure"
   description="描述如何使用访问控制记录 (ACRs) 来确定哪些主机可以连接到一个卷 StorSimple 设备上。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/01/2015"
   ms.author="alkohli" />

# 使用 StorSimple 管理器服务来管理访问控制记录

## 概述

访问控制记录 (ACRs) 允许您指定哪些主机可以连接到 StorSimple 设备上的卷。 ACRs 被设置为某个特定卷，包含主机的 iSCSI 限定名称 (Iqn)。 试图连接到卷的主机，设备会检查 ACR 与此卷以用于 IQN 名称，如果没有匹配项，则建立连接。 在**配置**页面上的访问控制记录部分显示与主机对应 Iqn 的所有访问控制记录。

本教程介绍了以下常见 ACR 相关任务︰

- 添加访问控制记录 
- 编辑访问控制记录 
- 删除访问控制记录 

> [AZURE.IMPORTANT] 
> 
> - 将 ACR 分配给卷，请注意，该卷没有同时访问多个非群集主机因为这可能会破坏该卷。 
> - 当 ACR 删除卷，请确保相应的主机不访问卷，因为删除操作可能会导致读写中断。

## 添加访问控制记录

您可以使用 StorSimple 管理器服务**配置**页添加 ACRs。 通常情况下，会将一个 ACR 与一个卷相关联。

执行以下步骤以添加 ACR。

#### 要添加访问控制记录

1. 在服务登录页上，选择您的服务，双击该服务名称，然后单击**配置**选项卡。

2. 在**访问控制记录**在表格列表中，您 ACR 提供一个**名称**。

3. 提供您的 Windows 主机**iSCSI 启动器名称**下的 IQN 名称。 若要获取 Windows Server 主机的 IQN，执行以下操作︰

   - 在 Windows 主机上启动 Microsoft iSCSI 发起程序。
   - 在 iSCSI 发起程序属性窗口在配置选项卡上选择并复制启动器名称字段中的字符串。
   - 管理门户中 ACRs 表上的**iSCSI 启动器名称**字段中粘贴此字符串。

4. 单击**保存**以保存新创建的 ACR。 表格列表将更新以反映此添加。

## 编辑访问控制记录

您可以使用在管理门户的**配置**页来编辑 ACRs。 

> [AZURE.NOTE] 您可以修改仅在当前不使用那些 ACRs。 若要编辑 ACR 与目前正在使用的卷，您必须首先将卷脱机。

执行以下步骤来编辑 ACR。

#### 若要编辑访问控制记录

1. 在服务登录页上，选择您的服务，双击该服务名称，然后单击**配置**选项卡。

2. 在访问控制记录的表格格式列表中，将鼠标悬停在您想要修改 ACR。

3. ACR 为提供一个新的名称和/或 IQN。

4. 单击**保存**以保存修改的 ACR。 表格列表将更新以反映此更改。

## 删除访问控制记录

管理门户中使用**配置**页中删除 ACRs。 

> [AZURE.NOTE] 可以删除只在当前不使用那些 ACRs。 若要删除 ACR 与目前正在使用的卷，您必须首先使卷脱机。

执行以下步骤以删除访问控制记录。

#### 若要删除访问控制记录

1. 在服务登录页上，选择您的服务，双击该服务名称，然后单击**配置**选项卡。

2. 在访问控制记录 (ACRs) 表格列表中，将鼠标悬停在要删除 ACR。

3. (**X**) 中的删除图标将至尊的右侧列中显示选择的 ACR。 单击要删除的 ACR 的**x**图标。

4. 当系统提示您确认时，单击**是**继续进行删除。 将更新的表格列表以反映删除。

## 下一步行动

了解更多关于[管理 StorSimple 卷](storsimple-manage-volumes.md)。

 