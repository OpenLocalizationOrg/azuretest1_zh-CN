---
ms.openlocfilehash: 8d6122577e7fd46135b1f7d5b6ac8370c8354e11
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="选项 1︰ 使用 Windows PowerShell 的 StorSimple 安装更新 1"
   description="说明如何使用 StorSimple 的 Windows PowerShell 安装 StorSimple 8000 系列更新 1。"
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
   ms.date="05/22/2015"
   ms.author="v-sharos" />

#### StorSimple 从 Windows PowerShell 安装更新 1

1. 执行以下步骤来下载软件更新。

    1. 启动 Internet Explorer，定位到[http://catalog.update.microsoft.com/v7/site/Home.aspx](http://catalog.update.microsoft.com/v7/site/Home.aspx)。
    2. 如果您是初次使用的用户，系统将提示您安装 Microsoft 更新目录。 单击**安装**。
    
        ![安装目录](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)

    3. 您将看到一个目录搜索屏幕。 在搜索框中输入**3063418** ，然后单击**搜索**。

        ![搜索目录](./media/storsimple-install-update-option-1/HCS_SearchCatalog-include.png)

    4. 您将看到**StorSimple 更新 1.0 装置更新**包。 单击**添加**。 此更新将添加到购物篮。 

        ![更新包](./media/storsimple-install-update-option-1/HCS_UpdateBundle-include.png) 

    5. 单击**查看选择篮**。
 
        ![查看选择篮](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png) 

    6. 单击**下载**。 指定或浏览到您想下载显示本地位置。 将在 StorSimple 更新 1.0 装置更新包 (KB3063418) 下载更新 (所有 hcsmdssoftwareupdate_288da2cc8cd2e3c3958b603a79346cb586fb8fe3.exe)"到所选位置的文件夹。 也可以将文件夹复制到从设备可访问的网络共享。
        
2. 若要安装软件更新，请访问 StorSimple 设备串行控制台上的 Windows PowerShell 接口。 按照中[使用 PuTTy 连接到串行控制台](#use-putty-to-connect-to-the-serial-console)的详细的说明。

3. 在命令提示符处，按 enter 键。

4. 选择**选项 1**以登录到具有完全访问权限的设备。

5. 要安装此更新程序包，在命令提示符下，键入︰

    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`

    仅当您访问一个已通过身份验证的共享使用凭据参数。

    示例输出如下所示。

        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
      
        Confirm

        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not 
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y

        ````
 
6. 键入**Y**当系统提示您确认安装修复程序。

7. 通过使用 Get HcsUpdateStatus cmdlet 监视更新。

    下面的示例输出显示更新正在进行中。

        ````
        Controller0>Get-HcsUpdateStatus
        RunInprogress       : True
        LastHotfixTimestamp : 4/13/2015 10:56:13 PM
        LastUpdateTimestamp : 4/13/2015 10:35:25 PM
        Controller0Events   :
        Controller1Events   : 
        ````
 
     下面的示例输出表明该更新已完成。

        ````
        Controller1>Get-HcsUpdateStatus

        RunInprogress       : False
        LastHotfixTimestamp : 4/13/2015 10:56:13 PM
        LastUpdateTimestamp : 4/13/2015 10:35:25 PM
        Controller0Events   :
        Controller1Events   :

        ````
 
8. 软件更新过程完成后，导航到维护页面管理门户中。 扫描可用的更新。 您应该看到更多的软件更新可用。

9. 单击**安装更新**从门户中应用所有可用的软件更新。 

10. 软件更新完成后，验证系统的软件、 驱动程序和固件版本。 键入以下命令︰

    `Get-HcsSystem`

    您应该看到以下版本︰

    - HcsSoftwareVersion: 6.3.9600.17491
    - CisAgentVersion: 1.0.9037.0
    - MdsAgentVersion: 26.0.4696.1433 
 
11. 要验证已正确更新固件，请键入︰

    `Start-HcsFirmwareCheck`

    固件状态应该是**UpToDate**。
