---
ms.openlocfilehash: 90baa88b2a85b8fab78b43d670008340266c2c00
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置和注册您的设备"
   description="介绍如何使用 Windows PowerShell 的 StorSimple 配置和注册 StorSimple 设备运行更新 1。"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/22/2015"
   ms.author="alkohli" />


### 若要配置和注册设备

1. 访问 StorSimple 设备串行控制台上的 Windows PowerShell 接口。 说明，请参阅[使用 PuTTY 连接到设备的串行控制台](#use-putty-to-connect-to-the-device-serial-console)。 **一定要完全遵循该过程或您将不能访问控制台。**

2. 在会话中打开的按 enter 键一次获取命令提示符。 

3. 系统将提示您选择您想要为您的设备设置的语言。 指定的语言，然后再按 Enter。 

    ![StorSimple 配置和注册设备 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)

4. 在串行控制台菜单显示，选择选项 1 登录具有完全访问权限。 

    ![StorSimple 注册设备 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
  
5. 执行以下步骤来配置您的设备的最小所需的网络设置。

    > [AZURE.IMPORTANT] 这些配置步骤需要执行活动设备的控制器上。 串行控制台菜单指示标题消息中的控制器状态。 如果不是连接到有效的控制器时，断开连接，然后连接到有效的控制器。

      1. 在命令提示符下，键入您的密码。 默认设备密码是**密码 1**。

      2. 键入以下命令︰

           `Invoke-HcsSetupWizard`

      3. 安装向导将帮助您配置该设备的网络设置。 提供以下信息︰ 

       - 数据 0 网络接口的 IP 地址
       - 子网掩码
       - 网关
       - 主 DNS 服务器的 IP 地址
       - 主 NTP 服务器的 IP 地址
 
        > [AZURE.NOTE] 您可能需要等待几分钟以子网掩码和 DNS 设置要应用。 

      4. （可选） 配置 web 代理服务器。

      > [AZURE.IMPORTANT] 尽管 web 代理服务器配置是可选的但请注意，如果您使用 web 代理服务器，您可以只配置它这里。 有关详细信息，请转到[配置 web 代理，为您的设备](https://msdn.microsoft.com/library/azure/dn764937.aspx)。 

6. 按 Ctrl + C 以退出安装向导。
 
7. 安装的更新，如下所示︰
      1. 使用以下 cmdlet 设置两个控制器的 Ip:

         `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`

      2. 在命令提示符下，运行`Get-HcsUpdateAvailability`。 应该会通知您有可用更新。

      3. 运行`Start-HcsUpdate`。 您可以在任何节点上运行此命令。 更新将应用第一个控制器上，控制器将进行故障转移，则更新将应用在另一个控制器上。

      您可以通过运行监视进度更新的`Get-HcsUpdateStatus`。    

       The following sample output shows the update in progress.
  
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

      它可能需要长达 11 小时应用所有更新，包括 Windows 更新。

8. 成功安装的所有更新之后，请运行以下 cmdlet 以确认正确地应用了软件更新︰

     `Get-HcsSystem`

    您应该看到以下版本︰
    - HcsSoftwareVersion: 6.3.9600.17491
    - CisAgentVersion: 1.0.9037.0
    - MdsAgentVersion: 26.0.4696.1433
 
9. 运行以下 cmdlet 以确认已正确应用固件更新︰

    `Start-HcsFirmwareCheck`.

     固件状态应该是**UpToDate**。

10. 运行以下 cmdlet 以指向 Microsoft Azure 政府门户设备 （因为它指向公共 Azure 管理门户默认情况下）。 这将重新启动这两种控制器。 我们建议使用两个 PuTTY 会话以便您可以看到每个控制器重新启动时，同时连接到这两个域控制器。

     `Set-CloudPlatform -AzureGovt_US`

    您将看到一条确认消息。 接受默认值 (**Y**)。

11. 运行以下 cmdlet 以继续运行安装程序︰

     `Invoke-HcsSetupWizard`

     ![继续安装向导](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)

    当您继续运行安装程序时，该向导将更新 1 版本 （它对应版本 17469）。 

12. 接受的网络设置。 接受的每个设置后，您将看到一条验证消息。
 
13. 出于安全考虑，设备管理员密码过期后的第一个会话，并将需要立即对其进行更改。 出现提示时，提供设备的管理员密码。 有效的设备管理员密码必须是 8 到 15 个字符之间。 密码必须包含的以下三种︰ 小写字母、 大写字母、 数字和特殊字符。

    <br/>![StorSimple 注册设备 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)

14. 在安装向导的最后一步使用 StorSimple 管理器服务注册您的设备。 对于这一点，您将需要获得的服务注册密钥[第 2 步︰ 获取服务注册密钥](storsimple-get-service-registration-key-gov.md)。 提供注册密钥后，您可能需要等待 2-3 分钟，然后注册该设备。

      > [AZURE.NOTE] 在任何时候，若要退出安装向导，您可以按 Ctrl + C。 如果已经输入了所有的网络设置 （数据 0、 子网掩码和网关的 IP 地址），将保留您的条目。

    ![StorSimple 注册进度](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)

15. 注册设备后，将出现一个服务的数据加密密钥。 复制此注册表项并将其保存在安全的地方。 **此注册表项将需要与服务的注册键 StorSimple 管理器服务中注册其他设备。** 有关此参数的详细信息，请参阅[StorSimple 安全](../articles/storsimple/storsimple-security.md)。
    
    ![StorSimple 注册设备 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    

      > [AZURE.IMPORTANT] 要从串行控制台窗口中复制文本，只需选择文本。 您应该能够将代码粘贴到剪贴板或任意文本编辑器。 
      > 
      > 不要使用 Ctrl + C 复制服务数据的加密密钥。 使用 Ctrl + C 将导致您可以退出安装向导。 因此，设备管理员密码不会被更改，该设备将恢复为默认的密码。

16. 退出的串行控制台。

17. 返回到政府门户网站，并完成以下步骤︰
  1. 双击您的 StorSimple 管理器服务以访问**快速启动**页。
  2. 单击**查看已连接的设备**。
  3. 在**设备**页中，验证设备已成功连接到该服务通过查找状态。 设备状态应该是**在线**。
   
        ![StorSimple 设备页面](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
  
        如果设备状态为**脱机**，请等待几分钟的设备联机。 
      
        如果几分钟后，该设备是仍处于脱机状态，则需要确保您的防火墙网络已配置[StorSimple 设备的网络要求](https://msdn.microsoft.com/library/dn772371.aspx)中所述。 如果您没有支持 HTTP 1.1，检查端口 9354 以确保它是打开用于出站通信。 此端口用于 StorSimple 管理器服务和 StorSimple 设备之间的通信。
     
        
