---
ms.openlocfilehash: 7ed7f1e8fd7d08ff4206474d08877a29d0c60234
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="StorSimple 的故障切换和灾难恢复 |Microsoft Azure"
   description="了解故障转移您的 StorSimple 设备与自身、 另一个物理设备或虚拟设备。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="adinah"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/28/2015"
   ms.author="alkohli" />

# StorSimple 设备的故障切换和灾难恢复

## 概述

本教程描述在发生灾难时进行故障转移的 StorSimple 设备所需的步骤。 故障切换将允许您将数据从数据中心中的源设备迁移到另一个物理或甚至虚拟设备位于相同或不同的地理位置。 设备故障转移灾难恢复 (DR) 功能通过统一协调，从设备页启动。 此页列出所有连接到您的 StorSimple 管理器服务的 StorSimple 设备。 每个设备显示的友好名称、 状态、 资源调配和大容量、 类型和型号。

![设备页面](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

## 灾难恢复 (DR) 和设备故障转移

在灾难恢复 (DR) 方案中，主要的设备停止工作。 在此情况下，您可以移动云数据作为*源*中使用主要设备并指定另一个设备作为*目标*与故障设备连接到另一台设备。 您可以选择一个或多个卷容器将迁移到目标设备。 此过程称为*故障转移*。 在故障转移期间来自源设备的卷容器更改所有权和传输到目标设备。

## 设备故障切换的注意事项

在发生灾难时，您可以选择您的 StorSimple 设备出现故障︰

- 到一个物理设备 
- 给自己
- 到虚拟设备

对于任何设备故障转移，请记住以下原则︰

- 灾难恢复的先决条件是卷容器内的所有卷都处于脱机状态且卷容器具有关联云快照。 
- 用于灾难恢复的可用目标设备有足够的空间来容纳所选的卷容器设备。 
- 设备连接到您的服务，但不是符合条件的足够的空间，不能作为目标设备。

## 故障转移到另一个物理设备

执行以下步骤来将您的设备恢复到目标的物理设备。

1. 验证故障转移所需的卷容器相关联云快照。

1. 在**设备**页中，单击**卷容器**选项卡。

1. 选择您想要故障转移到另一台设备的卷容器。 请单击该卷容器显示在此容器内的卷的列表。 选择一个卷，然后单击**脱机**脱机卷。 重复此过程，卷容器中的所有卷。

1. 对您想要故障转移到另一台设备的所有卷容器重复上面的步骤。

1. 在**设备**页中，单击**故障转移**。

1. 在向导中**选择故障转移到的卷容器**下，打开︰

    1. 在卷容器的列表中，选择您想要进行故障转移的卷容器。

        >[AZURE.NOTE] **显示仅包含相关联的云快照和离线的卷卷容器。**

    1. 下**选择目标设备**的选定容器中的卷中，从下拉列表中可用的设备中选择目标设备。 只有具有可用容量的设备将显示在下拉列表中。

    1. 最后，检查**确认故障转移**下的所有故障切换设置。 单击检查图标![检查图标](./media/storsimple-device-failover-disaster-recovery/IC740895.png)。

1. 故障转移完成后，请转到**设备**页。                                         

    1. 选择用作故障转移过程的目标设备的设备。

    1. 转至**卷容器**页面。 应列出所有卷容器，以及旧设备中的卷。

## 使用单个设备的故障切换

如果您只有一台设备并需要执行故障切换，请执行以下步骤。

1. 在设备中采用云的所有卷的快照。

1. 重置设备到出厂默认值。 按照中[如何重置为出厂默认设置的 StorSimple 设备](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings)的详细的说明。

1. 配置设备并将其注册到再次 StorSimple 管理器服务。

1. 在**设备**页中，旧设备应显示为**脱机**。 新注册的设备应显示为**联机**。

1. 对于新的设备，请首先完成设备的最低配置。 
                                                
    >[AZURE.IMPORTANT] **如果未首先完成的最小配置，您的灾难恢复将失败的当前实现中的错误的结果。 这种情况将在以后的版本中得到解决。**

1. 选择旧设备 （脱机状态），然后单击**故障转移**。 在向导中显示，故障转移此设备，为新注册的设备指定的目标设备。 有关详细说明，请参阅[故障转移到另一个物理设备](#fail-over-to-another-physical-device)。

1. 您可以监视从**作业**页将创建设备还原作业。

1. 作业成功完成后，访问新设备并导航到**卷容器**页面。 旧设备中的所有卷容器现在应该都迁移到新设备。

## 故障转移到 StorSimple 虚拟设备

您必须具有 StorSimple 虚拟设备创建和配置之前运行该过程。
 
>[AZURE.NOTE] **在此版本中，StorSimple 虚拟设备上受支持量是存储的 30 TB。**

执行以下步骤以还原到目标 StorSimple 虚拟设备的设备。

1. 验证故障转移所需的卷容器相关联云快照。

1. 在**设备**页中，单击**卷容器**选项卡。

1. 选择您想要故障转移到另一台设备的卷容器。 请单击该卷容器显示在此容器内的卷的列表。 选择一个卷，然后单击**脱机**脱机卷。 重复此过程，卷容器中的所有卷。

1. 对您想要故障转移到另一台设备的所有卷容器重复上面的步骤。

1. 在**设备**页中，单击**故障转移**。

1. 在向导中**选择故障转移到的卷容器**下, 打开，完成以下任务︰
                                                    
    一。 在卷容器的列表中，选择您想要进行故障转移的卷容器。

    >[AZURE.NOTE] **显示仅包含相关联的云快照和离线的卷卷容器。**

    b。 在**选择目标设备的选定容器中的卷**中，从下拉列表中可用的设备中选择 StorSimple 虚拟设备。 只有有足够容量的设备将显示在下拉列表中。  
    
    >[AZURE.NOTE] **如果您的物理设备正在运行更新 1，您可以故障转移到一个虚拟的设备更新 1 只。 如果目标虚拟设备运行的较低的软件版本，您将看到错误目标设备软件需要更新的效果。**

1. 最后，检查**确认故障转移**下的所有故障切换设置。 单击检查图标![检查图标](./media/storsimple-device-failover-disaster-recovery/IC740895.png)。

1. 故障转移完成后，请转到**设备**页。
                                                    
    一。 选择用作故障转移过程的目标设备的 StorSimple 虚拟设备。
    
    b。  转至**卷容器**页面。 现在应列出所有卷容器，以及从旧设备的卷。

## 业务连续性和灾难恢复 (BCDR)

整个 Azure 数据中心停止工作时发生了业务连续性灾难恢复 (BCDR) 方案。 这可能会影响您的 StorSimple 管理器服务和相关的 StorSimple 设备。

如果没有灾难发生前刚刚注册的 StorSimple 设备，则这些 StorSimple 设备可能需要经过出厂重置。 在发生灾难之后 StorSimple 设备将显示为脱机。 StorSimple 设备必须删除从门户，并应执行出厂重置，跟新注册。

## 下一步行动

执行故障切换后，您可能需要︰

- [停用您的 StorSimple 设备](storsimple-deactivate-and-delete-device.md#deactivate-a-device)
- [删除 StorSimple 设备](storsimple-deactivate-and-delete-device.md#delete-a-device)

有关如何管理您的设备使用 StorSimple 管理器服务，请转到[使用 StorSimple 管理器来管理您的 StorSimple 设备的服务](storsimple-manager-service-administration.md)。
 
