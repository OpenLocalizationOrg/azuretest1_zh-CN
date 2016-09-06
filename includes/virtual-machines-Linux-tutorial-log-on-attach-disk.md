---
ms.openlocfilehash: dc0206c17ea9ef8d31d2e4b4566a0cf19b71cfc2
ms.sourcegitcommit: bab1265d669c3e6871daa7cb8a5640a47104947a
translationtype: MT
---


## <a id="logon"> </a>如何登录到虚拟机之后创建它 ##

若要管理的虚拟机，并在计算机运行的应用程序的设置，您可以使用 SSH 客户端。 若要执行此操作，必须在您想要用来访问虚拟机的计算机上安装 SSH 客户端。 有许多可供选择的 SSH 客户端程序。 以下是可能的选项︰

- 如果您使用的计算机运行 Windows 操作系统的系统，您可以使用如 PuTTY SSH 客户端。 有关详细信息，请参阅[PuTTY 下载](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。
- 如果您使用的正在运行 Linux 操作系统的计算机，您可以使用 OpenSSH 如 SSH 客户端。 有关详细信息，请参阅[OpenSSH](http://www.openssh.org/)。

本教程展示如何使用 PuTTY 程序来访问虚拟机。

1. 从管理门户中找到的**主机名**和**端口信息**。 您可以找到您需要从虚拟机的仪表板的信息。 单击虚拟机名称，查找中的仪表板的**快速概览**部分的**SSH 详细信息**。

    ![SSH 详细信息](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/SSHdetails.png)

2. 打开的 PuTTY 程序。

3. 输入**主机名**和**端口信息**从控制板，收集，然后单击**打开**。

    ![输入主机名和端口信息](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/putty.png)

4. 登录到虚拟机使用的 NewUser1 帐户，当您创建了虚拟机添加。

    ![登录到新的虚拟机](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/sshlogin.png)

    您现在可以使用虚拟机就像使用任何其他服务器一样。


## <a id="attachdisk"> </a>如何附加到新的虚拟机的数据磁盘 ##

您的应用程序可能需要存储的数据。 若要对此进行设置，则附加到以前创建的虚拟机的数据磁盘。 若要执行此操作的最简单方法是将空数据磁盘连接到计算机。

在 Linux 上，资源磁盘是通常由 Azure Linux 代理管理，自动装载到**/mnt/resource** （或**/mnt**上 Ubuntu 的图像）。 另一方面，在 Linux 上数据磁盘的名称可以是作为内核`/dev/sdc`，和用户都需要进行分区、 格式化并装载该资源。 请[Azure Linux 代理用户指南](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md)的详细信息，参阅。

>[AZURE.NOTE] 不要将数据存储的资源磁盘上。 此磁盘提供临时存储应用程序和过程，用来存储数据，您不需要保存，如交换文件。 数据磁盘以.vhd 文件的页面 blob 中驻留 Azure 存储，并提供存储冗余，以保护您的数据。 有关详细信息，请参阅[有关磁盘和 Azure 中的图像](http://msdn.microsoft.com/library/jj672979.aspx)。

1. 如果您还没有，请登录到 Azure 管理门户。

2. 单击**虚拟机**，然后选择您先前创建的**MyTestVM1**虚拟机。

3. 在命令栏上，单击**连接**，然后单击**附加空白磁盘**。
    
    显示**附加空白磁盘**对话框。

    ![定义磁盘详细信息](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/attachnewdisklinux.png)

4. 已经为您定义的**虚拟机的名称**、**存储位置**和**文件名称**。 所有您需要做的是输入所需的磁盘的大小。 在**大小**字段中输入**5** 。

    **注意︰**在 Azure 存储中从 VHD 文件创建的所有磁盘。 您可以添加到存储，VHD 文件的名称，但磁盘的名称自动生成的。

5. 单击复选标记以将数据磁盘连接到虚拟机。

6. 您可以验证数据磁盘已成功附加到虚拟机通过仪表板查看。 单击虚拟机以显示仪表板的名称。

    磁盘数目前为该虚拟机 2 并且**磁盘**表中列出了您附加的磁盘。

    ![将附加磁盘成功](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/attachemptysuccess.png)


您只是连接到虚拟机的数据磁盘脱机并不初始化后将其添加。 您必须登录到该计算机并初始化要将其用于存储数据的磁盘。

1. 使用中**如何登录到虚拟机，您在创建后**上面列出的步骤连接到虚拟机。


2. 在 SSH 窗口中，键入下面的命令，然后输入帐户密码︰

    `sudo grep SCSI /var/log/messages`

    您可以查找最后一个显示的消息中添加的数据磁盘的标识符。

    ![确定磁盘](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskmessages.png)


3. 在 SSH 窗口中，键入下面的命令以创建一个新的设备，然后输入帐户密码︰

    `sudo fdisk /dev/sdc`

    >[AZURE.NOTE] 在此示例中，您可能需要使用`sudo -i`上如果 /sbin 或/usr/sbin 不在某些分配您`$PATH`。


4. 键入**n**以创建新的分区。

    ![创建新的设备](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskpartition.png)


5. 要建立主磁盘分区类型**1**分区，以使它的第一个分区，然后键入的类型**p**输入可接受的默认值为圆柱。

    ![创建分区](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskcylinder.png)


6. 键入**p**有关进行分区的磁盘的详细信息，请参阅。

    ![列出磁盘信息](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskinfo.png)


7. 键入**w**要写入磁盘的设置。

    ![写入磁盘的更改](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskwrite.png)


8. 您必须在新分区上创建文件系统。 例如，键入下面的命令创建了文件系统，然后输入帐户密码︰

    `sudo mkfs -t ext4 /dev/sdc1`

    ![创建文件系统](./media/virtual-machines-Linux-tutorial-log-on-attach-disk/diskfilesystem.png)

    >[AZURE.NOTE] 请注意，SUSE Linux 企业 11 系统提供的 ext4 文件系统只有只读访问权限。 对于这些系统，我们建议为 ext3 而不是 ext4 格式的新文件系统。


9. 创建一个目录来安装新的文件系统。 例如，键入下面的命令，然后键入帐户的密码︰

    `sudo mkdir /datadrive`


10. 键入以下命令以装载驱动器︰

    `sudo mount /dev/sdc1 /datadrive`

    现在可以使用**/datadrive**作为数据磁盘了。


11. 将新驱动器添加到 /etc/fstab:

    确保该驱动器已重新装入自动重新启动后它必须添加到 /etc/fstab 文件。 此外，强烈建议的 UUID （通用唯一标识符） 用于在 /etc/fstab 引用驱动器，而不是只是设备名称 (例如 /dev/sdc1)。 若要查找新驱动器的 UUID 可以使用**blkid**实用程序︰
    
        `sudo -i blkid`

    输出将类似于以下形式︰

        `/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"`
        `/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"`
        `/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"`

    >[AZURE.NOTE] blkid 可能不需要在所有情况下的 sudo 访问，但是，它可能更容易使用运行`sudo -i`上如果 /sbin 或/usr/sbin 不在某些分配您`$PATH`。

    **注意︰**不正确地编辑 /etc/fstab 文件中，可能会导致系统无法启动。 如果不确定，请参阅有关如何正确编辑此文件的信息的分发文档。 此外建议，在编辑之前创建 /etc/fstab 文件的备份。

    使用文本编辑器，输入新的文件系统在 /etc/fstab 文件末尾的信息。  在本例中我们将使用 UUID 值在前面的步骤，并装载点**/datadrive**中创建的新**/dev/sdc1**设备︰

        `UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2`

    或者，基于 SUSE Linux 系统上，您可能需要使用稍有不同的格式︰

        `/dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /   ext3   defaults   1   2`

    如果创建附加的数据驱动器或分区将需要输入/等/fstab 分别也。

    现在可以测试，只需卸载，然后即重新挂载文件系统中，通过正确装载文件系统 该示例使用装入点`/datadrive`在前面步骤中创建︰ 

        `sudo umount /datadrive`
        `sudo mount /datadrive`

    如果第二个命令将产生一个错误，请检查 /etc/fstab 文件中的，语法正确。


    >[AZURE.NOTE] 随后删除数据磁盘，而无需编辑 fstab 可能会导致虚拟机无法启动。 如果这是很常见，那么大多数版本提供以下任一`nofail`和/或`nobootwait`fstab 选项，将允许系统引导即使磁盘不存在。 请有关这些参数的详细信息，参阅您的发行版的文档。


