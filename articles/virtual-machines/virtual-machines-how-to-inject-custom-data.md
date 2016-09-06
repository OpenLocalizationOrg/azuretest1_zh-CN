---
ms.openlocfilehash: eefa6b17ad598b1c020d004d66ba58f077d5df3c
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="注入到 Azure 的虚拟机的自定义数据"
    description="本主题介绍如何将自定义数据注入到 Azure 的虚拟机创建实例时，如何找到 Windows 或 Linux 上的自定义数据。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/14/2015"
    ms.author="rasquill"/>


#注入到 Azure 的虚拟机中的自定义数据

如果正在配置到 Azure 的虚拟机注入脚本或其他数据是很常见的情况下，无论操作系统是 Windows 或 Linux 分发。 本主题介绍如何︰

- 正在配置时将数据注入 Azure 的虚拟机。

- 为 Windows 和 Linux 中检索它。

- 在某些系统上使用特殊的工具来检测和自动处理自定义数据。

> [AZURE.NOTE] 本文介绍了如何自定义数据通过使用 Azure 服务管理 API 创建一个虚拟机可以被注入。 若要查看如何使用 Azure 资源管理 API，请参阅[示例模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata)。

## 注入到 Azure 的虚拟机的自定义数据

目前仅在[Azure 命令行界面](https://github.com/Azure/azure-xplat-cli)支持此功能。 尽管可以使用任何选项`azure vm create`命令，下面演示一个非常基本的方法。

```
    PASSWORD='AcceptablePassword -- more than 8 chars, a cap, a num, a special'
    VMNAME=mycustomdataubuntu
    USERNAME=username
    VMIMAGE= An image chosen from among those listed by azure vm image list
    azure vm create $VMNAME $VMIMAGE $USERNAME $PASSWORD --location "West US" --json -d ./custom-data.txt -e 22
```


## 在虚拟机中使用自定义数据

+ 如果您 Azure 的虚拟计算机是基于 Windows 的虚拟机，然后将自定义数据文件保存到`%SYSTEMDRIVE%\AzureData\CustomData.bin`。 虽然它是 base64 编码，将从本地计算机转移到新的虚拟机，但它是自动解码和可以打开或立即使用。

   > [AZURE.NOTE] 如果该文件存在，则将覆盖它。 在目录上的安全设置为**︰ 完整系统的控制**和**管理员︰ 完全控制**。

+ 如果您 Azure 的虚拟机是基于 Linux 的虚拟机，然后将以下两个位置位于自定义数据文件。 数据将是 base64 编码的因此您将需要先对数据进行解码。

    + 在 `/var/lib/waagent/ovf-env.xml`
    + 在 `/var/lib/waagent/CustomData`



## 在 Azure 的云初始化

如果将 Azure 的虚拟机从 Ubuntu 或 CoreOS 图像，则可以使用 CustomData 将云配置发送到云中初始化。 或者，如果您的自定义数据文件是一个脚本，然后云初始化可以只执行它。

### Ubuntu 云图像

在大多数的 Azure Linux 图像，您将编辑"/ etc/waagent.conf"来配置临时资源磁盘和交换文件。 有关详细信息，请参阅[Azure Linux 代理用户指南](virtual-machines-linux-agent-user-guide.md)。

但是，在 Ubuntu 的云图像，您必须使用云初始化配置资源磁盘 （即"暂时"磁盘） 和交换分区。 在 Ubuntu wiki 的更多详细信息，请参阅以下页面︰ [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions)。



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 下一步行动︰ 使用云初始化

有关详细信息，请参阅[Ubuntu 的云初始化文档](https://help.ubuntu.com/community/CloudInit)。

<!--Link references-->
[添加角色服务管理 REST API 参考](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure 的命令行界面](https://github.com/Azure/azure-sdk-tools-xplat)
