---
ms.openlocfilehash: 47973920e2539120dd212e91efccb0f0c7315842
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---

有关磁盘的详细信息，请参阅[有关磁盘和虚拟机的 Vhd](../articles/virtual-machines-disks-vhds.md)。

##<a id="attachempty"></a>如何︰ 附加一个空的磁盘
附加一个空的磁盘是更简单的方法添加数据磁盘，因为 Azure 将.vhd 文件为您创建并将其存储在存储帐户。

1. 单击**虚拟机**，然后选择相应的虚拟机。

2. 在命令栏上，单击**连接**，然后单击**附加空白磁盘**。


    ![附加一个空的磁盘](./media/howto-attach-disk-window-linux/AttachEmptyDisk.png)

3.  **附加一个空磁盘**对话框中出现。


    ![添加新的空磁盘](./media/howto-attach-disk-window-linux/AttachEmptyDetail.png)


    请执行以下操作︰

    - 在**文件名**接受默认名称，或键入另一个名称为.vhd 文件，用于磁盘。 数据磁盘使用一个自动生成的名称，即使您键入.vhd 文件的另一个名称。

    - 键入数据磁盘的**大小 (GB)** 。

    - 单击复选标记来完成。

4.  创建并附加数据磁盘后，列为在仪表板中的虚拟机。

    ![已成功附加的空数据磁盘](./media/howto-attach-disk-window-linux/AttachEmptySuccess.png)

##<a id="attachexisting"></a>如何︰ 附加现有的磁盘

附加一个现有磁盘需要您有存储帐户中.vhd。 使用[添加 AzureVhd](http://go.microsoft.com/FWLink/p/?LinkID=391684) cmdlet 将.vhd 文件上载到的存储帐户。 您已经创建并上载.vhd 文件之后，可以将它附加到虚拟机。

1. 单击**虚拟机**，然后选择相应的虚拟机。

2. 在命令栏上，单击**连接**，然后选择**附加磁盘**。


    ![附加数据磁盘](./media/howto-attach-disk-window-linux/AttachExistingDisk.png)

    显示**附加磁盘**对话框。



    ![输入数据磁盘详细信息](./media/howto-attach-disk-window-linux/AttachExistingDetail.png)

3. 选择您要附加到虚拟机的数据磁盘。

4. 单击复选标记以将数据磁盘连接到虚拟机。

5.  附加数据磁盘后，它将列出在仪表板中的虚拟机。


    ![已成功附加数据磁盘](./media/howto-attach-disk-window-linux/AttachExistingSuccess.png)

> [AZURE.NOTE]
> 添加数据磁盘后，您将需要登录到虚拟机并初始化该磁盘，以便虚拟机可以使用该磁盘的存储。
