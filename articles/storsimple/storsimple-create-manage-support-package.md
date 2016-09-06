---
ms.openlocfilehash: c60fd206f43130c36729c4bc1b5a8cac470400a7
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建一个 StorSimple 支持包 |Microsoft Azure"
   description="了解如何创建、 解密，以及编辑 StorSimple 设备的支持包。"
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
   ms.date="08/31/2015"
   ms.author="alkohli" />


# 创建和管理 StorSimple 支持包

## 概述

本教程描述与创建和管理支持包相关的各种任务。 支持包加密、 压缩的格式包括所有相关的日志，并可用来帮助 Microsoft 技术支持小组提供任何 StorSimple 设备问题进行故障排除。

本教程包含创建和使用管理的支持包的分步说明:

- 页中的 StorSimple 管理器服务的**维护****支持包**部分
- StorSimple 的 Windows PowerShell

先阅读本教程中，您将能够︰

- 创建支持包
- 解密和编辑支持包


## 在管理门户中创建支持包

若要解决 StorSimple 管理器服务，您可能遇到的任何问题，可以创建并上载到 Microsoft 支持网站通过管理门户中服务的**维护**页支持包。 您将需要提供支持密钥以允许用户上载。 由您的电子邮件中的支持工程师，应该向您提供支持的密钥。 （.cab 文件） 创建的未加密、 压缩的支持包。 工程师提供的密钥时，然后可以从支持网站支持工程师检索此包。

管理门户创建支持包中执行以下步骤︰

#### 在管理门户中创建支持包

1. 定位到**设备 > 维护**。

2. 在**支持包**部分中，单击**创建并上载支持包**。

3. 在**创建并上载支持包**对话框中，执行下列操作︰

    ![创建支持包](./media/storsimple-create-manage-support-package/IC740923.png)
                                            
    - 提供**支持的密钥**。 通过您的电子邮件中的 Microsoft 技术支持工程师，此项应发送给您。
    
    - 选中组合框提供**自动上载到 Microsoft 支持网站的支持包**的同意。
    
    - 单击检查图标 ![检查图标](./media/storsimple-create-manage-support-package/IC740895.png).


## StorSimple 中 Windows PowerShell 创建支持包

如果需要编辑您的日志文件，然后再创建一个包，您需要为 StorSimple 创建您的软件包通过 Windows PowerShell。 

执行以下步骤来创建 StorSimple 在 Windows PowerShell 支持包︰


#### StorSimple 中 Windows PowerShell 创建支持包

1. 键入以下命令来启动 Windows PowerShell 会话作为远程计算机上的管理员用来连接到您的 StorSimple 设备︰

    `Start PowerShell`

2. 在 Windows PowerShell 会话中，连接到您的设备的 SSAdmin 控制台运行空间︰ 


    - 在命令提示符下，键入︰ 
            
        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
        
        
    1. 在打开的对话框中，键入您的设备管理员密码。 默认密码为︰
     
        `Password1`

        ![SSAdminConsole 运行空间 PowerShell 会话](./media/storsimple-create-manage-support-package/IC740962.png)

    2. 单击**确定**。
    1. 在命令提示符下，键入︰ 
        
        `Enter-PSSession $MS`


3. 在会话中打开，键入相应的命令。 


    - 对于受密码保护的网络共享，请键入︰

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        （因为被加密的支持包），将提示输入密码，网络共享的文件夹，并且加密密码的路径。 当这些参数提供时，则将指定文件夹中创建支持包。
                                            

    - 对于打开的网络共享文件夹 （那些不受密码保护的），不需要`-Credential`参数。 键入以下命令︰ 

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        将创建两个指定的网络共享文件夹中的控制器的支持包。 它是加密的压缩文件可以发送给 Microsoft 支持部门进行故障排除。 有关详细信息，请参阅[与 Microsoft 客户支持联系](storsimple-contact-microsoft-support.md)。


### 有关导出 HcsSupportPackage cmdlet 的详细信息
可用于导出 HcsSupportPackage cmdlet 的不同参数都列入表格下方。

| S。 No. | 参数            | 必需/可选 | 说明                                                                                                                                                             |
|--------|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1      | Path                 | 是否必需          | 使用提供的支持包将处于的网络共享文件夹的位置。                                                                 |
| 2      | EncryptionPassphrase | 是否必需          | 用于提供密码来加密的支持包。                                                                                                        |
| 3      | Credential           | 可选          | 使用此参数来提供网络共享文件夹的访问凭据。                                                                                        |
| 4      | Force                | 可选          | 使用跳过加密密码的确认步骤。                                                                                                                |
| 5      | PackageTag           | 可选          | 用于指定将在其中放置的支持包的路径下的目录。 默认值是 [设备名称]-[当前日期和 time:yyyy-MM-dd-HH-mm-ss]。       |
| 6      | Scope                | 可选          | 两个控制器，指定为**群集**（默认值） 来创建支持包。 如果要只为当前的控制器创建程序包，指定**控制器**。 |


## 编辑支持包

生成一个支持程序包后，您可能需要编辑包从日志文件中删除特定于客户的信息，如卷名、 设备的 IP 地址、 和备份的名称。 

> [AZURE.IMPORTANT] 您仅可以编辑为 StorSimple 生成通过 Windows PowerShell 支持包。 您不能编辑与 StorSimple 管理器服务管理门户中创建的包。 

若要在将其上载到 Microsoft 支持网站上之前编辑支持包，您需要解密支持包编辑文件，然后再次将其加密。 执行以下步骤来编辑支持包︰

#### 若要编辑 StorSimple 在 Windows PowerShell 支持包

1. 生成[创建在 StorSimple 的 Windows PowerShell 支持包](#create-a-support-package-in-windows-powershell-for-storsimple)中所述的支持包。

2. 本地客户端上的[下载该脚本](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65)。

3. 导入 Windows PowerShell 模块。 您需要指定您在其中下载脚本的本地文件夹的路径。 若要导入的模块，请键入︰
 
    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. 打开支持包文件夹。 请注意，所有文件压缩和加密的*.aes*文件。 打开的文件。 若要打开文件，请键入︰

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    这将解压缩并解密这些文件。 您将注意，所有的文件现在显示实际的文件扩展名。
    
    ![编辑支持包 3](./media/storsimple-create-manage-support-package/IC750706.png)


5. 当提示您的加密密码，请键入创建的支持包时所使用的密码。

        cmdlet Open-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:EncryptionPassphrase: ****
    
6. 导航到包含日志文件的文件夹。 当现在，日志文件的解压缩和解密，这将具有原始文件扩展名。 修改这些文件以删除卷的名称和设备的 IP 地址等的任何特定于客户的信息并保存文件。

7. 关闭该文件。 关闭文件将用 Gzip 压缩，然后使用 aes-256 进行加密。 这是出于安全和速度时通过网络传输的支持包。 若要关闭文件，请键入︰

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![编辑支持包 2](./media/storsimple-create-manage-support-package/IC750707.png)

8. 出现提示时，提供加密密码的修改的支持包。

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. 记新的密码，以便您可以与 Microsoft 技术支持请求时共享它。


### 示例︰ 编辑支持包中受密码保护的共享上的文件

下面显示了一个示例，演示如何解密、 编辑和重新加密支持包。

![编辑支持 Package1](./media/storsimple-create-manage-support-package/IC750708.png)

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1
    
        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw
    
        cmdlet Open-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:
    
        EncryptionPassphrase: ****
    
        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw
    
        cmdlet Close-HcsSupportPackage at command pipeline position 1
    
        Supply values for the following parameters:
    
        EncryptionPassphrase: ****
    
        PS C:\WINDOWS\system32>

## 下一步行动

了解如何[使用支持包和设备日志来解决您设备的部署](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)。 


