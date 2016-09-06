---
ms.openlocfilehash: 9da6103b99315bdd0477a3f8c3c0fc8db8970e53
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="配置和注册您的设备"
   description="介绍如何使用 Windows PowerShell 的 StorSimple 配置和注册您的设备。"
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
   ms.date="08/05/2015"
   ms.author="v-sharos" />


#### 若要配置和注册设备

1. 访问 StorSimple 设备串行控制台上的 Windows PowerShell 接口。 说明，请参阅[使用 PuTTY 连接到设备的串行控制台](#use-putty-to-connect-to-the-device-serial-console)。 **一定要完全遵循该过程或您将不能访问控制台。**

2. 在会话中打开的按 enter 键一次获取命令提示符。 

3. 系统将提示您选择您想要为您的设备设置的语言。 指定的语言，然后再按 Enter。 

    ![StorSimple 配置和注册设备 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)

4. 在串行控制台菜单显示，选择选项 1 登录具有完全访问权限。 

    ![StorSimple 注册设备 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
  
     完成步骤 5-12 配置所需的最小网络设置您的设备。 **这些配置步骤需要执行活动设备的控制器上。** 串行控制台菜单指示标题消息中的控制器状态。 如果您未连接到有效的控制器，断开连接，然后连接到有效的控制器。

5. 在命令提示符下，键入您的密码。 默认设备密码是**密码 1**。

6. 键入以下命令︰

     `Invoke-HcsSetupWizard` 

7. 安装向导将帮助您配置该设备的网络设置。 提供以下信息︰ 
   - 数据 0 网络接口的 IP 地址
   - 子网掩码
   - 网关
   - 主 DNS 服务器的 IP 地址
   - 主 NTP 服务器的 IP 地址
   
      > [AZURE.NOTE] 您可能需要等待几分钟以子网掩码和 DNS 设置要应用。 如果您收到"设备未就绪。" 错误消息，请检查活动控制器的物理网络连接数据 0 的网络接口上。

8. （可选） 配置 web 代理服务器。 尽管 web 代理服务器配置是可选的但**请注意，如果使用 web 代理服务器，则可以仅配置在此处**。 有关详细信息，请转到[配置 web 代理，为您的设备](https://msdn.microsoft.com/library/azure/dn764937.aspx)。 如果在此步骤期间遇到任何问题，请参阅[web 代理服务器配置期间发生的错误](storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings)的故障排除指南。
 

      > [AZURE.NOTE] 在任何时候，若要退出安装向导，您可以按 Ctrl + C。 发出此命令之前应用的任何设置将被保留。

9. 出于安全考虑，设备管理员密码过期后的第一个会话，您将需要将其更改为后续会话。 出现提示时，提供设备的管理员密码。 有效的设备管理员密码必须是 8 到 15 个字符之间。 密码必须包含小写字符、 大写字符、 数字和特殊字符的组合。

10. 这里还设置 StorSimple 快照管理器密码。 当通过运行 StorSimple 快照管理器查看 Windows 主机身份验证设备，您可以使用此密码。 出现提示时，提供了 14 到 15 个字符的密码。 密码必须包含下列三个组合︰ 小写字母、 大写字母、 数字和特殊字符。 

    ![StorSimple 注册设备 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)

    您可以重置 StorSimple 管理器服务接口的 StorSimple 快照管理器密码。 有关详细步骤，请转到[更改 StorSimple 密码使用 StorSimple 管理器服务](storsimple-change-passwords.md)。

    要在这一步解决任何问题，请参阅故障排除指南[与密码相关的错误](storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords)。

11. 在安装向导的最后一步使用 StorSimple 管理器服务注册您的设备。 为此，您需要在步骤 2 中获得的服务注册密钥。 提供注册密钥后，您可能需要等待 2-3 分钟，然后注册该设备。

    要解决任何可能的设备注册失败，请参阅[设备注册期间发生的错误](storsimple-troubleshoot-deployment.md#errors-during-device-registration)。 有关详细的疑难解答，您也可以指[逐步故障诊断的示例](storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example)。

12. 注册设备后，将出现一个服务的数据加密密钥。 复制此注册表项并将其保存在安全的地方。
    
    > [AZURE.WARNING] 此注册表项将需要与服务的注册键 StorSimple 管理器服务中注册其他设备。 有关此参数的详细信息，请参阅[StorSimple 安全](../articles/storsimple/storsimple-security.md)。

     ![StorSimple 注册设备 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)

     要从串行控制台窗口中复制文本，只需选择文本。 您应该能够将代码粘贴到剪贴板或任意文本编辑器。 不要使用 Ctrl + C 复制服务数据的加密密钥。 使用 Ctrl + C 将导致您可以退出安装向导。 因此，将不会更改设备管理员密码和 StorSimple 快照管理器密码和设备将恢复为默认密码。

13. 退出的串行控制台。

14. 返回到管理门户，并完成以下步骤︰
  1. 双击您的 StorSimple 管理器服务以访问**快速启动**页。
  2. 单击**查看已连接的设备**。
  3. 在**设备**页中，验证设备已成功连接到该服务通过查找状态。 设备状态应该是**在线**。 如果设备状态为**脱机**，请等待几分钟的设备联机。
   
    ![StorSimple 设备页面](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
  
      > [AZURE.IMPORTANT] 设备处于联机状态后，必须在开始此步骤已拔下网络电缆插入。

设备已成功注册并不联机后，您可以运行`Test-HcsmConnection -Verbose`以确保网络连接正常。 此 cmdlet 的详细用法，请转到[测试 HcsmConnection 的 cmdlet 参考](https://technet.microsoft.com/library/dn715782.aspx)。
