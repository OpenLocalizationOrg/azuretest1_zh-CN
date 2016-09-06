---
ms.openlocfilehash: 13b25aeee093ac6ed4552a4cafed80b5dc1fb42b
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties
    pageTitle="在 Windows 服务器的虚拟机上安装 MongoDB"
    description="了解如何在运行 Windows Azure 虚拟机上安装 MongoDB。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2015"
    ms.author="dkshir"/>

#在运行 Windows 服务器的虚拟机上安装 MongoDB

[MongoDB][MongoDB]是流行的开源的、 高性能 NoSQL 数据库。  使用[Azure 的门户网站][AzureManagementPortal]，您可以创建从图像库中，运行 Windows 服务器的虚拟机使用经典的部署模型。 然后，可以安装，并在虚拟机上配置 MongoDB 数据库。


## 创建运行 Windows 服务器的虚拟机

按照这些说明来创建虚拟机。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] 您可以创建虚拟机，同时为 MongoDB 中添加终结点并对其进行配置，如下所示︰ **Mongo**为其命名，使用**TCP**协议，作为将公钥和私钥端口设置为**27017**。

## 附加数据磁盘
若要为虚拟机提供存储，附加数据磁盘，然后将其初始化，以便 Windows 可以使用它。 如果您已经使用，或附加一个空磁盘所需的数据，或者可以附加现有的磁盘。

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

初始化磁盘的说明，请参见"如何︰ 初始化 Windows 服务器中的新数据磁盘"中[如何附加到 Windows 虚拟机的数据磁盘](storage-windows-attach-disk.md)。

## 安装并运行在虚拟机上的 MongoDB

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

##摘要
在本教程中，您学习了如何创建运行 Windows 服务器的虚拟机、 远程连接到它，以及如何将数据磁盘。  您还学习了如何安装和配置的基于 Windows 的虚拟机上的 MongoDB。 您现在可以在基于 Windows 的虚拟机上，访问 MongoDB，按照[MongoDB 文档][MongoDocs]中的高级的主题。

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzureManagementPortal]: http://manage.windowsazure.com
