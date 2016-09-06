---
ms.openlocfilehash: 26b5490328952d4873bb9f09c8a12b3e40bac3fb
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties writer="kathydav" editor="tysonn" manager="timlt" />

当不再需要连接到虚拟机的数据磁盘时，您可以很容易地进行分离。 这从虚拟机中，删除磁盘，但不将其从存储中删除。 如果要再次使用磁盘上的现有数据，可以将它重新连接到同一个虚拟机或另一个。  

> [AZURE.NOTE] Azure 中的虚拟机使用不同类型的磁盘-操作系统磁盘、 本地临时磁盘和可选的数据磁盘。 数据磁盘是推荐的方式来存储虚拟机的数据。 有关详细信息，请参阅[有关磁盘和虚拟机的 Vhd](../../virtual-machines-disks-vhds.md)。 不能分离操作系统磁盘，除非您还删除虚拟机。

## 找到磁盘##

如果您不知道磁盘的名称，或想要使其脱离对象之前，请验证它，请按照下列步骤。

> [AZURE.NOTE] Azure 自动当您将它附加到磁盘指定一个名称。 名称由云服务名称、 虚拟机的名称和一个数字组成。

1. 如果您还没有登录到[Azure 门户](http://manage.windowsazure.com)的这么做。

2. 单击**虚拟机**，请单击虚拟机的名称，然后单击**仪表板**。

3. 在**磁盘**上下, 表列出的名称和类型的连接的所有磁盘。 例如，此屏幕将显示一个操作系统 (OS) 磁盘上，一个数据磁盘的虚拟机︰

    ![查找数据磁盘](./media/howto-detach-disk-windows-linux/FindDataDisks.png)


## 分离磁盘##

查找磁盘的名称后，您就可以将其分离︰

1. 单击**虚拟机**，请单击具有您要分离，数据磁盘的虚拟机的名称，然后单击**仪表板**。
2. 从命令栏中，单击**分离磁盘**。

3. 选择数据的磁盘，然后单击复选标记以使其脱离对象。

    ![分离磁盘详细信息](./media/howto-detach-disk-windows-linux/DetachDiskDetails.png)

磁盘存储中仍保留，但不再连接到虚拟机。
