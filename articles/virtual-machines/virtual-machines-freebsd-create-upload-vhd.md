---
ms.openlocfilehash: 67736d51de832018ded1eb1630e0c45f102e8055
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties 
   pageTitle="创建并上载到 Azure FreeBSD VHD" 
   description="了解如何创建并上载 Azure 虚拟硬盘 (VHD) 包含 FreeBSD 操作系统" 
   services="virtual-machines" 
   documentationCenter="" 
   authors="KylieLiang" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services" 
   ms.date="05/19/2015"
   ms.author="kyliel"/>

# 创建并上载到 Azure FreeBSD VHD 

本文演示如何创建并上载包含 FreeBSD 操作系统，因此您可以将它用作您自己的图像可以在 Azure 中创建虚拟机 (VM) 虚拟硬盘 (VHD)。 

##先决条件##
本文假定您具有以下各项︰

- **Azure 订阅**-如果您没有，您可以在几分钟创建一个帐户。 如果您订阅了 MSDN，请参阅[MSDN 订户的 Azure 福利](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。 否则，请参阅[创建免费的试用帐户](http://azure.microsoft.com/pricing/free-trial/)。  

- **Azure PowerShell 工具**-有 Microsoft Azure PowerShell 模块安装和配置为使用您的订购。 若要下载模块，请参阅[Azure 下载](http://azure.microsoft.com/downloads/)。 这里的教程来安装和配置模块才可用。 您将使用[Azure 下载](http://azure.microsoft.com/downloads/)cmdlet 上载 VHD。

- **FreeBSD 操作系统安装的.vhd 文件中**的受支持的 FreeBSD 操作系统安装到虚拟硬盘。 存在多个工具来创建.vhd 文件的形式，例如使用如 Hyper-V 虚拟化解决方案创建.vhd 文件并安装操作系统。 有关说明，请参阅[安装 Hyper-V 角色并配置虚拟机](http://technet.microsoft.com/library/hh846766.aspx)。 

> [AZURE.NOTE] 在 Azure 中不支持较新的 VHDX 格式。 您可以将磁盘转换为使用 Hyper-V 管理器或 cmdlet[将 vhd](https://technet.microsoft.com/library/hh848454.aspx)的 VHD 格式。

此任务包括以下五个步骤。

## 第 1 步︰ 准备要上载的图像 ##

用于 FreeBSD 安装 hyper-v 中，教程，说明如何可[在此处](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx)。

从 FreeBSD 操作系统安装到虚拟机，请完成以下过程︰

1. **启用 DHCP**

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. **启用 SSH**

    从光盘安装完成后，如果默认启用 SSH。 如果不是，或者直接使用 FreeBSD VHD，请键入︰

        # echo 'sshd_enable="YES"' >> /etc/rc.conf 
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key 
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key 
        # service sshd restart

3. **设置串行控制台**

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. **安装 sudo**

    在 Azure 中禁用超级用户帐户，然后您需要利用 sudo 与非特权用户可以使用提升的权限运行命令。

        # pkg install sudo

5. Azure 的代理的系统必备组件

    **安装 python** 5.1

        # pkg install python27 py27-asn1
        # ln -s /usr/local/bin/python2.7 /usr/bin/python

    **安装 wget** 5.2

        # pkg install wget 

6. **安装 Azure 的代理**

    始终可以在[github](https://github.com/Azure/WALinuxAgent/releases)上找到 Azure 代理程序的最新版本。 版本 2.0.10 和更高版本正式支持 FreeBSD 10 和更高版本。

        # wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.10/waagent --no-check-certificate
        # mv waagent /usr/sbin
        # chmod 755 /usr/sbin/waagent
        # /usr/sbin/waagent -install

    **重要提示**︰ 安装完成后，请仔细检查其运行。

        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

    现在您可以**关闭**您的 VM。 此外可以执行第 7 步在关机之前向下，但它是可选的。

7. 重复数据消除供应是可选的。 它是清洁系统并使其适用于重新调配。

    以下命令还会删除最后一个配置的用户帐户和相关联的数据。

        # waagent –deprovision+user

## 步骤 2︰ 创建在 Azure 存储帐户。 ##

您需要在 Azure 上载.vhd 文件，以便它可用于在 Azure 创建虚拟机的存储帐户。 您可以使用 Azure 管理门户创建存储帐户。

1. 登录到 Azure 的管理门户。

2. 在命令栏上，单击**新建**。

3. 单击**数据服务** > **存储** > **快速创建**。

    ![快速创建存储帐户。](./media/virtual-machines-freebsd-create-upload-vhd/Storage-quick-create.png)

4. 请填写字段，如下所示︰
    
    - 在**URL**下键入用于在 URL 中使用的存储帐户。 该条目可以包含从 3 日至 24 日小写字母和数字。 此名称将成为用于订阅的 Blob、 队列或表资源的地址的 URL 中的主机名称。
            
    - 选择**位置或相关性组**存储帐户。 相似性组允许您将您的云服务和存储放在同一个数据中心。
         
    - 决定是否使用**地区复制**的存储帐户。 默认情况下，各地区复制打开。 此选项将数据复制到辅助位置，免费给您，以便您的存储故障转移到该位置在主站点发生重大故障时。 第二的位置，自动分配，不能更改。 如果您需要更好地控制您的云存储由于法律要求或其组织的策略位置，您可以关闭地区复制。 但是，请注意，如果您以后打开地区复制，将收取一次性数据传输费用将您现有的数据复制到辅助位置。 而地理复制存储服务均提供折扣。 管理地区复制存储帐户的详细信息可以在以下位置找到︰[创建、 管理或删除存储帐户](../storage-create-storage-account/#replication-options)。

    ![输入存储帐户详细信息](./media/virtual-machines-freebsd-create-upload-vhd/Storage-create-account.png)


5. 单击**创建存储帐户**。 该帐户现在显示在**存储**下。

    ![已成功创建存储帐户](./media/virtual-machines-freebsd-create-upload-vhd/Storagenewaccount.png)

6. 对于您上载 Vhd，接下来，创建一个容器。 单击存储帐户的名称，然后单击**容器**。

    ![存储帐户详细信息](./media/virtual-machines-freebsd-create-upload-vhd/storageaccount_detail.png)

7. 单击**创建一个容器**。

    ![存储帐户详细信息](./media/virtual-machines-freebsd-create-upload-vhd/storageaccount_container.png)

8. 键入您的容器的**名称**，然后选择**访问**策略。

    ![容器名称](./media/virtual-machines-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] 默认情况下，容器是私有的并可以仅通过帐户所有者。 要允许公共的 blob 容器，但不是容器属性和元数据的读取访问权限，请使用"公钥 Blob"选项。 要允许完整公钥容器和 blob 的读取访问权限，请使用"公共容器"选项。

## 第 3 步︰ 准备到 Microsoft Azure 的连接 ##

您可以上载.vhd 文件之前，您需要建立您的计算机和您的订购 Azure 之间的安全连接。 您可以使用 Microsoft Azure Active Directory 方法或证书方法要这样做。

###使用 Microsoft Azure 的广告方法

1. 打开 Azure PowerShell 控制台。

2. 键入以下命令︰  
    `Add-AzureAccount`
    
    此命令会打开登录窗口，以便您可以与您的工作或学校帐户签名。

    ![PowerShell 窗口](./media/virtual-machines-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure 进行身份验证并保存的凭据信息，然后关闭该窗口。

###使用证书的方法

1. 打开 Azure PowerShell 控制台。 

2. 类型︰  `Get-AzurePublishSettingsFile`。

3. 打开一个浏览器窗口，并提示您下载的.publishsettings 文件。 它包含信息以及 Microsoft Azure 订阅的证书。

    ![浏览器下载页面](./media/virtual-machines-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. 将.publishsettings 文件保存。 

4. 类型︰ `Import-AzurePublishSettingsFile <PathToFile>`

    其中`<PathToFile>`到.publishsettings 文件的完整路径。 

   有关详细信息，请参阅[Microsoft Azure Cmdlet 入门](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx) 
    
   有关安装和配置 PowerShell 的详细信息，请参阅[如何安装和配置 Microsoft Azure PowerShell](../install-configure-powershell.md)。 

## 步骤 4︰ 将.vhd 文件上载 ##

在上载.vhd 文件时，可以在 blob 存储内任意位置.vhd 文件。 在下面的命令示例中， **BlobStorageURL**是一个 URL，您在步骤 2 中创建的存储帐户， **YourImagesFolder**是 blob 存储中的容器要存储图像的位置。 **VHDName**是在管理门户来标识虚拟硬盘中显示的标签。 **PathToVHDFile**是完整路径和名称的.vhd 文件。 


1. 从前面的步骤中使用 Azure PowerShell 窗口，请键入︰

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>        

## 步骤 5︰ 创建一个虚拟机上载 VHD ##
.Vhd 上载后，可以为图像添加到列表中的与订阅相关的自定义图像，并使用此自定义映像创建虚拟机。

1. 从前面的步骤中使用 Azure PowerShell 窗口，请键入︰

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    **重要提示**︰ 请使用 Linux 操作系统现在因为 Azure PowerShell 新版只接受"Linux"或"Windows"作为参数类型。

2. 完成上述步骤后，当选择 Azure 管理门户上的**图像**选项卡列出新的映像。  

    ![添加图像](./media/virtual-machines-freebsd-create-upload-vhd/addfreebsdimage.png)

3. 从库中创建的虚拟机。 此新图像现下**我的图像**。 选择新的图像，并且经过提示设置主机名、 密码/SSH 密钥和等。 

    ![自定义图像](./media/virtual-machines-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. 完成设置后，您将看到在 Azure 中运行 FreeBSD VM。 

    ![freebsd 在 azure 中的图像](./media/virtual-machines-freebsd-create-upload-vhd/freebsdimageinazure.png)
 
