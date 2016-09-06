---
ms.openlocfilehash: 4564b86ef4e50a6410a3bbd67d3018d94257e6c8
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 适配器升级为 SharePoint |Microsoft Azure"
   description="介绍如何升级 SharePoint，然后安装 SharePoint StorSimple 适配器的新版本。"
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
   ms.date="07/13/2015"
   ms.author="v-sharos" />

### 升级到 SharePoint 2013 的 SharePoint 2010，然后安装 SharePoint StorSomple 适配器

>[AZURE.IMPORTANT] 通过 RBS 以前移动到外部存储任何文件将不可用，直到完成升级并重新启用了 RBS 功能。 若要限制对用户的影响，执行任何升级或重新安装在一个计划中的维护窗口期间。

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
#### 若要升级到 SharePoint 2013 的 SharePoint 2010，然后安装适配器

1. 在 SharePoint 2010 场中，外置的 Blob 和内容数据库为其启用 RBS 注意 BLOB 存储路径。 

2. 安装和配置新的 SharePoint 2013 场。 

3. 将数据库、 应用程序和网站集从 SharePoint 2010 场移到新的 SharePoint 2013 场。 有关说明，请转到 SharePoint 2013 升级过程的概述。

4. 安装新场 SharePoint StorSimple 适配器。 有关过程，请转到[安装 SharePoint StorSimple 适配器](#install-the-storsimple-adapter-for-sharepoint)。

5. 使用您在步骤 1 中记下的信息，RBS 让一组相同的内容数据库，并提供在 SharePoint 2010 安装中使用相同的 BLOB 存储路径。 有关过程，请转到[配置 RBS](#configure-rbs) 。 完成此步骤后，以前外置的文件应该可以访问新的服务器场。 

### 针对 SharePoint 升级 StorSimple 适配器

>[AZURE.IMPORTANT] 您应计划升级计划中的维护窗口期间发生，原因如下︰
>
>- 以前外置的内容不能重新安装适配器之前。
>
>- 但为 SharePoint，卸载以前的版本的 StorSimple 适配器后才能安装新版本，上载到网站的任何内容将存储在内容数据库中。 您将需要安装新的适配器后，将内容移到 StorSimple 设备。 


#### SharePoint 升级 StorSimple 适配器 

1. SharePoint 的卸载早期版本的 StorSimple 适配器。

    >[AZURE.NOTE] 这会自动将内容数据库上禁用 RBS。 但是，现有的 Blob 仍在 StorSimple 设备上。 因为 RBS 禁用，Blob 尚未迁移到内容数据库时，这些 Blob 任何请求将失败。 
 
2. 为 SharePoint 中安装新的 StorSimple 适配器。 新的适配器将自动识别以前启用或禁用的 RBS 的内容数据库，并将使用以前的设置。
