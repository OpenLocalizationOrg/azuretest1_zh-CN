---
ms.openlocfilehash: 573666bcd9a74736d6ed642e8865a599b9223e58
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
    pageTitle="设置内部部署 VMware 站点之间的保护" 
    description="本文用于配置两个 VMware 站点使用 Azure 站点恢复之间的保护。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/05/2015" 
    ms.author="raynew"/>


# 设置内部部署 VMware 站点之间的保护


## 概述

在 Azure 站点恢复 InMage 侦察提供实时内部部署 VMware 站点之间的复制。 InMage 侦察包含在 Azure 站点恢复服务预订。


## 先决条件

- **Azure 帐户**— — 您需要[Microsoft Azure](http://azure.microsoft.com/)帐户。 您可以[免费试用版](pricing/free-trial/)开始。


## 步骤 1︰ 创建存储库

1. 登录到[管理门户](https://portal.azure.com)。
2. 单击**数据服务** > **恢复服务** > **站点恢复存储库**。
3. 单击**创建新** > **快速创建**。
4. 在**名称**输入好记的名称来标识该存储库。
5. 在**地区**选择该存储库的地理区域。 检查支持的地区，请参阅地理[Azure 站点恢复定价详细信息](pricing/details/site-recovery/)中的可用性。

检查状态栏，以确认已成功创建存储库。 存储库将被列为**活动**在恢复服务的主页上。

## 步骤 2︰ 配置该存储库并下载 InMage 侦察组件

1. 单击**创建存储库**。
2. 在**恢复服务**页中，单击存储库，以打开快速启动页。
3. 在下拉列表中，选择**两个内部部署 VMware 站点之间**。
4. 下载 InMage 侦察。 下载的 zip 文件中的所有必需的组件安装程序文件。


## 步骤 3︰ 安装组件的更新

了解最新的[更新](#updates)。 您将按以下顺序安装更新文件︰

1. RX 服务器，如果有的话
2. 配置服务器
3. 进程内服务器
3. 主目标服务器。
4. vContinuum 服务器。

按照以下步骤安装︰

1. 下载[更新](http://download.microsoft.com/download/9/F/D/9FDC6001-1DD0-4C10-BDDD-8A9EBFC57FDF/ASR Scout 8.0.1 Update1.zip)的 zip 文件。 此压缩文件包含以下文件︰

    -  RX_8.0.1.0_GA_Update_1_3279231_23Jun15.tar.gz
    -  CX_Windows_8.0.1.0_GA_Update_1_3259146_23Jun15.exe
    -  UA_Windows_8.0.1.0_GA_Update_1_3259401_23Jun15.exe
    -  UA_RHEL6 64_8.0.1.0_GA_Update_1_3259401_23Jun15.tar.gz
    -  vCon_Windows_8.0.1.0_GA_Update_1_3259523_23Jun15.exe
2. 将 zip 文件解压缩。
2. **RX 服务器**︰ 复制**RX_8.0.1.0_GA_Update_1_3279231_23Jun15.tar.gz**到 RX 服务器并将其解压。 ****安装/运行提取文件夹中。
2. **服务器/流程服务器配置**︰ 配置服务器和进程内服务器副本**CX_Windows_8.0.1.0_GA_Update_1_3259146_23Jun15.exe** 。 双击以运行它。
3. **Windows 主目标服务器**︰ 要与主目标服务器更新统一的代理副本**UA_Windows_8.0.1.0_GA_Update_1_3259401_23Jun15.exe** 。 双击它以运行它。 请注意，Windows 的统一的代理并不适用在源服务器上。 应在 Windows 主机的目标服务器上安装它。
4. **Linux 主目标服务器**︰ 要更新到主目标服务器**UA_RHEL6 64_8.0.1.0_GA_Update_1_3259401_23Jun15.tar.gz**的统一的代理副本并将其解压。 ****安装/运行提取文件夹中。
5. **vContinuum 服务器**︰ 将**vCon_Windows_8.0.1.0_GA_Update_1_3259523_23Jun15.exe**复制到 vContinuum 服务器。 请确保已关闭 vContinuum 向导。 双击该文件以运行它。

## 步骤 4︰ 设置复制
5. 设置源之间复制和 VMware 网站为目标。
6. 有关指导，与产品一起使用下载的 InMage 侦察文档。 也可以访问文档，如下所示︰

    - [发行说明](http://download.microsoft.com/download/4/5/0/45008861-4994-4708-BFCD-867736D5621A/InMage_Scout_Standard_Release_Notes.pdf)
    - [兼容性矩阵](http://download.microsoft.com/download/C/D/A/CDA1221B-74E4-4CCF-8F77-F785E71423C0/InMage_Scout_Standard_Compatibility_Matrix.pdf)
    - [用户指南](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf)
    - [RX 用户指南](http://download.microsoft.com/download/A/7/7/A77504C5-D49F-4799-BBC4-4E92158AFBA4/InMage_ScoutCloud_RX_User_Guide_8.0.1.pdf)
    - [快速安装指南](http://download.microsoft.com/download/6/8/5/685E761C-8493-42EB-854F-FE24B5A6D74B/InMage_Scout_Standard_Quick_Install_Guide.pdf)


## 更新

### ASR 侦察 8.0.1 版更新 1

此最新的更新包括错误修复和新功能︰

- 每个服务器实例的自由保护的 31 天。 这使您可以测试功能，或者设置一个概念证明。
    - 在服务器上，包括故障转移和故障自动恢复的所有操作都是免费的从第一次使用 ASR 侦察保护服务器的时间开始第 31 天。
    - 从 32nd 一天开始，每个受保护的服务器都征收标准实例速率用于向客户负责网站 Azure 站点恢复保护。
    - 在任何时候，目前收取的受保护的服务器数是 Azure 站点恢复存储库的仪表板页面上可用。
- 添加有关 vCLI 5.5 更新 2 支持。
- 添加用于在源服务器上的 Linux 操作系统的支持︰
    - RHEL 6 更新 6
    - RHEL 5 更新 11
    - CentOS 6 更新 6
    - CentOS 5 更新 11
- Bug 修复程序解决了下列问题︰
    - 配置服务器或 RX 服务器，存储库注册将失败。
    - 预期的那样群集虚拟机的 reprotected 恢复运行时，不会出现群集卷。
    - 切换回主目标服务器位于内部生产虚拟机的不同 ESXi 服务器失败。
    - 升级到 8.0.1 版，影响保护和操作时，会更改配置文件的权限。
    - 重新同步阈值不实施按预期的方式，从而导致不一致的复制行为。
    - RPO 设置配置服务器界面中无法正确显示。 未压缩的数据值不正确地显示压缩的值。
    -  删除操作不会删除 vContinuum 向导中按预期方式和复制不会删除从配置服务器界面。
    -  VContinuum 在向导磁盘时，自动选定在 MSCS 虚拟机保护期间在磁盘视图中单击**详细信息**。
    - P2V 方案需要 HP 服务期间如 CIMnotify、 CqMgHost 并没有移动到手册中恢复虚拟机，从而是其他引导时间。
    - 当主目标服务器上有 26 个以上的磁盘时，Linux 虚拟机保护无法正常工作。
    
## 下一步行动

在[Azure 恢复服务论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)上发布的任何问题。 < 
