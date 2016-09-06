---
ms.openlocfilehash: 5ba3d1edd19ef131384c51033ba990728e6562ec
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置和注册您的设备"
   description="介绍如何使用 Windows PowerShell 的 StorSimple 配置和注册您的设备运行更新 1。"
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
   ms.date="05/26/2015"
   ms.author="alkohli" />


### 若要配置和注册设备

1. 访问 StorSimple 设备串行控制台上的 Windows PowerShell 接口。 说明，请参阅[使用 PuTTY 连接到设备的串行控制台](#use-putty-to-connect-to-the-device-serial-console)。 **一定要完全遵循该过程或您将不能访问控制台。**

2. 在会话中打开的按 enter 键一次获取命令提示符。 

3. 系统将提示您选择您想要为您的设备设置的语言。 指定的语言，然后再按 Enter。 

    ![StorSimple 配置和注册设备 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)

4. 在串行控制台菜单显示，选择选项 1 登录具有完全访问权限。 

    ![StorSimple 注册设备 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
  
     完成步骤 5-12 配置所需的最小网络设置您的设备。 **这些配置步骤需要执行活动设备的控制器上。** 串行控制台菜单指示标题消息中的控制器状态。 如果您未连接到有效的控制器，断开连接，然后连接到有效的控制器。

5. 在命令提示符下，键入您的密码。 默认设备密码是**密码 1**。

6. 键入下面的命令︰ `Invoke-HcsSetupWizard`。 

7. 安装向导将帮助您配置该设备的网络设置。 提供以下信息︰ 
   - 数据 0 网络接口的 IP 地址
   - 子网掩码
   - 网关
   - 主 DNS 服务器的 IP 地址
    
        注意︰ 系统流程中的每一步之后验证网络设置。
   
      > [AZURE.NOTE] 您可能需要等待几分钟以子网掩码和 DNS 设置要应用。 如果您收到"请检查网络连接数据 0 到"错误消息，请检查活动控制器数据 0 的网络接口上的物理网络连接。

8. （可选） 配置 web 代理服务器。 尽管 web 代理服务器配置是可选的但**请注意，如果使用 web 代理服务器，则可以仅配置在此处**。 有关详细信息，请转到[配置 web 代理，为您的设备](https://msdn.microsoft.com/library/azure/dn764937.aspx)。

9. 配置主 NTP 服务器为您的设备。 NTP 服务器，则需要为您的设备必须同步时间，以便它可以与云服务提供商中进行身份验证。 请确保您的网络允许 NTP 通信通过从您的数据中心到互联网。 这是不可能的如果指定的内部的 NTP 服务器。 
 
10. 出于安全考虑，设备管理员密码过期后的第一个会话，并将需要立即对其进行更改。 出现提示时，提供设备的管理员密码。 有效的设备管理员密码必须是 8 到 15 个字符之间。 密码必须包含的以下三种︰ 小写字母、 大写字母、 数字和特殊字符。

    <br/>![StorSimple 注册设备 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)

11. 在安装向导的最后一步使用 StorSimple 管理器服务注册您的设备。 为此，您需要在步骤 2 中获得的服务注册密钥。 提供注册密钥后，您可能需要等待 2-3 分钟，然后注册该设备。

      > [AZURE.NOTE] 在任何时候，若要退出安装向导，您可以按 Ctrl + C。 如果已经输入了所有的网络设置 （数据 0、 子网掩码和网关的 IP 地址），将保留您的条目。

    ![StorSimple 注册设备 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)

12. 注册设备后，将出现一个服务的数据加密密钥。 复制此注册表项并将其保存在安全的地方。 **此注册表项将需要与服务的注册键 StorSimple 管理器服务中注册其他设备。** 有关此参数的详细信息，请参阅[StorSimple 安全](../articles/storsimple/storsimple-security.md)。
    
    ![StorSimple 注册设备 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    

      > [AZURE.NOTE] 要从串行控制台窗口中复制文本，只需选择文本。 您应该能够将代码粘贴到剪贴板或任意文本编辑器。 不要使用 Ctrl + C 复制服务数据的加密密钥。 使用 Ctrl + C 将导致您可以退出安装向导。 因此，设备管理员密码不会被更改，该设备将恢复为默认的密码。

13. 退出的串行控制台。

14. 返回到管理门户，并完成以下步骤︰
  1. 双击您的 StorSimple 管理器服务以访问**快速启动**页。
  2. 单击**查看已连接的设备**。
  3. 在**设备**页中，验证设备已成功连接到该服务通过查找状态。 设备状态应该是**在线**。
   
        ![StorSimple 设备页面](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
  
        如果设备状态为**脱机**，请等待几分钟的设备联机。 
      
        如果几分钟后，该设备是仍处于脱机状态，则需要确保您的防火墙网络已配置[StorSimple 设备的网络要求](https://msdn.microsoft.com/library/dn772371.aspx)中所述。 如果您没有支持 HTTP 1.1，检查端口 9354 以确保它是打开用于出站通信。 此端口用于 StorSimple 管理器服务和 StorSimple 设备之间的通信。
     
       