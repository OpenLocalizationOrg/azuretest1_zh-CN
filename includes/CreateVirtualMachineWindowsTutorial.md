---
ms.openlocfilehash: a9257fb5a3d3d4e8fd07e63dc63853e108dd7349
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---
<properties title="Create a Virtual Machine Running Windows Server" pageTitle="如何创建虚拟机运行 Windows 服务器" description="描述如何创建 Windows 虚拟机、 添加数据的磁盘，然后远程登录" metaKeywords="" services="virtual machines" solutions="" documentationCenter="" authors="kathydav" videoId="" scriptId="" />

# 创建运行 Windows 服务器的虚拟机 #

本教程将介绍创建运行 Windows 服务器上，使用 Windows Azure 管理门户中的图像库 Azure 虚拟机是多么容易。 图像库提供不同的图像，包括 Windows 操作系统、 基于 Linux 的操作系统和应用程序的图像。 

> [AZURE.NOTE] 您不需要任何体验与 Azure 虚拟机来完成本教程。 但是，您需要一个 Azure 帐户。 您可以免费试用帐户中只需几分钟。 有关详细信息，请参阅[创建 Azure 帐户](http://www.windowsazure.com/develop/php/tutorials/create-a-windows-azure-account/)。 

本教程演示︰

- [如何创建虚拟机](#createvirtualmachine)
- [如何登录到虚拟机之后创建它](#logon)
- [如何附加到新的虚拟机的数据磁盘](#attachdisk)

如果您想要了解的详细信息，请参阅[虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=271224)。


##<a id="createvirtualmachine"> </a>如何创建虚拟机##

本节说明了如何在管理门户中使用**剪辑库中的**选项来创建虚拟机。 此选项提供了更多比**快速创建**选项的配置选项。 例如，如果您想要将虚拟机加入到虚拟网络，您需要使用**剪辑库中的**选项。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../includes/virtual-machines-create-WindowsVM.md)]

## <a id="logon"> </a>如何登录到虚拟机之后创建它 ##

本节说明了如何登录到虚拟机使您可以管理它的设置，并将在其运行的应用程序。

1. 登录到 Azure 的[管理门户](http://manage.windowsazure.com)。

2. 单击**虚拟机**，然后选择**MyTestVM**虚拟机。

    ![选择 MyTestVM](./media/CreateVirtualMachineWindowsTutorial/selectvm.png)

3. 在命令栏上，单击**连接**。

    ![连接到 MyTestVM](./media/CreateVirtualMachineWindowsTutorial/commandbarconnect.png)
    
4. 单击**打开**要用于虚拟机自动创建的远程桌面协议文件。

    ![打开 rdp 文件](./media/CreateVirtualMachineWindowsTutorial/openrdp.png)
    
5. 单击**连接**。

    ![继续进行连接](./media/CreateVirtualMachineWindowsTutorial/connectrdc.png)

6. 在密码框中，键入用户名和密码时创建虚拟机，您指定，然后单击**确定**。

7. 单击**是**以验证虚拟机器的身份。

    ![验证计算机的身份](./media/CreateVirtualMachineWindowsTutorial/certificate.png)

    您现在可以使用虚拟机就像一台服务器在您的办公室。

## <a id="attachdisk"> </a>如何附加到新的虚拟机的数据磁盘 ##

这一节演示了如何将一个空数据磁盘连接到虚拟机。 请参见 [附加数据磁盘教程] (.../ articles/virtual-machines/storage-windows-attach-disk.md) 的附加空磁盘以及如何将现有的磁盘的详细信息。

1. 登录到 Azure 的[管理门户](http://manage.windowsazure.com)。

2. 单击**虚拟机**，然后选择**MyTestVM**虚拟机。

    ![选择 MyTestVM](./media/CreateVirtualMachineWindowsTutorial/selectvm.png)
    
3. 您可能会被带到快速启动页第一。 如果是这样，从顶部选择**仪表板**。

    ![选择仪表板](./media/CreateVirtualMachineWindowsTutorial/dashboard.png)

4. 在命令栏上，单击**连接**，然后单击**附加空白磁盘**时弹出。

    ![从命令栏中选择附加](./media/CreateVirtualMachineWindowsTutorial/commandbarattach.png) 

5. **虚拟机名称**、**存储位置**、**文件名**和**主机缓存首选项**已为您定义。 所有您需要做的是输入所需的磁盘的大小。 在**大小**字段中输入**5** 。 然后单击复选标记以将空磁盘连接到虚拟机。

    ![指定空磁盘的大小](./media/CreateVirtualMachineWindowsTutorial/emptydisksize.png)    
    
    >[AZURE.NOTE] 从 Windows Azure 存储中的 VHD 文件创建的所有磁盘。 在**文件的名称**，您可以添加到存储，VHD 文件的名称，但 Azure 将自动生成磁盘的名称。

6. 返回控制板以验证空数据磁盘已成功附加到虚拟机。 它将列为 OS 磁盘**的磁盘**列表中的第二个磁盘。

    ![附加的空磁盘](./media/CreateVirtualMachineWindowsTutorial/disklistwithdatadisk.png)

    将数据磁盘连接到虚拟机之后，磁盘脱机，无法初始化。 您可以使用它来存储数据之前，您需要登录到虚拟机并初始化该磁盘。

7. 通过使用上一节[如何登录到虚拟机创建它后](#logon) 中的步骤连接到虚拟机。

8. 登录到虚拟机后，打开**服务器管理器**。 在左窗格中，选择**文件和存储服务**。

    ![展开文件和存储服务在服务器管理器](./media/CreateVirtualMachineWindowsTutorial/fileandstorageservices.png)

9. 从展开的菜单中选择**磁盘**。

    ![展开文件和存储服务在服务器管理器](./media/CreateVirtualMachineWindowsTutorial/selectdisks.png)  
    
10. 在**磁盘**部分中，有三个磁盘的列表中︰ 磁盘 0、 1、 磁盘和磁盘 2。 磁盘 0 是 OS 磁盘、 磁盘 1 是临时资源磁盘 （它不应该使用的数据存储），和磁盘 2 是具有附加到虚拟机的数据磁盘。 请注意，数据磁盘容量为 5GB 指定前面。 用鼠标右键单击磁盘 2，然后选择**初始化**。

    ![开始初始化](./media/CreateVirtualMachineWindowsTutorial/initializedisk.png)

11. 单击**是**以启动初始化进程。

    ![继续初始化](./media/CreateVirtualMachineWindowsTutorial/yesinitialize.png)

12. 再次右键单击磁盘 2，并选择**新的卷**。 

    ![创建卷](./media/CreateVirtualMachineWindowsTutorial/initializediskvolume.png)

13. 完成向导，使用提供的默认值。 完成该向导后，将**卷**一节中列出新的卷。 

    ![创建卷](./media/CreateVirtualMachineWindowsTutorial/newvolumecreated.png)

    磁盘现在处于联机状态并且准备使用新的驱动器号。 
    
##下一步行动 

若要了解有关在 Azure 上配置 Windows 虚拟机的详细信息，请参阅下列文章︰

[如何连接在云服务中的虚拟机](../articles/virtual-machines/cloud-services-connect-virtual-machine.md)

[如何创建并上载自己包含 Windows 服务器操作系统的虚拟硬盘](../articles/virtual-machines/virtual-machines-create-upload-vhd-windows-server.md)

[连接到虚拟机的数据磁盘](../articles/virtual-machines/storage-windows-attach-disk.md)

[管理虚拟机的可用性](../articles/manage-availability-virtual-machines.md)

[有关在 Azure 中的虚拟机]: #virtualmachine
[如何创建虚拟机]: #custommachine
[如何登录到虚拟机之后创建它]: #logon
[如何附加到新的虚拟机的数据磁盘]: #attachdisk
[如何设置与虚拟机进行通信]: #endpoints


